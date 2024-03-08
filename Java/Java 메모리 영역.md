## Java 메모리 영역

JVM 은 다음과 같이 구성되어 있는데

![image](https://github.com/seominsu1/TIL/assets/97941148/066f5a72-1618-473a-9c48-ef3e6ac45c6c)

Runtime Data Area 가 프로그램 수행을 위해 OS로부터 할당받은 메모리 영역을 뜻한다.

자바의 메모리는 총 5개가 있음.

 PC register / native method / stack / heap / Method area,  총 5개의 영역

1. **PC register** → 현재 어떤 명령을 실행해야 할 지에 대한 기록을 담당.
2. **native method** → 자바 이외의 언어에 제공되는 method 정보가 저장되는 공간. 
3. **JVM stack** → 호출된 메서드의 파라미터, 지역 변수, 리턴 값 및 연산값 등이 저장되는 공간. 프로그램 실행 시 임시로 할당되었다가 메서드를 빠져나가게 되면 소멸되는 특성의 데이터가 저장되는 공간. 
4. **Heap** → GC의 대상이 되는 영역. 객체를 생성하게 되면 heap 영역의 메모리에 할당됨. 단 레퍼런스 변수( 객체의 주소를 가리키는) 는 heap 이 아닌 포인터에 저장됨.
5. **Method Area →** 클래스의 정보를 처음 메모리에 올릴 때 초기화되는 대상을 저장. 올라가는 정보는 
    1. 필드 정보 → 멤버변수에 대한 정보(이름, 타입, 접근 지정자 등)
    2. 메서드 정보 → 이름, 리턴타입, 파라미터, 접근 지정자 등
    3. 타입 정보 → class 인지 interface인지? 타입의 속성, 이름, super class 이름

**⇒ Java는 멀티스레드 환경으로 모든 스레드는 Heap, Method Area를 공유함.**
