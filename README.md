# SpringFrameworkAOP

# AOP : AOP(관점 기반 프로그래밍)
관점 기반 프로그래밍과 클래스 계측을 지원하는 모듈을 포함한다. spring-app모듈은 AOP기능을, spring-instrument 모듈은 클래스 계측 지원을 제공한다.

팁: 
1. 스프링5에서는 더 이상 포틀릿 개발을 지원하지 않는다. 포틀릿 애플리케이션을 스프링으로 개발하고 싶을 경우 4.3을 활용
2. 스프링 배포판 명명규약
   spring-<짧은 모듈 이름>-<스프링버전>.jar
   
   짧은 모듈 이름:
   1) aop
   2) beans
   3) context
   4) expressions
   
   스프링 버전:
   1) Spring 5.0.1.RELEASE
   
   즉, spring-aop-5.0.1.RELEASE.jar
   
   
# 스프링 IOC 컨테이너 (== 스프링 컨테이너)

스프링이 다루는 애플리케이션 개발 영역에 대해서 기본적으로 알았을 경우에는 IoC 컨테이너를 공부한다.

자바 애플리케이션은 애플리케이션의 행동 방식을 제공하기 위해 상호 작용하는 객체로 이뤄진다. 객체가 다른 객체와 상호작용하는 경우를 객체의 의존관계(dependency)라고 한다. 예를 들어 X객체가 Y,Z객체와 상호 작용을 한다면 X객체는 Y와Z객체와 의존관계이다 DI는 객체간의 의존관계를 생성자인수(constructor argument)나 세터메서드인수(setter method argments)로 명시하고 객체를 생성 할 때 생성자나 세터를 통해 의존관계를 주입하는 방식을 따르는 디자인 패턴이다.

스프링 애플리케이션에서 애플리케이션에 존재하는 객체를 생성하고 의존관계를 주입하는 일을 담당한다. 스프링 컨테이너가 생성하고 관리하는 애플리케이션 객체들을 빈(bean)이라고 부른다.
스프링 컨테이너는 애플리케이션 객체를 한꺼번에 책임지므로 애플리케이션을 조합하기 위해 팩토리(Factory)나 서비스 로케이터(Service Locator) 등의 디자인 패턴으 직접 구현할 필요가 없다. 
의존 관계를 만들고 주입하는 책임은 애플리케이션 객체가 아닌 스프링 컨테이너에 있어 DI를 제어하는 역전(IoC)이라고 부른다.

# AOP의 개념

조인포인트(JoinPoint) :
조인포인트는 애플리케이션 실행 중의 특정 지점을 의미한다. 전형적인 조인트의 예로는 메서드 호출, 메서드 실행 자체, 클래스 초기화, 객체 생성시점등이 있다. 조인트는 AOP 핵심개념이며 애플리케이션의 어떤 지점에서 AOP를 사용하여 추가적인 로직을 삽입 할지 정의한다.

어드바이스(Advice):
특정 조인트포인트에 실행하는 코드를 어드바이라고한다. 조인트포인트 이전에 실행하는 Before 어드바이스와 이후에 실행하는 After어드바이스를 비롯한 여러 종류의 어드바이스가 있다.

포인트컷(PointCut):
하나의 포인트컷은 여러 조인포인트의 집합체로 언제 어드바이스를 실행할지 정의할때 사용한다. 포인트컷을 만들어서, 애플리케이션 구성 요소에 어드바이스를 어떻게 적용할지 상세하게 제어할 수 있다. 앞서 언급했듯이, 가장 일반적으로 사용하는 조인트포인트는 메서드 호출이다. 가장 일번적인 포인트컷은 특정 클래스에 있는 모든 메서드 호출로 구성된다. 종종 여러분은 어드바이스 실행지점을 좀더 다양하게 제어할 필요가 있을때 복잡한 형태로 포인트컷을 구성할 수 도 있다. 

애스팩트(Aspect): 
애스팩트는 어드바이스와 포인트컷을 조합한 것이다. 즉, 이조합물은 애플리케이션이 가지고 있어야 할 로직과 그것을 실행해야하는 지점을 정의한 것이라고 할 수 있다.

위빙(Weaving):
이것은 애플리케이션 코드 해당 지점에 애스팩트를 실제로 주입하는 과정을 말한다. 당연히 컴파일 시점AOP솔루션은 이 작업을 컴파일 시점에서하며, 빌드과정중에 별도의 과정을 거친다. 마찬가지로, 실행시점 AOP 솔루션은 위빙 과정이 실행중에 동적으로 일어난다.

