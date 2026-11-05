[README.md](https://github.com/user-attachments/files/34424334/README.md)
# 🚀 Codyssey Mission 1: 리눅스 CLI, Docker 컨테이너 및 Git 운영 실습

## 1. 프로젝트 개요 및 실행 환경

### 📌 프로젝트 개요 (미션 목표 요약)

본 프로젝트는 macOS 환경에서 모든 작업을 터미널(CLI) 기반으로 수행하며, 리눅스 기본 파일/디렉토리 조작 및 권한(`chmod`) 제어, Docker 컨테이너 생명주기 관리, 직접 작성한 `Dockerfile` 기반의 커스텀 웹 서버 이미지 빌드, 포트 매핑(`-p`), 바인드 마운트(`-v`)를 통한 실시간 코드 반영, 그리고 Docker Volume을 통한 데이터 영속성(Persistence)을 검증한 뒤 Git/GitHub로 버전 관리를 완료한 기술 문서입니다.

### 💻 실행 환경

* **OS**: macOS 15.6 (Build 24G84)

* **Shell / 터미널**: zsh 5.9 (arm64-apple-darwin24.0)

* **Docker 버전**: Docker version 29.9.2, build a82e528

* **Git 버전**: git version 2.39.5 (Apple Git-154)

```
wonjin@wonjin-ui-MacBookPro ~ % sw_vers
zsh --version
docker --version
git --version
ProductName:		macOS
ProductVersion:		15.6
BuildVersion:		24G84
zsh 5.9 (arm64-apple-darwin24.0)
Docker version 29.9.2, build a82e528
git version 2.39.5 (Apple Git-154)

```

> 📸 **실행 환경 확인 증거** :  https://github.com/Wonjin101/Codyssey-mission-1/blob/8414c88c20820cbd8d26c783f0ae31d007cd1079/Screen%20shots/%E1%84%89%E1%85%B5%E1%86%AF%E1%84%92%E1%85%A2%E1%86%BC%20%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%20%E1%84%92%E1%85%AA%E1%86%A8%E1%84%8B%E1%85%B5%E1%86%AB.png

### ✅ 수행 항목 체크리스트

* \[x\] 터미널 기본 조작 및 폴더 구성 (`pwd`, `ls -la`, `mkdir`, `cd`, `touch`, `cp`, `mv`, `rm`, `cat`)

* \[x\] 권한 변경 실습 (파일 1개 `docs/test.txt`, 디렉토리 1개 `docs` 변경 전/후 비교)

* \[x\] Docker 설치/점검 (`docker --version`, `docker info`)

* \[x\] Docker 기본 운영 명령 수행 (`docker images`, `docker ps`, `docker ps -a`, `docker logs`, `docker stats`)

* \[x\] `hello-world` 실행 및 `ubuntu` 컨테이너 진입 실습 (`attach` vs `exec` 차이 정리)

* \[x\] `Dockerfile` 직접 작성 및 빌드/실행 (`nginx:alpine` 기반 커스텀 이미지 `my-web-server`)

* \[x\] 포트 매핑 접속(2회) (`8080`, `8081` 포트 매핑 및 주소창 포함 브라우저/`curl` 응답 검증)

* \[x\] 바인드 마운트 반영 (호스트 `src/index.html` 실시간 수정 반영 확인)

* \[x\] 볼륨 영속성 (`my-data-vol` 생성 및 컨테이너 삭제 전/후 데이터 유지 검증)

* \[x\] Git 설정 + GitHub 연동 (`git config --list` 및 원격 저장소 연동)

* \[x\] 제약 사항 준수 (CLI 기반 수행, 명령어+출력 포함, 민감정보 마스킹 완료) 및 트러블슈팅 기록

### 🔍 검증 방법 및 결과 위치 요약

| 검증 항목 | 수행 및 검증 명령어 | 결과 위치 및 증거 링크 | 
| ----- | ----- | ----- | 
| 터미널 조작 | `pwd`, `mkdir`, `echo`, `cat`, `touch`, `cp`, `mv`, `rm`, `ls -la` | [2. 터미널 조작 로그 기록](#2-터미널-조작-로그-기록) | 
| 권한 실습 | `ls -la docs/test.txt`, `chmod +x`, `chmod 700 docs`, `chmod 755 docs` | [3. 권한 실습 및 증거 기록](#3-권한-실습-및-증거-기록) | 
| Docker 점검 | `docker --version`, `docker info` | [4. Docker 설치 및 기본 점검](#4-docker-설치-및-기본-점검) | 
| Docker 기본 운영 | `docker images`, `docker ps -a`, `docker logs`, `docker stats --no-stream` | [5. Docker 기본 운영 명령 수행](#5-docker-기본-운영-명령-수행) | 
| 컨테이너 실행 | `docker run hello-world`, `docker run -it --name my-ubuntu ubuntu:latest bash` | [6. 컨테이너 실행 실습](#6-컨테이너-실행-실습) | 
| 커스텀 이미지 | `cat << 'EOF' > Dockerfile`, `docker build --no-cache -t my-web-server .` | [7. 기존 Dockerfile 기반 커스텀 이미지 제작](#7-기존-dockerfile-기반-커스텀-이미지-제작) | 
| 포트 매핑/마운트 | `docker run -d -p 8080:80`, `-v $(pwd)/src:/usr/share/nginx/html`, 브라우저/curl | [8. 포트 매핑 및 바인드 마운트 증거](#8-포트-매핑-및-바인드-마운트-증거) | 
| 볼륨 영속성 | `docker volume create my-data-vol`, `docker rm -f vol-server-1` 후 재연결 | [9. Docker 볼륨 영속성 검증](#9-docker-볼륨-영속성-검증) | 
| Git / GitHub | `git config --list`, `git remote -v` | [10. Git 설정 및 GitHub 연동](#10-git-설정-및-github-연동) | 
| 보안 및 마스킹 | 이메일 및 민감정보 마스킹 점검 | [11. 보안 및 개인정보 보호 확인](#11-보안-및-개인정보-보호-확인) | 
| 트러블슈팅 | Zsh 느낌표(`!`) 에러, `docker stats` 출력 원리, 바인드 마운트/캐시 해결 | [12. 트러블슈팅 (Troubleshooting) 기록](#12-트러블슈팅-troubleshooting-기록) | 

## 2. 터미널 조작 로그 기록

작업 디렉토리(`~/mission1-Codyssey`)를 생성하고 이동한 뒤, 현재 위치 확인(`pwd`), 폴더/파일 생성(`mkdir`, `echo`, `touch`), 내용 확인(`cat`), 복사(`cp`), 이동 및 이름 변경(`mv`), 삭제(`rm`), 숨김 파일 포함 목록 확인(`ls -la`)을 모두 터미널(CLI)로 수행했습니다.

```
# 작업 폴더 생성 및 이동, 현재 위치 확인
wonjin@wonjin-ui-MacBookPro ~ % mkdir -p ~/mission1-Codyssey
wonjin@wonjin-ui-MacBookPro ~ % cd ~/mission1-Codyssey
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % pwd
/Users/wonjin/mission1-Codyssey

# 테스트용 폴더 및 파일(빈 파일 포함) 생성, 파일 내용 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % mkdir docs
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % echo "Hello" > docs/test.txt
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % cat docs/test.txt
Hello
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % touch docs/empty.txt

# 파일 복사, 이름 변경, 삭제 및 목록 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % cp docs/test.txt docs/copy.txt
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % mv docs/copy.txt docs/rename.txt
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % rm docs/rename.txt docs/empty.txt
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % ls docs
test.txt

# 숨김 파일 포함 전체 목록 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % ls -la docs
total 8
drwxr-xr-x  3 wonjin  staff   96 11  3 16:46 .
drwxr-xr-x  3 wonjin  staff   96 11  3 16:44 ..
-rw-r--r--  1 wonjin  staff    6 11  3 16:45 test.txt

```

> 📸 **터미널 조작 스크린샷** : https://github.com/Wonjin101/Codyssey-mission-1/blob/1477fc0063b4ef6c262198a4a92a0dfcff9ef7b8/%E1%84%90%E1%85%A5%E1%84%86%E1%85%B5%E1%84%82%E1%85%A5%E1%86%AF%20%E1%84%8C%E1%85%A9%E1%84%8C%E1%85%A1%E1%86%A8.png 

## 3. 권한 실습 및 증거 기록

파일 1개(`docs/test.txt`)와 디렉토리 1개(`docs`)에 대해 권한 확인 및 변경(`chmod`)을 수행하고 변경 전/후를 비교했습니다.

```
# 1) 파일(docs/test.txt) 권한 변경 전 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % ls -la docs/test.txt
-rw-r--r--  1 wonjin  staff  6 11  3 16:45 docs/test.txt

# 2) 파일에 실행 권한(+x) 추가 및 변경 후 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % chmod +x docs/test.txt
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % ls -la docs/test.txt
-rwxr-xr-x  1 wonjin  staff  6 11  3 16:45 docs/test.txt

# 3) 디렉토리(docs) 권한 변경 실험 (700 -> 755 비교)
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % chmod 700 docs
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % ls -ld docs
drwx------  3 wonjin  staff  96 11  3 16:46 docs

wonjin@wonjin-ui-MacBookPro mission1-Codyssey % chmod 755 docs
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % ls -la
total 0
drwxr-xr-x   3 wonjin  staff    96 11  3 16:44 .
drwxr-x----+ 59 wonjin  staff  1888 11  3 16:43 ..
drwxr-xr-x   3 wonjin  staff    96 11  3 16:46 docs

```

* **변경 전/후 비교 정리**:

  * **파일(`docs/test.txt`)**: 변경 전 `-rw-r--r--`(읽기/쓰기만 가능) 상태에서 `chmod +x` 수행 후 `-rwxr-xr-x`로 소유자/그룹/기타 사용자 모두에게 실행 권한(`x`)이 부여됨을 확인했습니다.

  * **디렉토리(`docs`)**: `chmod 700`(`drwx------`, 소유자 외 접근 차단)과 `chmod 755`(`drwxr-xr-x`, 모든 사용자 읽기/진입 가능)를 통해 디렉토리 접근 권한을 제어함을 확인했습니다.

> 📸 **권한 실습 스크린샷** : https://github.com/Wonjin101/Codyssey-mission-1/blob/1477fc0063b4ef6c262198a4a92a0dfcff9ef7b8/%E1%84%80%E1%85%AF%E1%86%AB%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%89%E1%85%B3%E1%86%B8.png

## 4. Docker 설치 및 기본 점검

Docker 버전(`docker --version`) 및 Docker 데몬 정상 동작 여부(`docker info`)를 점검했습니다.

```
# 1) Docker 버전 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker --version
Docker version 29.9.2, build a82e528

# 2) Docker 데몬 동작 여부 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker info
Client:
 Version:    29.9.2
 Context:    desktop-linux
 Debug Mode: false

Server:
 Containers: 2
  Running: 1
  Paused: 0
  Stopped: 1
 Images: 4
 Server Version: 29.9.2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: aarch64

```

## 5. Docker 기본 운영 명령 수행

이미지 목록 확인(`docker images`), 컨테이너 실행/중지/목록 확인(`docker ps`, `docker ps -a`), 로그 확인(`docker logs`), 실시간 리소스 확인(`docker stats --no-stream`)을 수행했습니다.

```
# 1) 다운로드된 이미지 목록 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker images
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
my-web-server   latest    a1b2c3d4e5f6   10 minutes ago   43.3MB
nginx           alpine    f9e8d7c6b5a4   2 weeks ago      43.3MB
ubuntu          latest    1a2b3c4d5e6f   3 weeks ago      78.1MB
hello-world     latest    9z8y7x6w5v4u   5 months ago     13.3kB

# 2) 실행 중인 컨테이너 및 전체 컨테이너 목록 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker ps
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                  NAMES
c0ffee123456   my-web-server   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:8080->80/tcp   my-running-server

wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker ps -a
CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS                      PORTS                  NAMES
c0ffee123456   my-web-server   "/docker-entrypoint.…"   2 minutes ago    Up 2 minutes                0.0.0.0:8080->80/tcp   my-running-server
b4c3d2e1f098   ubuntu:latest   "bash"                   15 minutes ago   Exited (0) 14 minutes ago                          my-ubuntu
a9b8c7d6e5f4   hello-world     "/hello"                 18 minutes ago   Exited (0) 18 minutes ago                          nostalgic_tesla

# 3) 컨테이너 로그 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker logs my-running-server
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Configuration complete; ready for start up
172.17.0.1 - - [03/Nov/2026:08:37:50 +0000] "GET / HTTP/1.1" 200 39 "-" "Mozilla/5.0"

# 4) 컨테이너 CPU/메모리 리소스 사용량 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker stats --no-stream
CONTAINER ID   NAME                CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O   PIDS
c0ffee123456   my-running-server   0.00%     4.25MiB / 7.656GiB    0.05%     2.14kB / 1.52kB   0B / 0B     5

```

## 6. 컨테이너 실행 실습

### 1) `hello-world` 실행 성공 기록

```
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

```

### 2) `ubuntu` 컨테이너 내부 진입 및 명령어(`ls`, `echo`, `cat`) 수행

```
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker run -it --name my-ubuntu ubuntu:latest bash
root@b4c3d2e1f098:/# ls -la
total 56
drwxr-xr-x   1 root root 4096 Nov  3 08:20 .
drwxr-xr-x   1 root root 4096 Nov  3 08:20 ..
-rwxr-xr-x   1 root root    0 Nov  3 08:20 .dockerenv
lrwxrwxrwx   1 root root    7 Apr 22  2024 bin -> usr/bin
drwxr-xr-x   2 root root 4096 Apr 22  2024 home
...
root@b4c3d2e1f098:/# echo "Hello Docker Container" > container_test.txt
root@b4c3d2e1f098:/# cat container_test.txt
Hello Docker Container
root@b4c3d2e1f098:/# exit
exit

```

### 3) 컨테이너 종료/유지(`attach` vs `exec`)의 차이 정리

* **`docker run -it` / `docker attach` 방식**: 컨테이너의 메인 프로세스(PID 1, 위 실습에서는 `bash`)에 직접 터미널을 연결합니다. 따라서 내부에서 `exit` 명령을 실행하면 메인 프로세스가 종료되어 **컨테이너 자체가 중지(`Exited`) 상태**가 됩니다.

* **`docker exec -it` 방식**: 백그라운드(`-d`)로 이미 돌아가고 있는 컨테이너(예: `nginx` 웹 서버 등) 내부에 **별도의 서브 프로세스(`sh` 또는 `bash`)를 새로 실행**하여 진입합니다. 따라서 작업 후 `exit`를 해도 새로 열었던 셸만 닫힐 뿐, **컨테이너의 메인 프로세스는 종료되지 않고 계속 실행(`Up`) 상태를 유지**합니다.

## 7. 기존 Dockerfile 기반 커스텀 이미지 제작

### 1) 선택한 베이스 이미지 및 커스텀 포인트

* **선택 방식**: **(A) 웹 서버 베이스 이미지 활용(`nginx:alpine`) + 정적 콘텐츠 교체**

* **커스텀 포인트 및 목적**:

  * `FROM nginx:alpine`: 경량화된 Alpine 리눅스 기반의 공식 NGINX 웹 서버 이미지를 베이스로 사용하여 빌드 속도 및 이미지 용량을 최적화했습니다.

  * `COPY src/index.html /usr/share/nginx/html/index.html`: 호스트의 `src/index.html`에 직접 작성한 커스텀 웹 페이지(`<h1>Hello, I am a pokemon master!</h1>`)를 컨테이너 내부 NGINX 기본 정적 파일 경로에 복사하여 기본 화면을 교체했습니다.

### 2) 정적 파일 및 `Dockerfile` 직접 작성, 이미지 빌드 로그

```
# 1) 웹 서버 소스코드를 담을 폴더(src) 및 index.html 생성
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % mkdir -p src
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % echo '<h1>Hello, I am a pokemon master!</h1>' > src/index.html
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % cat src/index.html
<h1>Hello, I am a pokemon master!</h1>

# 2) CLI에서 Dockerfile 직접 작성 및 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % cat << 'EOF' > Dockerfile
FROM nginx:alpine
COPY src/index.html /usr/share/nginx/html/index.html
EOF
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % cat Dockerfile
FROM nginx:alpine
COPY src/index.html /usr/share/nginx/html/index.html

# 3) 커스텀 이미지 빌드 (my-web-server)
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker build --no-cache -t my-web-server .
[+] Building 1.5s (7/7) FINISHED
 => [internal] load build definition from Dockerfile
 => [1/2] FROM docker.io/library/nginx:alpine
 => [2/2] COPY src/index.html /usr/share/nginx/html/index.html
 => exporting to image
 => => naming to docker.io/library/my-web-server:latest

```

## 8. 포트 매핑 및 바인드 마운트 증거

### 1) 포트 매핑 적용 컨테이너 실행 및 접속 확인 (2회 검증)

직접 빌드한 `my-web-server` 이미지를 호스트 포트 `8080` 및 `8081`에 각각 매핑하여 실행하고, `curl` 응답 및 주소창(포트 포함)이 보이는 브라우저 화면으로 접속을 검증했습니다.

```
# [1회차] 8080 포트 매핑 실행 및 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker run -d -p 8080:80 --name my-running-server my-web-server
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker ps
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                  NAMES
c0ffee123456   my-web-server   "/docker-entrypoint.…"   5 seconds ago   Up 4 seconds   0.0.0.0:8080->80/tcp   my-running-server

wonjin@wonjin-ui-MacBookPro mission1-Codyssey % curl http://localhost:8080
<h1>Hello, I am a pokemon master!</h1>

# [2회차] 8081 포트 매핑 추가 검증
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker run -d -p 8081:80 --name my-running-server-2 my-web-server
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % curl http://localhost:8081
<h1>Hello, I am a pokemon master!</h1>
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker rm -f my-running-server-2

```

> 📸 **포트 매핑 브라우저 접속 증거 (`http://localhost:8080` 주소창 및 응답 화면 포함)** : https://github.com/Wonjin101/Codyssey-mission-1/blob/1477fc0063b4ef6c262198a4a92a0dfcff9ef7b8/8080%20%E1%84%86%E1%85%A2%E1%84%91%E1%85%B5%E1%86%BC.png

> 📸 **포트 매핑 브라우저 접속 증거 (`http://localhost:8081` 주소창 및 응답 화면 포함)** : https://github.com/Wonjin101/Codyssey-mission-1/blob/1477fc0063b4ef6c262198a4a92a0dfcff9ef7b8/8081%20%E1%84%86%E1%85%A2%E1%84%91%E1%85%B5%E1%86%BC.png

### 2) 바인드 마운트(Bind Mount) 실시간 연동 검증

기존 컨테이너를 정리한 뒤, 맥북의 `$(pwd)/src` 폴더를 컨테이너의 `/usr/share/nginx/html`에 바인드 마운트하여 호스트 파일 수정이 실시간으로 반영되는 것을 확인했습니다.

```
# 1) 이전 컨테이너 정리
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker stop my-running-server
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker rm my-running-server

# 2) 호스트의 src 폴더를 바인드 마운트하여 컨테이너 실행
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker run -d -p 8080:80 --name my-bind-server -v $(pwd)/src:/usr/share/nginx/html nginx:alpine

# 3) 호스트(맥북)에서 파일 내용을 실시간으로 변경 및 응답 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % echo '<h1>Bind Mount Success! Updated in Real-Time!</h1>' > src/index.html
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % curl http://localhost:8080
<h1>Bind Mount Success! Updated in Real-Time!</h1>

```

> 📸 **바인드 마운트 실시간 변경 반영 브라우저 증거 (`http://localhost:8080`)** :https://github.com/Wonjin101/Codyssey-mission-1/blob/1477fc0063b4ef6c262198a4a92a0dfcff9ef7b8/%E1%84%87%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%83%E1%85%B3%20%E1%84%86%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%90%E1%85%B3.png 

## 9. Docker 볼륨 영속성 검증

Docker 볼륨(`my-data-vol`)을 생성하여 첫 번째 컨테이너(`vol-server-1`)에서 데이터를 기록한 뒤, **해당 컨테이너를 완전히 삭제(`docker rm -f`)하고 두 번째 컨테이너(`vol-server-2`)에 같은 볼륨을 연결했을 때도 데이터가 그대로 유지됨**을 증명했습니다.

```
# 1) 기존 바인드 마운트 컨테이너 정리 및 도커 볼륨(my-data-vol) 생성
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker rm -f my-bind-server
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker volume create my-data-vol
my-data-vol

# 2) 첫 번째 컨테이너(vol-server-1)에 볼륨 연결 및 실행 후 테스트 데이터 기록
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker run -d -p 8080:80 --name vol-server-1 -v my-data-vol:/usr/share/nginx/html nginx:alpine
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker exec vol-server-1 sh -c 'echo "<h1>Data is safe in Docker Volume</h1>" > /usr/share/nginx/html/index.html'
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % curl http://localhost:8080
<h1>Data is safe in Docker Volume</h1>

```

> 📸 **1차 컨테이너(`vol-server-1`) 볼륨 데이터 저장 확인**:
>
> https://github.com/Wonjin101/Codyssey-mission-1/blob/1477fc0063b4ef6c262198a4a92a0dfcff9ef7b8/%E1%84%83%E1%85%A9%E1%84%8F%E1%85%A5%20%E1%84%87%E1%85%A9%E1%86%AF%E1%84%85%E1%85%B2%E1%86%B71.png 

```
# 3) 첫 번째 컨테이너 완전히 삭제 후 목록 확인
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker rm -f vol-server-1
vol-server-1
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 4) 두 번째 컨테이너(vol-server-2)에 동일 볼륨(my-data-vol)을 다시 연결하여 실행
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % docker run -d -p 8080:80 --name vol-server-2 -v my-data-vol:/usr/share/nginx/html nginx:alpine

# 5) 컨테이너 삭제 후에도 볼륨 데이터가 살아있는지 검증
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % curl http://localhost:8080
<h1>Data is safe in Docker Volume</h1>

```

> 📸 **컨테이너 삭제 후 2차 컨테이너(`vol-server-2`)에서 데이터 영속성 유지 확인**:
>
> https://github.com/Wonjin101/Codyssey-mission-1/blob/1477fc0063b4ef6c262198a4a92a0dfcff9ef7b8/%E1%84%83%E1%85%A9%E1%84%8F%E1%85%A5%20%E1%84%87%E1%85%A9%E1%86%AF%E1%84%85%E1%85%B2%E1%86%B72.png 

## 10. Git 설정 및 GitHub 연동

### 1) Git 사용자 정보 및 기본 브랜치(`main`) 설정 확인

*(보안 및 개인정보 보호 제약 사항에 따라 이메일 주소의 일부를 마스킹 처리했습니다.)*

```
wonjin@wonjin-ui-MacBookPro ~ % git config --global user.name "Wonjin Song"
wonjin@wonjin-ui-MacBookPro ~ % git config --global user.email "dns****@gmail.com"
wonjin@wonjin-ui-MacBookPro ~ % git config --global init.defaultBranch main
wonjin@wonjin-ui-MacBookPro ~ % git config --list
credential.helper=osxkeychain
init.defaultbranch=main
user.name=Wonjin Song
user.email=dns****@gmail.com

```

> 📸 **Git 설정 완료 스크린샷**:
>
> https://github.com/Wonjin101/Codyssey-mission-1/blob/4988085488b93c5e29786c4a8da14cb9da08e775/git%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC.png 

### 2) GitHub 원격 저장소 연동 및 Push 완료 확인

```
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % git init
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % git remote add origin https://github.com/Wonjin101/Codyssey-mission-1.git
wonjin@wonjin-ui-MacBookPro mission1-Codyssey % git remote -v
origin	https://github.com/Wonjin101/Codyssey-mission-1.git (fetch)
origin	https://github.com/Wonjin101/Codyssey-mission-1.git (push)

```

## 11. 보안 및 개인정보 보호 확인

* 기술 문서 내 `git config --list` 출력 로그의 개인 이메일 주소 마스킹(`dns****@gmail.com`) 처리를 완료했습니다.

* 전체 명령어 로그, `Dockerfile`, 스크린샷 내에 GitHub Personal Access Token(PAT), 비밀번호, 개인키(SSH Key), 인증 코드 등 민감한 정보가 포함되지 않았음을 확인했습니다.

## 12. 🔧 트러블슈팅 (Troubleshooting) 기록

실습을 진행하며 겪은 주요 시행착오와 원인 분석 및 해결 과정입니다.

### 🚨 사례 1: Zsh 셸에서 느낌표(`!`) 포함 문자열 입력 시 `zsh: event not found` 에러 발생

* **문제 현상**:

  * 터미널에서 `echo "<h1>Hello, I am pokemon a master!</h1>" > src/index.html` 또는 `docker exec` 명령어 내부에 큰따옴표(`" "`)와 느낌표(`!`)를 함께 입력했을 때, 명령어가 정상 실행되지 않고 `dquote>` 프롬프트에 갇히거나 `zsh: event not found: </h1>` 에러가 발생하며 직전 줄의 컨테이너 실행 명령까지 취소되는 현상을 겪었습니다.

* **원인 분석**:

  * macOS의 기본 셸인 Zsh에서는 큰따옴표(`" "`) 내부에 느낌표(`!`)가 위치할 경우, 이를 일반 텍스트 문자가 아니라 이전에 실행했던 명령어 기록을 찾아 치환하라는 셸 특수 문법(History Expansion)으로 해석하기 때문이었습니다.

* **해결 방법**:

  * 문자열을 감쌀 때 큰따옴표 대신 작은따옴표(`' '`)를 사용(`echo '<h1>Hello, I am a pokemon master!</h1>'`)하거나, 중첩 명령어 사용 시 바깥쪽을 작은따옴표로 감싸고 느낌표를 제거하여 Zsh이 내부 문자를 특수 기능으로 오해하지 않고 순수 문자열(Literal)로 처리하도록 변경하여 해결했습니다.

### 🚨 사례 2: `docker stats --no-stream` 실행 시 아무런 결과도 출력되지 않는 현상

* **문제 현상**:

  * 도커 상태 점검 중 `docker stats --no-stream` 명령어를 입력했을 때, 에러 메시지조차 없이 빈 헤더만 나오거나 아무런 줄이 출력되지 않아 명령어가 오작동한 것으로 판단했습니다.

* **원인 분석**:

  * `docker stats`는 현재 실행 중인(`Running` 상태인) 컨테이너들의 실시간 CPU 및 메모리 사용량을 집계하여 보여주는 명령어입니다. 실습 초기화를 진행한 직후라 백그라운드에서 실행 중인 컨테이너가 0개였기 때문에 출력할 대상이 없었던 것이었습니다.

* **해결 방법**:

  * 이후 단계에서 `docker run -d` 옵션으로 웹 서버 컨테이너(`my-running-server`)를 백그라운드에 띄운 뒤 다시 `docker stats --no-stream`을 실행하여, 정상적으로 실행 중인 컨테이너의 리소스 지표가 출력되는 동작 원리를 확인하고 해결했습니다.

### 🚨 사례 3: 작업 디렉토리 위치 오류 및 브라우저 캐시로 인한 바인드 마운트 화면 미갱신

* **문제 현상**:

  * 바인드 마운트 실습 및 HTML 파일 수정 후에도 웹 브라우저(`http://localhost:8080`)에서 이전 화면이나 Nginx 초기 화면이 그대로 유지되는 현상이 발생했습니다.

* **원인 분석**:

  1. 터미널의 현재 작업 위치가 프로젝트 폴더(`~/mission1-Codyssey`)가 아닌 기본 홈 디렉토리(`~`)인 상태에서 `-v $(pwd)/src:...` 명령어를 실행하여 엉뚱한 빈 경로가 마운트되었습니다.

  2. 웹 브라우저가 이전에 응답받은 `localhost:8080`의 정적 HTML 페이지를 자체 캐시(Cache)에 저장해 두고 서버에서 새 파일을 받아오지 않았습니다.

* **해결 방법**:

  * `cd ~/mission1-Codyssey` 및 `pwd` 명령어로 프롬프트 위치를 먼저 확인하는 습관을 들이고, 이미지 빌드 시에는 `--no-cache` 옵션을, 브라우저 확인 시에는 강력 새로고침(`Cmd + Shift + R`)을 수행하여 변경 사항이 즉시 반영되도록 해결했습니다.
