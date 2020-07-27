# WAS(Tomcat) 프로세스가 죽지 않는 문제

> shutdown.sh을 실행해도 프로세스가 죽지않고 남아있는 문제

---------------------------------------------------------------------------------------

### 1. 문제
- 패치 도중 서버가 올라가지 않는 문제 발생 

### 2. 원인
- WAS를 shutdown 시킨 후 배포후에 startup을 시키는데 프로세스가 죽지않고 계속 남아있어 메모리영역을 잡고있어 메모리풀이 발생한 것

### 3. 대안
> 1) kill 명령어로 해당 프로세스를 강제로 종료처리 -> 매번 일일이 pid를 찾아 제거해야하며 실수로 타시스템의 프로세스 종료 시 문제 발생 -> 위험도가 큼

> 2) startup 할 때 해당 프로세스 id를 남기고  그 pid값을 가지고 shutdown시키면 매번 pid값을 찾아서 강제종료 시키지 않아도 됨

### 4. 해결방법
- 3-2의 방법을 적용

- TOMCAT_HOME/bin/startup.sh 수정

- exec "$PRGDIR"/"$EXECUTABLE" start "$@"  줄  바로 위
export CATALINA_PID=/home/server/tomcat/bin/catalina.pid 추가

- 위 옵션값을 추가하고 startup.sh을 실행 시 CATALINA_PID가 적용되어 해당 경로에 catalina.pid 파일이 저장되었다고 나옴

- TOMCAT_HOME/bin/shutdown.sh 수정

- exec "$PRGDIR"/"$EXECUTABLE" stop "$@" 줄 바로 위
export CATALINA_PID=/home/server/tomcat/bin/catalina.pid 추가

- exec "$PRGDIR"/"$EXECUTABLE" stop "$@" -> exec "$PRGDIR"/"$EXECUTABLE" stop -force "$@"로 변경

### 5.평가
- shutdown시에 남아있던 프로세스가 잘 종료되어 사용중인 메모리 영역도 평균 최대 용량의 30%수준에 머무름 

### 6.비고
- startup, shutdown 시 CATALINA_PID 옵션 값을 추가 하기 전에 비해 3~5초 가량 걸리지만 주기적으로 종료되지않던 프로세스들을 직접 종료시켜줘야되었던 것에 비해 훨씬 편리해짐.  