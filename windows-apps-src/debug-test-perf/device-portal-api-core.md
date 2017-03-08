---
author: dbirtolo
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: "디바이스 포털 핵심 API 참조"
description: "데이터에 액세스하고 디바이스를 프로그래밍 방식으로 제어하는 데 사용할 수 있는 Windows Device Portal 핵심 REST API에 대해 알아봅니다."
ms.author: dbirtolo
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 10d8799b73a941a4a0eb89fd369d67b2fc6a68c2
ms.lasthandoff: 02/07/2017

---

# <a name="device-portal-core-api-reference"></a>디바이스 포털 핵심 API 참조

Windows Device Portal의 모든 작업은 데이터에 액세스하고 디바이스를 프로그래밍 방식으로 제어하는 데 사용할 수 있는 REST API를 기반으로 합니다.

## <a name="app-deployment"></a>앱 배포

---
### <a name="install-an-app"></a>앱 설치

**요청**

다음 요청 형식을 사용하여 앱을 설치할 수 있습니다.

메서드      | 요청 URI
:------     | :-----
게시 | /api/app/packagemanager/package
<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
package   | (**필수**) 설치할 패키지의 파일 이름입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 앱에는 종속성뿐만 아니라 .appx 또는 .appxbundle 파일이 필요합니다. 
- 인증서는 디바이스가 IoT 또는 Windows 데스크톱인 경우 앱에 서명하는 데 사용됩니다. 다른 플랫폼은 인증서를 요구하지 않습니다. 

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 수락되어 처리 중인 요청 배포
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="get-app-installation-status"></a>앱 설치 상태 가져오기

**요청**

다음 요청 형식을 사용하여 현재 진행 중인 앱 설치의 상태를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/app/packagemanager/state
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 마지막 배포의 결과
204 | 설치가 진행 중임
404 | 설치 작업을 찾을 수 없음
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="uninstall-an-app"></a>앱 제거

**요청**

다음 요청 형식을 사용하여 앱을 제거할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/app/packagemanager/package
<br />

**URI 매개 변수**

URI 매개 변수 | 설명
:---          | :---
package   | (**필수**) 대상 앱의 PackageFullName(GET /api/app/packagemanager/packages 사용)

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="get-installed-apps"></a>설치된 앱 가져오기

**요청**

다음 요청 형식을 사용하여 시스템에 설치된 앱 목록을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/app/packagemanager/packages
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 관련 세부 정보가 있는 설치된 패키지 목록이 포함됩니다. 이 응답에 대한 템플릿은 다음과 같습니다.
```
{"InstalledPackages": [
    {
        "Name": string,
        "PackageFamilyName": string,
        "PackageFullName": string,
        "PackageOrigin": int, (https://msdn.microsoft.com/en-us/library/windows/desktop/dn313167(v=vs.85).aspx)
        "PackageRelativeId": string,
        "Publisher": string,
        "Version": {
            "Build": int,
            "Major": int,
            "Minor": int,
            "Revision": int
     },
     "RegisteredUsers": [
     {
        "UserDisplayName": string,
        "UserSID": string
     },...
     ]
    },...
]}
```
**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
## Device manager
---
### <a name="get-the-installed-devices-on-the-machine"></a>컴퓨터에 설치된 디바이스 가져오기

**요청**

다음 요청 형식을 사용하여 컴퓨터에 설치된 디바이스 목록을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/devicemanager/devices
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 디바이스에 연결된 디바이스 JSON 배열이 포함됩니다.
``` 
{"DeviceList": [
    {
        "Class": string,
        "Description": string,
        "ID": string,
        "Manufacturer": string,
        "ParentID": string,
        "ProblemCode": int,
        "StatusCode": int
    },...
]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* IoT

---
## Dump collection
---
### <a name="get-the-list-of-all-crash-dumps-for-apps"></a>앱에 대한 모든 크래시 덤프 목록 가져오기

**요청**

다음 요청 형식을 사용하여 테스트용으로 로드된 모든 앱에 대한 사용 가능한 모든 크래시 덤프 목록을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/debug/dump/usermode/dumps
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 테스트용으로 로드된 각 응용 프로그램의 크래시 덤프 목록이 포함됩니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile(Windows 참가자 프로그램)
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="get-the-crash-dump-collection-settings-for-an-app"></a>앱의 크래시 덤프 컬렉션 설정 가져오기

**요청**

다음 요청 형식을 사용하여 테스트용으로 로드된 앱의 크래시 덤프 컬렉션 설정을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/debug/dump/usermode/crashcontrol
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
packageFullname   | (**필수**) 테스트용으로 로드된 앱 패키지의 전체 이름입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답 형식은 다음과 같습니다.
```
{"CrashDumpEnabled": bool}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile(Windows 참가자 프로그램)
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="delete-a-crash-dump-for-a-sideloaded-app"></a>테스트용으로 로드된 앱의 크래시 덤프 삭제

**요청**

다음 요청 형식을 사용하여 테스트용으로 로드된 앱의 크래시 덤프를 삭제할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/debug/dump/usermode/crashdump
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
packageFullname   | (**필수**) 테스트용으로 로드된 앱 패키지의 전체 이름입니다.
fileName   | (**필수**) 삭제해야 하는 덤프 파일의 이름입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile(Windows 참가자 프로그램)
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="disable-crash-dumps-for-a-sideloaded-app"></a>테스트용으로 로드된 앱의 크래시 덤프 사용 안 함

