Class Loader : 런타임 시 컴파일 된 .class 파일을 메모리에 로딩하는 역할을 수행.  


![image](https://user-images.githubusercontent.com/67637716/187332972-2f8bae66-8145-45c9-b18a-ec76a0bbaecc.png) 

JVM은 RAM에 위치하며, 실행중에 클래스로더 서브시스템을 이용하여 클래스파일을 RAM으로 가져온다.  
이를 자바의 동적 클래스 로딩기능이라고한다.  
이 과정은 컴파일 타임이 아니라 런타임에 일어나며, 처음으로 클래스를 참조할때 `로딩, 링크, 초기화` 순으로 진행된다.  

# Loading
클래스 로더가 .class 파일을 읽고 그 내용에 따라 적절한 바이너리 데이터를 만들고 '메서드' 영역에 저장.  
메서드 영역에 저장되는 데이터는 다음과 같다.  
- FQCN(Full Qualified Class Name) : 클래스가 속한 모든 패키지 명을 모두 포함하는 이름
- 클래스, 인터페이스, ENUM
- 메서드와 변수

클래스 로딩 과정은 main() 메서드 선언이 있는 클래스부터 시작.  
다음과 같은 상황에서도 로딩된다.  
![image](https://user-images.githubusercontent.com/67637716/187347397-050f9163-0298-46db-b688-874dae00b33f.png)  


또한 부모클래스를 로딩해야 자식클래스를 로딩할 수 있다. 
클래스 사용 시점에 로딩됨.  
![image](https://user-images.githubusercontent.com/67637716/187347574-33f9f20c-7f60-45f5-b14c-f166c218d3f4.png)  



로딩이 끝나면 해당 클래스 타입의 Class 객체를 생성하여 '힙'영역에 저장

#### 로딩 과정
![image](https://user-images.githubusercontent.com/67637716/187333235-aa3b0246-68b8-42f7-ac8f-f2a32eb1c59c.png)  

1. 우선 클래스가 메모리에 로드되었는지 확인 한다. (ClassLoader 클래스의 findLoadedClass() 메서드)
![image](https://user-images.githubusercontent.com/67637716/187333577-e35ed295-3bc8-4684-987e-1b0f417f48f1.png)  


2. 클래스가 메모리에 로드되어 있다면 실행을 진행한다.

3. 클래스가 메모리에 로드되어 있지 않으면 JVM은 ClassLoader Sub Sysytem에 해당 클래스를 로드하도록 요청 하고

    (loadClass 메서드)  ClassLoader 하위 시스템은 그 역할을 Application ClassLoader에게 넘긴다.  
    
    
    ![image](https://user-images.githubusercontent.com/67637716/187352591-6a0ce71e-c475-492e-af85-8b67a0bfd4e1.png)  



4. 클래스 로딩 요청을 받은  Application ClassLoader는 스스로 직접 로딩하지 않고 Extension ClassLoader 에게 위임한다.

5. 클래스 로딩 요청을 받은 Extension ClassLoader는 스스로 직접 로딩하지 않고 Bootstrap ClassLoader 에게 위임한다.


6.  Bootstrap ClassLoader는  rt.jar 에서 요청한 클래스를 찾은 뒤, 만약에 있으면 로딩 후 반환하고 없으면 Extension ClassLoader에게 위임한다.
![image](https://user-images.githubusercontent.com/67637716/187346895-1fd3acfb-f047-4cdb-8950-0f3da1f3371c.png)  

7. Extension ClassLoader는 jre/lib/ext 폴더나 java.ext.dirs 환경 변수로 지정된 폴더에서 요청한 클래스를 찾은 뒤, 만약에 있으면 로딩 후 반환하고 없으면 Application ClassLoader에게 위임한다.
![image](https://user-images.githubusercontent.com/67637716/187346917-7e8baea1-4d94-40b3-914a-4d587a019875.png)  


8.  Application ClassLoader는 클래스패스에서 요청한 클래스를 찾은 뒤, 만약에 있으면 로딩 후 반환하고 없으면

ClassNotFoundException 이 발생한다.  
![image](https://user-images.githubusercontent.com/67637716/187347792-3bb3c298-2479-4c9b-8f5e-0e7232ce21d2.png)  


![image](https://user-images.githubusercontent.com/67637716/187348069-a6713966-dd43-4d1b-8db0-a5253e0b4439.png)  
Bootstrap ClassLoader는 네이티브하게 짜여져있기 때문에 null이 나온다.

# Linking
로드된 클래스나 인터페이스, 그 직계 부모클래스나 인터페이스, 필요한 경우 요소 타입(배열 타입인 경우)을 검증하고, 준비하고, 해석하는 과정을 거침.  
Verification , Preparation , 그리고 Resolution 이라는 세 가지 단계로 이루어져 있다.  

#### 검중(Verification)
내부적으로 바이트코드 검증기(Bytecode verifier)은 java spec에 따라 코드를 잘 작성했는지 .class파일이 생성되는지 확인하여 .class파일의 정확성을 보장.  
검증이 실패하면 런타임 에러(java.lang.VerifyError)를 발생시킴.  
아래와 같은 검사들 수행.  
- 접근 지정자에 따른 접근 범위에서 메서드에 접근하고 있는지 검사
- 메서드의 매개변수 수와 자료형이 올바른지 검사
- final 메서드와 클래스가 오버라이드 되지는 않았는지 검사
- 변수를 읽기 전에 초기화되었는지 검사
- 변수가 올바른 타입의 값인지 검사.

#### 준비(Preparation)
JVM에서 쓰이는 자료구조나 정적 기억 영역을 위해 메모리를 할당한다.  
이 때 메모리가 부족하면 java.lang.OutOfMemoryError(OOM)이 발생.  
이 단계에서 정적 필드가 만들어지고 기본값으로 초기화된다.  
원래의 값은 초기화단계에서 할당되므로 아직은 초기화 블록이나 초기화 코드는 실행되지 않는다.  

#### 해석(Resolution) - optional  
compile time에 자바 클래스는 다른 클래스의 실제 주소값을 알지 못하므로 실제 주소값 대신 symbolic reference로 대체.  
Runtime constant pool에 있는 symbolic reference(심볼릭 참조)를 direct reference(직접 참조)로 대체하는 과정.  
=> 프로그램 실행을 위한 API를 클래스가 직접 가지는 것이 아닌 이름을 통한 참조값을 이용해서 실행할 때 메모리 상에서 API를 호출할 수 있도록 이름을 주소로 대체하는 특징을 의미  
예를 들면 Book book = new Book(); book이라는 참조 변수가 Heap에 저장되있는 Book instance를 연결해주는 과정  
이 단계는 JVM 구현에 따라 클래스를 검증할 때 한 번에 해석할 수도(eager resolution), 당장 심볼릭 참조를 직접 참조로 바꿀 필요가 없다면 뒤로 밀려날 수도(lazy resolution) 있다.  

# 초기화(Initialization)
로드된 각 클래스나 인터페이스의 초기화 로직이 실행.  
클래스의 위에서 아래로, 클래스 계층 구조에서 부모에서 자식까지 한 줄씩 실행.  









