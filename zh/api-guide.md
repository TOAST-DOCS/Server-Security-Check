## Security > Server Security Check > API Guide

This document describes Server Security Check Public API.

## Common Requirements

API endpoint and token for using the API.

### API Endpoint

| Region | Endpoint |
| --- | ----- |
| Every Region | https://kr1-server-security-check.api.nhncloudservice.com |

### Authentication Token Issue

Server Security Check uses the NHN cloud token to obtain API authentication/authorization.
Please check [User Access Key Token](https://docs.nhncloud.com/en/nhncloud/en/public-api/user-access-key-token/) to confirm the information required to use the authentication token.

## Common Information for API Use

### Common API Request Information

You need the following information to use API:

* Include token information in the API header after token issuance.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| x-nhn-authorization | Header | String | O | Token |

* Service Appkey
    * You can check it in the **URL & Appkey** menu at the top of the Server Security Check console or in **Services in Use** in Project Management.
    * Service URL Path includes Appkey.

### Common API Response Information

* In response to an API request, a response code can be returned as follows:
    * **200 OK**
    * **400 Bad Request**
    * **401 Unauthorized**
    * **404 Not Found**
    * **413** **Payload Too Large**
    * **405 Method Not Allowed**
    * **500 Internal Server Error**
* Every response code includes a common response body.
    * Common response body

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| header | Body | Object |  |
| header.isSuccessful | Body | Boolean | true: Normal<br>false: error |
| header.resultCode | Body | Integer | 0: Normal<br>Other: error |
| header.resultMessage | Body | String | "SUCCESS": Normal<br>Other: error cause message |

* <span style="color: rgb(49, 51, 56);">For detailed response results other than the common response body, refer to the response body header.</span>

> [Caution] Fields not specified in the guide may appear in API responses. These fields are used internally by NHN Cloud and are subject to change without prior notice, so they are not used.

## Server Security Check

### View Inspection Summary

Summarize the inspection summary for the desired period you want.
(The maximum view period is one month.)

```
GET "/ssc/v1.0/appKey/{appKey}/inspection_result/summary"
x-nhn-authorization: {token-id}
```

<br>

##### Request

This API does not request a response body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| appKey | URL Path | String | O | Service Appkey |
| regionCode | Query | String | O | Region Info (KR1, KR2, ...) |
| language | Query | String | X | KO, EN, JA (default : KO) |
| from | Query | <span style="color: rgb(49, 51, 56);">DateTime</span> | O | Search start time<span style="color: rgb(49, 51, 56);">(</span><span style="color: oklch(0.3039 0.04 213.68);">YYYY-MM-DDTHH:mm:ss±hh:mm</span><span style="color: rgb(49, 51, 56);">)</span><br>ex: 2025-06-17T00:00:00%2B09:00 |
| to | Query | <span style="color: rgb(49, 51, 56);">DateTime</span> | O | Search end time<span style="color: rgb(49, 51, 56);">(</span><span style="color: oklch(0.3039 0.04 213.68);">YYYY-MM-DDTHH:mm:ss±hh:mm</span><span style="color: rgb(49, 51, 56);">)</span><br>ex: 2025-06-17T23:59:59%2B09:00 |
| page | Query | Integer | X |  Page number to view (default: 1) |
| limit | Query | Integer | X | Page size to view (default: 10, max: 1000) |
| kind | Query | ENUM | X | Inspection type (OS, WAS)<br>Currently, only OS is supported |
| bss | Query | ENUM | X | Inspection standard ("M": Main information and communication facilities, "F": Electronic financial infrastructure) |

##### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| usageStasNo | Body | String | Inspection result sn |
| instanceName | Body | String | Inspection instance name |
| os | Body | ENUM | Inspection instance os<br>(Window, Linux) |
| systemVersion | Body | String | Inspection instance os version |
| bss | Body | ENUM | Inspection standard ("M": Main information and communication facilities, "F": Electronic financial infrastructure) |
| scriptVersion | Body | String | Inspection script version |
| executionTime | Body | DateTime | Inspection execution time |
| checkCount | Body | Integer | No. of inspections |
| weakCount | Body | Integer | No. of vulnerabilities |
| level3WeakCount | Body | Integer | vulnerability level high |
| level2WeakCount | Body | Integer | vulnerability level medium |
| level1WeakCount | Body | Integer | vulnerability level low |

<details>
<summary><span>Example</span></summary>

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "success": true
    },
    "results": [
        {
            "usageStasNo": 1,
            "instanceName": "test-ubuntu-1",
            "os": "Linux",
            "kind": "OS",
            "systemVersion": "ubuntu Server 22.04 LTS",
            "bss": "M",
            "scriptVersion": "G-1",
            "executionTime": "2025-07-17T11:42:20+09:00",
            "checkCount": 65,
            "weakCount": 15,
            "level3WeakCount": 5,
            "level2WeakCount": 5,
            "level1WeakCount": 5
        },
        {
            "usageStasNo": 2,
            "instanceName": "test-ubuntu-2",
            "os": "Linux",
            "kind": "OS",
            "systemVersion": "ubuntu Server 22.04 LTS",
            "bss": "M",
            "scriptVersion": "G-1",
            "executionTime": "2025-07-16T15:11:23+09:00",
            "checkCount": 65,
            "weakCount": 15,
            "level3WeakCount": 5,
            "level2WeakCount": 5,
            "level1WeakCount": 5
        }
    ],
    "page": {
        "itemPerPage": 20,
        "page": 1,
        "totalCount": 2
    }
}
```

<br>

</details>

### Inspection Result Details

After viewing the inspection result summary, you can search for a specific inspection result in detail using the inspection result number.

```
GET "/ssc/v1.0/appKey/{appKey}/inspection_result/details/{usageStasNo}"
x-nhn-authorization: {token-id}
```

#### Request

This API does not request a response body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| appKey | URL | String | O | Service Appkey |
| usageStasNo | URL | Integer | O | Inspection result number |
| language | Query | String | X | KO, EN, JA (default : KO) |

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| categoryName | Body | String | Inspection classification |
| resultId | Body | String | Anaysis result id |
| weakLevel | Body | ENUM | Vulnerability level ("H", "M", "L") |
| weakLevelName | Body | String | Vulnerability enum name |
| resultCode | Body | String | Inspection cycle setting |
| itemName | Body | String | Item name |
| manageMethod | Body | String | Countermeasure |

<details>
<summary><span>Example</span></summary>

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "success": true
    },
    "results": [
        {
            "categoryName": "1. Account management",
            "resultId": "U-01",
            "weakLevel": "H",
            "resultCode": "X",
            "weakLevelName": "High",
            "itemName": "Limit the remote access of the root account",
            "manageMethod": "1. Remove or comment out pts/0 ~ pts/x settings in the \"/etc/securetty\" file.<br>2. Modify \"/etc/pam.d/login\" file or insert a new file<br>auth required /lib/security/pam_securitty.so"
        },
        {
            "categoryName": "1. Account management",
            "resultId": "U-02",
            "weakLevel": "H",
            "resultCode": "X",
            "weakLevelName": "High",
            "itemName": "Password complexity setting",
            "manageMethod": "[Linux - RHEL5]<br>1. Check password complexity setting file<br>Edit # /etc/pam.d/system-auth, /etc/login.defs contents to conform to internal policies<br>2. Set /etc/pam.d/system-auth file<br>※ Set the password policy on the next line:<br>- Example of password policy settings<br>password   requisite  /lib/security/$ISA/pam_cracklib.so retry=3 minlen=8 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1<br>3. Inspect /etc/login.defs file<br>pass_warn_age = 7(Password Expiration Warning)<br>pass_max_days = 60 (set maximum password period)<br>pass_min_day = 1 (set minimum password period)<br><br>[Linux - RHEL7]<br>1. Check password complexity setting file<br>Edit # /etc/pam.d/system-auth, /etc/login.defs contents to conform to internal policies<br>2. Set /etc/pam.d/system-auth file<br>※ Set the password policy on the next line:<br>- Example of password policy settings<br>password   requisite  /lib/security/$ISA/pam_cracklib.so retry=3 minlen=8 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1<br>or<br>password   requisite  /lib/security/$ISA/pam_pwquality.so retry=3 minlen=8 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1<br>3. Inspect /etc/login.defs file<br>pass_warn_age = 7(Password Expiration Warning)<br>pass_max_days = 60 (set maximum password period)<br>pass_min_day = 1 (set minimum password period)<br><br>[Linux - Ubuntu]<br>1. Check password complexity setting file<br>Edit /etc/pam.d/common-auth to conform to internal policies<br>2. Set /etc/pam.d/common-auth file<br>※ Set the password policy on the next line:<br>- Example of password policy settings<br>password   requisite  /lib/security/$ISA/pam_cracklib.so retry=3 minlen=8 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1"
        },
        {
            "categoryName": "1. Account management",
            "resultId": "U-03",
            "weakLevel": "H",
            "resultCode": "X",
            "weakLevelName": "High",
            "itemName": "Account lockout threshold setting",
            "manageMethod": "1. Open \"/etc/pam.d/system-auth\" file by using vi editor<br>2. Modify it or insert a new file as below:<br>Add the following on /etc/pam.d/system-auth file.  <br>auth required /lib/security/pam_tally2.so deny=5 unlock_time=120 no_magic_root<br>account required /lib/security/pam_tally2.so no_magic_root reset"
        },
        {
            "categoryName": "1. Account management",
            "resultId": "U-04",
            "weakLevel": "H",
            "resultCode": "O",
            "weakLevelName": "High",
            "itemName": "Password file protection",
            "manageMethod": "1. Check for /shadow file existence<br>(usually located in the /etc directory)<br># ls /etc<br>2. Make sure the second field in the /etc/passwd file displays \"x\"<br># cat /etc/passwd<br>root:x:0:0:root:/root:/bin/bash"
        },
        {
            "categoryName": "2. File and directory management",
            "resultId": "U-05",
            "weakLevel": "H",
            "resultCode": "O",
            "weakLevelName": "High",
            "itemName": "root home, path directory permission and path setting",
            "manageMethod": "1. Open root account's setting file (~/.profile and /etc/profile) by using vi editor<br># vi /etc/profile<br>2. Modify as below:<br>(Before) PATH=.:$PATH:$HOME/bin<br>(After) PATH=$PATH:$HOME/bin:."
        }
    ]
}
```

<br>

</details>

<br>

