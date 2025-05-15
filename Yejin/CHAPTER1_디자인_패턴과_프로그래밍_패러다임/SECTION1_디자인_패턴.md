# 1.1 디자인 패턴

프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 '규약' 형태로 만들어 놓은 것

### 1.1.1 싱글톤 패턴

- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
- 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈이 공유하며 사용
- 보통 데이터베이스 연결 모듈에 많이 사용
- 장점: 하나의 인스턴스만 사용하기 때문에 인스턴스 생성 비용이 줄어듦
- 단점: 의존성이 높아짐 <br> 
=> TDD(Test Driven Development)에 걸림돌 (각 단위테스트 간 독립성 보장 X) <br>
해결방안: 의존성 주입

##### 의존성 주입
- 장점: 
    - 약한 결합 (디커플링)=> 모듈을 쉽게 교체할 수 있어 테스트가 쉽고 마이그레이션이 수월
    - 모듈 간 관계 명확
- 단점
    - 클래스 수가 늘어나 복잡성 증가
    - 약간의 런타임 패널티


```java
class Singleton {
    
    private static class singlelnstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
        return singlelnstanceHolder.INSTANCE;
    }
}
```

### 1.1.2 팩토리 패턴

- 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴
- 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용 결정
- 의존성 주입이라고 볼 수 있음
- 장점
    - 느슨한 결합 & 높은 유연성 & 유지보수성 증가
    - 정적메서드로 객체를 생성하도록 한다면, 해당 메서드에 대한 메모리 할당을 한 번만 할 수 있음

```java
enum CoffeeType {
    LATTE,
    ESPRESSO
}

abstract class Coffee {

    protected String name;

    public String getName() {
        return name;
    }
}

class Latte extends Coffee {

    public Latte() {
        name = "latte";
    }
}

class Espresso extends Coffee {
    
    public Espresso() {
        name = "Espresso";
    }
}

class CoffeeFactory {
    public static Coffee createCoffee(Coffee Type type) {
        switch (type) {
            case LATTE:
                return new Latte() ;
            case ESPRESSO :
                return new Espresso();
            default:
                throw new IllegalArgumentException("Invalid coffee type:" + type);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Coffee coffee = CoffeeFactory.createCoffee(CoffeeType.LATTE);
        System.out.println(coffee.getName()); //latte
    }
}
```

### 1.1.3 전략 패턴

- 객체의 행위를 바꾸고 싶은 경우 '직접' 수정하지 않음
- 캡슐화한 알고리즘(전략)을 컨텍스트 안에서 바꿔주며 상호 교체가 가능하게 만듦

```java
interface PaymentStrategy {
    public void pay(int amount);
}

class KAKAOCardStrategy implements PaymentStrategy {
    
    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using KAKAOCard.");
    }
}

class LUNACardStrategy implements PaymentStrategy {
    
    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard.");
    }
}
```

### 1.1.4 옵저버 패턴

- 주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 옵저버들에게 변화를 알려주는 디자인패턴
- 주로 이벤트 기반 시스템에 사용

```java
interface Subject {
    public void register(Observer obj);
    public void unregister(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer {
    public void update(); 
}

class Topic implements Subject {
    private List<Observer> observers;
    private String message; 

    public Topic() {
        this.observers = new ArrayList<>();
        this.message = "";
    }

    @Override
    public void register(Observer obj) {
        if (!observers.contains(obj)) observers.add(obj); 
    }

    @Override
    public void unregister(Observer obj) {
        observers.remove(obj); 
    }

    @Override
    public void notifyObservers() {   
        this.observers.forEach(Observer::update); 
    }

    @Override
    public Object getUpdate(Observer obj) {
        return this.message;
    } 
    
    public void postMessage(String msg) {
        System.out.println("Message sended to Topic: " + msg);
        this.message = msg; 
        notifyObservers();
    }
}

class TopicSubscriber implements Observer {
    private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic) {
        this.name = name;
        this.topic = topic;
    }

    @Override
    public void update() {
        String msg = (String) topic.getUpdate(this); 
        System.out.println(name + ":: got message >> " + msg); 
    } 
}

public class HelloWorld { 
    public static void main(String[] args) {
        Topic topic = new Topic(); 
        Observer a = new TopicSubscriber("a", topic);
        Observer b = new TopicSubscriber("b", topic);
        Observer c = new TopicSubscriber("c", topic);
        topic.register(a);
        topic.register(b);
        topic.register(c); 
   
        topic.postMessage("amumu is op champion!!"); 
    }
}
```

