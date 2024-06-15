### 작성자 : 홍영환

# 인터럽스 (Interrupt)


프로세서에 이벤트를 알려 CPU가 현재 수행하던 일을 저장 후 중단하고 다른 일을 하도록 처리하는 비동기적 방법을 뜻한다.

인터럽트에는 **외부 인터럽트** / **내부 인터럽트**가 있다.

## 📚 외부 인터럽트


![Untitled](https://github.com/EN-CS-STUDY/CS_STUDY/assets/77156858/e4c9ef6c-c85e-4a55-a65f-2d763c26c32e)

키보드, 마우스, 모니터 등과 같은 외부 장치를 **입출력 장치(I/O 장치)** 라고 한다. 

하드웨어 장치마다 **“컨트롤러”** 라고 불리는 **작은 CPU**가 붙어있다.

메모리를 제어하는 컨트롤러는 메모리 컨트롤러, 디스크를 제어하는 컨트롤러는 디스크 컨트롤러라고 부른다.

그리고 장치로 부터 들어오고 나가는 데이터를 임시로 저장하기 위한 작은 메모리를 가지고 있는데 그것을 **로컬 버퍼**라고 불린다.

예를 들어, 디스크에서 데이터를 읽어오는 작업을 하고 있다고 가정하자. 그러면 디스크 컨트롤러가 디스크에서 내용을 읽어서 로컬버퍼에 저장한다. 이때 CPU가 로컬 버퍼로 읽어오는 작업이 끝났는지 체크하는 것이 아니라 장치에 있는 **컨트롤러가 인터럽스 신호를 발생시켜 CPU에 전달을 한다.**

정확히는 CPU 옆에 **인터럽트 라인**이라는 친구가 있는데 이곳에 인터럽트 신호가 들어온다.

즉, CPU는 열심히 일을 중 → CPU가 인터럽트 라인에 신호가 들어왔는지도 확인 → 인터럽트 라인에서 인터럽트 발생을 CPU에게 알려줌 → CPU는 하던 일을 멈추고 해당 인터럽스 처리.

```
💡 이렇게 컨트롤러 등 하드웨어 장치가 인터럽트 라인을 세팅하고 CPU에게 이를 통보하는 방법을 외부 인터럽트라고 한다.
```


### 📕 외부 인터럽트 종류

---

**✅ 타이머 인터럽트**

CPU한테 각 프로세스를 수행할 수 있는 제한된 시간이 주어져있다.

만약, 타이머 인터럽트가 없으면 CPU는 하나의 프로세스를 완전히 끝날 때 까지 한 프로세스만 처리하는 문제가 발생한다.

입출력 장치 중에 타이머라는 친구가 있는데 그 친구가 일정 시간이 지나면 CPU에게 다른 프로세스를 수행해야 한다고 알려준다.

타이머 인터럽트는 보통 10ms마다 인터럽트를 걸어 문맥 교환이 일어나도록 한다.

**✅  I/O 인터럽트**

프린터를 출력한다던가 마우스 커서를 움직이고 클릭하는 상황 등에 I/O 인터럽트가 발생한다.

[마우스 커서가 움직일 때]

- 마우스를 움직이면 CPU에 전기신호(인터럽트)를 보내준다.

- CPU가 인터럽트를 감지하면 현재 작업을 중단한다.

- 그리고 OS내부에 있는 Mouse Interrupt Service Routine으로 점프한다.

- Mouse Interrupt Service Routine은 커서를 이동시킨 좌표를 모니터상에 표현해주는 역할을 한다**.**

**✅ 전원 이상 인터럽트**

컴퓨터가 실행 도중 전원 공급이 중단되면 CPU에 인터럽트를 걸어서 현재 작업 중이던 프로세스를 대피시킬 수 있도록 하는 신호를 걸어줘야 한다. 이를 전원 이상 인터럽트라고 한다.

## 📚 내부 인터럽트

![Untitled](https://github.com/EN-CS-STUDY/CS_STUDY/assets/77156858/7a3a4e0f-7bce-4b4b-b6d7-87d39055999b)

---

### 📚인터럽트 발생 과정

![Untitled](https://github.com/EN-CS-STUDY/CS_STUDY/assets/77156858/f698d7f9-8845-42ba-ab0a-acf28a854ace)

운영체제 커널에 인터럽트 발생 시 해야할 일을 미리 가지고 있다.

인터럽트가 발생하면 CPU는 운영체제에게 제어권을 주고 커널 영역 내에서 해당 인터럽트의 처리를 위해서 정의된 코드를 찾는다.

운영체제는 할 일을 쉽게 찾아가기 위해 **인터럽트 벡터**라는 놈을 가지고 있는데 인터럽트 벡터란 **인터럽트 종류마다 번호를 정해서, 번호에 따라 처리해야 할 코드가 위치한 부분을 가리키고 있는 자료구조이다.**

실제 처리해야 할 코드는 **인터럽트 처리루틴( ISR : Interrupt service routine )** 또는 **인터럽트 핸들러(interrupt handler)**라고 부릅니다.  인터럽트 처리루틴은 **여러가지 인터럽트에 대해 각각 처리해야 할 업무들을 정의해 놓았다.**

### 📕 내부 인터럽트 종류

---

✅ **Exception 인터럽트 (Trap)**

보통 **트랩**이라고도 많이 불리는 인터럽트이다.

대표적으로 0으로 나누거나, 오버플로우 / 언더플로우와 같이 자신의 메모리 영역 바깥에 접근하려는 시도 등 권한 없는 작업을 시도할 때 처리하는 인터럽트이다.

즉, CPU가 명령어를 수행하다가 처리할 수 없는 예외상황을 만나면 자기 자신을 인터럽트 걸어서 커널의 처리루틴을 수행할 수 있도록 한다.

✅ **SVC 인터럽트**

Supervisor Call 이라는건데 운영체제를 호출해서 처리하라는 의미이다. 사용자 프로그램에서 "open("a.txt") " 라는 명령어가 들어왔을 때 사용자 프로그램이 처리할 수 없을 때, 운영체제 내부에 정의된 코드를 실행해야해서 커널모드로 전환을 해야한다

즉, 운영체제에 서비스를 요청하기 위해 인터럽트를 걸고 커널모드로 전환하는 인터럽트이다.

### 🤔 DMA란?

---

**DMA는 Direct Memory Access의 약자로, I/O 디바이스 등의 장치가 CPU의 도움 없이 데이터를 메모리로 직접 전송할 수 있게 하는 방식이다.**

DMA 컨트롤러는 I/O 디바이스가 메모리에 직접 접근할 수 있도록 하는 하드웨어 장치이다

### **DMA의 등장 배경**

---

DMA가 등장하기 전, I/O 디바이스가 데이터를 전송할 때마다 CPU가 인터럽트를 받았다. 이러한 데이터 처리가 효율적인 것처럼 보일 수 있지만, 빈번한 인터럽트는 CPU에 심각한 오버헤드를 야기했다.

DMA의 등장으로 이러한 인터럽트 오버헤드를 줄이고 CPU가 기본적인 컴퓨팅 작업에 더 집중할 수 있도록 만들었다.

### 📚 참고문헌

https://github.com/devSquad-study/2023-CS-Study/blob/main/OS/os_interrupt.md

[[운영체제 7편] 인터럽트가 무엇인가](https://baebalja.tistory.com/354)

[🖥️ [운영체제] 인터럽트(Interrupt) & DMA(Direct Memory Access)란?](https://scottxchoo.xyz/cs-os-004/)