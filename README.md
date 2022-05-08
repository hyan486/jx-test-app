# Tekton
[Tekton Hub](https://hub.tekton.dev)

## Tekton 설치
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

## 명령어
* `kubectl get task` or `tkn task list`: task 조회
* `kubectl get taskrun` or `tkn taskrun list`: 실행된 task 조회
* `kubectl create [taskrun yaml file]` or `tkn task start hello` : task 실행
* `kubectl get pipeline` or `tkn pipeline list`: pipeline 조회
* `kubectl get pipelinerun` or `tkn pipelinerun list`: 실행된 pipeline 조회
* `kubectl create [pipelinerun yaml file]` or `tkn pipeline start hello` : pipeline 실행

* `tkn tr logs [taskrun name]` : task 로그 확인 (가장 최근에 실행한 로그부터 계속 출력: --last -f)
* `tkn pr logs [pipelinerun name]` : pipelinerun 로그 확인 (가장 최근에 실행한 로그부터 계속 출력: --last -f)



## Docker registry에 push 하기 위한 ServiceAccount 생성
[Tekton Documentation](https://tekton.dev/docs/pipelines/auth/)
#### 1. Docker Secret 생성 
아래는 두 종류의 secret을 생성하는 예제이다. ( [secret type에 대한 설명](https://kubernetes.io/ko/docs/concepts/configuration/secret/#secret-types) )
* kubernetes.io/dockerconfigjson: 직렬화 된 ~/.docker/config.json 파일
    ```shell  
    kubectl create secret docker-registry docker-secret --docker-username={USERNAME} --docker-password={PASSWD} --docker-email={EMAIL}
    ```
* kubernetes.io/basic-auth: 기본 인증을 위한 자격 증명(credential)
    ```yaml   
    apiVersion: v1
    kind: Secret
      metadata:
        name: docker-secret
    annotations:
      tekton.dev/docker-0: https://index.docker.io # 자격 증명이 속한 웹 주소를 지정
      #tekton.dev/docker-0: https://gcr.io
      #tekton.dev/git-0: https://github.com # git hub에 대한 secret일 경우
    type: kubernetes.io/basic-auth
    stringData:
      username: <username>
      password: <password>
    ```  
  
#### 2. ServiceAccount 생성
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tekton-sa
secrets:
  - name: docker-secret
```

#### 3. 생성한 ServiceAccount는 pipelinerun에 연결
```yaml
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  generateName: pipelinerun-demo-
spec:
  serviceAccountName: tekton-sa
  params:
    - name: msa_repo_url
      value: https://github.com/chanmi910/cicd-tekton
  pipelineRef:
    name: pipeline-demo
  workspaces:
    - name: pipeline-shared-data
      persistentvolumeclaim:
        claimName: task-pvc
```       

## DemoApplication
Application의 Port 정보를 출력함 