### 1.1.5 프록시 패턴과 프록시 서버

##### 프록시 패턴

- 대상 객체(subject)에 접근하기 전 그 접근에 대한 흐름을 가로채 해당 접근을 필터링하거나 수정하는 등의 역할을 하는 계층이 있는 디자인패턴
- 객체의 속성, 변환 등 보완
- 보안, 데이터 검증, 캐싱, 로깅에 사용
- 프록시 서버로도 활용

##### 프록시 서버

- 서버와 클라이언트 사이에서 클라이언트가 자신을  통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램
- ex) NGINX, CloudFlare
- 익명 사용자의 직접적인 서버 접근 차단 & 실제 포트 은닉 => 보안 강화
- 서버 앞단에서의 로깅
- 정적 자원 gzip => 데이터 전송량 감소 but 압축 해제 시 CPU 오버헤드 가능
- 의심스러운 트래픽 자동 차단 => DDOS 공격으로부터 보호
- 프론트엔드 프록시서버 => CORS 에러 해결
- 프록시서버 캐싱 (원격 서버 대신 캐시에서 정보 제공) => 트래픽 줄일 수 있음

### 1.1.6 이터레이터 패턴

- iterator를 사용하여 collection의 요소들에 접근하는 디자인 패턴
- 순회할 수 있는 여러 가지 자료형의 구조와는 상관 없이 이터레이터라는 하나의 인터페이스로 순회 가능
- 이터레이터 프로토콜: 이터러블한 객체들을 순회할 때 쓰이는 규칙
- 이터러블한 객체: 반복 가능한 객체로 배열을 일반화한 객체

### 1.1.7 노출모듈 패턴

- 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴
- javascript는 접근 제어자가 존재하지 않기 때문에 노출모듈 패턴을 통해 private와 public 접근 제어자를 구현하기도 함

### 1.1.8 MVC 패턴

- Model, View, Controller로 이루어진 디자인 패턴
- 장점: 역할 분리 => 재사용성 & 확장성
- 단점: 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해짐
- ex) Spring

##### 모델 (Model)

- 데이터베이스, 상수, 변수 등
- 뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신

##### 뷰 (View)

- inputbox, checkbox, textarea 등의 사용자 인터페이스 요소
- model을 기반으로 사용자가 볼 수 있는 화면
- model이 가지고 있는 정보를 따로 저장하면 안 됨
- 단순히 화면에 표시하는 정보만 가지고 있어야 함
- 변경이 일어나면 controller에 전달

##### 컨트롤러 (Controller)

- 하나 이상의 Model과 하나 이상의 View를 잇는 다리 역할
- 이벤트 등 메인 로직 담당
- Model과 View의 생명주기 관리
- Model이나 View의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용에 대해 알려 줌

### 1.1.9. MVP 패턴

- MVC 패턴으로부터 파생
- MVC의 Controller가 Presenter로 교체된 패턴
- View와 Presenter는 1:1 관계 (MVC 패턴보다 더 강한 결합)

### 1.1.10 MVVM 패턴

- MVC의 Controller가 View Model로 바뀐 패턴
- View Model은 View를 더 추상화한 계층
- MVC 패턴과는 다르게 커맨드와 데이터 바인딩을 가짐
    - 커맨드: 여러 가지 요소에 대한 처리를 하나의 액션으로 처리할 수 있게 하는 기법
    - 데이터 바인딩: 화면에 보이는 데이터와 웹 브라우저의 메모리 데이터를 일치시키는 기법. View Model을 변경하면 View가 변경됨
- View와 View Model 사이의 양방향 데이터 바인딩 지원
- UI를 별도의 코드 수정 없이 재사용 가능
- 단위테스트 용이
- ex) Vue.js
