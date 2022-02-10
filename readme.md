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

wsl.exe --set-default-version 2

MEMORY ISSUE

Workaround: Create a %UserProfile%\.wslconfig file in Windows and use it to limit memory assigned to WSL2 VM.

[wsl2]

memory=6GB

swap=0

localhostForwarding=true


## Minikube: enabling SystemD (WLS에서 실행 시)

# Install the needed packages
sudo apt install -yqq daemonize dbus-user-session fontconfig

# Create the start-systemd-namespace script
sudo vi /usr/sbin/start-systemd-namespace
#!/bin/bash

SYSTEMD_PID=$(ps -ef | grep '/lib/systemd/systemd --system-unit=basic.target$' | grep -v unshare | awk '{print $2}')
if [ -z "$SYSTEMD_PID" ] || [ "$SYSTEMD_PID" != "1" ]; then
    export PRE_NAMESPACE_PATH="$PATH"
    (set -o posix; set) | \
        grep -v "^BASH" | \
        grep -v "^DIRSTACK=" | \
        grep -v "^EUID=" | \
        grep -v "^GROUPS=" | \
        grep -v "^HOME=" | \
        grep -v "^HOSTNAME=" | \
        grep -v "^HOSTTYPE=" | \
        grep -v "^IFS='.*"$'\n'"'" | \
        grep -v "^LANG=" | \
        grep -v "^LOGNAME=" | \
        grep -v "^MACHTYPE=" | \
        grep -v "^NAME=" | \
        grep -v "^OPTERR=" | \
        grep -v "^OPTIND=" | \
        grep -v "^OSTYPE=" | \
        grep -v "^PIPESTATUS=" | \
        grep -v "^POSIXLY_CORRECT=" | \
        grep -v "^PPID=" | \
        grep -v "^PS1=" | \
        grep -v "^PS4=" | \
        grep -v "^SHELL=" | \
        grep -v "^SHELLOPTS=" | \
        grep -v "^SHLVL=" | \
        grep -v "^SYSTEMD_PID=" | \
        grep -v "^UID=" | \
        grep -v "^USER=" | \
        grep -v "^_=" | \
        cat - > "$HOME/.systemd-env"
    echo "PATH='$PATH'" >> "$HOME/.systemd-env"
    exec sudo /usr/sbin/enter-systemd-namespace "$BASH_EXECUTION_STRING"
fi
if [ -n "$PRE_NAMESPACE_PATH" ]; then
    export PATH="$PRE_NAMESPACE_PATH"
fi


# Create the enter-systemd-namespace
sudo vi /usr/sbin/enter-systemd-namespace
#!/bin/bash

if [ "$UID" != 0 ]; then
    echo "You need to run $0 through sudo"
    exit 1
fi

SYSTEMD_PID="$(ps -ef | grep '/lib/systemd/systemd --system-unit=basic.target$' | grep -v unshare | awk '{print $2}')"
if [ -z "$SYSTEMD_PID" ]; then
    /usr/sbin/daemonize /usr/bin/unshare --fork --pid --mount-proc /lib/systemd/systemd --system-unit=basic.target
    while [ -z "$SYSTEMD_PID" ]; do
        SYSTEMD_PID="$(ps -ef | grep '/lib/systemd/systemd --system-unit=basic.target$' | grep -v unshare | awk '{print $2}')"
    done
fi

if [ -n "$SYSTEMD_PID" ] && [ "$SYSTEMD_PID" != "1" ]; then
    if [ -n "$1" ] && [ "$1" != "bash --login" ] && [ "$1" != "/bin/bash --login" ]; then
        exec /usr/bin/nsenter -t "$SYSTEMD_PID" -a \
            /usr/bin/sudo -H -u "$SUDO_USER" \
            /bin/bash -c 'set -a; source "$HOME/.systemd-env"; set +a; exec bash -c '"$(printf "%q" "$@")"
    else
        exec /usr/bin/nsenter -t "$SYSTEMD_PID" -a \
            /bin/login -p -f "$SUDO_USER" \
            $(/bin/cat "$HOME/.systemd-env" | grep -v "^PATH=")
    fi
    echo "Existential crisis"
fi


# Edit the permissions of the enter-systemd-namespace script
sudo chmod +x /usr/sbin/enter-systemd-namespace
# Edit the bash.bashrc file
sudo sed -i 2a"# Start or enter a PID namespace in WSL2\nsource /usr/sbin/start-systemd-namespace\n" /etc/bash.bashrc



# kustomize 3.2 버전 설치

wget https://github.com/kubernetes-sigs/kustomize/releases/download/v3.2.0/kustomize_3.2.0_linux_amd64