**요청**

다음 요청 형식을 사용하여 테스트용으로 로드된 앱의 크래시 덤프를 사용하지 않도록 설정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/debug/dump/usermode/crashcontrol

<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
packageFullname   | (**필수**) 테스트용으로 로드된 앱 패키지의 전체 이름입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile(Windows 참가자 프로그램)
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="download-the-crash-dump-for-a-sideloaded-app"></a>테스트용으로 로드된 앱의 크래시 덤프 다운로드

**요청**

다음 요청 형식을 사용하여 테스트용으로 로드된 앱의 크래시 덤프를 다운로드할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/debug/dump/usermode/crashdump
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
packageFullname   | (**필수**) 테스트용으로 로드된 앱 패키지의 전체 이름입니다.
fileName   | (**필수**) 다운로드하려는 덤프 파일의 이름입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 덤프 파일이 포함됩니다. WinDbg 또는 Visual Studio를 사용하여 덤프 파일을 검사할 수 있습니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile(Windows 참가자 프로그램)
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="enable-crash-dumps-for-a-sideloaded-app"></a>테스트용으로 로드된 앱의 크래시 덤프 사용

**요청**

다음 요청 형식을 사용하여 테스트용으로 로드된 앱의 크래시 덤프를 사용하도록 설정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/debug/dump/usermode/crashcontrol
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
packageFullname   | (**필수**) 테스트용으로 로드된 앱 패키지의 전체 이름입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile(Windows 참가자 프로그램)
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="get-the-list-of-bugcheck-files"></a>오류 검사 파일 목록 가져오기

**요청**

다음 요청 형식을 사용하여 오류 검사 미니덤프 파일 목록을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/debug/dump/kernel/dumplist
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 덤프 파일의 이름 및 크기 목록이 포함됩니다. 이 목록의 형식은 다음과 같습니다. 
```
{"DumpFiles": [
    {
        "FileName": string,
        "FileSize": int
    },...
]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="download-a-bugcheck-dump-file"></a>오류 검사 덤프 파일 다운로드

**요청**

다음 요청 형식을 사용하여 오류 검사 덤프 파일을 다운로드할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/debug/dump/kernel/dump
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
filename   | (**필수**) 덤프 파일의 파일 이름입니다. 덤프 목록을 가져오기 위해 API를 사용하여 이 파일을 찾을 수 있습니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 덤프 파일이 포함됩니다. WinDbg를 사용하여 이 파일을 검사할 수 있습니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="get-the-bugcheck-crash-control-settings"></a>오류 검사 크래시 제어 설정 가져오기

**요청**

다음 요청 형식을 사용하여 오류 검사 크래시 제어 설정을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/debug/dump/kernel/crashcontrol

<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 크래시 제어 설정이 포함됩니다. CrashControl에 대한 자세한 내용은 [CrashControl](https://technet.microsoft.com/library/cc951703.aspx) 문서를 참조하세요. 응답 템플릿은 다음과 같습니다.
```
{
    "autoreboot": bool (0 or 1),
    "dumptype": int (0 to 4),
    "maxdumpcount": int,
    "overwrite": bool (0 or 1)
}
```

**덤프 유형**

0: 사용 안 함

1: 전체 메모리 덤프(사용 중 메모리 모두 수집)

2: 커널 메모리 덤프(사용자 모드 메모리 무시)

3: 제한된 커널 미니덤프

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="get-a-live-kernel-dump"></a>라이브 커널 덤프 가져오기

**요청**

다음 요청 형식을 사용하여 라이브 커널 덤프를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/debug/dump/livekernel
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 전체 커널 모드 덤프가 포함됩니다. WinDbg를 사용하여 이 파일을 검사할 수 있습니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="get-a-dump-from-a-live-user-process"></a>라이브 사용자 프로세스에서 덤프 가져오기

**요청**

다음 요청 형식을 사용하여 라이브 사용자 프로세스 덤프를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/debug/dump/usermode/live
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
pid   | (**필수**) 관심 있는 프로세스에 대한 고유 프로세스 id입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 프로세스 덤프가 포함됩니다. WinDbg 또는 Visual Studio를 사용하여 이 파일을 검사할 수 있습니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="set-the-bugcheck-crash-control-settings"></a>오류 검사 크래시 제어 설정 지정

**요청**

다음 요청 형식을 사용하여 오류 검사 데이터를 수집하기 위한 설정을 지정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/debug/dump/kernel/crashcontrol
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
autoreboot   | (**선택**) True 또는 false입니다. 실패 또는 잠긴 후 시스템을 자동으로 다시 시작하는지 여부를 나타냅니다.
dumptype   | (**선택**) 덤프 유형입니다. 지원되는 값은 [CrashDumpType 열거](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx)를 참조하세요.
maxdumpcount   | (**선택**) 저장할 최대 덤프 수입니다.
overwrite   | (**선택**) True 또는 false입니다. *maxdumpcount*에 의해 지정된 덤프 카운터 한도에 도달한 경우 이전 덤프를 덮어쓸지 여부를 나타냅니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
## ETW
---
### <a name="create-a-realtime-etw-session-over-a-websocket"></a>Websocket을 통해 실시간 ETW 세션 만들기

**요청**

다음 요청 형식을 사용하여 실시간 ETW 세션을 만들 수 있습니다. 이 세션은 Websocket을 통해 관리됩니다.  ETW 이벤트는 서버에서 일괄 처리되며 1초에 한 번씩 클라이언트로 전송됩니다. 
 
메서드      | 요청 URI
:------     | :-----
GET/WebSocket | /api/etw/session/realtime
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 활성화된 공급자의 ETW 이벤트가 포함됩니다.  아래의 ETW WebSocket 명령을 참조하세요. 

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

### <a name="etw-websocket-commands"></a>ETW WebSocket 명령
이러한 명령은 클라이언트에서 서버로 전송됩니다.

명령 | 설명
:----- | :-----
provider *{guid}* enable *{level}* | 지정된 수준에서 *{guid}*(괄호 없음)로 표시된 공급자를 사용하도록 설정합니다. 여기서 *{level}*은 1(가장 대략적인 정보)부터 5(자세한 정보)까지의 **int**입니다.
provider *{guid}* disable | *{guid}*(괄호 없음)로 표시된 공급자를 사용하지 않도록 설정합니다.

이 응답은 서버에서 클라이언트로 전송됩니다. 텍스트로 전송되며 JSON을 구문 분석하여 다음 형식을 가져옵니다.
```
{
    "Events":[
        {
            "Timestamp": int,
            "ProviderName": string,
            "ID": int, 
            "TaskName": string,
            "Keyword": int,
            "Level": int,
            payload objects...
        },...
    ],
    "Frequency": int
}
```

페이로드 개체는 원래 ETW 이벤트에서 제공되는 추가 키-값 쌍(문자열:문자열)입니다.

예:
```
{
    "ID" : 42, 
    "Keyword" : 9223372036854775824, 
    "Level" : 4, 
    "Message" : "UDPv4: 412 bytes transmitted from 10.81.128.148:510 to 132.215.243.34:510. ",
    "PID" : "1218", 
    "ProviderName" : "Microsoft-Windows-Kernel-Network", 
    "TaskName" : "KERNEL_NETWORK_TASK_UDPIP", 
    "Timestamp" : 131039401761757686, 
    "connid" : "0", 
    "daddr" : "132.245.243.34", 
    "dport" : "500", 
    "saddr" : "10.82.128.118", 
    "seqnum" : "0", 
    "size" : "412", 
    "sport" : "500"
}
```

---
### <a name="enumerate-the-registered-etw-providers"></a>등록된 ETW 공급자 열거

**요청**

다음 요청 형식을 사용하여 등록된 공급자를 열거할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/etw/providers
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 ETW 공급자 목록이 포함됩니다. 목록에 포함된 각 공급자 이름과 GUID 형식은 다음과 같습니다.
```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="enumerate-the-custom-etw-providers-exposed-by-the-platform"></a>플랫폼에 의해 노출된 사용자 지정 ETW 공급자 열거

