운영체제
===

1강
---
1.운영체제의 역사

Hardware의 변화에 따라 다섯 단계로 구분.

1. First Gen. (1945 - 1955)
   
   Vaccum tubes & Plugboards

   이떄의 프로그래밍은 machine language를 사용했다. 사실 Programming language, Operating systerm도 없었다.

   Signup sheet 에 컴퓨터를 사용할 시간을 계약하고 그 시간에 내려가서 시간만큼 컴퓨터를 사용한다.

   훗날 Plugboard 대신 Punched cards 사용

2. Second Gen. (1955 - 1965)
   
   Transistors & Batch systems

   프로그래머가 프로그램을 작성한 뒤 펀치카드에 구멍을 뚫는 식으로 프로그램을 형상화한다.

   input room 에 있는 오퍼레이터에게 카드뭉치들을 건네준 뒤 후에 결과를 회수하러 간다.

   Batch(일괄 처리)

   프로그래머가 여러 프로그램들을 Card reader 에 인식시킨뒤 일정 시간이 지나면 Card Reader 의 용량이 차게 되는데, 이때 오퍼레이터가 용량이 찰때에만 Card reader가 쓴 Input tape를 컴퓨터에 보낸다. 데이터가 어느정도 찬 뒤에 컴퓨터를 이용해 일괄처리 하므로 Batch system이라고 불린다.

3. Third Gen (1965 - 1980)
   
   ICs & Multiprogramming


>   Multiprogramming : avoiding CPU IdleS 중요함!

   How to avoid Ideling : make partition of memeory & use CPU when I/O happens by fetching other memories.

   Sceintific vs Commercial Computers
   numeric  vs sorting / printing

   IBM System/360

   Spooling : 카드로부터 값을 읽어서 바로 테이프 변환 과정 없이
   바로 디스크에 넣음. 디스크에 계속 값이 쌓이고 메모리에 빌때마다 디스크에서 연산할 데이터를 계속 가져와서 연산이 비지 않도록 함.

   SPOOL O <- 디스크 모양

   Timesharing : 같은 컴퓨터 한대지만, 1st Gen과는 달리 2,3세대에서는 컴퓨터와 1대1로의 어떠한 커넥션을 느끼기 힘들었다. 연산할 프로그램과 데이터를 가지고 가면 오퍼레이터가 대신해서 컴퓨터와 인터랙션을 하기 때문에, 개발자들을 이를 해결하기 위해

   컴퓨터 한대에, 여러개의 Online Terminal을 연결해 프로그래머가 직접 컴퓨터와 Interaction을 하는듯한 느낌을 주는, 연산은 Time을 쪼개 여러 사용자들의 연산을 처리할 수 있게 하는 방식이다.

   대표작 : CTSS

   MULTIX : 기계이면서 운영체제. Timesharing을 수백명이서 가능하게 해줌

   Minicomputers 의 등장 (PC보단 큼) DEC-PDP-1

   운영체제 UNIX의 등장

    Bell lab의 Ken Thomson이 PDP 컴퓨터에 맞추어 MULTIX를 간소화한 운영체제인 UNIX를 발표.

    소스코드가 공개되며 대표적으로 System-V와 BSD 두 버전으로 갈림.

    이를 표준화하고자 한 POSIX (IEEE)

    교육용 목적으로 개발되 MINIX

    MINIX의 무료 & 오픈소스 버전인 LINUX (Linus Torvalds)


4. Fourth Gen (1980-)
   PC의 등장. microcomputer이라고도 불림.

   Intel의 8080 이라는 범용 cpu를 만들어서,
   이 cpu를 위한 운영체제를 만들고자 Gary kildall에게 부탁.

   CP/M (Control Program for Microcomputers) 이라는 운영체제를 만듬.

   후에 IBM이 Intel 8088 cpu를 이용한 PC를 만들었는데, Kildall과 협상이 제대로 안되자 Bill gates에게 새 OS를 요구. 로컬 컴퍼니 DOS 운영체제를 사서 MS-DOS를 출시. 앞선 기능들은 UNIX에서 차용. 후에 Windows 의 전신이 됨.

   GUI(Graphical user interface) 의 등장
   창/아이콘/메뉴 등이 존재. 사용자 편리함 제공.

    Windows 1.0 MS-DOS 위에서 돌아가는 Graphical evironment

    Classic MacOS -> Called As 
    
    "System + number" -> "Mac OS number" -> Mac OS X -> OS X -> macOS

    Windows

    UNIX
    주로 Workstation, high-end 컴퓨터에 적합
    RISC chip과 주로 사용

