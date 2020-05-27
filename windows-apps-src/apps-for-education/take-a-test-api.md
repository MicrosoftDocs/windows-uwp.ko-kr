---
Description: Microsoft에서 테스트 앱을 사용 하기 위한 JavaScript API를 통해 보안 평가를 수행할 수 있습니다. 테스트를 수행 하면 테스트 중에 학생이 다른 컴퓨터나 인터넷 리소스를 사용 하지 못하도록 하는 보안 브라우저가 제공 됩니다.
title: 테스트 JavaScript API를 사용 합니다.
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
keywords: windows 10, uwp, 교육
ms.localizationpriority: medium
ms.openlocfilehash: 3708252908c9f63bbb5070ef864b8418c857ac19
ms.sourcegitcommit: e51f9489d8c977c3498afb1a75c91f96ac3a642b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83854726"
---
# <a name="take-a-test-javascript-api"></a>테스트 JavaScript API를 사용 합니다.

[테스트 수행](https://docs.microsoft.com/education/windows/take-tests-in-windows-10) 은 한계가 테스트에 대 한 잠긴 온라인 평가를 렌더링 하는 브라우저 기반 UWP 앱으로, 교육자가 보안 테스트 환경을 제공 하는 방법이 아닌 평가 콘텐츠에 집중할 수 있게 해줍니다. 이를 위해 웹 응용 프로그램에서 활용할 수 있는 JavaScript API를 사용 합니다. 테스트 실행 API는 높은 한계가 일반 코어 테스트를 위한 [SBAC BROWSER api 표준을](https://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) 지원 합니다.

앱 자체에 대 한 자세한 내용은 [테스트 앱 사용 기술 참조](https://docs.microsoft.com/education/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) 를 참조 하세요. 문제 해결 도움말 [은 Microsoft의 이벤트 뷰어를 사용 하 여 테스트 수행](troubleshooting.md)을 참조 하세요.

## <a name="reference-documentation"></a>참조 설명서
다음 네임 스페이스에는 테스트 수행 Api가 있습니다. 모든 Api는 전역 개체에 따라 달라 집니다 `SecureBrowser` .

| 네임스페이스 | Description |
|-----------|-------------|
|[보안 네임 스페이스](#security-namespace)|테스트 환경을 테스트 하 고 적용 하기 위해 장치를 잠글 수 있는 Api를 포함 합니다. |

### <a name="security-namespace"></a>보안 네임 스페이스

보안 네임 스페이스를 사용 하 여 장치를 잠그고, 사용자 및 시스템 프로세스 목록을 확인 하 고, MAC 및 IP 주소를 가져오고, 캐시 된 웹 리소스를 지울 수 있습니다.

| 방법 | Description   |
|--------|---------------|
|[보안](#lockDown) | 테스트를 위해 장치를 잠급니다. |
|[Is환경 보안](#isEnvironmentSecure) | 잠금 컨텍스트가 장치에 계속 적용 되는지 여부를 결정 합니다. |
|[getDeviceInfo](#getDeviceInfo) | 테스트 응용 프로그램이 실행 되는 플랫폼에 대 한 세부 정보를 가져옵니다. |
|[examineProcessList](#examineProcessList)|실행 중인 사용자 및 시스템 프로세스 목록을 가져옵니다.|
|[닫습니다](#close) | 브라우저를 닫고 장치의 잠금을 해제 합니다. |
|[getPermissiveMode](#getPermissiveMode)|허용 모드가 설정 되어 있는지 여부를 확인 합니다.|
|[setPermissiveMode](#setPermissiveMode)|허용 모드를 설정 하거나 해제 합니다.|
|[emptyClipBoard](#emptyClipBoard)|시스템 클립보드를 지웁니다.|
|[getMACAddress만](#getMACAddress)|장치의 MAC 주소 목록을 가져옵니다.|
|[getStartTime](#getStartTime) | 테스트 앱이 시작 된 시간을 가져옵니다. |
|[getCapability](#getCapability) | 기능이 사용 되는지 여부를 쿼리 합니다. |
|[setCapability](#setCapability)|지정 된 기능을 사용 하거나 사용 하지 않도록 설정 합니다.| 
|[isRemoteSession](#isRemoteSession) | 현재 세션이 원격으로 로그인 했는지 확인 합니다. |
|[isVMSession](#isVMSession) | 현재 세션이 가상 컴퓨터에서 실행 되 고 있는지 확인 합니다. |

---

<span id="lockDown"/>

### <a name="lockdown"></a>보안
장치를 잠급니다. 장치를 잠금 해제 하는 데도 사용 됩니다. 테스트 웹 응용 프로그램은 학생이 테스트를 시작 하도록 허용 하기 전에이 호출을 호출 합니다. 구현자는 테스트 환경을 보호 하는 데 필요한 모든 작업을 수행 하는 데 필요 합니다. 환경을 보호 하기 위해 수행 하는 단계는 장치에 따라 다릅니다. 예를 들어 화면 캡처 사용 안 함, 보안 모드에서 음성 채팅 사용 안 함, 시스템 클립보드 지우기, 키오스크 모드로 전환, OSX 10.7 + 장치에서 공백 사용 안 함 등의 측면이 있습니다. 테스트 응용 프로그램은 평가 시작 됩니다 이전에 잠금을 사용 하도록 설정 하 고 학생이 평가를 완료 하 고 보안 테스트를 하지 않을 때 잠금을 사용 하지 않도록 설정 합니다.

**구문**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**매개 변수**  
* `enable` - 잠금 화면 위에서 테스트 후 앱을 실행 하 고이 [문서](https://docs.microsoft.com/education/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)에서 설명 하는 정책을 적용 하려면 **true로 설정** 합니다. **false로 설정** 하면 잠금 화면 위에서 테스트 실행이 중지 되 고 앱이 잠기지 않은 경우 닫힙니다. 이 경우에는 아무 효과가 없습니다.  
* `onSuccess`-[선택 사항] 잠금을 사용 하거나 사용 하지 않도록 설정 하 고 나면 호출할 함수입니다. 형식 이어야 합니다 `Function(Boolean currentlockdownstate)` .  
* `onError`-[선택 사항] 잠금 작업이 실패 한 경우 호출할 함수입니다. 형식 이어야 합니다 `Function(Boolean currentlockdownstate)` .  

**요구 사항**  
Windows 10, 버전 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>Is환경 보안
잠금 컨텍스트가 장치에 계속 적용 되는지 여부를 결정 합니다. 테스트 웹 응용 프로그램은 학생 들이 테스트 내부에서 테스트를 시작 하 고 주기적으로 테스트를 시작 하도록 허용 하기 전에이를 호출 합니다.

**구문**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**매개 변수**  
* `callback`-이 함수가 완료 될 때 호출할 함수입니다. 형식 이어야 합니다 `Function(String state)` `state` . 여기서은 두 필드를 포함 하는 JSON 문자열입니다. 첫 번째는 `secure` 필드입니다 .이 필드는 `true` 보안 테스트 환경을 사용 하도록 설정 하는 데 필요한 모든 잠금이 설정 된 경우에만 표시 되 고, 앱이 잠금 모드로 전환 된 후에는 이러한 기능이 모두 손상 된 경우에만 표시 됩니다. 다른 필드인에는 `messageKey` 공급 업체별 기타 세부 정보 또는 정보가 포함 되어 있습니다. 여기서의 목적은 공급 업체에서 부울 플래그를 보강 하는 추가 정보를 입력할 수 있도록 하는 것입니다 `secure` .

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
테스트 응용 프로그램이 실행 되는 플랫폼에 대 한 세부 정보를 가져옵니다. 사용자 에이전트에서 뚜렷한 된 정보를 보강 하는 데 사용 됩니다.

**구문**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**매개 변수**  
* `callback`-이 함수가 완료 될 때 호출할 함수입니다. 형식 이어야 합니다 `Function(String infoObj)` `infoObj` . 여기서는 여러 필드를 포함 하는 JSON 문자열입니다. 다음 필드를 지원 해야 합니다.
    * `os`OS 유형 (예: Windows, macOS, Linux, iOS, Android 등)을 나타냅니다.
    * `name`OS 릴리스 이름 (있는 경우)을 나타냅니다 (예: 시에라리온, Ubuntu).
    * `version`OS 버전을 나타냅니다 (예: 10.1, 10 Pro 등).
    * `brand`보안 브라우저 브랜딩을 나타냅니다 (예: OAKS, CA, SmarterApp 등).
    * `model`모바일 장치에 대 한 장치 모델만 나타냅니다. 데스크톱 브라우저의 경우 null/사용 되지 않습니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
사용자가 소유한 클라이언트 컴퓨터에서 실행 중인 모든 프로세스 목록을 가져옵니다. 테스트 응용 프로그램은이를 호출 하 여 목록을 검사 하 고 테스트 주기 중에 블랙 리스트에 된 것으로 간주 된 프로세스 목록과 비교 합니다. 이 호출은 평가를 시작할 때와 학생 들이 평가를 수행 하는 동안 주기적으로 호출 해야 합니다. 블랙 리스트에 프로세스가 검색 되 면 테스트 무결성을 유지 하기 위해 평가를 중지 해야 합니다.

**구문**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**매개 변수**  
* `blacklistedProcessList`-테스트 응용 프로그램에 블랙 리스트에 된 프로세스의 목록입니다.  
`callback`-활성 프로세스를 찾은 후 호출할 함수입니다. 형식 이어야 합니다. `Function(String foundBlacklistedProcesses)` 여기서 `foundBlacklistedProcesses` 는 형식 `"['process1.exe','process2.exe','processEtc.exe']"` 입니다. 블랙 리스트에 프로세스를 찾을 수 없는 경우 비어 있습니다. Null 인 경우에는 원래 함수 호출에서 오류가 발생 했음을 나타냅니다.

**설명** 이 목록에는 시스템 프로세스가 포함 되지 않습니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="close"/>

### <a name="close"></a>닫기
브라우저를 닫고 장치의 잠금을 해제 합니다. 사용자가 브라우저를 종료 하면 테스트 응용 프로그램에서이를 호출 해야 합니다.

**구문**  
`void SecureBrowser.security.close(restart);`

**매개 변수**  
* `restart`-이 매개 변수는 무시 되지만 제공 되어야 합니다.

**설명** Windows 10 버전 1607에서 장치는 처음에 잠가야 합니다. 이후 버전에서이 메서드는 장치가 잠겨 있는지 여부에 관계 없이 브라우저를 닫습니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
테스트 웹 응용 프로그램은이를 호출 하 여 관대 한 모드가 설정 되어 있는지 여부를 확인 해야 합니다. 관대 한 모드에서 브라우저는 보안 브라우저에서 보조 기술을 사용할 수 있도록 엄격한 보안 후크 중 일부를 완화할 것으로 예상 합니다. 예를 들어, 다른 응용 프로그램 Ui를 가장 먼저 표시 하지 않도록 하는 브라우저는 허용 모드에서이를 완화 하는 것이 좋습니다. 

**구문**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**매개 변수**  
* `callback`-이 호출이 완료 될 때 호출할 함수입니다. 형식 이어야 합니다. `Function(Boolean permissiveMode)` 여기서는 `permissiveMode` 브라우저가 현재 허용 모드 인지 여부를 나타냅니다. 정의 되지 않았거나 null 이면 가져오기 작업에서 오류가 발생 한 것입니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
테스트 웹 응용 프로그램은이를 호출 하 여 허용 모드를 설정 하거나 해제 해야 합니다. 관대 한 모드에서 브라우저는 보안 브라우저에서 보조 기술을 사용할 수 있도록 엄격한 보안 후크 중 일부를 완화할 것으로 예상 합니다. 예를 들어, 다른 응용 프로그램 Ui를 가장 먼저 표시 하지 않도록 하는 브라우저는 허용 모드에서이를 완화 하는 것이 좋습니다. 

**구문**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**매개 변수**  
* `enable`-의도 된 허용 모드 상태를 나타내는 부울 값입니다.  
* `callback`-이 호출이 완료 될 때 호출할 함수입니다. 형식 이어야 합니다. `Function(Boolean permissiveMode)` 여기서는 `permissiveMode` 브라우저가 현재 허용 모드 인지 여부를 나타냅니다. 정의 되지 않았거나 null 이면 설정 작업에서 오류가 발생 한 것입니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
시스템 클립보드를 지웁니다. 테스트 응용 프로그램은이를 호출 하 여 시스템 클립보드에 저장 될 수 있는 모든 데이터를 강제로 지워야 합니다. **[잠금](#lockDown)** 기능도이 작업을 수행 합니다.

**구문**  
`void SecureBrowser.security.emptyClipBoard();`

**요구 사항**  
Windows 10, 버전 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress만
장치의 MAC 주소 목록을 가져옵니다. 진단에 도움이 되도록 테스트 응용 프로그램에서이를 호출 해야 합니다. 

**구문**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**매개 변수**  
* `callback`-이 호출이 완료 될 때 호출할 함수입니다. 형식 이어야 합니다. `Function(String addressArray)` 여기서 `addressArray` 는 형식 `"['00:11:22:33:44:55','etc']"` 입니다.

**주의**  
방화벽/p s/프록시가 학교에서 일반적으로 사용 되기 때문에 원본 IP 주소를 사용 하 여 테스트 서버 내에서 최종 사용자 컴퓨터를 구분 하는 것은 어렵습니다. MAC 주소를 사용 하면 응용 프로그램에서 진단 목적을 위해 최종 클라이언트 컴퓨터를 일반 방화벽 뒤에 구분할 수 있습니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
테스트 앱이 시작 된 시간을 가져옵니다.

**구문**  
`DateTime SecureBrowser.security.getStartTime();`

**돌려**  
테스트 앱이 시작 된 시간을 나타내는 DateTime 개체입니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
기능이 사용 되는지 여부를 쿼리 합니다. 

**구문**  
`Object SecureBrowser.security.getCapability(String feature)`

**매개 변수**  
`feature`-쿼리할 기능을 결정 하는 문자열입니다. 유효한 기능 문자열은 "screenMonitoring", "인쇄" 및 "textSuggestions" (대/소문자 구분 안 함)입니다.

**반환 값**  
이 함수는 형식의 JavaScript 개체나 리터럴을 반환 `{<feature>:true|false}` 합니다. 쿼리 된 기능을 사용할 수 있으면 **true** 이 고, 기능을 사용할 수 없거나 기능 문자열이 잘못 된 경우 **false** 입니다.

**요구 사항** Windows 10, 버전 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
브라우저에서 특정 기능을 사용 하거나 사용 하지 않도록 설정 합니다.

**구문**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**매개 변수**  
* `feature`-설정할 기능을 결정 하는 문자열입니다. 유효한 기능 문자열은 `"screenMonitoring"` , `"printing"` 및 `"textSuggestions"` (대/소문자 구분 안 함)입니다.  
* `value`-기능에 대 한 의도 된 설정입니다. 또는 중 하나 여야 `"true"` 합니다 `"false"` .  
* `onSuccess`-[선택 사항] set 작업이 성공적으로 완료 된 후 호출할 함수입니다. `Function(String jsonValue)` *JsonValue* 가 형식의 형식 이어야 합니다 `{<feature>:true|false|undefined}` .  
* `onError`-[선택 사항] 설정 작업이 실패 한 경우 호출할 함수입니다. `Function(String jsonValue)` *JsonValue* 가 형식의 형식 이어야 합니다 `{<feature>:true|false|undefined}` .

**주의**  
대상 기능을 브라우저에서 알 수 없는 경우이 함수는 값을 `undefined` 콜백 함수에 전달 합니다.

**요구 사항** Windows 10, 버전 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
현재 세션이 원격으로 로그인 했는지 확인 합니다.

**구문**  
`Boolean SecureBrowser.security.isRemoteSession();`

**반환 값**  
현재 세션이 원격 이면 **true** 이 고, 그렇지 않으면 **false**입니다.

**요구 사항**  
Windows 10, 버전 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
현재 세션이 가상 컴퓨터 내에서 실행 되 고 있는지 확인 합니다.

**구문**  
`Boolean SecureBrowser.security.isVMSession();`

**반환 값**  
현재 세션이 가상 컴퓨터에서 실행 되 고 있으면 **true** 이 고, 그렇지 않으면 **false**입니다.

**주의**  
이 API 검사는 적절 한 Api를 구현 하는 특정 하이퍼바이저에서 실행 중인 VM 세션만 검색할 수 있습니다.

**요구 사항**  
Windows 10, 버전 1709

---
