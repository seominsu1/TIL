## Spring IoC

Sprign IoC 란?<br>
> Inversion of Control (제어의 역전) 이라는 원칙.
>
> 개발자가 아닌 툴(spring)이 제어권을 가져 어떠한 클래스에 필요한 의존 객체를 직접 만드는 것들이 아니라 주입받아서 사용하는 것입니다.
>
> → 코드의 흐름을 제어(관리)하는 주체가 개발자가 아닌 spring이 대신 하는 것
<br/>

- IoC가 적용된 경우, 객체의 생성을 Spring 에게 맡긴다
  - 이 경우 사용자는 객체를 직접 생성하지 않고, 객체의 생명주기를 컨트롤하는 주체는 spring이 된다
  - 즉, `사용자의 제어권을 다른 주체에게 넘기는 것` 을 제어의 역전, Inversion of Control 이라고 한다
- `ApplicationContext`와 `BeanFactory`(ApplicationContext는 BeanFactory를 상속받는다) 인터페이스 덕분에 `Bean으로 지정한 모든 클래스를 인스턴스로 등록`해주어 다른 클래스에서 의존관계가 필요하다면 이 Bean(스프링 IoC 컨테이너가 관리 하는 객체)으로 등록된 인스턴스들을 전달해주어 **의존성 주입(Dependency Injection)** 이 가능해진다.

- **사용이유와 예시**
```java
// 제어를 객체 내부에서 하는 경우
public class ItalianBLT {
    WhiteBread WhiteBread;

    public ItalianBLT() {
        this.memberRepository = new WhiteBread;
    }
}

// 제어를 외부에서 하는 경우
public class ItalianBLT {
    Bread bread;

    public ItalianBLT(Bread bread) {
        this.bread = bread;
    }
}
```
→ 위와같이 ioc가 적용되지 않은 코드는 이탈리안 비엘티를 만들기 위해서 객체 내부에서 화이트 브레드라는 재료를 직접 넣어주고 있습니다.<br>
하지만 ioc 원칙이 적용된 코드는 Bread를 외부에서 받으며 어떤 재료를 넣을지를 외부에서 관리하게 됩니다.<br>

→ 즉 ioc의 원칙을 적용한다면 `객체간의 역할과 책임을 분리`할 수 있고 `결합도를 낮추고` `유연한 코드`를 작성할 수 있게 도와줍니다.


## DI

DI 란?<br>
> Dependency Injection (의존성 주입) 이라는 디자인 패턴.
>
> IoC를 위해 사용되는 디자인 패턴입니다.
<br/>

- 클래스 간에 의존관계가 있다는 것은 한 클래스에 변화가 있으면 다른 클래스가 영향을 받는다는 뜻입니다.
- 의존성 주입이라는 것은 이런 의존성을 다른곳에서 주입시켜 준다는 것입니다.
### 의존성 주입 방법
- 생성자 주입 : 생성자 호출 시 외부로부터 의존성 주입 받습니다. 필요한 의존성을 모두 포함하는 생성자를 만들고 그 생성자에 의해 의존성을 주입 받습니다.
```java
public class ItalianBLT {
    Bread bread;
    Cheese cheese;
    Patty patty;

    public ItalianBLT(Bread bread, Cheese cheese, Patty patty) {
        this.bread = bread;
        this.cheese = cheese;
        this.patty = patty;
    }
}
```
- setter 주입 : 의존성을 주입받는 setter 메소드를 만들고 해당 메소드로 의존성을 주입 받습니다.
```java
public class ItalianBLT {
    WhiteBread whiteBread;
    MozzarellaCheese mozzarellaCheese;

    public void setWhiteBread(WhiteBread whiteBread) {
        this.whiteBread = whiteBread;
    }
    public void setMozzarellaCheese(MozzarellaCheese mozzarellaCheese) {
        this.mozzarellaCheese = mozzarellaCheese;
    }
}
```
- interface 주입 : 의존성 주입받는 메소드를 가진 interface를 작성하고 이를 구현하는 구현체에서 메소드를 override하여 주입 받습니다. setter처럼 외부에서 메소드 호출을 통해 주입받는 것은 동일하지만 `override 구현을 통해 이를 까먹지 않고 강제할 수 있습니다.`
```java
public class ItalianBLT implements RecipeInjection{
    WhiteBread whiteBread;
    MozzarellaCheese mozzarellaCheese;

    @Override
    public void inject (WhiteBread whiteBread, MozzarellaCheese mozzarellaCheese){
      this.whiteBread = whiteBread;
      this.mozzarellaCheese = mozzarellaCheese;
    }
}

interface RecipeInjection{
  void inject(WhiteBread whiteBread, MozzarellaCheese mozzarellaCheese);
}
```

### spring 에서의 DI
- spring 에서는 DI를 쉽게 도와줍니다.
- spring bean으로 등록된 객체에 대해 자동으로 인스턴스를 생성해주고 해당 객체에 필요한 의존성을 모두 주입해줍니다.
- @Autowired 어노테이션을 활용해 spring 은 DI를 도와줍니다.
- 필드 주입 : @Autowired 어노테이션을 멤버 변수에 붙이면 자동으로 의존성 주입을 해줍니다. 하지만 필드 주입은 `테스트시 입맛에 맞게 의존하는 객체를 바꾸고 싶어도 바꿀 수 없기에` 권장하는 방법은 아닙니다.
- setter 주입 : setter 메소드 위에 @Autowired를 붙여 사용. 
- 생성자 주입 : 생성자 주입으로 사용. new 를 사용해 주입받는 것이 아니기에 nullpointexception도 고려하지 않아도 됨!

### 정리
- IoC는 `원칙`이고
- DI는 IoC를 지키기 위한 `디자인 패턴` 이런 `DI를 자동으로 해줌`으로서 프로그램의 제어권을 가져가는 역할을 해주는 것이 `spring`!