5. Fifth Gen (1990~) 
 
   Mobile Computers.

   Nokia 9000 Phone + Computer (intel chips) 

   Blackberry (keyboard Added)


Operating System zoo
---
운영체제의 종류

1.Mainframe OS
        많은 양의 데이터를 한번에 처리. I/O 많음 High-end computers Batch / Transaction Processing / Timesharing

    : IBM OS/ Linux

2.Server OS
    네트워크를 통해 여러 사용자가 이용 가능해야 한다.
        서버상에서 돌아간다. 고성능PC, 워크스테이션 등

    : UNIX(Solaris, FreeBSD, Linux), Winodws Server

3.Multiprocess OS
    물리적으로 여러개의 CPU를 연결하여 동작할 수 있도록 하는 운영체제
    기존의 single cpu OS에 CPU간 Communicaiton 기능이 추가

4.Personal Computer OS
    유저들에게 좋은 사용자 경험을 제공해야함.

    : Window/macOS/Linux

5.Real-time OS
    시간이 중요한 요소임.
    Hard real-time system
    ex) car assembly, autopilot, military device

    Hard real time vs Sort real time
    Hard는 무조건 Deadline을 지켜야 함. 반면 Soft real time은
    가끔 Deadline이 안지켜저도 치명적이지는 않음.

6.Handheld computer OS
    PDA(personal digital assistant)

    :palmOS, Windows Mobile
    :iOS, Android, Windows 10 Mobile, Symbian OS

7.Embedded OS
    컴퓨터가 가전제품 등을 통제하기 위해 사용되는 OS
    TV, microwave ovens, cars ..etc
    컴퓨터의 성능이 제한적인 경우가 있으며 real-time os 인 경우가 흔함

8.Sensor node OS
    작은 Sensor nodes (Sensing , Communication 기능이 있는 작은 컴퓨터)

    빌딩 / 경계 감시용 센서 등으로 사용

9.Smart card OS
    신용카드 정도의 크기, 주로 성능에 제한을 받음.
    주로 한가지의 기능을 수행하기 위해 만들어지지만, 여러 기능을 가지는
    경우도 있음. ex) 전자결제 등


System Calls
---
> System Call : Interface. User program들이 OS 등이 제공하는 다양한 서비스 등을 사용할 수 있도록 하는 Interface
프로그램이 서비스를 받고 싶으면 system call의 형대로 서비스를 요청해야 한다.

CPU는 일반적으로 Kernel / User 모드를 가지고 있다.
System Call을 요청하면, User모드에 있던 프로그램이 Kernel 로 Trap되게된다. 이후 운영체제가 실행되기 시작한다.

Trap Instruction을 통해 Kernel로 들어가거나, 0으로 나누는 등 특수한
상황에서도 Trap이 일어나게 된다.

ex> C 프로그램으로부터 SysCall을 하기 위해서 library procedure call을 한다. 실질적으로 SysCall을 하는 과정이
Machine-dependent, CPU-dependant 하므로 각 장치마다 다르다. 따라서 Assembly코드 단계에서 콜이 수행된다.

Steps in making sysCalls
    읽기, 쓰기등의 동작이 필요한 경우 sysCall이 발생하고 kernel 모드로 들어간다.

procedure / funcion call : 호출된다고 user모드에서 kernel 모드로 바뀌지 않고, os가 실행되지 않는다.


C 프로그램에서 읽기 작업을 수행할때, SysCall을 사용자가 직접 호출하지 않고 SysCall을 담당하는 라이브러리(libc)를
호출하여 간접적으로 실행한다.

User program -> Library procedure -> kernel access


