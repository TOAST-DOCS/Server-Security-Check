<!-- pre-align:aligned sig=782f156d2e4a -->

<a id="security-server-security-check-console-guide"></a>
## Security > Server Security Check > Console Guide { #security-server-security-check-console-guide }

This section explains how to execute the check Agent.

<a id="procedure-to-execute-agent"></a>
## Procedure to execute Agent { #procedure-to-execute-agent }

Bring up the Agent execution script by selecting instance OS, check type and check standard.

![serversecuritycheck_01_20201015.png](https://static.toastoven.net/prod_serversecuritycheck/serversecuritycheck_01_20201015.png)

<a id="linux-family-agent"></a>
### Linux-family Agent { #linux-family-agent }

1\. Copy the execution script to the clipboard.

2\. Access the terminal of the target instance.

3\. Create and execute the Agent script as admin.

* Acquire root permissions.
* Create a script using an editor such as vi editor.
* Change the permissions of the created script file.
* Run the file.
```
[root@centos7 ~]# cd ~
[root@centos7 ~]# sudo su
[root@centos7 ~]# vi agent.sh
[root@centos7 ~]# chmod 744 agent.sh
[root@centos7 ~]# ./agent.sh
OS Security Check Success! :)
```

<a id="integrate-with-server-security-check-service-gateway"></a>
## Integrate with Server Security Check Service Gateway { #integrate-with-server-security-check-service-gateway }
Using the Service Gateway, communication between clients and Server Security Check within NHN Cloud can be done through the internal network without going through the external Internet.
How to integrate with Server Security Check Service Gateway is as below:

1. Go to **Network > Service Gateway** page and click **+ Create Service Gateway**.
2. Enter the **name**, **VPC**, and **subnet** of the Service Gateway you want to create, select **Server Security Check** as the **service**, and click **OK** to create the Server Security Check Service Gateway.

![ssc_sg_251117.png](https://static.toastoven.net/prod_serversecuritycheck/ssc_sg_251117.png)

<a id="register-host"></a>
### Register Host { #register-host }
Enter the Server Security Check Service Gateway IP address and the Server Security Check endpoint domain in the hosts file so that the instance can find the IP address of the Server Security Check endpoint.

You can find the IP address of the Server Security Check service gateway on the Network > Service Gateway page.

**Windows**

Open C:\Windows\System32\drivers\etc\hosts file and add the content below:

```
{IP address of the Server Security Check Service Gateway} api-serversecuritycheck.nhncloudservice.com
```

**Linux**

Open /etc/hosts file and add the content below:

```
{IP address of the Server Security Check Service Gateway} api-serversecuritycheck.nhncloudservice.com
```


<a id="operation-inquiry"></a>
## Operation Inquiry { #operation-inquiry }

<a id="inquiry-target"></a>
### Inquiry target { #inquiry-target }

1\. Inquiries about failing to execute Agent

2\. Reports about the misuse of check results

<a id="how-to-inquire"></a>
### How to Inquire { #how-to-inquire }

1\. How to inquire **Customer Center > 1:1 Inquiry**

2\. Response time: Weekdays: 09:00-18:00
