# 워터마크 세션 토큰 발급 ver2

## 1. 개요

이 프로젝트는 실시간 워터마크 세션 생성 시 발생하던 **토큰 발급 병목 현상**을 해결하기 위해 설계되었습니다.  
기존 RDB 기반의 동기 처리 구조를 개선하고, 서버리스 아키텍처와 비선형 인덱스 알고리즘을 적용하여  
**30% 이상의 응답 속도 향상**, **100% 중복 방지**, **확장성 확보**를 달성했습니다.

---

## 2. 문제

- 동시 요청 시 토큰 발급 처리 병목 발생
- RDB 기반의 동기식 구조로 인해 TPS 한계 도달
- 중복 방지를 위해 락 처리 또는 중복 체크 필요 → 응답 지연

---

## 3. 해결 방안

- Redis에 **미리 생성된 토큰 큐**를 적재하여 DB 접근 없이 즉시 응답
- **AWS Lambda**를 통해 비동기적으로 토큰을 생성 및 저장
- **중복 검사가 필요 없는 토큰 생성 알고리즘** 도입
- **CloudWatch + SNS**를 통해 Redis 상태를 모니터링하고 자동으로 토큰을 보충

---

## 4. 시스템 아키텍처


**토큰 발급 흐름 요약:**
1. 사용자가 API를 통해 토큰 요청
2. Redis에서 선발급된 토큰을 즉시 반환
3. SQS Message -> Lambda 호출
4. Lambda에서 토큰 정보 RDB에 저장
> 아래는 시스템 전체 흐름을 시각화한 설계도입니다:
![session1.png](session1.png)
**토큰 생성 흐름 요약:**
1. Redis 메모리 사용량이 200MB 미만으로 떨어질 경우 → CloudWatch 경고
2. SNS Topic → 단일 Lambda 호출
3. Lambda에서 토큰 생성 후 Redis에 저장
> 아래는 시스템 전체 흐름을 시각화한 설계도입니다:
![session_2.png](session_2.png)
---

## 5. 중복 없는 토큰 생성 알고리즘 (비선형 전단 변환 기반)

### 왜 이 알고리즘이 필요한가?

기존 방식에서는 랜덤 토큰을 생성하고, 중복 방지를 위해 RDB에서 확인이 필요했습니다.  
하지만 이는 다음과 같은 문제를 일으켰습니다:

- 동시 요청이 많은 환경에서 **중복 검사로 인한 병목**
- **락 처리 또는 트랜잭션 처리**로 TPS 감소

### 해결 방식

우리는 **비선형 전단 변환(Skew Transform)** 을 사용해  
**index 기반의 충돌 없는 고유 토큰**을 생성합니다.

```python
class PseudoRandomGenerator:
    def __init__(self, a, b):
        self.a = a
        self.b = b
    def skew_transform(self, x):
        return ((self.a ^ x) + (self.b * x)) % (2**56)
    def get_number(self, index):
        if 0 <= index < 2**56:
            return self.skew_transform(index)