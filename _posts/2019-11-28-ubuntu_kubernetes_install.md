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
toc: true
toc_sticky: true
toc_label: 목차
---

# 우분투 환경에서 쿠버네티스 설치하기(dashboard 까지)
## 개요
쿠버네티스를 설치하는 방법은 OS나 사용하는 플랫폼에 따라서 달라진다. <br>
본 포스팅에서는 우분투 환경에서 쿠버네티스를 설치하고 추가적으로 쿠버네티스 대시보드까지 설치해 보도록 하겠다. <br>

> 실행 환경: Ubuntu 18.04(LTS)<br>
> 도커 버전: 19.03

쿠버네티스의 [한글 문서](https://kubernetes.io/ko/docs/setup/) 에 보면 공식적으로 지원하는 커뮤니티와 생태계를 확인할 수 있다. <br>
<span style="color:#F9E79F">윈도우</span>의 경우에는 <span style="color:#F9E79F">**Docker Desktop**</span> 로 선택지가 정해져있지만 Linux의 경우는 MicroK8s, k3s, Ubuntu on LXD등 다양한 생태계를 공식적으로 지원하기 때문에 상황에 맞는 생태계를 선택하면 될 것 같다.<br>
본인은 이중에서 [ubuntu.com](https://ubuntu.com/kubernetes/install#multi-node) 에 나온 <span style="color:#F9E79F">**MicroK8s**</span>를 사용하여 설치 해보았다 <br>

## microk8s 설치 절차
<span style="color:#F1948A">**1. Install the microk8s snap**</span>
```
$ sudo snap install microk8s --classic
```

<span style="color:#F1948A">**2.  Check the status**</span>
```
$ sudo microk8s.status --wait-ready
```

<span style="color:#F1948A">**3. Turn on standard services**</span>
```
$ sudo microk8s.enable dns dashboard registry
```

<span style="color:#F9E79F">MicroK8s 명령을 통해 kubectl 명령을 다음과 같이 수행</span>할 수 있다. 
```
$ microk8s.kubectl
```

## kubernetes dashboard 설치 절차
이제 kubernetes dashboad를 설치하러 가보자
대시보드 UI는 기본으로 배포되지 않기 때문에 <span style="color:#F9E79F">[쿠버네티스 대시보드 문서](https://kubernetes.io/ko/docs/tasks/access-application-cluster/web-ui-dashboard/)</span> 를 참조하여 설치해 보도록 하겠다

<span style="color:#F1948A">**1. Install kubenetes dashboard**</span>
```
$ microk8s.kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml
```

설치가 종료되면 접속을 위한 토큰을 아래 명령을 통해 미리 생성해 둔다<br>

<span style="color:#F1948A">**2. Bearer Token**</span>
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

토큰이 생성되었다면 프록시 명령을 통해 대시보드를 사용할 수 있다<br>

<span style="color:#F1948A">**3. kubectl proxy**</span>
```
$ microk8s.kubectl proxy
```

그리고 [http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/]( http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/) 에 접속하여 token을 입력하면

![웰컴 뷰](https://d33wubrfki0l68.cloudfront.net/5f56e7ac82f10f46e70403a246c2b93efcf8b5b3/1c09f/images/docs/ui-dashboard-zerostate.png)

짜잔~! 대시보드를 확인할 수 있다