---
layout: post
title:  "컨테이너 오케스트레이션 쿠버네티스 살펴보기"
date:   2021-05-26 21:03:36 +0530
categories:
---
---

T아카데미 - 컨테이너 오케스트레이션 쿠버네티스 살펴보기

---

## [1강] 컨테이너 오케스트레이션

#### - **도커가 등장하기 전 서버 운영**
  <img data-action="zoom" src='{{ "/image/69.PNG" | relative_url }}' alt='absolute'>

  1. 자체 서버 운영
      - 서버주문 , 서버 설치, cpu 메모리 하드 조립 네트워크 연결, OS설치, 계정설정, 방화벽 설정
      - 서버를 설정하기 위해 많은 노력과 시간이 필요함
      - 성능이 좋은 걸 미리 구매하고 효율적인 사용을 위해 여러 애플리케이션을 설치

        <img data-action="zoom" src='{{ "/image/70.PNG" | relative_url }}' alt='absolute'>

      - 단계를 지날 때 마다 명령어를 이용함 ex)  cp, mv, rm
      - ruby 버전을 변경 하게 되면 이후 모든 상태에 영향이 감 (위험)
      - 특정 시점의 서버 상태를 누가 언제 어떻게 작업했는지 알기 어려움
      - 서버관리자 = 문제 생기면 다 해결 = 변화를 싫어함 = 확장이 어려움 = 혁신이 어려움

  2. 재현 가능한 상태 관리
      - Configuration Management
          * 명령어를 통한 서버 관리를 지양
          * 상태를 관리하는 코드를 이용하여 서버 관리 (설정 파일)
          * 선언적 서버 상태 정의
          * 서버의 상태가 재현 가능해짐
          * 서버 운영의 협업이 가능해짐 (소스관리, 코드리뷰)
      - Ansible

  3. 가상 머신의 등장
      - 하나의 서버에 여러개의 가상 서버를 설치할 수 있음
      - 다양한 버전의 java 여러개의 데이터베이스를 쉽게 사용
      - 서버의 상태를 이미지로 저장할 수 있음
      - 새로운 서버를 만들고 기존 서버의 내용을 복사할 수 있음

      - immutable 변하지 않는
          * 서버에 설치된 애플리케이션을 새로운 버전으로 업데이트 하면 mutable
          * 새로운 버전이 설치된 서버의 상태를 이미지로 만들고 교체하면 immutable
          * 기존 상태를 고려할 필요 없이 통째로 서버를 교체
          * 생각보다 어렵고 느리고 특정 회사 제품을 써야 함

  4. 클라우드
      - AWS, Google Cloud, Azura
      - 하드웨어 파편화 문제 해결
      - 가상화된 환경만으로 아키텍처 구성이 가능해짐
      - 이미지를 기반으로한 다수의 서버 상태관리
          * 상태관리에 대한 새로운 접근
          * 서버 운영의 문제는 여전히 남아있음
      - 마치 전기를 사용하는듯 편리함
      - 단점
          * (보통) 어떻게 만들었는지 모름
          * (보통) 처음부터 다시 만들 수 없음
          * 관리가 어려움 / 결과만 스냅샷을 찍기에..

  5. PaaS (Platform as a service)
      - Heroku, Netlify, AWS Elastic Beanstail, Google Cloud App Engine
      - 서버를 운영하는 것은 복잡하고 어렵다
      - 소스 코드만으로 배포가 가능하다.
      - 일반화된 프로비저닝 방법을 제공
      - 프로비저닝 과정에 개입 불가
      - 단점
          * 애플리케이션을 PaaS 방식에 맞게 작성해야함
          * 자바 버전도 PaaS가 제공하는 버전을 따라야 함
          * 서버에 대한 원격 접속 시스템을 제공하지 않음 (로그 못봄)
          * 서버에 파일 시스템을 사용할 수 없음
          * site 패키지를 설치할 수 없음
          * 로그수집을 제한적인 방식으로만 하용
          * 배포에 대한 새로운 패러다임을 이해해야 함
      - PaaS에서 할 수 있을까?
          * 크론잡
          * 데이터 분석
          * 로그 분석
          * 애플리케이션 성능 모니터링
          * A/B 테스트, Canary 배포
          * 네트워크 , 스토리지 설정

#### - **도커의 등장**

  1. 도커란?
      - 2013년 DocCloud 에서 첫 공개
      - 컨테이너 : 격리된 환경에서 작동하는 프로세스
      - 리눅스 커널의 여러 기술을 활용
      - 하드웨어 가상화 기술보다 가벼움
      - 이미지 단위로 프로세스 실행 환경을 구성
      - Vm은 os위에 os를 올려서 사용함 / 도커는 도커 엔진 위에 격리된 프로세스를 띄우므로 속도 빠름
      - 도커의  특징
          * 확장성 : 특정 회사나 서비스에 종속되지 않고 실행 가능, 개발서버 생성 간편
          * 표준성 : ruby nodejs go php 의 서비스 배포 방식이 제각각인데 docker run 방식 하나로 사용 가능
          * 이미지 : dockerfile 을 이용하여 이미지를 쉽게 빌드하며 hub로 쉽게 pull push 가능
          * 설정 : 환경변수로 쉽게 지정 가능
          * 자원 : 삭제 후 새로 만들 시 모든 데이터가 초기화됨
      - 도커가 가져온 변화
          * PaaS 와 같은 제한 없음
          * 클라우드 이미지보다 관리하기 쉬움
          * 다른 프로세스와 격리되어 가상머신처럼 사용하지만 성능저하가 없음
          * 복잡한 기술 namespace, cgroup 등을 몰라도 사용할 수 있음
          * 이미지 빌드 기록이 남음
          * 코드와 설정으로 관리 > 재현 및 수정 가능
          * 오픈소스 > 특정 회사 기술에 종속적이지 않음
      - 애플리케이션 업데이트를 위해 컨테이너를 교체하는 방식 사용 (앞단 Proxy)
      - 서비스 디스커버리
          * 서버들의 정보 (ip, port 등)을 포함한 다양한 정보를 저장하고 가져오고 값의 변화가 일어날 때, 이벤트를 받아 자동으로 서비스의 설정 정보를 수정하고 재시작하는 개념
              1. 새로운 서버가 추가되면 서버 정보를 key / value store에 추가 함
              2. key / value store는 디렉토리 형태로 값을 저장함. /services/web 하위를 읽으면 전체 web 서버 정보를 읽을 수 있음
              3. key / value store을 watch 하고 있던 configuration manager 가 값이 추가되었다는 이벤트를 받음
              4. 이벤트를 받으면 템플릿 파일을 기반으로 새로운 설정파일을 생성
              5. 새로운 설정 파일을 만들어 기존 파일을 대체하고 서비스 재시작

                  <img data-action="zoom" src='{{ "/image/71.PNG" | relative_url }}' alt='absolute'>
      - docker-gen
          * docker의 기본 기능을 적극 활용한 service discovery 도구
              - 도커 데몬이 가지고 있는 컨테이너의 정보를 그대로 이용
              - 컨테이너를 실행할 때 입력한 환경변수를 읽음
              - VIRTUAL_HOST=www.subicura.com 과 같이 환경변수를 지정하면 이를보고 nginx의 virtual host 설정 파일들을 구성함

