# python 설치 (linux)
### 1. 필요한 플러그인 설치
* 루트계정으로 로그인 후 플러그인 설치 명령어 실행
```
yum install gcc openssl-devel bzip2-devel libffi-devel -y
```
### 2. 파이썬 사이트에서 최신 버전 다운로드
* 파이썬 압축파일 링크는 파이썬 사이트(https://www.python.org/downloads/)에서 복사해올 수 있다.
	* `Linux/UNIX` 선택 후 원하는 버전의 `Download Gzipped source tarball` 부분 링크 복사
* 아래 명령어 실행
```
wget https://www.python.org/ftp/python/3.10.5/Python-3.10.5.tgz
tar -xvf Python-3.10.5.tgz
```
### 3. 컴파일
```
cd Python-3.10.5/
./configure --enable-optimizations
```
### 4. 설치(빌드)
```
make altinstall
```
### 5. 별칭(alias) 생성
```
vi /root/.bashrc
```
* `.bashrc` 들어가서 아래 내용을 작성한다.
	* `alias python3="/usr/local/bin/python3.10"`
* 작성 및 저장 후 적용시킨다.
```
source /root/.bashrc
```
* 적용되었는지 버전 확인, 버전이 정상적으로 나온다면 설치 완료
```
python -V
```
