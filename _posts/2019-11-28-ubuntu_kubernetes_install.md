---
title: 우분투 쿠버네티스 설치
layout: single
read_time: true
comments: true
related: true
categories:
- Ubuntu
tags:
- Ubuntu
- Docker
- Kubernetes
- Dashboard
---

# 우분투 환경에서 쿠버네티스 설치하기(dashboard 까지)

쿠버네티스를 설치하는 방법은 OS나 사용하는 플랫폼에 따라서 달라진다. <br>
본 포스팅에서는 우분투 환경에서 쿠버네티스를 설치하고 추가적으로 쿠버네티스 대시보드까지 설치해 보도록 하겠다. <br>
``` 실행 환경: Ubuntu 18.04(LTS)``` <br>
``` 도커 버전: 19.03``` <br>

쿠버네티스의 [한글 문서](https://kubernetes.io/ko/docs/setup/) 에 보면 공식적으로 지원하는 커뮤니티와 생태계를 확인할 수 있다. <br>
윈도우의 경우에는 **Docker Desktop** 을 설치하면 되겠구나 하는 생각이 들지만 나머지는 어떤걸 설치해야할지 막막해 진다. <br>
모든 생태계 마다 설치하는 방법이 링크를 통해 설명되어 있으니 자신이 원하는 생태계를 선택하면 될 것 같다. <br>
본인은 이중에서 [ubuntu.com](https://ubuntu.com/kubernetes/install#multi-node) 에 나온 MicroK8s를 사용하여 설치 해보았다 <br>

1. Install the microk8s snap
```
$ sudo snap install microk8s --classic
```

2.  Check the status
```
$ sudo microk8s.status --wait-ready
```

3. Turn on standard services
```
$ sudo microk8s.enable dns dashboard registry
```

MicroK8s를 통해 kubectl 명령을 다음과 같이 수행할 수 있다. 
```
$ microk8s.kubectl
```
이제 kubernetes dashboad를 설치하러 가보자
대시보드 UI는 기본으로 배포되지 않기 때문에 [쿠버네티스 대시보드 문서](https://kubernetes.io/ko/docs/tasks/access-application-cluster/web-ui-dashboard/) 를 참조하여 설치해 보도록 하겠다

4. Install kubenetes dashboard
```
$ microk8s.kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml
```

설치가 종료되면 접속을 위한 토큰을 아래 명령을 통해 미리 생성해 둔다
5. Bearer Token
```
$ microk8s.kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```
이 명령을 입력하면 아래와 같이 토큰이 나온다 <br>

```
Name:         admin-user-token-v57nw
Namespace:    kubernetes-dashboard
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: admin-user
              kubernetes.io/service-account.uid: 0303243c-4040-4a58-8a47-849ee9ba79c1

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1066 bytes
namespace:  20 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLXY1N253Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIwMzAzMjQzYy00MDQwLTRhNTgtOGE0Ny04NDllZTliYTc5YzEiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZXJuZXRlcy1kYXNoYm9hcmQ6YWRtaW4tdXNlciJ9.Z2JrQlitASVwWbc-s6deLRFVk5DWD3P_vjUFXsqVSY10pbjFLG4njoZwh8p3tLxnX_VBsr7_6bwxhWSYChp9hwxznemD5x5HLtjb16kI9Z7yFWLtohzkTwuFbqmQaMoget_nYcQBUC5fDmBHRfFvNKePh_vSSb2h_aYXa8GV5AcfPQpY7r461itme1EXHQJqv-SN-zUnguDguCTjD80pFZ_CmnSE1z9QdMHPB8hoB4V68gtswR1VLa6mSYdgPwCHauuOobojALSaMc3RH7MmFUumAgguhqAkX3Omqd3rJbYOMRuMjhANqd08piDC3aIabINX6gP5-Tuuw2svnV6NYQ
```

토큰이 생성되었다면 프록시 명령을 통해 대시보드를 사용할 수 있다
6. kubectl proxy
```
$ microk8s.kubectl proxy
```

그리고 [http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/]( http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/) 에 접속하여 token을 입력하면

![웰컴 뷰](https://d33wubrfki0l68.cloudfront.net/5f56e7ac82f10f46e70403a246c2b93efcf8b5b3/1c09f/images/docs/ui-dashboard-zerostate.png)

짜잔~! 대시보드를 확인할 수 있다