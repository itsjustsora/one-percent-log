## 01. 신뢰할 수 있고 확장 가능하며 유지보수하기 쉬운 애플리케이션

### 새롭게 알게된 점(New)

`성능 기술하기(13p)` 문단에서 응답 시간의 평균과 백분위를 다루는 부분입니다.

<br>

**[1] 개념 사전**

| 단어 | 의미 |
| --- | --- |
| 팬 아웃(fan-out) | 다른 게이트의 출력에 배속된 노리 게이트 입력의 수 (트랜잭션 처리 시스템에서 하나의 수신 요청을 처리하는 데 필요한 다른 서비스의 요청 수를 설명하기 위해 사용) |
| 처리량(throughput) | 초당 처리할 수 있는 레코드 수나 일정 크기의 데이터 집합으로 작업을 수행할 때 걸리는 전체 시간 |
| 응답 시간(response time) | 클라이언트가 요청을 보내고 응답을 받는 사이의 시간(네트워크 지연, 큐 지연 포함) |
| 지연 시간(latency) | 요청이 처리되길 기다리는 시간, 서비스를 기다리며 휴지(latent) 상태인 시간 |
| 산술 평균(arithmetic mean) | n개 값이 주어지면 모든 값을 더하고 n으로 나눔 |
| 꼬리 지연 시간(tail latency) | 상위 백분위 응답 시간  |
| 선두 차단(head-of-line blocking) | 소수의 느린 요청 처리만으로 후속 요청 처리가 지체되는 것 |
| 우발적 복잡도(accidental complexity) | 소프트웨어가 풀어야 할 (사용자에게 보이는) 문제에 내재하고 있지 않고 구현에서만 발생하는 것 |

<br>

**[2] 응답 시간 계산법**

- 응답 시간은 단일 숫자가 아니라 측정 가능한 값의 분포로 생각해야 한다.
- 사용자가 보통 얼마나 오랫동안 기다려야 하는지 알고 싶다면 중앙값이 좋은 지표다.
    - 중앙값은 단일 요청을 참고한다.
    - 사용자가 여러 개의 요청을 보내면 최소한 하나의 요청이 중앙값보다 느릴 확률이 50%보다 훨씬 높다.
- 일반적으로 상위 백분위에서 사용하는 백분위는 `95분위`, `99분위`, `99.9분위`이다.
    - 요청의 95%, 99%, 99.9%가 특정 기준치보다 더 빠르면 해당 특정 기준치가 각 백분위의 응답 시간 기준치가 된다.

<br>

**[3] 상위 백분위 응답시간**

- `꼬리 지연 시간(tail latency)`는 서비스의 사용자 경험에 직접 영향을 주기 때문에 중요하다.
- 99.9분위 같은 최상위 백분위는 통제할 수 없는 임의 이벤트에 쉽게 영향을 받기 때문에 응답 시간을 줄이기가 매우 어렵다.

<br>

**[4] 실전 백분위**

- 병렬로 호출하더라도 병렬 호출 중 가장 느린 호출이 완료되길 기다려야 하기 때문에 그 하나로 인해 전체 최종 사용자의 요청이 느리게 될 수 있다.
- 서비스의 모니터링 대시보드에 응답 시간 백분위를 추가하려면 지속적으로 백분위를 효율적으로 계산할 필요가 있다.
    - 방법 1. 단순한 구현으로 시간 구간 내 모든 요청의 응답 시간 목록을 유지하고 1분마다 목록을 정렬
    - 방법 2. 상황에 따라 포워드 디케이(forward decay), T 다이제스트(t-digest), Hdr히스토그램(hdrHistogram) 같은 CPU와 메모리 비용을 최소로 하면서 좋은 백분위 근사치를 계산할 수 있는 알고리즘 사용
- 응답 시간 데이터를 집계하는 올바른 방법은 히스토그램을 추가하는 것이다.

<br>

### 어려웠거나 이해하지 못한 부분(Difficulty)

`부하 기술하기(11p)`에서 다룬 트위터 사례입니다. 팬 아웃에 대한 개념이 낯설어서인지 접근 방식 2의 데이터 파이프라인이 아직 이해가 잘 가지 않았습니다.

> **Point**
> 
> 평균적으로 트윗 게시 요청량이 홈 타임 라인 읽기 요청량에 비해 수백 배 적다.

- 트위터의 확장성 문제는 주로 트윗 양이 아닌 팬 아웃 때문이다.
- 트위터 사례에서 사용자당 팔로워의 분포는 팬 아웃 부하를 결정하기 때문에 확장성을 논의할 때 핵심 부하 매개변수가 된다.

<br>

### 추가 내용(Amendment): 학습이 도움이 되었던 블로그, 유튭, 사례 등

**[1] 카오스 몽키**
참고:  [카카오 장애가 우리에게 던진 세가지 질문](https://brunch.co.kr/@brunchgpjz/43)
- '야생의 원숭이가 무장을 한채 우리 데이터센터에 난동을 부려도 우리 서비스는 살아남을 수 있을까?' 
- 서비스에 장애를 주입해 서비스가 견디는지를 보는 것을 의미한다.

<br>

**[2] Prometheus 메트릭 타입**  
참고: [[Chapter 05 | Observability] Prometheus 이해하기](https://jominseoo.tistory.com/77)
- `Counter` : 축적되는 단방향으로 증가하는 숫자 메트릭 
  - 앱이 재기동되면 다시 0부터 시작한다.
  - e.g. number of requests, number of tasks completed etc
- `Gauge` : 증가하거나 감소할 수 있는 숫자 값을 나타낼 때 쓰는 메트릭
  - e.g. current temperatures, active thread count etc
- `Histogram` : 관찰 대상에 대해서 샘플링해서 버킷 별로 구분 및 저장하고 조회할 수 있는 메트릭
  - e.g. database I/O latency = 0.99, 0.95, 0.50 percentile etc
- `Summary` : 히스토그램과 비슷하게 관찰 대상에 대해서 샘플링을 하며, 관찰 대상에 대한 Total count 혹은 관찰 값에 대한 sum도 제공한다.
- Histogram vs Summary
  - aggregate가 필요할 때 -> Histogram
  - aggregate 필요 X & value의 범위, 분포가 필요할 때 -> Histogram
  - aggregate 필요 X & value의 범위, 분포 X & 정확한 quantile 필요 -> Summary

<br>

**[3] fan-out**
> 한 개의 요청이 여러 개의 작업을 유발하는 정도
- 트위터에서 팬 아웃 부하를 이야기 하는 것은, 팔로우 기반의 SNS이기 때문이다. 한 사용자의 트윗이 N명의 팔로워들에게 전달하는 과정에서 부하가 생길 수 있다.
