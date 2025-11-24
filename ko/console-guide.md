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

## Server Security Check 서비스 게이트웨이 연동 
서비스 게이트웨이를 이용하면 NHN Cloud 내부에서 클라이언트와 Server Security Check가 통신할 때 외부 인터넷을 경유하지 않고, 내부 네트워크로 통신할 수 있습니다.
Server Security Check 서비스 게이트웨이를 연동하는 방법은 아래와 같습니다.

1. **Network > Service Gateway** 페이지로 이동하여 **+ 서비스 게이트웨이 생성**을 클릭합니다.
2. 생성할 서비스 게이트웨이의 **이름**, **VPC**, **서브넷**을 입력하고 **서비스**를 **Server Security Check**로 선택한 뒤 **확인**을 클릭하면 Server Security Check 서비스 게이트웨이가 생성됩니다.

![ssc_sg_251117.png](https://static.toastoven.net/prod_serversecuritycheck/ssc_sg_251117.png)

### 호스트 등록
인스턴스에서 Server Security Check Endpoint의 IP를 찾을 수 있도록 호스트 파일에 Server Security Check 서비스 게이트웨이 IP 주소와 Server Security Check Endpoint 도메인을 입력합니다.
Server Security Check 서비스 게이트웨이의 IP 주소는 **Network > Service Gateway** 페이지에서 확인할 수 있습니다.

**Windows**

C:\Windows\System32\drivers\etc\hosts 파일을 열고 다음 내용을 추가합니다.

```
{Server Security Check 서비스 게이트웨이 IP 주소} api-serversecuritycheck.alpha-nhncloudservice.com
```

**Linux**

/etc/hosts 파일을 열고 다음 내용을 추가합니다.

```
{Server Security Check 서비스 게이트웨이 IP 주소} api-serversecuritycheck.alpha-nhncloudservice.com
```


## 운영 문의

### 문의 대상

1\. Agent 실행 실패 문의

2\. 점검 결과에 대한 오용 탐지 신고

### 문의 방법

1\. 문의 방법: **고객 센터 > 1:1 문의**

2\. 대응 시간: 평일 09:00~18:00