Process And Threads
---
Process : 실행중인 프로그램.
    Multiprogramming 의 이용:
    입출력 등 CPU를 사용하지 않을때 발생하는 비효율성을 해결하기 위해,
    메모리에 여러 프로그램을 담아두고 Time sharing을 통해 여러 프로그램을 동시에 실행하는것처럼 만들 수 있다.
    Program counter은 실질적으로 한개로, Program counter을 돌아가면서 프로세스를 실행하지만 개념적으로는
    네개의 Program Counter가 있는 것처럼 볼수 있다.

각자의 Process에는 Address space(core image)와 process table entry가 있다.
    Address space : Code(text), data, heap, stack
    process table entry : 프로세스가 나중에 수행될것을 대비해 저장해야 할 정보들. 레지스터, 프로그램 카운터 등 실행중이던 정보


Process States
---
> Process States : 중요함 !!

세가지 State를 가짐. Running, Ready, Blocked

Running: Cpu를 차지하고 있는 상태.

Ready: 일정시간 CPU를 점유하고 나면, Running상태에서 Ready상태로 끌려내려옴 언제든지 Running 상태로 돌아갈 수 있음. 끌려내려오면 Queue의 마지막에 들어가고 Queue의 맨앞에 프로세스가 Running상태로 올라감. 이 과정들은
운영체제의 스케쥴러가 담당함.

Blocked : I/O 등으로 인해 일정시간 CPU를 사용할수 없는 상태가 되면, Running 상태에서 Blocked 상태로 내려오게 됨. 입출력이 완료되면 Blocked 상태의 Process 를 Ready Queue의 마지막에 들어가게 됨. 바로 Running 상태로 올라갈 수 없다!

Process Address space (core image)
---
Stack (High Address)
↓

↑
Heap
Data 
Text (Low Address)

Process Hierarchies
---
프로세스가 다른 프로세스를 호출하고, 종료하면서 일종의 트리 구조를 형성하게 된다.
UNIX에서는 해당 구조가 있으나, Windows는 모든 프로세스가 동등하게 취급된다.

Protection
---
프로세스는 자신을 호출한 유저의 UID(User ID)를 갖게된다.
유닉스 유저들으 시스템 유저로부터 UID를 갖게 된다.
sudo (superuser do) 라는 명령어를 쓸수있는 슈퍼유저는 특별한 권한을 가지고 있다.
User들은 Group의 멤버가 될수 있으며 GroupID (GID)도 가질 수 있다.

Proccess Table
---
프로세스마다 한 엔트리를 갖게 된다. 나중에 프로세스가 실행될떄를 대비하여 필요한 정보들이 있다.

Proccess management, Memory management, File management

Registers           , Pointer to text   , Root dir

Program counter     , Pointer to data   , Working dir

Program status word , Pointer to stack  , UID & GID

Stack pointer

등등

Process Creation
---
새 프로세스는 기존의 프로세스가 프로세스 생성 sysCall을 통해서만 만들어질 수 있다.

프로세스가 생성되는 네가지 상황
1. 시스템 부팅
2. 실행중인 프로그램이 프로세스를 생성하는 SysCall을 호출한 경우 (fork system call)
3. 유저가 프로세스 생성 요청을 했을떄 (커맨드 실행, 아이콘 클릭) 
4. batch job을 초기화할때

Process Termination
---
프로세스가 종료하는 경우
1. Normal exit (자발적)
2. Error exit (자발적) -> 예외처리를 한 경우
3. Fatal error (비자발적) -> 예외처리를 못한 경우
4. Killed by another process (비자발적)

System Calls for Process Managemnt
---
1. Process creation/terminationon
2. 메모리상의 코어 이미지를 다른 내용으로 덮어쓰는것
3. Child process 가 종료하기를 기다리는것
4. 메모리 추가 요청

Shell : UNIX-based 운영체제에서 bash, zsh, fish와 같이 터미널에 입력된 사용자의 명령어를 해석해 커널에
전달하는 명령어 해석기


```
while(TRUE) {
    type_promtpt();
    read_command(command,parameters);

    if (fork() != 0) {
        /* Parent Code */
        waitpid(-1,&status,0);
    }
    else{
        /* Child code */
        execve(command, parameters, 0);
    }
}
```

//0916//
