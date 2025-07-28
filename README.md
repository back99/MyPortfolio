# Hi, I'm Seung Bum Jung, a Backend Developer

## About Me

Hello! I’m Seung Bum Jung — a backend engineer with several years of experience. I’m currently pursuing a Master’s in Computer Science at Saint Louis University.

I specialize in designing scalable backend systems with a strong focus on system architecture, distributed infrastructure, and real-world service reliability.

Before grad school, I worked as a backend developer where I contributed to building scalable media processing pipelines and improving performance in high-traffic environments.

At SLU, I’m expanding my skills in advanced topics like cloud-native systems, microservices, and event-driven architecture. My goal is to combine hands-on industry experience with deep academic knowledge to become a backend architect capable of solving complex engineering problems at scale.

I believe in writing clean, maintainable code, building systems that scale, and collaborating with people who care about clean systems and great products.

---
## 🔧 Experience

### ✅ Distributed Transcoding [📄 Read More](Distributed_transcoding)
- Designed a scalable video transcoding system to replace Hybrik, reducing costs by 50% and improving throughput by 30%.
- Deployed parallel transcoding pods on AWS EKS based on GOP-aware segmentation.
- Used Redis for job queuing and failure recovery; modularized packaging and media services for maintainability.  
  **Stack:** Kotlin, Spring Boot, AWS EKS, Redis, Docker, FFmpeg

### ✅ Watermark Session Token Optimization [📄 Read More](Watermark_session_token_ver2)
- Rebuilt token generation with Redis and AWS Lambda for low-latency issuance (↓30%).
- Implemented a globally unique token strategy across Seoul, Oregon, and Frankfurt.
- Integrated event-based regeneration via SNS and CloudWatch, enabling 5M+ token support.  
  **Stack:** Kotlin, Redis, AWS Lambda, CloudWatch, SNS

### ✅ Legacy API Refactoring & Kotlin Migration
- Refactored legacy Java APIs to Kotlin with DDD structure and handler-resolver pattern.
- Improved interface design, testability, and async communication via Kafka.
- Boosted developer onboarding speed and deployment maintainability.  
  **Stack:** Kotlin, Spring Boot, Kafka, DDD

### ✅ CMS System Optimization
- Refactored Redis caching and pub/sub architecture for collaboration tools.
- Reduced CMS latency and increased cache hit ratio by cleaning stale keys and streamlining logic.
- Modularized chat, alarm, and account services for future scalability.  
  **Stack:** Java, Redis, Spring Boot, Pub/Sub

## Education

- **Master’s in Computer Science (in progress)**  
  *(Saint Louis University, USA, 2024–2026 expected)*

- **B.S. in Computer Engineering, Ajou University, South Korea**
