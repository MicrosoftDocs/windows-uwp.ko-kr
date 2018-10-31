---
Description: The JavaScript API for the Microsoft Take a Test app allows you to do secure assessments. Take a Test provides a secure browser that prevents students from using other computer or internet resources during a test.
title: JavaScript API 시험 응시.
author: PatrickFarley
ms.author: pafarley
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
keywords: windows 10, uwp, 교육
ms.localizationpriority: medium
ms.openlocfilehash: d64901c08e2945f34e66055d8e2e7d3a8301f66e
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5816159"
---
# <a name="take-a-test-javascript-api"></a>JavaScript API 시험 응시

[시험 응시](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) 평가에 초점을 교육자 안전한 테스트 환경을 제공 하는 방법 대신 콘텐츠 허용 위험 테스트를 위해 잠긴 온라인 평가 렌더링 하는 브라우저 기반 UWP 앱입니다. 이를 위해 이 앱에서는 모든 웹 응용 프로그램이 활용할 수 있는 JavaScript API를 사용합니다. 시험 응시 API는 고강도 일반 코어 테스트를 위해 [SBAC 브라우저 API 표준](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)을 지원합니다.

앱 자체에 대한 자세한 내용은 [시험 응시 앱 기술 참조](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)를 참조하세요. 문제 해결 도움말은 [이벤트 뷰어를 사용하여 Microsoft 시험 응시 문제 해결](troubleshooting.md)을 참조하세요.

## <a name="reference-documentation"></a>참조 설명서
시험 응시 API는 다음 네임스페이스로 존재합니다. 모든 API는 글로벌 `SecureBrowser`개체에 따라 달라짐에 주의하십시오.