#### - **컨테이너 오케스트레이션**

  - 여러 대의 서버와 여러 개의 서비스를 편리하게 관리해주는 작업
  - 스케줄링
      * 컨테이너를 적당한 서버에 배표해주는 작업
      * 여러 대의 서버 중 가장 할일 없는 서버에 배포하거나 그냥 차례대로 배포 또는 랜덤하게 배포
      * 컨테이너 개수를 여러 개로 늘리면 적당히 나눠서 배포하고 서버가 죽으면 실행중이던 컨테이너를 다른서버에 띄워줌
  - 클러스터링
      * 여러 개의 서버를 하나의 서버처럼 사용함
      * 작게는 몇 개 안되는 서버부터 많게는 수천대의 서버를 하나의 클러스터로
      * 여기저기 흩어져 있는 컨테이너도 가상 네트워크를 이용하여 마치 같은 서버에 있는것처럼 쉽게 통신
  - 서비스 디스커버리
      * 서비스를 찾아주는 기능
      * 클러스터 환경에서 컨테이너는 어느 서버에 생성될지 알 수 없고 다른 서버로 이동할 수 있음
      * 따라서 컨테이너와 통신을 하기 위해서 어느 서버에서 실행중인지 알아야 하고 컨테이너가 생성되고 중지될 때 어딘가에 ip와 port 같은 정보를 업데이트 해주어야 함
      * 키 벨류 스토리지에 정보를 저장할 수 도 있고 내부 DNS 서버를 이용
  - 로깅, 모니터링
      * 여러 대의 서버를 관리하는 경우 로그와 서버 상태를 한곳에서 관리
      * ELK 와 prometheus 등 다양한 도구 사용
  - 컨테이너 오케스트레이션 도구
      * docker swarm
          - 호스트 os에 Agent만 설치하면 간단하게 작동하고 빠름
          - 단순한 구조에서 효과적임
      * **<span style="color:red">kubernetes</span>**
          - 구글에서 개발한 컨테이너 배포 확장 운영 도구
          - 표준
          - 대규모에 적함
          - 다양한 생태계 구축되어 있음

  - **<span style="color:red">서비스 메시</span>**
      * 서비스마다 프록시를 모두 붙이고 서로간에 retry 함
      * 기능
          - 서비스 발견
          - 부하 분산
          - 경로 관리
          - 트래픽 관리
          - 운영 탄력성
          - 오류 주입
          - 로깅 / 모니터링
          - 분산 추적
          - 보안
          - 인증 , 인가

#### - **자체서버부터 컨테이너까지**
  - 결론
      * 서버 생성을 쉽고 편리하게
      * 명령아가 아닌 코드와 설정을 사용
      * 격리된 환경을 이용하여 독립적인 실행환경 구성
      * immutable 하게 애플리케이션을 관리하고 스토리지 또는 로그를 분리하여 관리
      * 이미지를 쉽게 만들고 편리하게 사용
      * 로드 밸런서와 프록시 서버 등 자주 사용하는 기술을 쉽게 사용
      * 확장 가능한 설계

        <img data-action="zoom" src='{{ "/image/72.PNG" | relative_url }}' alt='absolute'>

## [2강] 쿠버네티스 & 쿠버네티스 아키텍쳐

#### - **쿠버네티스란?**
  - Docker - Build, Ship, Run, Deployment
  - docker 는 Deploy 까지 보장하지 않음
  - $DOCKER_HOST=production.host.ip docker run -d -p 80:80 awsome-App
  - 매번 docker 명령어를 입력하는 것을 자동화하자!
  - 복잡한 서버를 효과적으로 관리하기 위해 오케스트레이션 툴을 도입해 보자!
  - 컨테이너 관리도구의 춘추전국시대
  - 구글이 컨테이너 배포 시스템으로 사용하던 borg 를 기반으로 만든 오픈소스
  - 쿠버네티스를 기반으로 한 플랫폼들의 등장

#### - **쿠버네티스의 특징**
  1. 상태관리 : 상태를 선언하고 선언한 상태를 유지 / 노드가 죽거나 컨테이너의 응답이 없을경우 자동으로 복구
  2. 스케줄링 : 클러스터의 여러 노드 중 조건에 맞는 노드를 찾아 컨테이너를 배치
  3. 클러스터 : 가상 네트워크를 통해 하나의 서버에 있는 것처럼 통신
  4. 서비스 디스커버리 : 서로 다른 서비스를 쉽게 찾고 통신 가능
  5. 리소스 모니터링 : cAdvisor 를 통한 리소스 모니터링
  6. 스케일링 : 리소스에 따라 자동으로 서비스를 조정함
  7. RollOut / RollBack : 배포 / 롤백 및 버전 관리
  8. 빠른 업데이트
  9. 다양한 배포 방식

      <img data-action="zoom" src='{{ "/image/73.PNG" | relative_url }}' alt='absolute'>

        * DAEMON SET : 모든 노드에 꼭 하나씩 띄우고 싶을 경우, EX 모든서버 모니터링
        * REPLICA SET : 갯수 제어, EX 3대를 띄우고 싶음
        * STATEFUL SETS : 순서를 지정하고 싶을 경우
        * JOB
        * REPLICATION CONTROLLER  : 현재 사용 x

  10. Ingress 설정 : 도메인, ip, port 도메인과 path를 지정하면 자동으로 인지
  11. namespace & label
  12. RBAC : 누가 어떤권한을 가지고 있을지 제어 (누가 - user, 어떻게? - pod)
  13. 다양한 환경 지원

#### - **기본 개념**

  <img data-action="zoom" src='{{ "/image/74.PNG" | relative_url }}' alt='absolute'>

  - pod
      * 컨테이너를 한번 더 감싼 개념을 pod 이라고 함
      * 하나의 팟에 여러 컨테이너가 들어가면 서로 통신 가능

        <img data-action="zoom" src='{{ "/image/75.PNG" | relative_url }}' alt='absolute'>

  -  ReplicaSet
      * replicas 라는 옵션으로 갯수 지정

        <img data-action="zoom" src='{{ "/image/76.PNG" | relative_url }}' alt='absolute'>

  - 상태를 어떻게 관리할까 ? Object Spec - YAML
      * 나는 XX 라는 이름의 팟을 갖고싶다.
        그 안에는 N개의 XX 라는 이름을 가진 컨테이너가 있고 XX 이미지를 사용합니다.
      * desired state

  - **<span style="color:red">원하는 상태 (desired state)를 다양한 오브젝트에 라벨을 붙여 정의(yaml) 하고 API 서버에 전달한다.</span>**

  - Architecture

      <img data-action="zoom" src='{{ "/image/77.PNG" | relative_url }}' alt='absolute'>

      * Master : 마스터 서버는 다양한 모듈이 확장성을 고려하여 기능별로 쪼개져 있는 것이 특징, 관리자만 접속할 수 있도록 보안 설정을 해야 하고 마스터서버가 죽으면 클러스터를 관리할 수 없기 때문에 보통 3대를 구성하여 안정성을 높인다.
      * Node : 노드 서버는 마스터서버와 통신하면서 필요한 Pod를 생성하고 네트워크와 볼륨을 설정
      * kubectl : 명령 도구

      <img data-action="zoom" src='{{ "/image/78.PNG" | relative_url }}' alt='absolute'>

      * API Server 운영자 및 내부 노드와 통신하기 위한 인터페이스 /HTTP(S) RestAPI 로 노출되어 있고 모든 명령은 여기로 통한다.
      * Controller Mnager 다양한 컨트롤러 (복제,배포,상태,크론,,) 을 관리하고 API Server와 통신하여 작업을 수행
      * Scheduler 서비스를 리소스 상황에 맞게 적절한 노드에 배치
      * etcd 가볍고 빠른 분산형 key-value 저장소 / 설정 및 상태 저장

      <img data-action="zoom" src='{{ "/image/79.PNG" | relative_url }}' alt='absolute'>

      * 실제 서비스 (컨테이너) 가 실행되는 서버, 마스터의 Api server 와 통신하며 서비스를 생성, 상태관리
      * kubelet 서비스를 실행 중지하고 상태를 체크하여 계속해서 살아있는 상태로 관리
      * proxy 네트워크 프록시와 로드 발란서 역할 cretes a iptable rule..
      * Docker(containerd 또는 rkt 또는 Hyper) 도커 뿐만 아니라 다양한 컨테이너 엔진 지원

      <img data-action="zoom" src='{{ "/image/80.PNG" | relative_url }}' alt='absolute'>

