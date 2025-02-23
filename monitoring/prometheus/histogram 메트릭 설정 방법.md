### application.yml 설정
```yaml
management:
  endpoints:
    web:
      exposure:
        include: "prometheus"
  metrics:
    enable:
      http.server.requests: true
    distribution:
      percentiles:
        http.server.requests: 0.5, 0.95, 0.99
      percentiles-histogram:
        http.server.requests: true
```

<br>

### percentiles 설정 정보
출처 : https://docs.spring.io/spring-boot/docs/2.0.5.RELEASE/reference/html/production-ready-metrics.html
<img width="1161" alt="Image" src="https://github.com/user-attachments/assets/6ae6d6c6-97ec-4a60-8fc9-cea460950a23" />