타켓(Target):
자신의 실행흐름이 어떠한 AOP처리로 인해 수정되는 객체를 타켓객체라고 한다. 흔히 타켓객체를 어드바이스가 적용된 객체라고도한다.

인트로덕션(Introduction):
인트로턱션은 객체구조를 수정하여 새로운 메서드나 필드를 그안에 추가할 수 있는 처리를 뜻한다. 여러분은 인트로덕션을 사용해서 어떤 객체가 특정 인터페이스를 명시적으로 구현하지않고도 구현한것으로 만들 수 있다.


# 정적인 AOP
초기 AOP 구현체들은 대부분 정적인 것이었다. 정적 AOP는 위빙 처리가 애플리케이션 빌드 과정중 별도의 단계로 구성된다. 자바 용어로 설명하자면, 정적 AOP의 위빙 처리는 애플리케이션의 실제 바이트코드를 필요한 대로 변경하거나 확장하여 수정하는 작업이다. 분명히 이방법은 위빙 처리를 잘 수행하고 있다. 왜냐면 처리 결과가 일반 자바 바이트코드이기 때문에 애플리케이션 실행 도중 언제 어드바이스를 실행해야하는가를 알려주기 위한 어떤 특별한 조취를 취할 필요가 없기 때문이다.
      이 메커니즘의 단점은 간단한 조인포인트를 하나 더 추가하는 등의 애스팩트에 어떠한 변경이 있을때마다 반드시 전체 애플리케이션을 다시 컴파일해야한다는 것이다.

AspectJ는 정적 AOP 구현체의 대표적인 예에 해당한다.

# 동적인 AOP
스프링 AOP 같은 동적인 AOP구현체는 위빙처리를 실행 도중 동적으로 수행한다는 점에서 정적인 AOP구현체와 다르다. 위빙을 하는 방법은 구현을 어떻게 하느냐에 달렸지만,  스프링의 접근 방법은 모든 어드바이스를 적용할 객체의 프록시를 만들어서 필요에 따라 어드바이스가 호출되도록 하는 것이다. 동적 AOP의 단점은 일반적으로 정적 AOP보다 성능이 좋지 않다는것인데, 계속해서 성능은 좋아지고 있다.  동적 AOP 구현체의 가장 큰 장점은 주요 애플리케이션 코드를 다시 컴파일할 필요없이 모든 애스팩트만 수정할 수 있어 편리하다는점

# AOP 종류 선택하기
 정적 AOP를 사용할 것인지 동적 AOP를 사용할 것인지 결정하는 것은 고이장히 힘든 의사결정이다. 둘 모두 각각의 장점을 지니고 있기 때문에, 둘중 하나만 선택해서 그것만 사용해야하는 이유는 없다. 스프링1.1은 애스펙트J연동 기능을 추가하여, 프로그래머가 두종류의 AOP를 모두 간단하게 사용할 수 있게 하고있다.
많은 사람들이 동적 AOP에 없는 기능을 사용하고 싶을때는 aspectj를 사용해서 가져오고 싶을거다 하지만 대부분의 경우는 스프링AOP를 사용하는 것이 더 나은 선택이다. 선언적인 트랜잭션 관리처럼 스프링은 이미 AOP를 기반으로 만들어진 많은 기능을 제공하고 있다. 스프링에서 이미 여러분이 사용할 수 있도록 시도와 검증을 거친 구현체를 제공하고 있기 때문에, 이것들을 Aspectj를 사용하여 다시 구현하는 것은 시간과 노력을 낭비하는 행동이다.
중요한 부분은 애플리케이션의 요구 사항에 따라 AOP 구현체를 선택하는 것이다. 따라서 여러 구현체를 조합하여 사용하는 것이 여러분 애플리케이션에 적합 할 수 있음에도 불구하고, 오직 하나의 구현체로만 스스로를 제한하지 않기 바란다. 보통 우리는 스프링AOP가 AspectJ보다는 덜 복잡하기 때문에 맨 먼저 스프링AOP를 사용하는 경향이 있는걸 알지만, 만약 스프링AOP가 우리가 원하는 것을 해주지않거나 애플리케이션 성능이 나쁘다는 것을 발견하면 AspectJ로 변경하면된다.



# 참고:

