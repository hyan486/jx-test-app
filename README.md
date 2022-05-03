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
tektoncli - releases
rpm -Uvh https://github.com/tektoncd/cli/releases/download/v0.18.0/tektoncd-cli-0.18.0_Linux-64bit.rpm
tkn version
```

* tekton-dashboard 설치
```shell
kubectl apply --filename https://github.com/tektoncd/dashboard/releases/latest/download/tekton-dashboard-release.yaml
kubectl get pods --namespace tekton-pipelines
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