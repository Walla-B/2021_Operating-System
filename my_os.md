![image](https://t1.daumcdn.net/cfile/tistory/992C6D365B64159A07)
![image](https://t1.daumcdn.net/cfile/tistory/995D19435B641C1805)
![image](https://www.linuxbnb.net/wp-content/uploads/2018/06/system-call-overview-1.png)

쉘 mkdir 명령어를 이용하여 디렉토리를 만들떄 syscall 호출방법 :

Overhead : 어떠한 작업을 "준비하는데" 들어가는 비용(메모리, 시간). 문맥상으로 여러 해석이 가능하나, 일반적으로 stack frame 생성, 변수 및 주소 복사와 같은 작업들을 하는데 필요한 비용들을 통틀어 얘기한다.

[참고](https://stackoverflow.com/questions/2860234/what-is-overhead/2860263)

> job, task, proccess, thread?

![image](https://gmlwjd9405.github.io/images/os-process-and-thread/process.png)

Process : (사전적 의미)- 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램.

메모리상에 올라와 실행되고 있는 프로그램의 독립적인 개체.

프로세스는 시스템 자원을 할당받는 작업의 단위이다. 할당받는 시스템 자원은 CPU시간, Code, Data, Stack, Heap으로 구성된 독립된 메모리 영역 등이 있다.

각 프로세스 들은 각각 각각 독립된 메모리 영역을 할당받는다.

![image](https://gmlwjd9405.github.io/images/os-process-and-thread/thread.png)

Thread : (사전적 의미) - 프로세스 내에서 실행되는 여러 흐름의 단위.

프로세스의 특정한 수행 경로이며, 프로세스가 할당받은 자원을 이용하는 실행의 단위이다.

각 프로세스는 적어도 하나의 스레드(메인 스레드) 가 존재한다.

스레드는 각 프로세스 내에서 각각 Stack만 따로 할당받으며, Code, Data, Heap 영역은 공유한다.



[참고](https://stackoverflow.com/questions/3073948/job-task-and-process-whats-the-difference/31212568#31212568)


> 멀티 프로세스와 멀티 스레드의 차이

멀티 프로세스 : 하나의 응용 프로그램을 여러개의 프로세스로 구분하여 각 프로세스가 하나의 작업을 처리함.

    장점 : 여러개의 자식 프로세스들중 하나에 문제가 발생해도 그 문제가 확산되진 않는다.
    단점 : Context switching(한 프로세스를 멈추고 다른 프로세스를 실행하는것)이 발생할떄 큰
            오버헤드가 발생한다. 프로세스가 각각 독립된 메모리 영역을 할당받았기 때문에 프로세스
            사이에서 공유하는 메모리가 없고, 캐쉬에 있는 데이터를 리셋해야한다.

            각각 다른 메모리 영역을 할당받았기에, 하나의 실행중인 프로그램 속 프로세스들끼리 변수를 공유할 수 없다.

멀티 스레딩 : 하나의 응용 프로그램을 여러개의 스레드로 구성하고 각 스레드에게 하나의 작업을 처리하도록 하는것.

    장점 : 시스템의 자원 소모가 감소한다. 프로세스 생성/자원 할당 시스템 콜이 줄어든다.
            스레드는 프로세스 내 Stack 영역을 제외한 모든 메모리를 공유하기에 응답시간이 단축된다.
    단점 : 설계하기가 어렵다. 자원 공유 등의 문제가 발생함.
            하나의 스레드에 문제가 생기면 전체 프로세스에 영향을 준다.


> Windows vs Linux의 프로세스 / 스레드  매니지먼트 차이

Windows: 

에서는 프로세스가 부모/자식 관계를 가지지 않는다.
어떤 프로세스가 다른 프로세스를 호출하고 종료하더라도, 그 다른 프로세스는 종료되지 않으며 그 자체로 부모 프로세스가 된다.

[process explorer 다운로드](https://download.sysinternals.com/files/ProcessExplorer.zip)

[참고](https://shankaraman.wordpress.com/tag/differences-between-windows-and-linux-process/)

Process Explorer을 이용해 이를 확인할 수 있다.

명령 프롬프트에서 start mspaint.exe 명령어를 실행하면 process Tree에 cmd.exe 아래로 mspaint.exe가

들어가게 되는데, 이때 cmd.exe 프로세스를 종료하더라도 msapint.exe가 종료되지 않는 것을 볼 수 있다.

윈도우의 스레드는, 기본 스케쥴링의 단위가 된다.


Linux :

프로세스는 "Task" 라는 이름으로도 불리며,
프로세스의 부모-자식 관계가 존재하는데, 부모 프로세스가 종료될시 자식 프로세스들도 모두 종료된다.

프로세스가 스케쥴링의 단위가 된다.

리눅스에서는 스레드라는 개념이 모호하나 
프로세스가 handling table, PID, address space를 공유하여 윈도우의 스레드와 같게 동작할 수 있다.


> 프로세스 / job 확인위한 Linux 명령어

```
$ ps 
$ ps aux // 현재 실행중인 모든 프로세스와 그 PID를를를를 보여준다.
         // STAT 항목의 값이 프로세스의 현재 상태를 보여준다.
         // 자세한 내용은 man ps 명령어를 통해 ps 의 매뉴얼을 line 420을 참고하자.
$ jobs   // 현재 실행중인 모든 Job들을 보여준다.
```

> Linux signal

[참고](https://jhnyang.tistory.com/143)

시그널 : (사전적 의미) - 프로세스끼리 서로 통신할 떄 사용함. 특정 프로세스가 다른 프로세스에게 메시지를 보낼때 시그널을 이용한다.

리눅스가 사용하는 시그널에는, 프로세스가 발생시키는 시그널 외에도 

하드웨어가 자첵으로 발생하는 시그널, 

사용자가 인터럽트 키를 사용해
발생하는 시그널 , 대표적으로 Ctrl - C (SIGINT), Ctrl -Z(SIGSTOP) 등이 있다.

Ctrl-C, Ctrl-Z와 같은 시그널은 kiil 명령어를 통해 
```
$ kill -<SIGNALID> <PID>
$ kill -2 <PID>
$ kill -19 <PID>
```
와 같이 구체적으로 PID를 정해 시그널을 보낼 수도 있다.

man kill 또는 kill --help 명령어를 통해 매뉴얼을 살펴보자. kill 명령어는 이름과는 다르게,

무조건 프로세스를 "종료" 시키는 명령어가 아니다.