**요청**

다음 요청 형식을 사용하여 등록된 공급자를 열거할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/etw/customproviders
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

200 정상입니다. 응답에는 ETW 공급자 목록이 포함됩니다. 목록에는 각 공급자의 식별 이름 및 GUID가 포함됩니다.

```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**상태 코드**

- 표준 상태 코드입니다.
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
## OS information
---
### <a name="get-the-machine-name"></a>컴퓨터 이름 가져오기

**요청**

다음 요청 형식을 사용하여 컴퓨터의 이름을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/os/machinename
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 다음 형식의 컴퓨터 이름이 포함됩니다. 

```
{"ComputerName": string}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="get-the-operating-system-information"></a>운영 체제 정보 가져오기

**요청**

다음 요청 형식을 사용하여 컴퓨터의 OS 정보를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/os/info
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 다음 형식의 OS 정보가 포함됩니다.

```
{
    "ComputerName": string,
    "OsEdition": string,
    "OsEditionId": int,
    "OsVersion": string,
    "Platform": string
}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="get-the-device-family"></a>디바이스 패밀리 가져오기 

**요청**

다음 요청 형식을 사용하여 디바이스 패밀리(Xbox, 휴대폰, 데스크톱 등)을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/os/devicefamily
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 디바이스 패밀리(SKU - 데스크톱, Xbox 등)이 포함됩니다.

```
{
   "DeviceType" : string
}
```

DeviceType은 "Windows.Xbox", "Windows.Desktop" 등과 같이 표시됩니다. 

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드

**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="set-the-machine-name"></a>컴퓨터 이름 설정

**요청**

다음 요청 형식을 사용하여 컴퓨터의 이름을 설정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/os/machinename
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
name | (**필수**) 컴퓨터의 새 이름입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
## Performance data
---
### <a name="get-the-list-of-running-processes"></a>실행 중인 프로세스 목록 가져오기

**요청**

다음 요청 형식을 사용하여 현재 실행 중인 프로세스 목록을 가져올 수 있습니다.  또한 1초에 한 번씩 클라이언트로 푸시 중인 동일한 JSON 데이터를 사용하여 WebSocket 연결로 업그레이드할 수 있습니다. 
 
메서드      | 요청 URI
:------     | :-----
GET | /api/resourcemanager/processes
GET/WebSocket | /api/resourcemanager/processes
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 각 프로세스에 대한 세부 정보가 있는 프로세스 목록이 포함됩니다. 정보는 JSON 형식이며 템플릿은 다음과 같습니다.
```
{"Processes": [
    {
        "CPUUsage": int,
        "ImageName": string,
        "PageFileUsage": int,
        "PrivateWorkingSet": int,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": int,
        "WorkingSetSize": int
    },...
]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="get-the-system-performance-statistics"></a>시스템 성능 통계 가져오기

**요청**

다음 요청 형식을 사용하여 시스템 성능 통계를 가져올 수 있습니다. 읽기/쓰기 주기 및 사용된 메모리 사용량과 같은 정보가 포함됩니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/resourcemanager/systemperf
GET/WebSocket | /api/resourcemanager/systemperf
<br />
이 WebSocket 연결로 업그레이드할 수도 있습니다.  1초에 한 번씩 아래 동일한 JSON 데이터를 제공합니다. 

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 CPU 및 GPU 사용, 메모리 액세스 및 네트워크 액세스와 같은 시스템에 대한 성능 통계가 포함됩니다. 이 정보는 JSON 형식이며 템플릿은 다음과 같습니다.
```
{
    "AvailablePages": int,
    "CommitLimit": int,
    "CommittedPages": int,
    "CpuLoad": int,
    "IOOtherSpeed": int,
    "IOReadSpeed": int,
    "IOWriteSpeed": int,
    "NonPagedPoolPages": int,
    "PageSize": int,
    "PagedPoolPages": int,
    "TotalInstalledInKb": int,
    "TotalPages": int,
    "GPUData": 
    {
        "AvailableAdapters": [{ (One per detected adapter)
            "DedicatedMemory": int,
            "DedicatedMemoryUsed": int,
            "Description": string,
            "SystemMemory": int,
            "SystemMemoryUsed": int,
            "EnginesUtilization": [ float,... (One per detected engine)]
        },...
    ]},
    "NetworkingData": {
        "NetworkInBytes": int,
        "NetworkOutBytes": int
    }
}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
## Power
---
### <a name="get-the-current-battery-state"></a>현재 배터리 상태 가져오기

**요청**

다음 요청 형식을 사용하여 배터리의 현재 상태를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/power/battery
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

다음 형식을 사용하여 현재 배터리 상태 정보를 반환합니다.
```
{
    "AcOnline": int (0 | 1),
    "BatteryPresent": int (0 | 1),
    "Charging": int (0 | 1),
    "DefaultAlert1": int,
    "DefaultAlert2": int,
    "EstimatedTime": int,
    "MaximumCapacity": int,
    "RemainingCapacity": int
}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="get-the-active-power-scheme"></a>현재 전원 구성표 가져오기

**요청**

다음 요청 형식을 사용하여 현재 전원 구성표를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/power/activecfg
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

활성 전원 구성표 형식은 다음과 같습니다.
```
{"ActivePowerScheme": string (guid of scheme)}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="get-the-sub-value-for-a-power-scheme"></a>전원 구성표에 대한 하위 값 가져오기

**요청**

다음 요청 형식을 사용하여 전원 구성표에 대한 하위 값을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/power/cfg/*<power scheme path>*
<br />
옵션:
- SCHEME_CURRENT

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

사용할 수 있는 전원 상태의 전체 목록은 응용 프로그램을 기반으로 작성되며 낮거나 위험한 배터리 수준과 같은 다양한 전원 상태의 플래그를 지정하기 위한 설정입니다. 

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="get-the-power-state-of-the-system"></a>시스템의 전원 상태 가져오기

**요청**

다음 요청 형식을 사용하여 시스템의 전원 상태를 확인할 수 있습니다. 시스템이 절전 상태인지 확인할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/power/state
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

전원 상태 정보 템플릿은 다음과 같습니다.
```
{"LowPowerStateAvailable": bool}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="set-the-active-power-scheme"></a>현재 전원 구성표 설정

**요청**

다음 요청 형식을 사용하여 현재 전원 구성표를 설정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/power/activecfg
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
scheme | (**필수**) 시스템의 현재 전원 구성표로 설정하려는 구성표의 GUID입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="set-the-sub-value-for-a-power-scheme"></a>전원 구성표의 하위 값 설정

**요청**

다음 요청 형식을 사용하여 전원 구성표에 대한 하위 값을 설정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/power/cfg/*<power scheme path>*
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
valueAC | (**필수**) A/C 전원에 사용할 값입니다.
valueDC | (**필수**) 배터리 전원에 사용할 값입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="get-a-sleep-study-report"></a>절전 연구 보고서 가져오기

**요청**

메서드      | 요청 URI
:------     | :-----
GET | /api/power/sleepstudy/report
<br />
다음 요청 형식을 사용하여 절전 연구 보고서를 가져올 수 있습니다.

**URI 매개 변수**
URI 매개 변수 | 설명
:---          | :---
FileName | (**필수**) 다운로드하려는 파일의 전체 이름입니다. 이 값은 hex64로 인코드되어야 합니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답은 절전 연구를 포함하는 파일입니다. 

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="enumerate-the-available-sleep-study-reports"></a>사용 가능한 절전 연구 보고서 열거

**요청**

다음 요청 형식을 사용하여 사용 가능한 절전 연구 보고서를 열거할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/power/sleepstudy/reports
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

사용 가능한 보고서 목록 템플릿은 다음과 같습니다.

```
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
### <a name="get-the-sleep-study-transform"></a>절전 연구 변환 가져오기

**요청**

다음 요청 형식을 사용하여 절전 연구 변환을 가져올 수 있습니다. 이 변환은 절전 연구 보고서를 사람이 읽을 수 있는 XML 형식으로 변환하는 XSLT입니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/power/sleepstudy/transform
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 절전 연구 변환이 포함되어 있습니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* IoT

---
## Remote control
---
### <a name="restart-the-target-computer"></a>대상 컴퓨터 다시 시작

**요청**

다음 요청 형식을 사용하여 대상 컴퓨터를 다시 시작할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/control/restart
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="shut-down-the-target-computer"></a>대상 컴퓨터 종료

**요청**

다음 요청 형식을 사용하여 대상 컴퓨터를 종료할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/control/shutdown
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
## Task manager
---
### <a name="start-a-modern-app"></a>최신 앱 시작

**요청**

다음 요청 형식을 사용하여 최신 앱을 시작할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/taskmanager/app
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
appid   | (**필수**) 시작하려는 앱의 PRAID입니다. 이 값은 hex64로 인코드되어야 합니다.
package   | (**필수**) 시작하려는 앱 패키지의 전체 이름입니다. 이 값은 hex64로 인코드되어야 합니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="stop-a-modern-app"></a>최신 앱 중지

**요청**

다음 요청 형식을 사용하여 최신 앱을 중지할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/taskmanager/app
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
package   | (**필수**) 중지하려는 앱 패키지의 전체 이름입니다. 이 값은 hex64로 인코드되어야 합니다.
forcestop   | (**선택**) 값 **yes**는 시스템이 모든 프로세스를 강제로 중지함을 나타냅니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
## Networking
---
### <a name="get-the-current-ip-configuration"></a>현재 IP 구성 가져오기

**요청**

다음 요청 형식을 사용하여 현재 IP 구성을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/networking/ipconfig
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

응답에는 다음 템플릿의 IP 구성이 포함됩니다.

```
{"Adapters": [
    {
        "Description": string,
        "HardwareAddress": string,
        "Index": int,
        "Name": string,
        "Type": string,
        "DHCP": {
            "LeaseExpires": int, (timestamp)
            "LeaseObtained": int, (timestamp)
            "Address": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "WINS": {(WINS is optional)
            "Primary": {
                "IpAddress": string,
                "Mask": string
            },
            "Secondary": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "Gateways": [{ (always 1+)
            "IpAddress": "10.82.128.1",
            "Mask": "255.255.255.255"
            },...
        ],
        "IpAddresses": [{ (always 1+)
            "IpAddress": "10.82.128.148",
            "Mask": "255.255.255.0"
            },...
        ]
    },...
]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

--
### <a name="enumerate-wireless-network-interfaces"></a>무선 네트워크 인터페이스 열거

**요청**

다음 요청 형식을 사용하여 사용 가능한 무선 네트워크 인터페이스를 열거할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/wifi/interfaces
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

세부 정보가 있는 사용 가능한 무선 인터페이스 목록 형식은 다음과 같습니다.

``` 
{"Interfaces": [{
    "Description": string,
    "GUID": string (guid with curly brackets),
    "Index": int,
    "ProfilesList": [
        {
            "GroupPolicyProfile": bool,
            "Name": string, (Network currently connected to)
            "PerUserProfile": bool
        },...
    ]
    }
]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="enumerate-wireless-networks"></a>무선 네트워크 열거

**요청**

다음 요청 형식을 사용하여 지정된 인터페이스에 무선 네트워크의 목록을 열거할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/wifi/networks
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
interface   | (**필수**) 무선 네트워크 검색에 사용할 네트워크 인터페이스의 GUID입니다(괄호 없음). 
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

제공된 *interface*에서 찾은 무선 네트워크 목록입니다. 여기에 포함된 네트워크 세부 정보 형식은 다음과 같습니다.

```
{"AvailableNetworks": [
    {
        "AlreadyConnected": bool,
        "AuthenticationAlgorithm": string, (WPA2, etc)
        "Channel": int,
        "CipherAlgorithm": string, (e.g. AES)
        "Connectable": int, (0 | 1)
        "InfrastructureType": string,
        "ProfileAvailable": bool,
        "ProfileName": string,
        "SSID": string,
        "SecurityEnabled": int, (0 | 1)
        "SignalQuality": int,
        "BSSID": [int,...],
        "PhysicalTypes": [string,...]
    },...
]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="connect-and-disconnect-to-a-wi-fi-network"></a>Wi-Fi 네트워크에 연결 및 연결 해제

**요청**

다음 요청 형식을 사용하여 Wi-Fi 네트워크에 연결 및 연결 해제할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/wifi/network
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
interface   | (**필수**) 네트워크 연결에 사용할 네트워크 인터페이스에 대한 GUID입니다.
op   | (**필수**) 수행할 작업을 나타냅니다. 가능한 값은 connect 또는 disconnect입니다.
ssid   | (**op == connect인 경우 필수****) 연결할 SSID입니다.
key   | (**op == connect이고 네트워크에 인증이 필요한 경우 필수****) 공유 키입니다.
createprofile | (**필수**) 디바이스에서 네트워크에 대한 프로필을 만듭니다.  이렇게 하면 다음부터 디바이스에서 네트워크에 자동 연결합니다. **예** 또는 **아니요**일 수 있습니다. 

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="delete-a-wi-fi-profile"></a>Wi-Fi 프로필 삭제

**요청**

다음 요청 형식을 사용하여 특정 인터페이스의 네트워크와 연결된 프로필을 삭제할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/wifi/network
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
interface   | (**필수**) 삭제할 프로필과 연결된 네트워크 인터페이스의 GUID입니다.
profile   | (**필수**) 삭제할 프로필의 이름입니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
## Windows Error Reporting (WER)
---
### <a name="download-a-windows-error-reporting-wer-file"></a>WER(Windows 오류 보고) 파일 다운로드

**요청**

다음 요청 형식을 사용하여 WER 관련 파일을 다운로드할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/wer/report/file
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
user   | (**필수**) 보고서와 연결된 사용자 이름입니다.
type   | (**필수**) 보고서의 유형입니다. **queried** 또는 **archived**가 될 수 있습니다.
name   | (**필수**) 보고서의 이름입니다. Base64 인코드되어야 합니다. 
file   | (**필수**) 보고서에서 다운로드할 파일의 이름입니다. Base64 인코드되어야 합니다. 
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 응답에는 요청한 파일이 포함됩니다. 

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="enumerate-files-in-a-windows-error-reporting-wer-report"></a>WER(Windows 오류 보고) 보고서에 파일 열거

**요청**

다음 요청 형식을 사용하여 WER 보고서에 파일을 열거할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/wer/report/files
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
user   | (**필수**) 보고서와 연결된 사용자입니다.
type   | (**필수**) 보고서의 유형입니다. **queried** 또는 **archived**가 될 수 있습니다.
name   | (**필수**) 보고서의 이름입니다. Base64 인코드되어야 합니다. 
<br />
**요청 헤더**

- 없음

**요청 본문**

```
{"Files": [
    {
        "Name": string, (Filename, not base64 encoded)
        "Size": int (bytes)
    },...
]}
```

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="list-the-windows-error-reporting-wer-reports"></a>WER(Windows 오류 보고) 보고서 나열

**요청**

다음 요청 형식을 사용하여 WER 보고서를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/wer/reports
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

WER 보고서 형식은 다음과 같습니다.

```
{"WerReports": [
    {
        "User": string,
        "Reports": [
            {
                "CreationTime": int,
                "Name": string, (not base64 encoded)
                "Type": string ("Queue" or "Archive")
            },
    },...
]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows 데스크톱
* HoloLens
* IoT

---
## Windows Performance Recorder (WPR) 
---
### <a name="start-tracing-with-a-custom-profile"></a>사용자 지정 프로필을 사용하여 추적 시작

**요청**

다음 요청 형식을 사용하여 WPR 프로필을 업로드하고 해당 프로필을 사용하여 추적을 시작할 수 있습니다.  추적은 한 번에 하나만 실행할 수 있습니다. 프로필은 디바이스에서 유지되지 않습니다. 
 
메서드      | 요청 URI
:------     | :-----
POST | /api/wpr/customtrace
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 사용자 지정 WPR 프로필을 포함하는 http 본문을 준수하는 다중 파트입니다.

**응답**

WPR 세션 상태 형식은 다음과 같습니다.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="start-a-boot-performance-tracing-session"></a>부팅 성능 추적 세션 시작

**요청**

다음 요청 형식을 사용하여 부팅 WPR 추적 세션을 시작할 수 있습니다. 성능 추적 세션이라고도 합니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/wpr/boottrace
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
profile   | (**필수**) 시작 시 이 매개 변수가 필요합니다. 성능 추적 세션을 시작해야 하는 프로필의 이름입니다. 가능한 프로필은 perfprofiles/profiles.json에 저장됩니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

시작할 때 이 API는 다음 형식의 WPR 세션 상태를 반환합니다.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="stop-a-boot-performance-tracing-session"></a>부팅 성능 추적 세션 중지

**요청**

다음 요청 형식을 사용하여 부팅 WPR 추적 세션을 중지할 수 있습니다. 성능 추적 세션이라고도 합니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/wpr/boottrace
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

-  없음.  **참고:** 장기 실행 작업입니다.  ETL에서 디스크에 쓰기가 완료되면 반환됩니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="start-a-performance-tracing-session"></a>성능 추적 세션 시작

**요청**

다음 요청 형식을 사용하여 WPR 추적 세션을 시작할 수 있습니다. 성능 추적 세션이라고도 합니다.  추적은 한 번에 하나만 실행할 수 있습니다. 
 
메서드      | 요청 URI
:------     | :-----
POST | /api/wpr/trace
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
profile   | (**필수**) 성능 추적 세션을 시작해야 하는 프로필의 이름입니다. 가능한 프로필은 perfprofiles/profiles.json에 저장됩니다.
<br />
**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

시작할 때 이 API는 다음 형식의 WPR 세션 상태를 반환합니다.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="stop-a-performance-tracing-session"></a>성능 추적 세션 중지

**요청**

다음 요청 형식을 사용하여 WPR 추적 세션을 중지할 수 있습니다. 성능 추적 세션이라고도 합니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/wpr/trace
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음.  **참고:** 장기 실행 작업입니다.  ETL에서 디스크에 쓰기가 완료되면 반환됩니다.  

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="retrieve-the-status-of-a-tracing-session"></a>추적 세션의 상태 검색

**요청**

다음 요청 형식을 사용하여 현재 WPR 세션의 상태를 검색할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/wpr/status
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

WPR 추적 세션 상태의 형식은 다음과 같습니다.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="list-completed-tracing-sessions-etls"></a>완료된 추적 세션(ETL) 나열

**요청**

다음 요청 형식을 사용하여 디바이스의 ETL 추적 목록을 가져올 수 있습니다. 

메서드      | 요청 URI
:------     | :-----
GET | /api/wpr/tracefiles
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

완료된 추적 세션 목록은 다음 형식으로 제공됩니다.

```
{"Items": [{
    "CurrentDir": string (filepath),
    "DateCreated": int (File CreationTime),
    "FileSize": int (bytes),
    "Id": string (filename),
    "Name": string (filename),
    "SubPath": string (filepath),
    "Type": int
}]}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="download-a-tracing-session-etl"></a>추적 세션(ETL) 다운로드

**요청**

다음 요청 형식을 사용하여 추적 파일(부팅 추적 또는 사용자 모드 추적)을 다운로드할 수 있습니다. 

메서드      | 요청 URI
:------     | :-----
GET | /api/wpr/tracefile
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
filename   | (**필수**) 다운로드할 ETL 추적의 이름입니다.  /api/wpr/tracefiles에서 찾을 수 있습니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 추적 ETL 파일을 반환합니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
### <a name="delete-a-tracing-session-etl"></a>추적 세션(ETL) 삭제

**요청**

다음 요청 형식을 사용하여 추적 파일(부팅 추적 또는 사용자 모드 추적)을 삭제할 수 있습니다. 

메서드      | 요청 URI
:------     | :-----
DELETE | /api/wpr/tracefile
<br />

**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
filename   | (**필수**) 삭제할 ETL 추적의 이름입니다.  /api/wpr/tracefiles에서 찾을 수 있습니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 추적 ETL 파일을 반환합니다.

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* IoT

---
## DNS-SD Tags 
---
### <a name="view-tags"></a>태그 보기

**요청**

디바이스에 대해 현재 적용된 태그를 봅니다.  이러한 태그는 T 키의 DNS-SD TXT 레코드를 통해 알려집니다.  
 
메서드      | 요청 URI
:------     | :-----
GET | /api/dns-sd/tags
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답** 다음 형식의 현재 적용된 태그입니다. 
```
 {
    "tags": [
        "tag1", 
        "tag2", 
        ...
     ]
}
```

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
5XX | 서버 오류 

<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="delete-tags"></a>태그 삭제

**요청**

DNS-SD에 의해 현재 알려진 모든 태그를 삭제합니다.   
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/dns-sd/tags
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**
 - 없음

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
5XX | 서버 오류 

<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

---
### <a name="delete-tag"></a>태그 삭제

**요청**

DNS-SD에 의해 현재 알려진 태그를 삭제합니다.   
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/dns-sd/tag
<br />

**URI 매개 변수**

URI 매개 변수 | 설명
:------     | :-----
tagValue | (**필수**) 제거할 태그입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**
 - 없음

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인

<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT
 
---
### <a name="add-a-tag"></a>태그 추가

**요청**

DNS-SD 알림에 태그를 추가합니다.   
 
메서드      | 요청 URI
:------     | :-----
POST | /api/dns-sd/tag
<br />

**URI 매개 변수**

URI 매개 변수 | 설명
:------     | :-----
tagValue | (**필수**) 추가할 태그입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**
 - 없음

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
401 | 태그 공간 오버플로.  제안된 태그가 생성되는 DNS-SD 서비스 레코드에 너무 긴 경우의 결과입니다.  

<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* Xbox
* HoloLens
* IoT

## <a name="app-file-explorer"></a>앱 파일 탐색기

---
### <a name="get-known-folders"></a>알려진 폴더 가져오기

**요청**

액세스 가능한 최상위 폴더의 목록을 가져옵니다.

메서드      | 요청 URI
:------     | :-----
GET | /api/filesystem/apps/knownfolders
<br />

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답** 다음 형식의 사용 가능한 폴더입니다. 
```
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 수락되어 처리 중인 요청 배포
4XX | 오류 코드
5XX | 오류 코드
<br />

**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* Xbox
* IoT

---
### <a name="get-files"></a>파일 가져오기

**요청**

폴더의 파일 목록을 가져옵니다.

메서드      | 요청 URI
:------     | :-----
GET | /api/filesystem/apps/files
<br />

**URI 매개 변수**

URI 매개 변수 | 설명
:------     | :-----
knownfolderid | (**필수**) 원하는 파일 목록이 있는 최상위 디렉터리입니다. 테스트용으로 로드된 앱에 액세스하려면 **LocalAppData**를 사용합니다. 
packagefullname | (**knownfolderid == LocalAppData**인 경우 필수**) 관심 있는 앱의 패키지 전체 이름입니다. 
path | (**선택**) 위에서 지정된 폴더 또는 패키지 내의 하위 디렉터리입니다. 

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답** 다음 형식의 사용 가능한 폴더입니다. 
```
{"Items": [
    {
        "CurrentDir": string (folder under the requested known folder),
        "DateCreated": int,
        "FileSize": int (bytes),
        "Id": string,
        "Name": string,
        "SubPath": string (present if this item is a folder, this is the name of the folder),
        "Type": int
    },...
]}
```
**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* Xbox
* IoT

---
### <a name="download-a-file"></a>파일 다운로드

**요청**

알려진 폴더 또는 appLocalData에서 파일을 가져옵니다.

메서드      | 요청 URI
:------     | :-----
GET | /api/filesystem/apps/file

**URI 매개 변수**

URI 매개 변수 | 설명
:------     | :-----
knownfolderid | (**필수**) 파일을 다운로드하려는 최상위 디렉터리입니다. 테스트용으로 로드된 앱에 액세스하려면 **LocalAppData**를 사용합니다. 
filename | (**필수**) 다운로드할 파일의 이름입니다. 
packagefullname | (**knownfolderid == LocalAppData**인 경우 필수**) 관심 있는 패키지 전체 이름입니다. 
path | (**선택**) 위에서 지정된 폴더 또는 패키지 내의 하위 디렉터리입니다.

**요청 헤더**

- 없음

**요청 본문**

- 요청된 파일(있는 경우)

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 요청된 파일
404 | 파일을 찾을 수 없습니다.
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* Xbox
* IoT

---
### <a name="rename-a-file"></a>파일 이름 바꾸기

**요청**

폴더 내 파일의 이름을 변경합니다.

메서드      | 요청 URI
:------     | :-----
POST | /api/filesystem/apps/rename

<br />
**URI 매개 변수**

URI 매개 변수 | 설명
:------     | :-----
knownfolderid | (**필수**) 파일이 위치한 최상위 디렉터리입니다. 테스트용으로 로드된 앱에 액세스하려면 **LocalAppData**를 사용합니다. 
filename | (**필수**) 이름을 변경할 원래 파일의 이름입니다. 
newfilename | (**필수**) 파일의 새 이름입니다.
packagefullname | (**knownfolderid == LocalAppData**인 경우 필수**) 관심 있는 앱의 패키지 전체 이름입니다. 
path | (**선택**) 위에서 지정된 폴더 또는 패키지 내의 하위 디렉터리입니다. 

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인. 파일 이름이 변경되었습니다.
404 | 파일을 찾을 수 없습니다.
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* Xbox
* IoT

---
### <a name="delete-a-file"></a>파일 삭제

**요청**

폴더의 파일을 삭제합니다.

메서드      | 요청 URI
:------     | :-----
DELETE | /api/filesystem/apps/file
<br />
**URI 매개 변수**

URI 매개 변수 | 설명
:------     | :-----
knownfolderid | (**필수**) 파일을 삭제하려는 최상위 디렉터리입니다. 테스트용으로 로드된 앱에 액세스하려면 **LocalAppData**를 사용합니다. 
filename | (**필수**) 삭제할 파일의 이름입니다. 
packagefullname | (**knownfolderid == LocalAppData**인 경우 필수**) 관심 있는 앱의 패키지 전체 이름입니다. 
path | (**선택**) 위에서 지정된 폴더 또는 패키지 내의 하위 디렉터리입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음 

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인. 파일이 삭제되었습니다.
404 | 파일을 찾을 수 없습니다.
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* Xbox
* IoT

---
### <a name="upload-a-file"></a>파일 업로드

**요청**

폴더에 파일을 업로드합니다.  이름이 같은 기존 파일을 덮어쓰지만 새 폴더를 만들지 않습니다. 

메서드      | 요청 URI
:------     | :-----
POST | /api/filesystem/apps/file
<br />
**URI 매개 변수**

URI 매개 변수 | 설명
:------     | :-----
knownfolderid | (**필수**) 파일을 업로드하려는 최상위 디렉터리입니다. 테스트용으로 로드된 앱에 액세스하려면 **LocalAppData**를 사용합니다.
packagefullname | (**knownfolderid == LocalAppData**인 경우 필수**) 관심 있는 앱의 패키지 전체 이름입니다. 
path | (**선택**) 위에서 지정된 폴더 또는 패키지 내의 하위 디렉터리입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
200 | 확인. 파일이 업로드되었습니다.
4XX | 오류 코드
5XX | 오류 코드
<br />
**사용 가능한 디바이스 패밀리**

* Windows Mobile
* Windows 데스크톱
* HoloLens
* Xbox
* IoT