## [3강] 실습 환경구성

* https://github.com/subicura/workshop-init/blob/master/0_aws_lightsail_console.md 참고
#### **1. aws 가입**
#### **2. lightsail 접속**
#### **3. 새 인스턴스 만들기**
#### **4. publicIP:4200 으로 접속**

  <img data-action="zoom" src='{{ "/image/84.PNG" | relative_url }}' alt='absolute'>

#### **5. jq 설치 : json을 파싱해서 사용할 수 있는 프로그램**

```c
sudo apt install -y jq
```

#### **6.  docker & docker compose 설치**
```c
curl -fsSL https://get.docker.com/ | sudo sh
sudo usermod -aG docker $USER

sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# check (re-login)
docker version
docker-compose version

# reboot
sudo reboot
```      
#### **7. k3s : 경량화된 쿠버네티스**

```c
# install
curl -sfL https://get.k3s.io | sh -
sudo chown ubuntu:ubuntu /etc/rancher/k3s/k3s.yaml

# 확인
kubectl get nodes

# cube config
cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
```  

#### **8. Local path provisioner**

```c
# install
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml

# set default
kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'

# 확인
kubectl get storageclass
```


#### **9. code server**
  ```c
  wget https://github.com/cdr/code-server/releases/download/v3.8.0/code-server-3.8.0-linux-amd64.tar.gz
  tar xvfz code-server-3.8.0-linux-amd64.tar.gz
  mkdir -p ~/.config/code-server
  curl https://gist.githubusercontent.com/subicura/d7ac0cc6e662e8382e191d81c140c35b/raw/d663f09e9730ab7fe7bb2dc17f7ef59d9da43d4f/config.yaml -o ~/.config/code-server/config.yaml
  mkdir ~/project
  sudo curl https://gist.githubusercontent.com/subicura/c803fd68262736d83ee67b201d87fb3c/raw/c6370798076c989becc198901ebd0d555d2f70d9/codeserver.service -o /lib/systemd/system/codeserver.service
  sudo systemctl start codeserver
  sudo systemctl enable codeserver
  ```    
  <img data-action="zoom" src='{{ "/image/85.PNG" | relative_url }}' alt='absolute'>

## [4강] Docker(도커) 실습
#### - **기본 명령어**

  - run : 컨테이너 실행
  - ps : 컨테이너 목록 확인
  - stop : 컨테이너 중지
  - rm : 컨테이너 제거
  - logs : 컨테이너 로그 조회
  - images : 이미지 목록 확인
  - pull : 이미지 다운로드
  - rmi : 이미지 삭제

#### - **실습**
  <img data-action="zoom" src='{{ "/image/86.PNG" | relative_url }}' alt='absolute'>

  <img data-action="zoom" src='{{ "/image/87.PNG" | relative_url }}' alt='absolute'>

  <img data-action="zoom" src='{{ "/image/88.PNG" | relative_url }}' alt='absolute'>  

- 네트워크 설정

  <img data-action="zoom" src='{{ "/image/89.PNG" | relative_url }}' alt='absolute'>  

  <img data-action="zoom" src='{{ "/image/88.PNG" | relative_url }}' alt='absolute'>  

- exam

  <img data-action="zoom" src='{{ "/image/90.PNG" | relative_url }}' alt='absolute'>  

  - 내가 쓴 답
      <img data-action="zoom" src='{{ "/image/91.PNG" | relative_url }}' alt='absolute'>

  - 정답
    ```c
    docker run --name=mongodb --network=app-network mongo:4

    docker run -d --name=backend --network=app-network -e PORT=8000 -e GUESTBOOK_DB_ADDR=mongodb:27017 subicura.guestbook-backend:latest

    docker run -d -p 3000:8000 -e PORT=8000 --network=app-network -e PORT=8000 -e GUESTBOOK_DB_ADDR=backend:8000 subicura.guestbook-frontend:latest
    ```