chmod u+x kustomize_3.2.0_linux_amd64

mv kustomize_3.2.0_linux_amd64 kustomize

mv kustomize /usr/local/bin/

kustomize version



# Kubeflow 1.3 설치

git clone https://github.com/kubeflow/manifests.git

cd manifests


# 임시

kustomize build common/kubeflow-namespace/base | kubectl apply -f -

kustomize build common/user-namespace/base | kubectl apply -f -

kustomize build common/cert-manager/cert-manager/base | kubectl apply -f -

kustomize build common/cert-manager/kubeflow-issuer/base | kubectl apply -f -

kustomize build common/istio-1-11/istio-crds/base | kubectl apply -f -

kustomize build common/istio-1-11/istio-namespace/base | kubectl apply -f -

kustomize build common/istio-1-11/istio-install/base | kubectl apply -f -

kustomize build common/dex/overlays/istio | kubectl apply -f -

kustomize build common/oidc-authservice/base | kubectl apply -f -

kustomize build common/knative/knative-serving/base | kubectl apply -f -

kustomize build common/istio-1-11/cluster-local-gateway/base | kubectl apply -f -

kustomize build common/kubeflow-namespace/base | kubectl apply -f -

kustomize build common/kubeflow-roles/base | kubectl apply -f -

kustomize build common/istio-1-11/kubeflow-istio-resources/base | kubectl apply -f -

kustomize build apps/pipeline/upstream/env/platform-agnostic-multi-user | kubectl apply -f -

kustomize build apps/kfserving/upstream/overlays/kubeflow | kubectl apply -f -

kustomize build apps/katib/upstream/installs/katib-with-kubeflow | kubectl apply -f -

kustomize build apps/admission-webhook/upstream/overlays/cert-manager | kubectl apply -f -

kustomize build apps/jupyter/notebook-controller/upstream/overlays/kubeflow | kubectl apply -f -

kustomize build apps/jupyter/jupyter-web-app/upstream/overlays/istio | kubectl apply -f -

kustomize build apps/profiles/upstream/overlays/kubeflow | kubectl apply -f -

kustomize build apps/volumes-web-app/upstream/overlays/istio | kubectl apply -f -

kustomize build apps/tensorboard/tensorboards-web-app/upstream/overlays/istio | kubectl apply -f -

kustomize build apps/tensorboard/tensorboard-controller/upstream/overlays/kubeflow | kubectl apply -f -

kustomize build apps/training-operator/upstream/overlays/kubeflow | kubectl apply -f -

kustomize build apps/mpi-job/upstream/overlays/kubeflow | kubectl apply -f -



# 확인용


kubectl get pods -n cert-manager

kubectl get pods -n istio-system

kubectl get pods -n auth

kubectl get pods -n knative-eventing

kubectl get pods -n knative-serving

kubectl get pods -n kubeflow

kubectl get pods -n kubeflow-user-example-com



#  ↓ ↓ kubenetes command ↓ ↓


# 네임스페이스 리스트 조회

kubectl get ns

# 네임스페이스 생성

yaml파일로 생성할 경우 kubectl create namespace workns를 따로 해줘야 kubectl get ns에서 조회된다.

kubectl create -f testns.yaml

create는 생성하는 것이라 수정한 것을 다시 적용하려면 apply를 사용해야 한다.

# 현재 활성화 네임스페이스 변경 (예시)


kubectl config set-context --current --namespace=<insert-namespace-name-here>

-- workns 네임스페이스로 변경

~$ kubectl config set-context --current --namespace=workns

Context "kubernetes-admin@kubernetes" modified.


# 현재 활성화 네임스페이스 조회

kubectl config view | grep namespace

# 네임스페이스 삭제

kubectl delete namespace testns

파드 조회

kubectl get pods -o wide

네임스페이스를 workns로 변경하였으므로 이전에 만든 nginx 파드가 조회되지 않는다.

-n 으로 네임스페이스를 default로 조회하면 nginx 파드가 조회된다.

kubectl get pods -o wide -n default

리소스 중에는 네임스페이스 영향을 받는 받는 리소스와 영향을 받지 않는 리소스로 나눌 수 있다.

kubectl api-resources로 조회 시 NAMESPACED가 true이면 연관된 리소스여서 네임스페이스를 구분하여 실행 할 수 있다.

false로 되어있으면 네임스페이스 영향을 받지 않기 때문에  어느 네임스페이스에서 작업을 하든 같다.

아래 3개의 네임스페이스는 사용하지 않는 것이 좋다.

kube-node-lease
kube-public
kube-system

