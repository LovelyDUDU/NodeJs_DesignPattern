# 182. macOS 설정

### kubectl
kubectl 설치: https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos

```bash
brew install kubectl

kubectl version --client

WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.4", GitCommit:"872a965c6c6526caa949f0c6ac028ef7aff3fb78", GitTreeState:"clean", BuildDate:"2022-11-09T13:36:36Z", GoVersion:"go1.19.3", Compiler:"gc", Platform:"darwin/arm64"}
Kustomize Version: v4.5.7
```

### MiniKube
MiniKube: https://minikube.sigs.k8s.io/docs/start/

MiniKube를 설치하여 로컬 개발 클러스터를 만들기전에 Hypervisor가 필요하다. Hypervisor는 가상 머신을 생성하는 소프트웨어이다. Hypervisor 에는 HyperKit, VirtualBox, VMware Fusion, Docker 등 다양하게 있다.


#### VirtualBox
VirtualBox: https://minikube.sigs.k8s.io/docs/drivers/virtualbox/

```bash
# minikube 설치
brew install minikube

# 드라이버 설정
minikube start --driver=virtualbo
 
minikube start --driver=docker

# minikube 상태 확인
minikube status

# minikube
# type: Control Plane
# host: Running
# kubelet: Running
# apiserver: Running
# kubeconfig: Configured


# minikube dashboard -> 클러스터의 웹 대시보드를 불러옴
minikube dashboard
```