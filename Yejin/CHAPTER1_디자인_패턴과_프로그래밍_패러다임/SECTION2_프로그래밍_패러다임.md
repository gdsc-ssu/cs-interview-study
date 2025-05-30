# 1.2 프로그래밍 패러다임

프로그래머에게 프로그래밍의 관점을 갖게 해주는 역할을 하는 개발 방법론

```
- 프로그래밍 패러다임
    - 선언형
        - 함수형 (Haskell)
    - 명령형
        - 객체지향형 (Java)
        - 절차지향형
```

### 1.2.1 선언형과 함수형 프로그래밍

##### 선언형 프로그래밍 (Declarative Programming)

- '무엇을' 풀어내는가에 집중하는 패러다임
- "프로그래밍은 함수로 이루어진 것이다."라는 명제가 담겨 있는 패러다임

##### 함수형 프로그래밍 (Functional Programming)

- 선언형 패러다임의 일종
- 작은 '순수 함수'들을 블록처럼 쌓아 로직을 구현하고 '고차 함수'를 통해 재사용성을 높인 프로그래밍 패러다임
    - 순수 함수: 출력이 입력에만 의존하는 것
    ```js
    const pure = (a, b) => {
        return a + b
    }
    ```
    - 고차 함수: 함수가 함수를 값처럼 매개변수로 받아 로직을 생성할 수 있는 것
        - 고차 함수를 쓰기 위해서는 해당 언어가 일급 객체여야 함
            - 일급 객체
                - 변수나 메서드에 함수 할당 가능
                - 함수 안에 함수를 매개변수로 담을 수 있음
                - 함수가 함수를 반환할 수 있음
- 커링
- 불변성

##### 객체지향 프로그래밍 (OOP, Object-Oriented Programming)

- 객체들의 집합으로 프로그램의 상호 작용 표현
- 데이터를 객체로 취급하여 객체 내부에 선언된 메서드 활용
- 단점
    - 설계에 많은 시간 소요됨
    - 처리 속도가 다른 프로그래밍 패러다임에 비해 상대적으로 느림
- 객체지향 프로그래밍의 특징
    - 추상화: 복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려내는 것
    - 캡슐화: 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것
    - 상속성: 상위 클래스의 특성을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것
    - 다형성: 하나의 메서드나 클래스가 다양한 방법으로 동작하는 것 (오버로딩, 오버라이딩)
        - 오버로딩: 같은 이름을 가지는 메서드를 여러 개 두는 것 (정적 다형성) (컴파일 타임에 발생)
        - 오버라이딩: 상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의하는 것 (동적 다형성) (런타임에 발생)
- 설계 원칙: SOLID 원칙
    - S (단일 책임 원칙) (SRP, Single Responsibility Principle) <br>
    모든 클래스는 각각 하나의 책임만 가져야 한다
    - O (개방-폐쇄 원칙) (OCP, Open Closed Principle) <br>
    유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야 함. <br>
    기존의 코드는 잘 변경하지 않으면서도 확장은 쉽게 할 수 있어야 함
    - L (리스코프 치환 원칙) (LSP, Liskov Substitution Principle) <br>
    객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 함
    - I (인터페이스 분리 원칙) (ISP, Interface Segregation Principle) <br>
    하나의 인터페이스보다 구체적인 여러 개의 인터페이스를 만들어야 하는 원칙
    - D (의존 역전 원칙) (DIP, Dependency Inversion Principle) <br>
    추상화된 인터페이스나 상위 클래스를 의존도록 해서 변하기 쉬운 것의 변화에 영향받지 않게 하는 원칙

### 1.2.3 절차형 프로그래밍

- 연속적인 계산 과정으로 이루어져 잇음
- 장점: 코드의 가독성이 좋으며 실행 속도가 빠름
- 단점: 모듈화가 어렵고 유지 보수성이 떨어짐
- ex) Fortran을 이용한 대기 과학 관련 연산 작업, 머신 러닝의 배치 작업 등 계산이 많은 작업

### 1.2.4 패러다임의 혼합

- 가장 좋은 패러다임은 없다.
- 비즈니스 로직이나 서비스의 특징을 고려해서 패러다임을 정하는 것이 좋다
- 여러 패러다임을 조합하여 상황과 맥락에 따라 패러다임 간의 장점만 취해 개발하는 것이 좋다
