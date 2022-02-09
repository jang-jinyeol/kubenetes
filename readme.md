### Minikube설치 한번에

<img src="https://img.shields.io/badge/kubernetes-brightgreen?logo=Kubernetes&logoColor=white"/>




## 주의 사항
minikube는 t2.micro(프리티어)에서는 실행되지 않습니다.

## git clone하기

```
sudo su -
mkdir git
cd git
git clone https://github.com/jang-jinyeol/kubenetes.git
```

## 사용 방법

sh docker_install.sh

sh minikube_install.sh

sh kubectl_install.sh

위 명령어 들을 순서대로 실행 합니다.

한번에 실행 하려면 아래와 같이 실행 합니다.

sh docker_install.sh; sh minikube_install.sh; sh kubectl_install.sh; sh utils.sh

## Docker Compose설치
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version

## minikube 실행

minikube start --vm-driver=none

에러 발생할 경우 sudo apt-get install -y conntrack

sudo minikube status (뜨는지 확인)

Hello server 띄우기

kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0

공개하기

kubectl expose deployment web --type=NodePort --port=8080

Nginx띄워보기

kubectl run webserver --image=nginx:1.14 --port 80

kubectl get po (확인)

kubectl get po

expose하기

kubectl expose pod webserver --type=NodePort --port=80

service (확인)

kubectl get svc


## WSL2 설치
2.1 WSL2 활성화를 위한 DISM 명령어 실행
2.1.1 Windows Subsystem for Linux 사용 가능하게 설정
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

2.1.2 Virtual Machine feature 활성화
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

STORE에서 우분투 설치
Windows Powershell에서 wsl -l 명령어를 통해 설치 확인

MEMORY ISSUE
Workaround: Create a %UserProfile%\.wslconfig file in Windows and use it to limit memory assigned to WSL2 VM.
[wsl2]
memory=6GB
swap=0
localhostForwarding=true
