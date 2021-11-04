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

Note : 1코어 2스레드와 같이 "하드웨어" 스레드를 말할때의 스레드와 헷갈리지 말아햐 함.

이떄의 (하드웨어적) 스레드는 1개의 코어로 2개의 코어가 있는것처럼 사용할수 있다는 것으로, Hyperthreading 이라고 불림.

[참고](https://stackoverflow.com/questions/11835046/multithreading-and-multicore-differences)

Jobs :
A set of processes comprising a pipeline, and any processes descended from it, that are all in the same process group.

라고 정의되어 있으며, 프로세스보다 조금 더 큰 개념이다.


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

확인하기 위해 다음 프로그램을 실행시켜보자:
```C
// forktest.c
#include <stdio.h>
#include <unistd.h>
#include <errno.h>

int main(int argc, const char * argv[]) {
    int pid;

    if (pid == -1) {
        perror("Failed to fork");
        return -1;
    }

    if (pid == 0) {
        printf("Child pid = %d\n", getpid());
        while (1);
    }
    else {
        printf("Parent pid = %d\n", getpid());
        while (1);
    }
    return 0;
}

```

```shell
$ ./forktest

출력결과 (환경에 따라 매번 다른 값을 갖는다) :
Parent pid = 15947
Child pid = 15948

이떄, 프로그램이 pid를 출력하고 while 루프를 돌고 있는동안, Ctrl Z 으로 sigstop 시그널을 보내
프로세스를 멈춘후 다음 명령어로 현재 프로세스를 확인하자

$ ps aux

출력결과 :
USER    PID      %CPU       %MEM    VSZ     RSS     TTY     ...     COMMAND
(다른 프로세스들)
park    15947    42.0%      0.0%    2488    584     pts/1           ./forktest
park    15948    42.0%      0.0%    2488     76     pts/1           ./forktest

위 명령어를 통해 부모 프로세스와, fork된 자식 프로세스를 볼 수 있다.

이참에 jobs 명령어를 통해 현재 실행중인 job 목록 또한 살펴보자

$ jobs

출력결과 : 
[2]- Stopped        ./forktest

프로세스가 두개 있는것과 비교해서, job은 한개가 생성된 것을 알 수 있다.

다음으로 kill 명령어를 통해서 부모, 혹은 자식 프로세스를 종료시켜 보자.

$ kill -9  15947
$ ps aux

출력결과 : 
USER    PID      %CPU       %MEM    VSZ     RSS     TTY     ...     COMMAND
(다른 프로세스들)

부모 프로세스가 종료된 경우, 거기서 fork된 자식 프로세스 또한 모두 종료된다.

$ kill -9 15948
$ ps aux

출력결과 :
USER    PID      %CPU       %MEM    VSZ     RSS     TTY     ...     COMMAND
(다른 프로세스들)
park    15947    42.0%      0.0%    2488    584     pts/1           ./forktest

자식 프로세스가 종료된 경우, 부모 프로세스에는 영향을 끼치지 않는다.

```

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

> System Boot process

[참고](https://www.freecodecamp.org/news/uefi-vs-bios/)

1. 전원버튼을 누른다
2. CPU에 전원이 공급되지만, 이 후의 작업들을 실행하기 위한 마중물과 같은 프로그램이 필요하다.
   이떄 휘발성인 메모리에는 아무것도 존재하지 않으므로, 마더보드에 물리적으로 저장되어있는
   프로그램인 펌웨어를 실행한다.
3. 펌웨어는 POST (Power On Self Test) 를 실행한다. 연결된 하드웨어들의 연결 상태를 확인한다.
   (컴퓨터의 비프음은 이것이 끝날떄 발생)
4. 펌웨어는 저장장치속 부트로더를 찾고 (주로 디스크의 첫번째 섹터에 위치), 찾았다면 부트로더에게
   컴퓨터의 컨트롤을 양도한다.
5. 부트로더는 아주 작은 (디스크 첫번쨰 섹터 512바이트 안에 존재하는) 프로그램이다. UNIX-like
   운영체제에서 GRUB이 부트로더로서 사용되는데, 부트로더가 하는 일은 나머지 운영체제를 로딩하는
   것이다.
6. 부트로더는 커널을 메모리상에 로드한다. UNIX-like 운영체제에서는 이후 모든 프로세스의 부모가 되는
    init 프로세스를 실행한다.
    (윈도우에서는, wininit.exe 이외에도 servicies.exe, lsass.exe, lsm.exe같은 프로세스를 실행)
7. 이 모든것이 끝나면 드라이버, GUI 등이 실행된다.

> Firmware

PC의 전원 버튼을 눌렀을때, 메모리는 비어있는 상태이므로 OS 등을 실행하기 위한 프로세스가 필요하다.

하지만 메모리는 비어있는 상태이므로, 하드웨어 자체에 하드코드된 명령어들이 CPU에서 실행되어야 하는데,

이를 펌웨어라고 한다. 펌웨어는 크게 다음과 같은 두가지 종류가 있다.

BIOS vs UEFI

BIOS : Basic Input/Output System
저장장치의 부트 섹터를 읽고 화면에 기본적인 입출력을 할수있는 기능들이 포함되어있다.
16비트로 구동되기에 조작은 키보드로만 가능하다.

UEFI : Unified Extensible Firmware Interface
BIOS와 같은 기능을 하지만, 시스템 startup/initialization에 필요한 모든 정보들을 .efi
파일 안에 저장하며, 이 .efi 파일은 저장장치의 EFI System Partition(ESP) 라 불리는
파티션에 저장된다. ESP 는 부트로더 또한 포함하고 있다.

UEFI는 BIOS와 비교해서 더 많은 저장장치 호환, Secure/Trusted Boot 제공, GUI 제공 등
몇가지 기능들이 추가로 제공된다.

> How Shell command works
[참고](https://medium.com/@765/how-the-shell-works-internally-when-entering-a-command-42f08458870)


ps 커맨드를 입력했을때 shell이 나오는 것처럼, shell 또한 /bin 폴더에 위치한 프로그램들
중 하나일 뿐이다. /bin 디렉토리에서 ./bash, ./zsh 처럼 직접 실행할 수도 있다.

이를 참고로, Shell 명령어가 어떻게 처리되는지 디렉토리 속 모든 파일/폴더를 출력하는
```bash
$ ls -l
```
커맨드를 예시로 한단계씩 살펴보자.

모두가 알듯이, 명령어는 Enter(\0) 을 기준으로 처리된다.

```bash
'l' 's' ' ' '-' 'l' '\0'
```
과 같이 입력된 스트링은, 토큰화 과정을 통해

```bash
'l' 's' '\0' '-' 'l' '\0'
```
과 같이 공백을 구분자로 나뉘어지게 되고,
각 토큰에 맞는 funcion call이 존재하는지 PATH 디렉토리에서 확인하게 된다.

```bash
$ echo $PATH
``` 
를 통해 PATH 디렉토리 목록을 확인할 수 있는데, 해당 디렉토리에서 맞는 function call이 존재하지
않으면 오류메시지를 출력한다.

PATH 디렉토리중 하나인 /bin 폴더를 보자.

해당 디렉토리 안에는 실행 가능한 프로그램들이 존재하는데, bash, zsh와 같이 셸 자체는 물론

ls, cp, cd, find 와 같은 셸 명령어에서 사용되는 프로그램들이 존재한다.

이들은 명목상으로만 존재하는 것이 아닌, ./ls -l 과 같이 직접 실행할 수도 있다.
> Linux /bin directory

[참고](https://unix.stackexchange.com/questions/152346/what-is-the-difference-between-building-from-source-and-using-an-install-package

위 글을 참고해서, 리눅스에서 프로그램을 설치하는 방법은 크게 두가지가 있는데, 하나는 소스코드로부터 직접

컴파일하여 바이너리 파일을 만드는 방법과, 바이너리 파일을 직접 다운받아 /usr/bin 폴더에 넣는 방법이 있다.
 
 각각 장단점이 있지만, 전자는 복잡하나 개개인에 맞게 커스텀이 가능하고, 후자는 간편하다는 장점이 있다.

> Invoking syscall without standard library (custom made system call function)

 
[참고](https://the-linux-channel.the-toffee-project.org/index.php?page=5-tutorials-a-linux-system-call-in-c-without-a-standard-library&lang=en)

(linux)

[syscall.h](https://github.com/torvalds/linux/blob/master/include/linux/syscalls.h)

[syscall table for x86_64](https://github.com/torvalds/linux/blob/v3.13/arch/x86/syscalls/syscall_64.tbl)

[asm/unistd.h](https://github.com/ilbers/linux/blob/master/arch/sh/include/uapi/asm/unistd_64.h)

(glibc)

[unistd.h](https://pubs.opengroup.org/onlinepubs/7908799/xsh/unistd.h.html)

[sys/stat.h](https://pubs.opengroup.org/onlinepubs/7908799/xsh/sysstat.h.html)

[sys/syscall.h](https://github.com/lattera/glibc/blob/master/sysdeps/unix/sysv/linux/sys/syscall.h) -> kernel의 asm/unistd.h 참조

[syscall.S](https://github.com/lattera/glibc/blob/master/sysdeps/unix/sysv/linux/x86_64/syscall.S)
> 어셈블리는 안들었지만 약간의 어셈블리

```asm
// exit program syscall for x86_64 (64-bit)
mov rdi,rax /* syscall param 1 = rax (ret value of main) */
mov rax,60 /* SYS_exit */
syscall
ret

// exit program syscall for x86 (32-bit)
mov eax,1
syscall // == int 0x80 == int 80h == call interupt 80h 
// int를 이용해 인터럽트를 직접 부르는것은 선택 사항이며, x86_64 환경에서는 사용할수 없다.

```
다음과 같이 같은 시스템 콜도 아키텍쳐에 따라 다른 것을 볼수있다. 이는 위의 syscall table 에서
볼 수 있듯이,

syscall_64.tbl
```
<number> <abi> <name> <entry point>

...

57	common	fork			stub_fork
58	common	vfork			stub_vfork
59	64	    execve			stub_execve
60	common	exit			sys_exit
61	common	wait4			sys_wait4
62	common	kill			sys_kill
63	common	uname			sys_newuname

...
```

syscall_32.tbl
```
<number> <abi> <name> <entry point> <compat entry point>

0	i386	restart_syscall		sys_restart_syscall
1	i386	exit			    sys_exit
2	i386	fork			    sys_fork			stub32_fork
3	i386	read			    sys_read

...
```

과 같이 다른 어셈블리 명령이 필요하고, linux에서

> 그래서 System Call 이란 무엇인가?

syscall은 근본적으로 CPU Instruction이다. 따라서 각 CPU 아키텍쳐마다 다르며, 각 아키텍쳐에 맞는 
각 레지스터에 알맞는 값을 전달해야 하지만, 사용자가 이를 직접 전달하는것은 매우 어렵다. 따라서
compiler와 System call을 wrap한 라이브러리를 통해 아키텍쳐 종속적인 부분들을 자동으로 해결하고
처리할 수 있도록 하는 것이다.

그 라이브러리가 바로 glibc이며, 컴파일러는 gcc 이다.

glibc 는 한개의 파일이 아닌 여러개의 헤더파일등이 포함된 standard library이며, glibc 안에
unistd.h, sys/stat.h 와 같은 헤더파일들이 여럿 존재한다. 바로 위 syscall을 어셈블리로 직접 구현한것이
이 glibc를 사용하지 않기 위해서였으며, 시스템 콜을 운영체제와 상관없이 손쉽게 사용할 수 있도록
한단계의 추상화를 거친것이 c standard library (glibc) 의 시스템 콜이다.

+ C가 아닌 다른 언어들에서도 Syscall이 필요할텐데, 어떻게 사용하는가?
  
  : dependency를 추가하여 간접적으로 각 플랫폼의 libc에서 부르거나, (Python, Java)
  위의 standard library 없이 직접 어셈블리로 system call wrapper function을 만든 것처럼 각 커널에 맞는 라이브러리를 직접 구현하는 방법이 있다(Go, Pascal)

  [참고](https://stackoverflow.com/questions/26297151/how-do-non-c-languages-interact-with-operating-system)




[where to find definition](https://www.quora.com/Where-is-the-function-printf-defined-The-header-file-stdio-h-just-contains-the-declaration)

[printf()가 결국 write()를 호출하는 원리](https://stackoverflow.com/questions/21084218/difference-between-write-and-printf)
```
<stdio.h> printf() -> 
결국 <unistd.h> 의 write() syscall을 부름  -> 
write() 는 'sys/syscall.h' (정확히는 asm/unistd.h) 의 매크로를 참조해 넘버링된 syscall 함수 호출 ->
syscall.S의 어셈블리로 정의된 함수에 전달하여 syscall 호출.
```

> shell의 수도코드

(C로 작성된) 쉘도 근본적으로 프로세스이다. open, cd, write 등을 할때 직접 작성한 프로그램과 마찬가지로 libc를 이용하여 간접적으로 시스템 콜을 호출하는 것이다.

[참고](http://www.cs.cornell.edu/courses/cs414/2002fa/homework/shell/shell.html)

이처럼 C로 씌여진 Shell도 libc에서 Syscall을 간접적으로 호출하는 프로그램에 지나지 않는 것을 알수있음.
```
int main (int argc, char **argv) {
	while (1) {
		int childPid;
		char * cmdLine;

	        printPrompt();

	        cmdLine= readCommandLine();
		
		if (isBuiltInCommand(cmdLine)) {
		    executeBuiltInCommand(cmdLine);
		} 
        else {		
		     childPid = fork();
		     if (childPid == 0){
			exec ( getCommand(cmdLine));
			
		     } else {
			if (runInForeground(cmdLine)){
				wait (childPid);
			} else {
			        record in list of background jobs
			}		
        }
    }
}
```