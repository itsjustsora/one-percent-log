## [Template Method Pattern](https://www.inflearn.com/course/lecture?courseSlug=%EC%9E%90%EB%B0%94-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4&unitId=3175&tab=curriculum)

### ⛳️ 학습 목표
- 일정한 프로세스를 가진 요구사항을 템플릿 메서드 패턴을 이용하여 구현할 수 있다.

---

### Template Method Pattern
**알고리즘의 구조를 메서드에 정의하고 하위 클래스에서 알고리즘 구조의 변경없이 알고리즘을 재정의 하는 패턴**이다.

<br>

**[적용 사례]**
- 구현하려는 알고리즘이 일정한 프로세스가 있다.
- 구현하려는 알고리즘이 변경 가능성이 있다.

<br>

**[구현 단계]**
- 알고리즘을 여러 단계로 나눈다.
- 나눠진 알고리즘의 단계를 메서드로 선언한다.
- 알고리즘을 수행할 템플릿 메서드로 만든다.
- 하위 클래스에서 나눠진 메서드들을 구현한다.

<br>

**[예제 코드]**  
게임의 접속을 구현한다. 이때, 보안 > 인증 > 권한 > 접속 과정을 거쳐야 한다. 

<br>

***AbstGameConnectHelper.class - A 패키지***
```java
public abstract class AbstGameConnectHelper {
	
	protected abstract String doSecurity(String string);
	protected abstract boolean authentication(String id, String password);
	protected abstract int authorization(String userName);
	protected abstract String connection(String info);
	
	// Template Method
	public String requestConnection(String encryptInfo) {
		// 보안 -> 암호화 된 문자열을 복호화
		String decryptInfo = doSecurity(encryptInfo); 
		
		// 복호화 된 정보에서 아이디, 비밀번호를 추출한다.
		String id = "aaa";
		String password = "bbb";
		
		// 인증
		if (!authentication(id, password)) {
			throw new Exception("아이디, 패스워드 불일치");
		}
		
		// 권한
        String userName = "userName";
        int level = authorization(userMame);
		
		switch (level) {
            case 0: // 게임 매니저
				break;
            case 1: // 유료 회원
				break;
            case 2: // 무료 회원
				break;
            case 3: // 권한 없음
				break;
            default: // 기본
		}
		
		// 접속
		return connection(encodedInfo);
    }
}
```

<br>

***DefaultGameConnectHelper.class - A 패키지***
```java
public class DefaultGameConnectHelper extends AbstGameConnectHelper {
	
	@Override
    protected String doSecurity(String string) {
        System.out.println("복호화 작업");
        return string;
    }

	@Override
	protected boolean authentication(String id, String password) {
		System.out.println("아이디, 암호 확인 과정");
        return true;
	}

	@Override
	protected int authorization(String userName) {
		System.out.println("권한 확인");
        return 0;
	}

	@Override
	protected String connection(String info) {
		System.out.println("마지막 접속 단계");
		return info;
	}
}
```

<br>

***Main.class - B 패키지***
```java
public class Main {
	public static void main(String[] args) {
		AbstGameConnectHelper helper = new DefaultGameConnectHelper();
		helper.requestconnection("게임 접속 과정 실행!");
    }
}
```

출력
> 복호화 작업  
> 아이디, 암호 확인 과정  
> 권한 확인  
> 마지막 접속 단계  

<br>

만약, 요구사항이 발생했을 시 추상 클래스가 아닌 하위 구현 클래스의 코드만 변경하면 된다.