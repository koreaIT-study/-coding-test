# JVM이란?
java 가상머신으로 java 바이트 코드를 실행할 수 있는 주체.

Java는 OS에 종속적이지 않다는 특성을 가지고 있다.

OS에 종속받지 않고 실행되기 위해선 OS위에서 Java를 실행시킬 무언가가 필요하다.

그게 바로 JVM이다.

![image](https://user-images.githubusercontent.com/82895809/189648919-8bdbb82d-3969-4de4-a8cf-a21df23a8e21.png)


JVM은 크게 4가지로 구성된다.

1. Class Loader
2. Execution Engine
3. Garbage Collector
4. Runtime Data Area

## Class Loader
자바에서 코딩시 .java파일이 생성되면 .java소스를 java컴파일러(javac)가 컴파일하여 .class같은 클래스 파일(바이트코드)로 변환해준다.

Class Loader는 이러한 class 파일들을 모아서 JVM이 운영체로부터 할당받은 메모리 영역인 Runtime Data Area로 적재하는 역할을 한다.

> 변환된 bytecode는 기계어가 아니기 때문에 OS에서 바로 실행되지 않는다.
> 이 때, JVM이 OS가 bytecode를 이해할 수 있도록 해석해준다. 따라서 Byte Code는 JVM 위에서 OS 상관없이 실행될 수 있는 것이다.

## Execution Engine
Execution Engine은 Class Loader에 의해 메모리에 적재된 클래스(바이트코드)들을 기계어로 변경해 명령어 단위(Operation Code)로 실행하는 것.

명령어단위 실행에는 2가지 방식이 있다.

* 인터프리터 방식 : 명령어를 하나씩 수행
* JIT(Just In Time Compiler) 방식 : 전체 바이트 코드를 네이티브 코드로 변환하고 그 이후에는 인터프리팅하지 않고 네이티브 코드 상태로 실행

인터프리터는 전체 수행은 느리나 명령어 하나싞의 동작은 빠르고, JIT방식은 전체적인 동작은 빠르나 컴파일하는데 상당 시간이 소요된다.

## Garbage Collector
GC는 Heap 메모리 영역에 생성된 객체들 중에 참조되지 않은 객체들을 제거하는 역할을 한다.

## Runtime Data Area
JVM의 메모리 영역으로 자바 애플리케이션 실행시 사용되는 데이터를 적재하는 영역이다.

# JVM Option
* -Xms : 초기 Heap Size (init, default 64m)
* -Xmx : 최대 Heap Size (Max, default 256m)
* -XX:PermSize : 초기 PermSize
* -XX:MaxPermSzie : 최대 PermSize
* -XX:NewSize : 최소 new size (객체가 생성되어 저장되는 초기공간의 Size로 Eden + Survivor 영역)
* -XX:MaxNewSize : 최대 new size
* -XX:SurvivorRatio : New/Survivor 영역 비율 (n으로 지정 시 Eden:Survivor = 1:n)
* -XX:NewRatio : Young Gen과 Old Gen의 비율 (n으로 지정 시 Young:Old = 1:n)
* -XX:+DisableExplicitGC : System.gc() 콜을 무시
* -XX:+UseConcMarkWeepGC : 표준 gc가 아니나 Perm Gen 영역도 gc하는 Concurrent Collector를 사용
* -XX:+CMSPermGenSweepingEnabled : Perm gen 영역도 GC의 대상이 되도록 지정
* -XX:+CMSClassUnloadingEnabled : 클래스 데이터도 GC의 대상이 되도록 지정

## Java PermGen
PermGen은 JDK 1.7 이하 버전에서 존재하였다.

PermGen은 Permanent Generation의 약자다. (클래스의 정의들과 연관된 메타데이터를 위해 사용되는 메모리 공간)

자바 내 메타데이터는 
 * Class Meta Data
 * Method Meta Data
 * Static Object Variable

이곳은 클래스 메타 데이터가 들어갈 곳인데, 이 공간의 크기는 예측하기가 어려웠다.

자바에서는 클래스의 메타데이터를 읽고 해당 메타데이터를 통해 객체를 생성할 수 있다.
 
> 메타데이터(Metadata)
> 일반적으로 데이터에 관한 구조화된 데이터로,
> 대량의 정보 가운데에서 확인하고자 하는 정보를 효율적으로 검색하기 위해
> 원시데이터(Raw Data)를 일정한 규칙에 따라 구조화 혹은 표준화한 정보를 의미.

* 클래스 메타데이터 : 클래스의 이름, 생성정보, 필드정보, 메서드 정보 등

그 때문에 아래와 같은 에러가 종종 일어난다.

```
java.lang.OutOfMemoryError: PermGen space
```
PermGen 영역은 OS, JVM버전마다 각기 다른 default값을 가지고 있으며 대부분 매우 작게 할당되어 있다.

![image](https://user-images.githubusercontent.com/82895809/189651614-bc5b0d28-4002-41c5-a0bc-56ee65c5b59d.png)

클래스 로딩을 많이 하다보면 Permgen이 부족할 때
```
-XX:PermSize=128m -XX:MaxPermSize=128m
```

위와 같이 설정하고, 추후에 추가로 용량이 필요할 때는 늘리는 식으로 진행한다.

![image](https://user-images.githubusercontent.com/82895809/190192404-01ce1c78-2648-42ec-a0c8-8d55f6cce2f1.png)

JDK 1.8부터 PermGen영역이 없으며 Metaspace로 완벽하게 대체되었다.

> Metaspace(= Native Area, Native Memory, Off-Heap, Non-heap, Direct Memory 등)
> 이 영역을 시스템의 기본 메모리라고 생각하면 된다.

Metaspace는 클래스 메타 데이터를 native메모리에 저장하고 부족할 경우 자동으로 늘려주게 된다.

이로 인해 더이상 PermSize, MaxPermSize를 설정할 필요가 없어졌고, MetaspaceSize나 MaxMetaspaceSize가 새롭되 사용되게 되었다.

만약 MaxMetaspaceSize를 등록하지 않으면 Native memory자원을 최대한 사용하게 된다.

![image](https://user-images.githubusercontent.com/82895809/190198372-b3c48e96-a567-4762-b80f-ebade01a73da.png)

찾다보니 끝이없다....

Metaspace영역은 JVM에 의해 관리되는 Heap이 아닌 OS 레벨에서 관리되는 Native 메모리 영역이다.


그러므로 Metaspace가 Native 메모리를 이용함으로서 개발자는 영역 확보의 상한을 크게 의식할 필요가 없어지게 되었다.

![image](https://user-images.githubusercontent.com/82895809/190197966-d111178b-c3e7-4aa9-8c1d-af2846ae7e4d.png)

JVM의 기본 메모리 영역은 OS의 Native 메모리의 1/64, 최대 1/4까지 늘어난다고 되어있다.

MaxHeapSize는 Native메모리의 1/4라고 했다.

만약 총 실제 메모리: 4,004MB 4GB니 최대 1GB이다.

영어가 된다면 밑에 사이트에서 보는 것도 좋음
출처: https://dzone.com/articles/permgen-and-metaspace

# 자바의 실행과정
![image](https://user-images.githubusercontent.com/82895809/190199224-7a34ac98-36af-43b0-8279-8b201082a1ad.png)
