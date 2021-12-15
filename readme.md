### Minikube설치 한번에

<img src="https://img.shields.io/badge/kubernetes-brightgreen?logo=springboot&logoColor=white"/>


## 주의 사항
minikube는 cpu가 2core이상에서 실행 됩니다. t2.micro에서는 실행되지 않습니다.

## git clone하기

```
sudo su -
mkdir git
cd git
git clone https://github.com/Kyeongrok/docker_minikube_kubectl_install
```

## 사용 방법

sh docker_install.sh

sh minikube_install.sh

sh kubectl_install.sh

위 명령어 들을 순서대로 실행 합니다.

한번에 실행 하려면 아래와 같이 실행 합니다.

sh docker_install.sh; sh minikube_install.sh; sh kubectl_install.sh; sh utils.sh

## minikube 실행

minikube start --vm-driver=none
