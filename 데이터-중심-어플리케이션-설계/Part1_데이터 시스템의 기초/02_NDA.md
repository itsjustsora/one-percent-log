## 02. 데이터 모델과 질의 언어

> 추후 수정본 업로드 예정

<br>

> 내 언어의 한계는 내 세계의 한계를 의미한다.  
> 
> 루트비히 비트겐슈타인, 논리-철학 논고(1922)

<br>

**[개념 사전]** 

| 단어 | 의미 |
| --- | --- |
| 다중 저장소 지속성(polyglot persistence) | 관계형 데이터베이스가 폭넓은 다양함을 가진 비관계형 데이터스토어와 함께 사용될 것이라는 개념 |
| 임피던스 불일치(impedance mismatch) | 객체지향 프로그래밍과 관계형 데이터베이스 모델 객체의 패러다임이 상충되는 것 |
| 문서 참조(document reference) | 문서 모델에서 의존 관계를 표현할 때 참조하는 고유한 식별자 |
| 슈레딩(shredding) 관계형 기법 | 문서와 비슷한 구조를 여러 테이블로 나누는 것  |
| 쓰기 스키마(schema-on-write) | 관계형 데이터베이스의 전통적인 접근 방식으로 스키마는 명시적이고 데이터 베이스는 쓰여진 모든 데이터가 스키마를 따르고 있음을 보장한다. |
| 읽기 쓰키마(schema-on-read) | 데이터 구조는 암묵적이고 데이터를 읽을 때만 해석된다. |
| 맵 리듀스(MapReduce) | 많은 컴퓨터에서 대량의 데이터를 처리하기 위한 프로그래밍 모델 |

<br>

### 새롭게 알게된 점(New)

**[1] 관계형 모델의 탄생**

- 1970년대에 많이 사용한 데이터베이스인 `IMS(Information Management System)`의 데이터 모델은 `계층 모델`이다.
- 계층 모델은 JSON 구조처럼 레코드 내에 중첩된 레코드 트리로 표현한다.
- 계층 모델의 한계를 위해 고안된 것들 중에 `관계형 모델`과 `네트워크 모델`이 양대산맥이었다.

```text
네트워크 모델

- 네트워크 모델에서 레코드는 다중 부모가 있을 수 있다.
- 레코드 간 연결은 외래 키보다는 프로그래밍 언어의 포인트와 더 비슷하다.
- 레코드에 접근하는 유일한 방법은 최상위 레코드(root record)에서부터 연속된 연결 경로를 따르는 방법이다.
```
⇒ 그런데, 관계형 모델이 대세가 된 이후에 또 JSON 구조의 NoSQL이 등장하게 된 것이 흥미롭다.

<br>

**[2] MySQL의 ALTER TABLE 성능 저하**

MySQL의 alter table 시에 전체 테이블을 복사 → 성능 저하의 위험이 있다.

> *MySQL 에서 테이블이 아주 큰 경우에 ALTER TABLE 은 성능 문제를 일으킬 수 있다. MySQL 에서 대부분의 변경 작업은 원하는 구조를 갖춘 빈 테이블을 만들고, 이 테이블에 기존 테이블의 데이터를 삽입한 뒤 기존 테이블을 삭제하는 식으로 수행한다. 테이블이 크거나 인덱스가 많을 때 그리고 메모리가 부족한 경우라면 이런 작업은 특히나 오래 걸릴 수 있다. 심지어 ALTER TABLE 연산에 수시간, 수일을 보낸 사람들도 많다.*  
> **<MySQL 성능 최적화 도서>, p167**

</aside>

<br>

**[3] 지역성 관리**

- 구글 Spanner 데이터베이스의 지역성 관리 기법

부모 테이블(Users) 내에 자식 테이블(Orders)의 데이터를 중첩 저장할 수 있다.

```java
Users
 ├── user_id: 1 (헤일리)
 │     ├── Orders (order_id: 101, item: 스마트폰)
 │     ├── Orders (order_id: 102, item: 키보드)
 │
 ├── user_id: 2 (더그)
 │     ├── Orders (order_id: 103, item: 모니터)
```

<br>

### 어려웠거나 이해하지 못한 부분(Difficulty)

맵 리듀스는 선언형 질의 언어도 완전한 명령형 질의 API도 아닌 그 중간 정도에 있다.

<br>

### 추가 내용(Amendment)

**선언형 질의 언어 vs 명령형 질의 API**

| 종류 | 의미 | 예 |
| --- | --- | --- |
| 선언형 질의 언어(Declarative Query Language) | 사용자가 무엇을 할지 기술, 어떻게 할지는 시스템이 알아서 최적화하여 실행하는 방식 | SQL |
| 명령형 질의 API(Imperative Query API) | 사용자가 무엇을 할지, 어떻게 처리할지 직접 명령하는 방식 | 전통적인 프로그래밍 언어(Java, Python 등) |

<br>

**맵 리듀스가 “그 중간”이라 설명되는 이유**

> 사용자는 `맵`과 `리듀스` 연산 수행 지시를 함과 동시에 무엇을 할지 내부 로직을 일부 구현해야 한다. 그리고 시스템이 내부적으로 분산 실행을 최적화한다.
>

```java
public static class Map extends Mapper<LongWritable,Text,Text,IntWritable> {
 
	public void map(LongWritable key, Text value, Context context) throws IOException,InterruptedException {
		String line = value.toString();
		StringTokenizer tokenizer = new StringTokenizer(line);
		while (tokenizer.hasMoreTokens()) {
		value.set(tokenizer.nextToken());
		context.write(value, new IntWritable(1));
	}
}
```