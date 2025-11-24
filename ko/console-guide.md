## Security > Server Security Check > 콘솔 사용 가이드

여기에서는 점검 Agent 실행 절차를 설명합니다. 

## Agent 실행 절차

인스턴스 OS, 점검 종류, 점검 기준을 선택하여 Agent 실행 스크립트를 불러옵니다.

![serversecuritycheck_01_20201015.png](https://static.toastoven.net/prod_serversecuritycheck/serversecuritycheck_01_20201015.png)

### Linux 계열 Agent

1\. 실행 스크립트를 복사하려면 클립보드 복사를 클릭합니다.

2\. 실행 대상 인스턴스 터미널에 접속합니다.

3\. 관리자 권한으로 Agent 스크립트를 생성하고 실행합니다.

* root 권한을 얻습니다.
* vi 편집기 등으로 스크립트를 생성합니다.
* 생성한 스크립트 파일의 권한을 변경합니다.
* 파일을 실행합니다.
```
[root@centos7 ~]# cd ~
[root@centos7 ~]# sudo su
[root@centos7 ~]# vi agent.sh
[root@centos7 ~]# chmod 744 agent.sh
[root@centos7 ~]# ./agent.sh
OS Security Check Success! :)
```

## 운영 문의

### 문의 대상

1\. Agent 실행 실패 문의

2\. 점검 결과에 대한 오용 탐지 신고

### 문의 방법

1\. 문의 방법: **고객 센터 > 1:1 문의**

2\. 대응 시간: 평일 09:00~18:00