| 네임스페이스 | 설명 |
|-----------|-------------|
|[보안 네임스페이스](#security-namespace)|테스트를 위해 장치를 잠그고 테스트 환경을 제어하도록 해주는 API가 들어 있습니다. |

### <a name="security-namespace"></a>보안 네임스페이스

보안 네임 스페이스를 사용 하면 장치를 잠그고, 사용자 및 시스템 프로세스 목록을 확인 하, MAC 및 IP 주소를 얻을 고 캐시 된 웹 리소스를 지울 수 있습니다.

| 메서드 | 설명   |
|--------|---------------|
|[LockDown](#lockDown) | 테스트를 위해 장치를 잠급니다. |
|[isEnvironmentSecure](#isEnvironmentSecure) | 잠금 컨텍스트가 디바이스에 적용되는지 결정합니다. |
|[getDeviceInfo](#getDeviceInfo) | 테스트 응용 프로그램이 실행되는 플랫폼에 대한 세부 정보를 가져옵니다. |
|[examineProcessList](#examineProcessList)|실행 중인 사용자 및 시스템 프로세스의 목록을 가져옵니다.|
|[닫기](#close) | 브라우저를 닫고 디바이스의 잠금을 해제합니다. |
|[getPermissiveMode](#getPermissiveMode)|허용 모드가 켜져 있는지 아니면 꺼져 있는지 확인합니다.|
|[setPermissiveMode](#setPermissiveMode)|허용 모드를 켜거나 끕니다.|
|[emptyClipBoard](#emptyClipBoard)|시스템 클립보드를 지웁니다.|
|[getMACAddress](#getMACAddress)|디바이스의 MAC 주소 목록을 가져옵니다.|
|[getStartTime](#getStartTime) | 테스트 앱이 시작된 시간을 가져옵니다. |
|[getCapability](#getCapability) | 기능의 사용 여부를 쿼리합니다. |
|[setCapability](#setCapability)|지정된 기능을 사용하거나 사용하지 않도록 설정합니다.| 
|[isRemoteSession](#isRemoteSession) | 현재 세션이 원격으로 로그인되었는지 확인합니다. |
|[isVMSession](#isVMSession) | 현재 세션이 가상 컴퓨터에서 실행되고 있는지 확인합니다. |

---

<span id="lockDown"/>

### <a name="lockdown"></a>lockDown
장치를 잠급니다. 장치의 잠금을 해제하는 데도 사용됩니다. 테스트 웹 응용 프로그램은 이 호출을 수행한 후에 학생들이 테스트를 시작하도록 허용합니다. 구현자는 테스트 환경의 보안을 유지하는 데 필요한 모든 작업을 수행해야 합니다. 환경의 보안을 유지하기 위해 수행하는 단계는 장치별로 다릅니다. 예를 들어, 화면 캡처 비활성화, 보안 모드에서 음성 채팅 비활성화, 시스템 클립보드 지우기, 키오스크 모드로 전환, OSX 10.7+ 장치의 Spaces 비활성화 등이 있습니다. 테스트 응용 프로그램은 평가가 시작되기 전에 잠금을 활성화하고 학생이 평가를 완료하고 보안 테스트 밖으로 이동하면 잠금을 비활성화합니다.

**구문**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**매개 변수**  
* `enable` - 잠금 화면 위에서 시험 응시 앱을 실행하고 이 [문서](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)에 설명된 정책을 적용하는 **true**입니다. 앱이 잠겨 있지 않으면 잠금 화면 위에서 시험 응시의 실행을 중지하고 닫는 **false**입니다. 이 경우 영향을 주지 않습니다.  
* `onSuccess` - [선택 항목] 잠금이 성공적으로 활성화되거나 비활성화된 후 호출하는 함수입니다. `Function(Boolean currentlockdownstate)` 형식이어야 합니다.  
* `onError` - [선택 사항] 잠금 작업이 실패하는 경우 호출하는 함수입니다. `Function(Boolean currentlockdownstate)` 형식이어야 합니다.  

**요구 사항**  
Windows 10, 버전 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
잠금 컨텍스트가 디바이스에 적용되는지 결정합니다. 테스트 웹 응용 프로그램은 학생이 테스트를 시작하도록 허용하기 전에 및 테스트 중 주기적으로 이 함수를 호출합니다.

**구문**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**매개 변수**  
* `callback` - 이 함수가 완료되면 호출하는 함수입니다. `Function(String state)` 형식이어야 하며, `state`는 두 개의 필드가 들어 있는 JSON 문자열입니다. 첫 번째는 `secure` 필드이며, 필요한 모든 잠금이 활성화되어 있는 경우에만(또는 기능이 비활성화된 경우) `true`가 표시되어 안전한 테스트 환경을 보장하며, 앱이 잠금 모드에 들어간 이후 이 중 어떤 것도 손상되지 않았음을 보장합니다. 다른 필드 `messageKey`에는 공급 업체 관련 정보가 들어 있습니다. 이 필드의 용도는 공급 업체가 부울 `secure` 플래그를 늘리는 추가 정보를 입력하도록 하는 것입니다.

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**요구 사항**  
Windows 10, 버전 1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getDeviceInfo
테스트 응용 프로그램이 실행되는 플랫폼에 대한 세부 정보를 가져옵니다. 사용자 에이전트에서 인식할 수 있었던 모든 정보를 확대하는 데 사용됩니다.

**구문**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**매개 변수**  
* `callback` - 이 함수가 완료되면 호출하는 함수입니다. `Function(String infoObj)` 형식이어야 하며, `infoObj`는 여러 개의 필드가 들어 있는 JSON 문자열입니다. 다음 필드를 지원해야 합니다.
    * `os` 운영 체제 종류를 나타냅니다(예: Windows macOS, Linux, iOS, Android 등).
    * `name` OS 릴리스 이름을 나타냅니다(있는 경우, 예: Sierra, Ubuntu).
    * `version` OS 버전을 나타냅니다(예: 10.1, 10 Pro 등).
    * `brand` 보안 브라우저 브랜드를 나타냅니다(예: OAKS, CA, SmarterApp 등).
    * `model` 모바일 장치 전용 장치 모델을 나타냅니다(데스크톱 브라우저의 경우 null/사용 안 함).

**요구 사항**  
Windows 10, 버전 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
사용자가 소유한 클라이언트 컴퓨터에서 실행 중인 모든 프로세스 목록을 가져옵니다. 테스트 응용 프로그램은 이 함수를 호출하여 목록을 확인하고 테스트 주기 동안 예상 차단 프로세스 목록과 비교합니다. 이 호출은 평가 시작 시 및 학생이 평가를 수행하는 동안 주기적으로 호출되어야 합니다. 차단된 프로세스가 발견되면 테스트 무결성을 유지하기 위해 평가를 중지해야 합니다.

**구문**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**매개 변수**  
* `blacklistedProcessList` - 테스트 응용 프로그램이 차단한 프로세스 목록입니다.  
`callback` - 활성 프로세스가 발견되면 호출되는 함수입니다. `Function(String foundBlacklistedProcesses)` 형식이어야 하며, `foundBlacklistedProcesses`는  `"['process1.exe','process2.exe','processEtc.exe']"` 형식이어야 합니다. 차단된 프로세스가 없으면 비어 있게 됩니다. null인 경우, 원래 함수 호출에서 오류가 발생했음을 나타냅니다.

**설명** 시스템 프로세스는 이 목록에 포함되지 않습니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="close"/>

### <a name="close"></a>닫기
브라우저를 닫고 디바이스의 잠금을 해제합니다. 사용자가 브라우저를 종료하도록 선택하는 경우 테스트 응용 프로그램에서 이 함수를 호출해야 합니다.

**구문**  
`void SecureBrowser.security.close(restart);`

**매개 변수**  
* `restart` - 이 매개 변수는 무시되지만 제공되어야 합니다.

**설명** Windows 10 버전 1607에서는 초기에 장치가 잠겨 있어야 합니다. 이후 버전에서 이 방법은 디바이스가 잠겨 있는지 여부와 관계 없이 브라우저를 닫습니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
허용 모드가 켜져 있는지 아니면 꺼져 있는지 확인하려면 테스트 웹 응용 프로그램에서 이 함수를 호출해야 합니다. 허용 모드에서는 보안 브라우저에 보조 기술이 작동할 수 있도록 브라우저의 엄격한 보안 후크 중 일부가 해제 가능 상태가 됩니다. 예를 들어, 다른 응용 프로그램 UI가 브라우저 위에 표시되지 않도록 적극적으로 방어하는 브라우저의 경우 허용 모드에 있을 때 이를 해제할 수 있습니다. 

**구문**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**매개 변수**  
* `callback` - 이 함수가 완료되면 호출하는 함수입니다. `Function(Boolean permissiveMode)` 형식이어야 하며, `permissiveMode`는 브라우저가 현재 허용 모드에 있는지를 나타냅니다. 지정되지 않거나 null인 경우 가져오기 작업에 오류가 발생합니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
허용 모드를 켜기 또는 끄기로 전환하려면 테스트 웹 응용 프로그램에서 이 함수를 호출해야 합니다. 허용 모드에서는 보안 브라우저에 보조 기술이 작동할 수 있도록 브라우저의 엄격한 보안 후크 중 일부가 해제 가능 상태가 됩니다. 예를 들어, 다른 응용 프로그램 UI가 브라우저 위에 표시되지 않도록 적극적으로 방어하는 브라우저의 경우 허용 모드에 있을 때 이를 해제할 수 있습니다. 

**구문**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**매개 변수**  
* `enable` - 의도된 허용 모드 상태를 나타내는 부울 값입니다.  
* `callback` - 이 함수가 완료되면 호출하는 함수입니다. `Function(Boolean permissiveMode)` 형식이어야 하며, `permissiveMode`는 브라우저가 현재 허용 모드에 있는지를 나타냅니다. 지정되지 않거나 null인 경우 설정 작업에 오류가 발생합니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
시스템 클립보드를 지웁니다. 시스템 클립보드에 저장되어 있을 수 있는 데이터를 강제로 지우려면 테스트 응용 프로그램에서 이 함수를 호출해야 합니다. **[lockDown](#lockDown)** 함수도 이 작업을 수행합니다.

**구문**  
`void SecureBrowser.security.emptyClipBoard();`

**요구 사항**  
Windows 10, 버전 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
장치의 MAC 주소 목록을 가져옵니다. 진단을 보조하려면 테스트 응용 프로그램에서 이 함수를 호출해야 합니다. 

**구문**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**매개 변수**  
* `callback` - 이 함수가 완료되면 호출하는 함수입니다. `Function(String addressArray)` 형식이어야 하며, `addressArray`는  `"['00:11:22:33:44:55','etc']"` 형식이어야 합니다.

**설명**  
학교에서는 방화벽/NAT/프록시가 일반적으로 사용되므로 원본 IP 주소를 사용하여 테스트 서버 내 최종 사용자 컴퓨터 간을 구별하기는 어렵습니다. MAC 주소는 앱이 진단 용도로 일반적인 방화벽으로 보호되고 있는 최종 클라이언트 컴퓨터를 구별하도록 해줍니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
테스트 앱이 시작된 시간을 가져옵니다.

**구문**  
`DateTime SecureBrowser.settings.getStartTime();`

**반환 값**  
테스트가 시작된 시간을 나타내는 DateTime 개체입니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
기능의 사용 여부를 쿼리합니다. 

**구문**  
`Object SecureBrowser.security.getCapability(String feature)`

**매개 변수**  
`feature` - 쿼리할 기능을 결정하는 문자열입니다. 유효한 기능 문자열은 "screenMonitoring", "printing", "textSuggestions"입니다(대소문자 구문).

**반환 값**  
이 함수는 JavaScript 개체 또는 리터럴을 `{<feature>:true|false}` 형식으로 반환합니다. 쿼리된 기능을 활성화하는 경우 **true**, 기능을 사용하지 않거나 기능 문자열이 유효하지 않은 경우 **false**입니다.

**요구 사항** Windows 10 버전 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
브라우저에 특정 기능을 사용하도록 설정하거나 사용하지 않도록 설정합니다.

**구문**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**매개 변수**  
* `feature` - 설정할 기능을 결정하는 문자열입니다. 유효한 기능 문자열은 `"screenMonitoring"`, `"printing"` 및 `"textSuggestions"`입니다(대/소문자 구문).  
* `value` - 이 기능에 대해 의도된 설정입니다. `"true"`또는 `"false"`이어야 합니다.  
* `onSuccess` - [선택 사항] 설정 작업이 완료된 후 호출하는 함수입니다. `Function(String jsonValue)` 형식이어야 하며, *jsonValue*는 `{<feature>:true|false|undefined}` 형식이어야 합니다.  
* `onError` - [선택 사항] 설정 작업이 실패하는 경우 호출하는 함수입니다. `Function(String jsonValue)` 형식이어야 하며, *jsonValue*는 `{<feature>:true|false|undefined}` 형식이어야 합니다.

**설명**  
대상 기능이 브라우저에 알려지지 않은 기능인 경우, 이 함수에서 `undefined` 값을 전달하여 함수를 콜백합니다.

**요구 사항** Windows 10 버전 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
현재 세션이 원격으로 로그인되었는지 확인합니다.

**구문**  
`Boolean SecureBrowser.security.isRemoteSession();`

**반환 값**  
현재 세션이 원격인 경우 **true** 이며, 그렇지 않은 경우 **false**입니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
현재 세션이 가상 컴퓨터 내에서 실행되고 있는지 확인합니다.

**구문**  
`Boolean SecureBrowser.security.isVMSession();`

**반환 값**  
현재 세션이 가상 컴퓨터에서 실행되고 있으면 **true**, 그렇지 않으면 **false**입니다.

**설명**  
이 API 확인은 적절한 API를 구현하는 특정 하이퍼바이저에서 실행되고 있는 VM 세션만 검색할 수 있습니다.

**요구 사항**  
Windows 10, 버전 1709

---