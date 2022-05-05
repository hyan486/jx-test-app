# Tekton
[Tekton Hub](https://hub.tekton.dev)

### Tekton 설치
* tekton 설치
```shell
kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
kubectl get pods --namespace tekton-pipelines
```

* tekton CLI 설치
```shell
# Get the tar.xz
curl -LO https://github.com/tektoncd/cli/releases/download/v0.14.0/tkn_0.14.0_Linux_x86_64.tar.gz
# Extract tkn to your PATH (e.g. /usr/local/bin)
sudo tar xvzf tkn_0.14.0_Linux_x86_64.tar.gz -C /usr/local/bin/ tkn
tkn version
```

* tekton-dashboard 설치
```shell
kubectl apply --filename https://github.com/tektoncd/dashboard/releases/latest/download/tekton-dashboard-release.yaml
kubectl get pods --namespace tekton-pipelines
```

* Longhorn 설치 (K8S StorageClass)
```shell
yum install -y iscsi-initiator-utils
kubectl apply -f https://kubetm.github.io/yamls/longhorn/longhorn.yaml
kubectl get pods -n longhorn-system

kubectl apply -f - <<END
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast
provisioner: driver.longhorn.io
parameters:
  dataLocality: disabled
  fromBackup: ""
  fsType: ext4
  numberOfReplicas: "3"
  staleReplicaTimeout: "30"
END
kubectl get storageclasses.storage.k8s.io
```




### 명령어
* `kubectl get task` or `tkn task list`: task 조회
* `kubectl get taskrun` or `tkn taskrun list`: 실행된 task 조회
* `kubectl create [taskrun yaml file]` or `tkn task start hello` : task 실행
* `kubectl get pipeline` or `tkn pipeline list`: pipeline 조회
* `kubectl get pipelinerun` or `tkn pipelinerun list`: 실행된 pipeline 조회
* `kubectl create [pipelinerun yaml file]` or `tkn pipeline start hello` : pipeline 실행

* `tkn tr logs [taskrun name]` : task 로그 확인 (가장 최근에 실행한 로그부터 계속 출력: --last -f)
* `tkn pr logs [pipelinerun name]` : pipelinerun 로그 확인 (가장 최근에 실행한 로그부터 계속 출력: --last -f)


### DemoApplication
Application의 Port 정보를 출력함 