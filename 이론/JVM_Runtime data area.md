![image](https://user-images.githubusercontent.com/67637716/187226172-9c2c336a-0bd7-40ff-bb14-9357cc0484fe.png)  

Runtime Data Area는 JVM이 프로그램을 수행하기 위해 OS로부터 할당받는 메모리영역.  
WAS의 성능에 문제가 발생했을 때, 대부분 이 영역들이 원인이 된다.(Memory Leak 혹은 GC)  

![image](https://user-images.githubusercontent.com/67637716/187221834-052571f6-94bd-47b4-942b-383e9e0480ce.png)  
Rumtime Data Area는 다섯가지로 구분.  

 좌측 3개의 영역은 Thread별로 생성되고 우측 2개의 영역은 모든 Thread가 공유한다. 
 Method Area와 Heap Area는 모든 스레드에서 접근 가능하고 프로그램의 시작부터 종료 될 때까지 메모리에 남아 어디에서든지 사용가능.  
 
 
 
 ## Method Area
 ![image](https://user-images.githubusercontent.com/67637716/187226837-2a06f65b-23cf-4dd3-a251-c6db2e2300dd.png)  
 클래스 로더가 .class 파일을 읽고, 내용에 맞는 binary데이터를 생성한 뒤, 메모리의 Method영역에 저장(MetaSpace)  
 클래스가 로드 되는 시점은 해당 클래스가 사용되기 위해 호출되는 시점.  
 JVM이 종료될때까지 유지.  
 
 
 #### Method Area에 저장되는 목록
 1. Type Information : 클래스와 인터페이스의 정보  
    1-1. Type 명 : Package name + Class name  
    1-2. Type의 종류 : Type이 Class인지 Interface인지에 대한 정보  
    1-3. Type의 제어자 : 접근 제어자(public, private, default ..), 그외 제어자(abstract, final 등)  
    1-4. 연관된 Interface 정보 : 사용된 Interface 정보  
 
 2. Runtime Constant Pool : Type의 상수 정보를 저장하는 Pool. 각 상수로는 인덱스를 통해 접근 가능
    2-1. Type, Field, Method로 접근하기 위한 Reference 정보 - 객체 접근을 위해 필요  
    2-2. Privarity Type 뿐아니라, String 객체, 레퍼런스 타입이 가진 주소 값도 상수 풀로 관리.  
 
 ![image](https://user-images.githubusercontent.com/67637716/187235732-8bbd1f5d-6fbc-4065-a134-84053356cffe.png)  
 
[ #n ] 은 상수풀의 n번 인덱스에 접근하는 바이트코드이다.  
[ #2 = 상수타입    상수값 ]으로 상수풀의 특정 인덱스에 상수 값을 등록 할 수 있다.  

![image](https://user-images.githubusercontent.com/67637716/187235917-5da0240c-1b19-4f4c-b535-f7965ebccc42.png)  
![image](https://user-images.githubusercontent.com/67637716/187235975-f05dd3ca-c8a5-440b-8e41-29cf192ec59a.png)  

3. Field Information : 인스턴스 변수의 정보를 저장(Type명, 제어자)  
4. Method Information  : 메서드의 모든 정보를 저장(Method명, 반환 타입, parameter수와 타입정보, 외 필요한 메서드에 대한 정보들)  
5. Class Variable : static 키워드로 선언된 변수 저장(static 키워드로 선언된 변수 저장, 실제 인스턴스는 Heap에 저장됨)  

#### Instance 생성시 일어나는 과정
![image](https://user-images.githubusercontent.com/67637716/187241881-eee5b721-d928-4833-a663-7fef3205b897.png)  



 ## Heap Area
인스턴스와 배열이 동적으로 생성되는 공간. Garbage Collection의 대상이 되는 영역.   
모든 Thread가 공유하기 때문에 동기화 문제가 발생할 수 있다.  
Method Area에 로드된 클래스만 생성 가능.  
효율적인 GC를 위해 메모리 영역 분리(Eden, Survivor1,2, Old)  
 
 ## Stack Area
 * Heap 영역에 생성된 Object 타입의 데이터의 참조값 할당
 * 원시타입의 데이터가 값과 함꼐 할당
    * 원시타입의 데이터들에 대해서는 참조값을 저장하는 것이 아닌 실제 값을 stack에 직접 저장
 * 지역변수들은 scope에 따른 visibility를 가짐
    * 전역변수가 아닌 지역변수가 foo()라는 함수에서 bar()를 호출했을때 bar()함수가 종료된 경우 bar()함수 내부에서 선언한 모든 지역 변수들은 stack에서 pop되어 사라짐
 * 각 Thread는 자신만의 stack을 가짐
    * 스레드 하나가 새롭게 생성되는 순간 해당 스레드를 위한 stack도 함께 생성되며, 각 스레드에서 다른 스레드의 stack영역에는 접근 불가.

## 메모리 예제
https://inspirit941.tistory.com/294  
 
 ## PC register (Thread별로 1개씩 존재)
 프로그램 카운터(Program counter, PC)는 다른 말로는 "명령어 주소 레지스터"라고도 하는데, 다음 번에 수행될 명령어의 주소를 가지고 있는 레지스터이다.   
 프로그램 카운터는 각 명령어가 인출된 후, 곧 이어질 명령어의 주소를 가리키는 값이 자동적으로 증가된다.  
 
* 거의 무시할 수 있는 작은 메모리 공간. 가장 빠른 저장 공간이기도 하다.
* JVM 사양에서 각 스레드에는 스레드 전용 프로그램 카운터가 있으며 해당 수명 주기는 스레드의 수명 주기와 일치
* 프로그램 카운터는 현재 스레드에서 실행 중인 Java 메소드의 JVM 명령 주소를 저장
* Java Virtual Machine 사양에서 outOtMemoryError 조건을 지정하지 않은 유일한 영역입니다. (GC 없음, OOM)

 
* PC 레지스터를 사용하여 바이트코드 명령어 주소를 저장하는 용도?  
-  CPU는 계속해서 쓰레드 사이를 전환해야 하기 때문에 이 시점에서 다시 전환한 후 실행을 계속할 위치를 알아야 함. 
-  이를 이용하여 스레드를 돌아가면서 수행할 수 있게함 (Context-switching)   

## Native Method Stack
자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역 (JNI)  
※ 네이티브 코드 : 메모리가 관리 되지 않는 코드(ex) C언어는 gc가 없음, free명령으로 직접 관리)
   os에 의해 직접적으로 컴파일 되는 코드  
※ managed code : 인터프리터라고 불리우는 다른 프로그램이 반드시 요구 되는 코드, gc가 있음

Native Method Libraries와 연동하여 실행에 필요한 Native Library(C, C++)를 제공하는 인터페이스.  
JVM이 C/C++ 라이브러리를 호출하고 하드웨어에 특정한 C/C++ 라이브러리에 의해 호출될 수 있도록 한다.  
일반적인 메서드를 실행하는 경우 JVM 스택에 쌓이다가 해당 메서드 내부에 네이티브 방식을 사용하는 메서드가 있다면 해당 메서드는 네이티브 스택에 쌓임
