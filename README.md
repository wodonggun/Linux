# Linux
Linux Github

# 파이프 통신

<unistd.h>헤더를 통해 
pipe 클래스를 호출해야 함.

- pipe는 배열크기2의 데이터를 갖는다. `fd[1]은 파이프 입구(write 위치), fd[0]는 파이프 출구(read 위치)`
```c++
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    int fd[2];
    pid_t pid;
    char buffer[255];
    int status;
 
    if(pipe(fd) == -1) { // pipe 함수를 사용해 파이프를 생성한다. pipe 함수의 인자로는 파일 기술자를 저장할 배열을 지정한다.
        puts("pipe 에러");
        exit(1);
    }
 
    switch(pid = fork()) { // fork 함수를 사용해 자식 프로세스를 생성한다.
        case -1: //생성 에러
            perror("fork");
            exit(1);
            break;
            
        case 0: //자식일때
            write(fd[1], "Child Process: 연결 성공! \n", 255);
            break;

        default:  //부모일때
            read(fd[0],buffer,255);
            puts(buffer);
            break;
    }
 
    return 0;
}
```
