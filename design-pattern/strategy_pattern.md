## [Strategy Pattern](https://www.inflearn.com/course/lecture?courseSlug=%EC%9E%90%EB%B0%94-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4&unitId=3171)

### ⛳️ 학습 목표
- Interface 개념을 이해한다.
- Delegate 개념을 이해한다.
- 전략 패턴 개념을 이해한다.

---

### 기본 개념
- `Interface(인터페이스)` : 기능을 사용하는 통로로서, 선언과 구현을 분리하여 구현체가 변경되더라도 의존하는 코드에 영향을 미치지 않고 유연성을 높이는 역할을 한다.
- `Delegate(델리게이트)` : 어떤 객체가 직접 작업을 수행하는 대신 다른 객체에 작업을 위임하는 방식이다.

***Delegate 예제***
```java
// Task 클래스: 실제로 작업을 수행하는 클래스
class Task {
    public void doTask() {
        System.out.println("작업을 수행 중입니다.");
    }
}

// Worker 클래스: Task에게 작업을 델리게이트하는 클래스
class Worker {
    private Task task;  // Task 객체를 가짐

    public Worker() {
        this.task = new Task();  // Task 객체 생성
    }

    public void performTask() {
        task.doTask();  // Task 객체에게 작업을 위임(델리게이트)
    }
}

// Main 클래스: 델리게이트 예시 실행
public class DelegateSimpleExample {
    public static void main(String[] args) {
        Worker worker = new Worker();  // Worker 객체 생성
        worker.performTask();  // Worker가 실제 작업을 Task에 위임
    }
}

```

<br>

### Strategy Pattern(전략 패턴)
여러 알고리즘을 바탕으로 하나의 추상적인 접근점을 만들어 이 접근점에서 서로 교환이 가능하도록 만든 패턴이다. 어떤 행위를 캡슐화하여 여러 전략을 생성하고 실행 시점에 원하는 전략으로 교체할 수 있도록 하는 것이다.

<br>

***캐릭터가 다양한 무기를 사용하는 예제 코드***
```java
// Strategy 인터페이스: 무기 행동 정의
interface WeaponStrategy {
    void attack();
}

// Sword (검) 무기 전략
class SwordStrategy implements WeaponStrategy {
    @Override
    public void attack() {
        System.out.println("검을 휘둘러 공격합니다!");
    }
}

// Bow (활) 무기 전략
class BowStrategy implements WeaponStrategy {
    @Override
    public void attack() {
        System.out.println("활을 쏘아 공격합니다!");
    }
}

// Context 클래스: 캐릭터가 무기를 사용하는 클래스
class Character {
    private WeaponStrategy weaponStrategy;  // 사용할 무기(전략)

    public Character(WeaponStrategy weaponStrategy) {
        this.weaponStrategy = weaponStrategy;  // 무기(전략)를 주입받음
    }

    public void attack() {
        weaponStrategy.attack();  // 주입된 무기에 따라 공격 실행
    }

    public void setWeaponStrategy(WeaponStrategy weaponStrategy) {
        this.weaponStrategy = weaponStrategy;  // 무기를 변경할 수 있음
    }
}

// Main 클래스: Strategy 패턴 사용 예시
public class StrategyWeaponExample {
    public static void main(String[] args) {
        // 캐릭터가 처음에는 검을 사용하여 공격
        Character character = new Character(new SwordStrategy());
        character.attack();  // 검으로 공격

        // 무기를 활로 변경 후 공격
        character.setWeaponStrategy(new BowStrategy());
        character.attack();  // 활로 공격
    }
}
```




