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

~~sh kubectl_install.sh~~

위 명령어 들을 순서대로 실행 합니다.

한번에 실행 하려면 아래와 같이 실행 합니다.

~~sh docker_install.sh; sh minikube_install.sh; sh kubectl_install.sh; sh utils.sh~~

★수정

*vmware가 아닌 우분투에선 위의 스크립트로 kubectl이 설치가 안됨*

*다음 명령으로 최신 릴리스를 다운로드한다.*

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

kubectl 설치

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

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





# kustomize 3.2 버전 설치

wget https://github.com/kubernetes-sigs/kustomize/releases/download/v3.2.0/kustomize_3.2.0_linux_amd64

chmod u+x kustomize_3.2.0_linux_amd64

mv kustomize_3.2.0_linux_amd64 kustomize

mv kustomize /usr/local/bin/

kustomize version



# Kubeflow 1.3 설치

git clone https://github.com/kubeflow/manifests.git

cd manifests

while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done


# 가상환경이 아닌 우분투에선 따로 설치 필요 X

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


# 파드 조회

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

