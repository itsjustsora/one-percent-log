## [Adapter Pattern](https://www.inflearn.com/course/lecture?courseSlug=%EC%9E%90%EB%B0%94-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4&unitId=3173)

### ⛳️ 학습 목표
- 알고리즘을 요구사항에 맞춰 사용할 수 있다.

---

### Adapter Pattern
**클래스의 인터페이스를 클라이언트가 원하는 다른 인터페이스로 변환하는 패턴**이다. 즉, 다른 인터페이스를 가진 클래스의 서비스를 사용하면서 클라이언트 요구 사항에 맞는 인터페이스를 제공하는 것이다.

<br>

**[구성]** 
- `Target Interface` : 클라이언트가 사용할 인터페이스 클래스 
- `Adapter class` : 원하는 인터페이스를 구현하고 특정 요청사항을 수행하는 클래스
- `Client` : Adapter 클래스와 상호 작용하는 클래스

<br>

**[예제 코드]**    
Math 클래스에 이미 구현된 메서드(twiceOf(), halfOf())를 기존의 double 이 아닌 Float 으로 사용하고 싶을 때의 예제 코드이다.

***Math.class - 이미 구현된 기능을 가진 Class***
```java
public class Math {
	public static double twiceOf(double num) {
		return num * 2;
    }
	
	public static double halfOf(double num) {
		return num / 2;
    }
}
```

<br>

***Target Interface - Adapter***
```java
public interface Adapter {
	// 원하는 기능
	public Float twiceOf(Float f);
	public Float halfOf(Float f);
}
```

<br>

***Adapter Class - AdapterImpl***
```java
public class AdapterImpl implements Adapter {
	@Override
	public Float twiceOf(Float f) {
		return (float) Math.twoTime(f.doubleValue());
	}

	@Override
	public Float halfOf(Float f) {
		return (float) Math.half(f.doubleValue());
	}
}
```

<br>

***Client Class - AdapterPatternDemo***
```java
public class AdapterPatternDemo {
	public static void main(String[] args) {
		Adapter adapter = new AdapterImpl();
		System.out.println(adapter.twiceOf(100f)); //200.0
		System.out.println(adapter.halfOf(80f));   //40.0
    }
}
```



