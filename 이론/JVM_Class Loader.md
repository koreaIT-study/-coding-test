Class Loader : 런타임 시 컴파일 된 .class 파일을 메모리에 로딩하는 역할을 수행.  


![image](https://user-images.githubusercontent.com/67637716/187332972-2f8bae66-8145-45c9-b18a-ec76a0bbaecc.png) 

로딩, 링크, 초기화 순으로 진행된다.  

## Loading
클래스 로더가 .class 파일을 읽고 그 내용에 따라 적절한 바이너리 데이터를 만들고 '메서드' 영역에 저장.  
메서드 영역에 저장되는 데이터는 다음과 같다.  
- FQCN(Full Qualified Class Name) : 클래스가 속한 모든 패키지 명을 모두 포함하는 이름
- 클래스, 인터페이스, ENUM
- 메서드와 변수

로딩이 끝나면 해당 클래스 타입의 Class 객체를 생성하여 '힙'영역에 저장

#### 로딩 과정
![image](https://user-images.githubusercontent.com/67637716/187333235-aa3b0246-68b8-42f7-ac8f-f2a32eb1c59c.png)  

1. 우선 클래스가 메모리에 로드되었는지 확인 한다. (ClassLoader 클래스의 findLoadedClass() 메서드)
![image](https://user-images.githubusercontent.com/67637716/187333577-e35ed295-3bc8-4684-987e-1b0f417f48f1.png)  


2. 클래스가 메모리에 로드되어 있다면 실행을 진행한다.

3. 클래스가 메모리에 로드되어 있지 않으면 JVM은 ClassLoader Sub Sysytem에 해당 클래스를 로드하도록 요청 하고

    (loadClass 메서드)  ClassLoader 하위 시스템은 그 역할을 Application ClassLoader에게 넘긴다.  
    
    
    AbstractClassLoader.class :: loadClass()  
    
    ![image](https://user-images.githubusercontent.com/67637716/187334068-f3a1f168-d496-46cd-a3f3-3bfd5ff266b3.png)  



4. 클래스 로딩 요청을 받은  Application ClassLoader는 스스로 직접 로딩하지 않고 Extension ClassLoader 에게 위임한다.

5. 클래스 로딩 요청을 받은 Extension ClassLoader는 스스로 직접 로딩하지 않고 Bootstrap ClassLoader 에게 위임한다.


6.  Bootstrap ClassLoader는  rt.jar 에서 요청한 클래스를 찾은 뒤, 만약에 있으면 로딩 후 반환하고 없으면 Extension ClassLoader에게 위임한다.
![image](https://user-images.githubusercontent.com/67637716/187346895-1fd3acfb-f047-4cdb-8950-0f3da1f3371c.png)  

7. Extension ClassLoader는 jre/lib/ext 폴더나 java.ext.dirs 환경 변수로 지정된 폴더에서 요청한 클래스를 찾은 뒤, 만약에 있으면 로딩 후 반환하고 없으면 Application ClassLoader에게 위임한다.
![image](https://user-images.githubusercontent.com/67637716/187346917-7e8baea1-4d94-40b3-914a-4d587a019875.png)  


8.  Application ClassLoader는 클래스패스에서 요청한 클래스를 찾은 뒤, 만약에 있으면 로딩 후 반환하고 없으면

ClassNotFoundException 이 발생한다. 
