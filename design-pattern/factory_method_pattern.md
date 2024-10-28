## [Factory Method Pattern](https://www.inflearn.com/course/lecture?courseSlug=%EC%9E%90%EB%B0%94-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4&unitId=3177)

### ⛳️ 학습 목표
- 팩토리 메서드 패턴에서 템플릿 메서드 패턴의 사용됨을 학습한다.
- 팩토리 메서드 패턴에서의 구조와 구현의 분리를 이해하고 분리의 장점을 학습한다. 

---

### Factory Method Pattern
**상위 클래스에서 객체들을 생성할 수 있는 인터페이스를 제공하여 하위 클래스들이 생성될 객체들의 유형을 변경할 수 있도록 하는 생성 패턴**이다.
![guru](https://refactoring.guru/images/patterns/diagrams/factory-method/structure.png?id=4cba0803f42517cfe8548c9bc7dc4c9b)

<br>

**[예제 코드]**  
- 데이터베이스에서 아이템 정보를 조회한다.
- 조회한 아이템 정보로 아이템을 생성한다.
- 아이템 생성 정보를 남긴다.

<br>

***Item interface - A 패키지***
```java
public interface Item {
	void use();
}
```

<br>

***ItemCreator.class - A 패키지***
```java
public abstract class ItemCreator {
	public Item create() {
		Item item;
		
		// 템플릿 메서드 처럼 동작
        // step 1
		requestItemsInfo();
		// step 2
		item = createItem();
		// step 3
		createItemLog();
		
		return item;
    }
	
	// 아이템을 생성하기 전에 데이터베이스에서 아이템 정보를 요청한다.
    abstract protected void requestItemsInfo();
	// 아이템 생성 후 아이템 복제 등의 불법을 방지하기 위해 데이터베이스에 아이템 생성 정보를 남긴다.
	abstract protected void createItemLog();
	// 아이템을 생성하는 알고리즘
    abstract protected Item createItem();
}

```

<br>

***HpPotion.class - B 패키지***
```java
public class HpPotion implements Item {
	@Override
    public void use() {
		System.out.println("체력 회복");
    }
}
```

<br>

***HpCreator.class - B 패키지***
```java
public class HpCreator extends ItemCreator {
	@Override
    protected void requestItemsInfo() {
		System.out.println("데이터베이스에서 체력 회복 물약의 정보를 가져온다.");
    }

	@Override
	protected void createItemLog() {
		System.out.println("체력 회복 물약을 새로 생성했습니다. " + new Date());
    }

	@Override
	protected Item createItem() {
		return new HpPotion();
    }
}
```

<br>

***MpPotion.class - B 패키지***
```java
public class HpPotion implements Item {
	@Override
    public void use() {
		System.out.println("마력 회복");
    }
}
```

<br>

***MpCreator.class - B 패키지***
```java
public class MpCreator extends ItemCreator {
	@Override
    protected void requestItemsInfo() {
		System.out.println("데이터베이스에서 마력 회복 물약의 정보를 가져온다.");
    }

	@Override
	protected void createItemLog() {
		System.out.println("마력 회복 물약을 새로 생성했습니다. " + new Date());
    }

	@Override
	protected Item createItem() {
		return new MpPotion();
    }
}
```

<br>

***Main.class - B 패키지***
```java
public class Main {
	public static void main(String[] args) {
		ItemCreator creator;
		Item item;
		
		creator = new HpCreator();
		item = creator.create();
		item.use();
		
		creator = new MpCreator();
		item = creator.create();
		item.use();
    }
}
```

