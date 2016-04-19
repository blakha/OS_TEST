Rotation-based reak/write lock
======
OS Project2 Due date April 20, 2016 9:00 pm


###커널과 테스트 프로그램 빌드하기
타이젠 커널을 빌드하기 위해 먼저 os-team3의 project2 브랜치를 클론해야 합니다.<br/>
bash 등의 터미널에 작업을 할 디렉토리에 들어가서 다음과 같은 명령어를 입력합니다.
```
git clone -b project2 https://github.com/swsnu/os-team3.git
```

클론된 os-team3 디렉토리로 들어가서 다음고 같은 명령어를 입력하면 커널을 빌드할 수 있습니다. 
```
./build.sh tizen_tm1 USR
```

커널의 빌드가 끝난 후 테스트 프로그램을 빌드해 주어야 합니다. testApp 디렉토리로 들어가서 Makefile 파일이 있는 것을 확인합니다. vim이나 gedit 등의 텍스트 편집기를 이용하여 Makefile 내의 테스트 프로그램 디렉토리를 현재 디렉토리에 맞게 수정합니다.<br/>
수정이 끝난 후 터미널에 `make`를 입력하면 테스트 프로그램이 빌드가 됩니다.<br/>
이제 다음과 같은 명령어를 입력하면 테스트 프로그램을 실행할 수 있습니다.
```
./test
```
<br/>
### rotation-based read/write lock의 구현
rotation-based read/write lock은 디바이스의 회전 각도에 따라 획득하게 되는 lock입니다. rotation based 이기 때문에 인자로 각도 범위를 함께 넘겨줍니다. 만약 현재 각도가 인자로 넘겨준 범위에 들어간다면 lock을 획득할 기회를 얻게 됩니다.
rotation-based read/write lock을 요청하는 함수의 프로토타입은 다음과 같습니다.
```
int rot_readlock(rotation_range *rot);
int rot_writelock(rotation_range *rot);
```
또, lock을 풀어주는 함수의 프로토타입은 다음과 같습니다.
```
void rot_readunlock();
void rot_writeunlock();
```
먼저 read lock는 현재 각도에 range가 포함되면 이미 겹치거나 같은 범위에 다른 프로세스가 read lock을 가지고 있더라도 획득할 수 있습니다. 하지만 write lock은 혅현재 각도에  range가 포함되더라도 이미 다른 프로세스에서 획득한 write lock와 범위가 겹친다면
write lock을 획득할 수 없습니다.


