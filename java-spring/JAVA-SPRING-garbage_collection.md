# Garbage Collection (GC)

- Garbage란?
  - 앞으로 사용되지 않는 객체의 메모리
- Garbage Collection(GC)이란?
  - Heap Area에서 Garbage를 정해진 스케줄에 의해서 정리해 주는 것

### 가비지 컬렉션
- 자바의 메모리 관리 기법으로, 어플리케이션이 동적으로 할당 한 메모리 영역 중 더이상 사용하지 않는 영역을 정리하는 것
- 개발자가 직접 개입하지 않아도 더이상 사용하지 않는 메모리를 제거해주는 역할

### GC의 원리
GC는 하단의 두 가지 가설(전제 조건)로 인해 만들어졌다

- 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.
  - 대부분의 객체는 중괄호 안에서 생성되는데 끝나는 시점부터는 더이상 사용되지 않기 때문에 GC의 대상이 됨
- 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.
  - 일반적으로 순차적인 로직에 의해 객체를 생성하여 활용하는데, 앞에 생성된 객체는 그 다음의 로직에서 사용되지 않기 때문에 GC를 할 수 있게 됨

위의 특징들로 인해 Heap Area는 2가지 영역으로 나누었다(java7까지는 3가지였는데 나머지 1가지가 제외 됨)

## Young Generation & Old Generation

### Young Generation
- 새롭게 생성된 객체들이 생성되고 노화되는 곳
- 대부분의 객체 금방 접근불가능 상태가 되기 때문에 많은 객체가 Young영역에 생성되었다가 사라지는데 **Minor GC가 발생**한다고 말함

### Old Generation
- 일반적으로 Young generation 객체에 수명 한계치가 설정되고, 해당 한계치에 도달하면 Old Generation으로 이동된다.
- 이 영역에서 객체가 사라지는 것을 **Major GC(=Full GC)가 발생**한다고 말함


모든 GC는 Stop the World 이벤트이다.

>### Stop The World
>- GC를 수행하기 위해 JVM이 멈추는 현상을 의미
>- GC가 작동하는 동안 GC관련 스레드를 제외한 모든 스레드는 멈춤
>- 일반적으로 GC튜닝이라고 하는것은 이 시간을 최소화 하는 것

## GC Process
Heap Area 구조<br>
![image](/java-spring/img/JAVA-SPRING-garbage_collection_heap_area.PNG)


### Young Generation의 구성
- Eden 영역
- Survivor 영역(2개)

### 일반적인 GC의 과정
1. 새로운 객체가 Eden 영역에 생성<br>
![image](/java-spring/img/JAVA-SPRING-garbage_collection_process_3.PNG)<br>
2. Eden 영역이 가득차면 Minor GC가 발생<br>
![image](/java-spring/img/JAVA-SPRING-garbage_collection_process_4.PNG)<br>
3. 아직 사용되고 있는 객체는 Survivor1영역 혹은 Survivor2 영역으로 이동시킴 <br>
![image](/java-spring/img/JAVA-SPRING-garbage_collection_process_5.PNG)<br>
단, 객체의 크기가 Survivor 영역의 크기보다 클 경우에는 바로 Old Generation으로 이동
4. 운영 특성 상, Survivor1과 Survivor2 영역은 둘 중 한곳에만 객체가 존재해야하며, 다른 한 곳은 비워져 있어야 함.<br> 보통 From, To로 구분하며 객체가 존재하는 영역(From)이 가득차면 다른영역(To)로 보내고, 기존의 영역(From)을 비우는 작업을 진행<br>
![image](/java-spring/img/JAVA-SPRING-garbage_collection_process_6.PNG)<br>
![image](/java-spring/img/JAVA-SPRING-garbage_collection_process_7.PNG)<br>
5. 1~3의 과정을 반복하면서 Survivor 영역에서 계속 살아남은 객체들의 한계치가 초과되면 Old Generation 영역으로 이동하게 됨
![image](/java-spring/img/JAVA-SPRING-garbage_collection_process_8.PNG)<br>
6. Old Generation 영역에서 살아남았던 객체들이 어느정도 쌓이게 되면, 미사용으로 판단된 객체들을 제거하는 Major GC(Full Gc)가 발생하고, 이 과정에서 **Stop the World**가 발생하게 됨


## GC 알고리즘 종류
- Serial GC
  - Minor GC와 Major GC가 하나의 CPU를 사용
  - GC를 처리하는 쓰레드가 1개이기 때문에 STW 시간이 길다
- Parallel GC
  - 자바8의 default GC
  - Serial GC와 기본적인 알고리즘은 같지만, Young 영역의 Minor GC를 멀티 쓰레드로 수행 (Old 영역은 싱글 쓰레드)
    - 개선한 알고리즘이 Parallel Old GC로, Old 영역에서도 멀티 쓰레드로 GC 수행
  - 해당 알고리즘의 목표는 다른 CPU가 GC의 진행시간 동안 대기상태로 남아있는 것을 최소화 하는것이기 대문에 GC작업을 병렬로 처리하여 STW 시간이 비교적 짧음
- CMS GC
  - 어플리케이션의 스레드와 GC스레드가 동시에 실행되어 STW를 최소화 하는 GC
  - Parallel GC와 가장 큰 차이점은 Compaction 작업 유무로 구분 됨<br>
    > Compaction : 메모리 공간에서 사용하지 않는 빈 공간이 없도록 옮겨서 메모리 분산을 제거하는 작업
  - 자바9 이후로는 사용되지 않고, 14부터는 지원이 종료됨
- G1 GC
  - 자바9버전부터의 default GC로 메모리와 CPU를 많이 차지하는 CMS GC 문제점을 개선한 방식
  - 큰 메모리에서 상용하기 적합한 GC(대규모 Heap 사이즈에서 짧은 GC시간을 보장하는데 목적을 둠)
  - 전체 Heap 영역을 Region이라는 영역으로 분할하여 상황에 따라 역할이 동적으로 부여됨
