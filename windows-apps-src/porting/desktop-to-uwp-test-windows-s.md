---
author: normesta
Description: Test your app for Windows 10 in S mode.
Search.Product: eADQiWindows 10XVcnh
title: Windows 10 S용 Windows 앱 테스트
ms.author: normesta
ms.date: 05/11/2017
ms.topic: article
keywords: windows 10 S, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3307506c5cf62d04cd19fbc302ad14bfcedd0045
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7432176"
---
# <a name="test-your-windows-app-for-windows-10-in-s-mode"></a>Windows 10 S 모드 Windows 앱 테스트

Windows 10 S 모드를 실행하는 디바이스에서 Windows 앱이 제대로 작동하는지 테스트할 수 있습니다. 사실, 앱을 Microsoft Store에 게시하려는 경우 Microsoft Store 요구 사항이기 때문에 이를 수행해야 합니다. 앱을 테스트하기 위해, Windows 10 Pro를 실행하는 장치에 Device Guard 코드 무결성 정책을 적용할 수 있습니다.

> [!NOTE]
> Device Guard 코드 무결성 정책을 적용하는 디바이스가 Windows 10 크리에이터스 에디션(10.0; 빌드 15063) 이상을 실행해야 합니다.

Device Guard 코드 무결성 정책은 Windows 10 S에서 실행을 위해 앱이 따라야 하는 규칙을 적용합니다.

> [!IMPORTANT]
>이러한 정책은 가상 컴퓨터에 적용할 것을 권장하지만, 로컬 컴퓨터에 적용하고 싶은 경우에는 정책을 적용하기 앞서 이 항목의 "그런 다음, 정책을 설치하고 컴퓨터를 다시 시작" 섹션에 나와 있는 모범 사례 지침을 참조하시기 바랍니다.

<a id="choose-policy" />

## <a name="first-download-the-policies-and-then-choose-one"></a>먼저, 정책을 다운로드하고 선택합니다.

[여기](https://go.microsoft.com/fwlink/?linkid=849018)에서 Device Guard 코드 무결성 정책을 다운로드합니다.

그런 다음, 가장 적합한 정책을 선택합니다. 다음은 각 정책의 요약 내용입니다.

|정책 |적용 |서명 인증서 |파일 이름 |
|--|--|--|--|
|감사 모드 정책 |문제를 기록하되, 차단하지 않습니다. |스토어 |SiPolicy_Audit.p7b |
|프로덕션 모드 정책 |예 |스토어 |SiPolicy_Enforced.p7b |
|자체 서명된 앱이 포함된 제품 모드 정책 |예 |AppX 테스트 인증서  |SiPolicy_DevModeEx_Enforced.p7b |

감사 모드 정책으로 시작하는 것이 좋습니다. 코드 무결성 이벤트 로그를 검토하고 앱을 조정하는 데 도움이 되도록 이 정보를 사용합니다. 그런 다음, 최종 테스트 준비가 완료되면 프로덕션 모드 정책을 적용합니다.

다음은 각 정책에 대한 몇 가지 추가 정보입니다.

### <a name="audit-mode-policy"></a>감사 모드 정책
이 모드에서는 Windows 10 S에서 지원되지 않는 작업을 수행하는 경우에도 앱이 실행됩니다. Windows는 코드 무결성 이벤트 로그로 차단된 모든 실행 파일을 기록합니다.

**이벤트 뷰어**를 열고, '응용 프로그램 및 서비스 로그->Microsoft->Windows->CodeIntegrity>Operational 위치를 검색해 로그를 찾을 수 있습니다.

![code-integrity-event-logs](images/desktop-to-uwp/code-integrity-logs.png)

이 모드는 안전하며 시스템 시작을 방해하지 않습니다.

#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>(선택 사항) 호출 스택에서 특정 실패 지점 찾기
차단 문제가 발생한 호출 스택에서 특정 지점을 찾으려면, 이 레지스트리 키를 추가한 후 [커널 모드 디버깅 환경](https://docs.microsoft.com/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging)을 설정합니다.

|키|이름|유형|값|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![reg-setting](images/desktop-to-uwp/ci-debug-setting.png)

### <a name="production-mode-policy"></a>프로덕션 모드 정책
이 정책은 Windows 10 S에서 실행을 시뮬레이션할 수 있도록 Windows 10 S에 맞는 코드 무결성 규칙을 적용합니다. 이는 엄격한 정책이기 때문에 최종 프로덕션 테스트에 적합합니다. 이 모드에서는 앱에 사용자의 장치에 적용되는 것과 동일한 제한이 적용됩니다. 이 모드를 사용하려면 Microsoft Store에서 앱에 서명해야 합니다.

### <a name="production-mode-policy-with-self-signed-apps"></a>자체 서명된 앱이 포함된 프로덕션 모드 정책
이 모드는 프로덕션 모드 정책과 유사하지만, 실행할 작업을 zip 파일에 포함된 테스트 인증서를 사용해 로그인할 수 있도록 허용합니다. 이 zip 파일의 **AppxTestRootAgency** 폴더에 포함된 PFX 파일을 설치합니다. 그런 다음 앱에 서명합니다. 이렇게 하면 스토어에 로그인하지 않고도 신속하게 ID를 반복할 수 있습니다.

인증서 게시자 이름이 앱 게시자 이름과 일치해야 하기 때문에, **ID** 요소의 **게시자** 속성 값을 일시적으로 "CN=Appx Test Root Agency Ex"로 변경합니다. 테스트를 완료한 후 속성을 원래 값으로 다시 변경할 수 있습니다.

## <a name="next-install-the-policy-and-restart-your-system"></a>그런 다음, 정책을 설치하고 시스템을 다시 시작

이 정책은 부팅 오류로 이어질 수 있기 때문에 가상 컴퓨터에 이러한 정책을 적용하는 것이 좋습니다. 이러한 정책들은 드라이버를 포함하여 Microsoft Store에 의해 서명되지 않은 코드의 실행을 차단하기 때문입니다.

이들 정책을 로컬 컴퓨터에 적용하려면 감사 모드 정책으로 시작하는 것이 가장 좋습니다. 이 정책에서는 코드 무결성 이벤트 로그를 검토해서 적용된 정책에서 중요한 기능이 차단되는 일이 없도록 할 수 있습니다.

정책을 적용할 준비가 끝나면 선택한 정책에 대한 .P7B 파일을 찾아서 이름을 **SIPolicy.P7B**로 변경한 다음, 파일을 시스템의 해당 위치 **C:\Windows\System32\CodeIntegrity\\**에 저장합니다.

그런 다음 컴퓨터를 다시 시작합니다.

>[!NOTE]
>시스템에서 정책을 제거하려면 .P7B 파일을 삭제한 다음 컴퓨터를 다시 시작합니다.

## <a name="next-steps"></a>다음 단계

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.

**저희 App Consult Team이 게시한 자세한 블로그 글을 참조하세요.**

[데스크톱 브리지로 전형적인 Windows 10 데스크톱 응용 프로그램을 포인팅 및 테스트](https://blogs.msdn.microsoft.com/appconsult/2017/06/15/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge/)를 참조하세요.

**Windows S 모드 테스트를 도와주는 도구 알아보기**

[APPX 패키지 풀기, 수정, 다시 패키징, 서명](https://blogs.msdn.microsoft.com/appconsult/2017/08/07/unpack-modify-repack-sign-appx/)을 참조하세요.