## [5강] Docker-Compose(도커 컴포즈) 실습
#### -**기본 명령어**

  - up : 시작
  - stop : 중지
  - down : 중지 후 삭제
  - ps : 목록
  - logs : 로그보기

    ```c
    # guide-02/docker-workshop-app/docker-compose.yml
    version: '3'
    services:
      web:
        image: subicura/docker-workshop-app:1
        ports:
          - "4567:4567"
    ```

    <img data-action="zoom" src='{{ "/image/92.PNG" | relative_url }}' alt='absolute'>

    <img data-action="zoom" src='{{ "/image/93.PNG" | relative_url }}' alt='absolute'>

    ```c
    # guide-02/wordpress/docker-compose.yml
    version: '3'
    services:
      wordpress:
        image: wordpress
        environment:
          WORDPRESS_DB_HOST: mysql
          WORDPRESS_DB_NAME: wp
          WORDPRESS_DB_USER: wp
          WORDPRESS_DB_PASSWORD: wp
        ports:
          - "8000:80"
        restart: always # mysql 이 살아있을 때 까지 재시작함
      mysql:
        image: mysql:5.7
        environment:
          MYSQL_ROOT_PASSWORD: wp
          MYSQL_DATABASE: wp
          MYSQL_USER: wp
          MYSQL_PASSWORD: wp

    # up
      docker-compose up -d
    ```

    <img data-action="zoom" src='{{ "/image/88.PNG" | relative_url }}' alt='absolute'>

    - docker-compose down
        * docker stop + rm

## [6강] Kubernetes(쿠버네티스) 실습 1 (kubectl/pod/replicaset/deployment)
#### - **Task 1. kubectl**
  - 쿠버네티스의 API 서버와 통신하기 위해 사용
  - alias k='kubectl' 을 입력하여 사용하기 쉽게 만들자
  - 기본 명령어
      * apply : desired status 적용
      * get : 현재 리소스의 목록 조회
      * describe : 상세 내용 조회
      * delete : 해당 리소스 삭제
      * log : 로그 조회
      * exec : 해당 컨테이너로 접속

  - 명령 vs 선언
      ```c
        # 명령
        kubectl scale --replicas=3 rs/whoami

        # 선언 (실제 운영에서 사용됨)
        apiVersion: apps/vlveta2
        kind: replicaset
        metadata:
          name: whoami
        spec:
          replicas:3
      ```  

  - 기본 오브젝트
    다음 오브젝트들을 생성하고 조회하고 삭제하는 작업을 합니다.
      * node : k get node
      * Pod : k get pod
      * ReplicaSet
      * deployment
      * service : k get service
      * loadbalancer
      * Ingress
      * volumes
      * configmap
      * secret
      * namespace
      * 전체 오브젝트 조회 k api-resouces

  - 다양한 사용범
      * get
        ```c
        # pod, replicaset, deployment, service 조회
        kubectl get all

        # node 조회
        kubectl get no
        kubectl get node
        kubectl get nodes

        # 결과 포멧 변경
        kubectl get nodes -o wide
        kubectl get nodes -o yaml
        kubectl get nodes -o json
        kubectl get nodes -o json |
              jq ".items[] | {name:.metadata.name} + .status.capacity" # json 특정 값 찾기
        ```
      * describe
        ```c
        # kubectl describe type/name
        # kubectl describe type name
        kubectl describe node <node name>
        kubectl describe node/<node name>
        ```  
      * 그 외 자주 쓰이는 명령어
        ```c
        kubectl exec -it <POD_NAME>
        kubectl logs -f <POD_NAME|TYPE/NAME>

        kubectl apply -f <FILENAME> ★
        kubectl delete -f <FILENAME> ★
        ```  
