# python 삭제 (linux)
* python을 직접 빌드해 설치했으나 `make uninstall`, `make -n install` 등의 명령어가 정상적으로 동작하지 않을 때 수동 삭제 방법 설명
### 방법
* 실행하는 모든 명령어는 root 권한으로 실행한다.
* 예제 코드는 python 3.7을 기준으로 작성하였다. 삭제하려는 python 버전에 맞춰 실행한다.

#### 1.
```
$ rm -r ~/.local/lib/python3.7
```
- 위 디렉토리는 진행했던 작업에 따라 존재하지 않을 수도 있다.

#### 2.
```
$ cd /usr/local/bin
```
- 위 경로 아래 있는 **2to3-3.7**, **easy_install-3.7**, **idle3.7**, **pip3.7**, **pydoc3.7**, **python3.7**, **python3.7-config**, **python3.7m**, **python3.7m-config**, **pyvenv-3.7** 파일들 및 연결된 symbolic link들을 삭제한다.

#### 3.
```
$ cd /usr/local/lib
```
- 위 경로 아래에 있는 **libpython3.7m** 파일, **python3.7** 디렉토리를 삭제한다.

#### 4.
```
$ cd /usr/local/share/man/man1
```
- 위 경로 아래에 있는 **python3.7.1** 파일 및 연관된 symbolic link들을 삭제한다.

#### 5.
```
$ cd /usr/local/include
```
- 위 경로 아래에 있는 **python3.7m** 디렉토리를 삭제한다.
