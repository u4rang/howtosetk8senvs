본 문서는 지급받은 개인 OA 기기에 K8S 환경을 구성하여 활용함에 있습니다.

Vagrant 을 기반으로 가상 머신 환경을 구성하고, Ansible 를 통하여 개발환경에 필요한 패키지 설치, Docker 를 이용하여 K8S 환경을 구성 합니다.

# 1. Docker Toolbox for Windows 설치

## 1.1 Hyper-V 기능 해제

![image-20210805111617140](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805111617140.png)



## 1.2 Docker Toolbox for Windows 다운로드 및 설치

- 다운로드 URL : https://github.com/docker/toolbox/releases

  ![](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805111859039.png)

- 설치파일 실행

  ![image-20210805112010996](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805112010996.png)

- 선택화면에서 "Git for Windows" 는 선택하지 않는다.

  ![image-20210805112643475](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805112643475.png)

  - Windows 의 git 클라이언트는 텍스트 파일의 개행 코드 CR+LF 와 LF 자동 변환 기능이 있는데, Windows와 Linux, Mac 사용자들이 함께 개발할 떄는 매우 유용한 기능이지만 컨테이너를 빌드 할 때는 문제를 야기한다.

    - 문제점
      - 컨테이너는 리눅스 기반의 기술이기 떄문에 텍스트의 개행 코드는 LF 이다. 
      - 그러나 Windows 의 개행 코드는 CR+LF 이므로 이 차이는 컨테이너를 빌드하고 실행할 떄 사용하는 쉘 스크립트 파일에서 문제를 야기한다.
    - 해결방법
      1. 'gitattributes' 에 Repository 단위, 확장자 기준으로 변환 여부 설정
      2. git 클라이언트 설치 시 변환 기능을 해제
    - 선택
      - 별도의 git 클라이언트를 설치하면서 변환 기능을 해제하는 방법을 선택

  - Install 버튼을 클릭

    ![image-20210805112806833](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805112806833.png)

- git 을 설치하지 않아서 바탕화면에 생성된 Docker Terminal 아이콘은 실행되지 않음

  ![image-20210805113227293](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805113227293.png)


## 1.3 Git Client 설치

- 다운로드 URL : https://git-scm.com/download/win

- 설치 진행 중 다음 단계에서 "Checkout as-is, commit as-is" 선택

  ![image-20210805115658518](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805115658518.png)



## 1.4 Docker 설치 확인

- "Docker Quickstart Terminal" 을 선택하면 다음과 같이 설치가 진행된다.

  ![image-20210805120207307](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805120207307.png)

- "$ docker version" 명령어를 수행하면 다음과 같은 정보를 확인한다.

  ![image-20210805120324898](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805120324898.png) 

# 2. Vagant 설치

Vagant 는 Hypervisor 역할을 수행하는 Virtual Box의 관리도구 이다.

- 다운로드 URL : https://www.vagrantup.com/downloads.html

  ![image-20210805151810045](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805151810045.png)

- 64-bit 를 선택하여 설치파일을 다운로드 하고 "vagrant_2.2.18_x86_64.msi" 을 실행하여 설치

# 3. Chocolatey 설치 및 필요한 패키지 설치

## 3.1 Chocolatey 설치

Chocolatey 는 Window의 패키지 매니저 이다. Linux 의 yum 이나 Mac의 brew 와 동일한 역할을 하는 패키지 관리 소프트웨어이다.

- 다운로드 URL : https://chocolatey.org

  ![image-20210805152537782](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805152537782.png)

  위 가이드에 따라 실행한다.

  1. Windows Powershell 을 오른쪽 마우스를 클릭하여 "관리자로 실행" 한다.

     ![image-20210805152816418](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805152816418.png)

  2. 다음 명령어를 실행한다.

     `Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

     ![image-20210805153122969](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805153122969.png)

## 3.2 curl 설치

- 명령어

  `cinst -y curl`

  ![image-20210805181028528](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805181028528.png)

- 설치한 패키지 확인

  `curl list -lo`

  ![image-20210805181146246](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805181146246.png)

## 3.3 kubectl 설치

- 명령어

  `cinst -y kubernetes-cli`

  ![image-20210805181331265](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805181331265.png)

- 설치한 패키지 확인

  `choco list -lo`

  ![image-20210805181422714](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805181422714.png)

# 4. VirtualBox 버전 업

## 4.1 설치된 VirtualBox 삭제

Docker Toolbox for WIndows 설치 과정에서 같이 설치된 VirtualBox 는 5.2 버전으로 더이상 지원하지 않는 버전이다. 

따라서 윈도우 프로그램 추가/삭제 기능을 통해 삭제한다.

## 4.2 최신버전의 VirtualBox 설치

VirtualBox 홈페이지에 방문하여 가장 최신의 VirtualBox(6.1.26, 2021년 08월 기준)를 설치한다.

- 다운로드 URL : https://www.virtualbox.org/wiki/Downloads

# 5. minikube 설치

minikube 1.0 버전부터 Chocolatey 를 이용하여 설치가 가능하다.

- 명령어

  `cinst -y minikube`

  ![image-20210805181937740](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805181937740.png)





# 9. 참고자료

# 9.1 vagrant 공유폴더 문제

리눅스의 /vagrant 로 마운트되는 VirtualBox 의 공유 폴더가 마운트 되지 않는 문제가 발생하는 경우에 대한 해결 방법이다.

![image-20210805222402910](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/image-20210805222402910.png)

- 문제 원인 : VirtualBox Guest Additions 

- 해결 방법 : vagrant-vbguest 플러그인 설치

- vagrant-vbguest 플러그인 : Guest Machine 과 VirtualBox Host의 Guest Additions 버전이 다를 경우에 알맞은 버전을 설치해 주는 플러그인

- 명령어

  `vagrant plugin install vagrant-vbguest`

## 9.2 kubernetes 필수 명령어

### 9.2.1 kubernetes Cluster 기동

`vagrant up`

### 9.2.2 kubernetes Cluster 정지

`vagrant halt`

### 9.2.3 kubernetes Cluster 삭제

`vagrant destory -f`



"끝"

![signature](https://raw.githubusercontent.com/u4rang/save-image-repo/main/img/signature.png)