#### - **Task 2. Pod**
  - 명령어

    <img data-action="zoom" src='{{ "/image/94.PNG" | relative_url }}' alt='absolute'>

    <img data-action="zoom" src='{{ "/image/95.PNG" | relative_url }}' alt='absolute'>

  - YAML 파일
    ```c
    #guide-03/whoami-pod.yml
      apiVersion: v1 #필수
      kind: Pod #필수
      metadata:
        name: whoami
        labels:
          type: app
      spec:
        containers:
        - name: app
          image: subicura/whoami:1

    # 적용
      k apply -f whoami-pod.yml
      k delete -f whoami-pod.yml
    ```
    <img data-action="zoom" src='{{ "/image/96.PNG" | relative_url }}' alt='absolute'>

  - Pod Ready

    <img data-action="zoom" src='{{ "/image/97.PNG" | relative_url }}' alt='absolute'>

      * app 이 생성 된 후, 상태 값 체크 - livenessProbe

        ```c
        apiVersion: v1
        kind: Pod
        metadata:
          name: whoami-lp
          labels:
            type: app
        spec:
          containers:
          - name: app
            image: subicura/whoami:1
            livenessProbe:
              httpGet:
                path: /not/exist
                port: 8080
              initialDelaySeconds: 5
              timeoutSeconds: 2 # Default 1
              periodSeconds: 5 # Defaults 10
              failureThreshold: 1 # Defaults 3        
        ```

          <img data-action="zoom" src='{{ "/image/98.PNG" | relative_url }}' alt='absolute'>

          - 하단 Liveness probe failed 에러 확인

      * 준비가 되어 있는지 조사 - readinessProbe

        ```c
        apiVersion: v1
        kind: Pod
        metadata:
        name: whoami-rp
        labels:
          type: app
        spec:
        containers:
        - name: app
          image: subicura/whoami:1
          readinessProbe:
            httpGet:
              path: /not/exist
              port: 8080
            initialDelaySeconds: 5
            timeoutSeconds: 2 # Default 1
            periodSeconds: 5 # Defaults 10
            failureThreshold: 1 # Defaults 3     
        ```

          <img data-action="zoom" src='{{ "/image/99.PNG" | relative_url }}' alt='absolute'>

          - 하단 Readiness probe failed 에러 확인

      * health check 예제 (실제로 살아있는 포트 헬스체크)

        ```c
        apiVersion: v1
        kind: Pod
        metadata:
        name: whoami-health
        labels:
          type: app
        spec:
        containers:
        - name: app
          image: subicura/whoami:1
          livenessProbe:
            httpGet:
              path: /
              port: 4567
          readinessProbe:
            httpGet:
              path: /
              port: 4567  
        ```

          <img data-action="zoom" src='{{ "/image/100.PNG" | relative_url }}' alt='absolute'>

          - 하단 정상 동작 확인

      * multi container 예제

        ```c
        apiVersion: v1
        kind: Pod
        metadata:
          name: whoami-redis
          labels:
            type: stack
        spec:
          containers:
          - name: app
            image: subicura/whoami-redis:1
            env:
            - name: REDIS_HOST
              value: "localhost"
          - name: db
            image: redis
        ```

          <img data-action="zoom" src='{{ "/image/101.PNG" | relative_url }}' alt='absolute'>

          <img data-action="zoom" src='{{ "/image/102.PNG" | relative_url }}' alt='absolute'>

          <img data-action="zoom" src='{{ "/image/103.PNG" | relative_url }}' alt='absolute'>

          - Pod 안에 있는 컨테이너는 같은 IP 다른 PORT 를 사용함
          - 컨테이너 개수 pod/whoami-redis 0/2 , 2/2 2개 확인
          - app 에 접속하여 redis 접근 가능 확인
