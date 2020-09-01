---
Description: WIP(Windows Information Protection)가 파일, 버퍼, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 나타내는 허브 항목입니다.
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: WIP(Windows Information Protection)
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Windows Information Protection, 엔터프라이즈 데이터, 엔터프라이즈 데이터 보호, edp, 지원 apps
ms.assetid: 08f0cfad-f15d-46f7-ae7c-824a8b1c44ea
ms.localizationpriority: medium
ms.openlocfilehash: 69bab48836d7679d8bcec5f9132bca88d7607cdb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173247"
---
# <a name="windows-information-protection-wip"></a>WIP(Windows Information Protection)

__참고__ Windows Information Protection (WIP) 정책은 Windows 10 버전 1607에 적용 될 수 있습니다.

WIP는 조직에서 정의한 정책을 적용 하 여 조직에 속한 데이터를 보호 합니다. 앱이 해당 정책에 포함 된 경우 앱에서 생성 되는 모든 데이터에 정책 제한이 적용 됩니다. 이 항목은 사용자의 개인 데이터에 영향을 주지 않고 이러한 정책을 보다 적절 하 게 적용 하는 앱을 빌드하는 데 도움이 됩니다.
<iframe src="https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Securing-Enterprise-Data-with-Windows-Information-Protection/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="first-what-is-wip"></a>먼저 WIP 란 무엇 인가요?

WIP는 조직의 MDM (모바일 장치 관리) 및 MAM (모바일 응용 프로그램 관리) 시스템을 지 원하는 데스크톱, 랩톱, 태블릿 및 휴대폰의 기능 집합입니다.

WIP를 MDM과 함께 사용 하면 조직에서 관리 하는 장치에서 데이터가 처리 되는 방식을 더 잘 제어할 수 있습니다. 사용자가 장치를 회사로 가져와서 조직의 MDM에 등록 하지 않는 경우가 있습니다.  이러한 경우, 조직은 MAM을 사용 하 여 사용자가 장치에 설치 하는 특정 lob (기간 업무) 앱에서 데이터를 처리 하는 방법을 보다 효과적으로 제어할 수 있습니다.

관리자는 MDM 또는 MAM을 사용 하 여 조직에 속한 파일에 액세스할 수 있는 앱과 사용자가 해당 파일에서 데이터를 복사 하 여 개인 문서에 붙여 넣을 수 있는지 여부를 식별할 수 있습니다.

작동 방식은 다음과 같습니다. 사용자는 조직의 MDM (모바일 장치 관리) 시스템에 장치를 등록 합니다. 관리 조직의 관리자는 SCCM (Microsoft Intune 또는 System Center Configuration Manager)을 사용 하 여 등록 된 장치에 정책을 정의 하 고 배포 합니다.

사용자가 장치를 등록 하지 않아도 되는 경우 관리자는 해당 MAM 시스템을 사용 하 여 특정 앱에 적용 되는 정책을 정의 하 고 배포 합니다. 사용자가 이러한 앱을 설치 하면 연결 된 정책을 받게 됩니다.

이 정책은 엔터프라이즈 데이터에 액세스할 수 있는 앱 (정책의 *허용 목록*이라고 함)을 식별 합니다. 이러한 앱은 클립보드의 엔터프라이즈 보호 된 파일, VPN (가상 사설망) 및 엔터프라이즈 데이터에 액세스할 수 있습니다. 또한이 정책은 데이터를 제어 하는 규칙을 정의 합니다. 예를 들어 enterprise 소유의 파일에서 데이터를 복사한 다음 enterprise 소유의 파일이 아닌 파일에 붙여 넣을 수 있습니다.

사용자가 조직의 MDM 시스템에서 장치 등록을 취소 하거나 조직 MAM 시스템에서 식별 된 앱을 제거 하는 경우 관리자는 장치에서 엔터프라이즈 데이터를 원격으로 초기화할 수 있습니다.

![Wip 수명 주기](images/wip-lifecycle.png)

> **WIP에 대해 자세히 읽기** <br>
* [Windows Information Protection 소개](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/bg-p/Windows10Blog)
* [WIP(Windows Information Protection)를 사용하여 엔터프라이즈 데이터 보호](/windows/whats-new/edp-whats-new-overview)

앱이 허용 되는 목록에 있는 경우 앱에 의해 생성 된 모든 데이터에 정책 제한이 적용 됩니다. 즉, 관리자가 엔터프라이즈 데이터에 대 한 사용자의 액세스 권한을 취소 하면 해당 사용자는 앱이 생성 한 모든 데이터에 액세스할 수 없게 됩니다.

앱이 기업용 으로만 설계 된 경우에는이 방법을 사용할 수 없습니다. 그러나 앱이 사용자에 게 개인 개인 데이터를 만드는 경우에는 엔터프라이즈와 개인 데이터를 지능적으로 파악 하도록 *앱을 작성 하는* 것이 좋습니다. 사용자의 개인 데이터에 대 한 무결성을 유지 하면서 엔터프라이즈 정책을 정상적으로 적용할 수 있으므로이 유형의 앱을 *엔터프라이즈 지원* 라고 합니다.

## <a name="create-an-enterprise-enlightened-app"></a>엔터프라이즈 지원 앱 만들기

WIP Api를 사용 하 여 앱을 지원 한 다음 앱을 엔터프라이즈급으로 선언 합니다.

조직 및 개인용 용도에 모두 사용 되는 경우 앱을 사용 합니다.

정책 요소의 적용을 정상적으로 처리 하려면 앱을 선택 합니다.

예를 들어 정책을 통해 사용자가 엔터프라이즈 데이터를 개인 문서에 붙여 넣을 수 있는 경우 사용자가 데이터를 붙여넣기 전에 동의 대화 상자에 응답 하지 않도록 방지할 수 있습니다. 마찬가지로 이러한 종류의 이벤트에 대 한 응답으로 사용자 지정 정보 대화 상자를 표시할 수 있습니다.

앱을 사용할 준비가 되 면 다음 가이드 중 하나를 참조 하세요.

**C를 사용 하 여 빌드한 UWP (유니버설 Windows 플랫폼) 앱 #**

[Windows Information Protection (WIP) 개발자 가이드를 참조](wip-dev-guide.md)하십시오.

**C + +를 사용 하 여 빌드하는 데스크톱 앱의 경우**

[Windows Information Protection (WIP) 개발자 가이드 (c + +)](/previous-versions/windows/desktop/EDP/wip-developer-guide)


## <a name="create-non-enlightened-enterprise-app"></a>비 지원 엔터프라이즈 앱 만들기

개인적으로 사용 하기에 적합 하지 않은 LOB (기간 업무) 앱을 만드는 경우이를 사용 하지 않아도 됩니다.

### <a name="windows-desktop-apps"></a>Windows 데스크톱 앱
Windows 데스크톱 앱을 사용할 필요는 없지만 정책에서 제대로 작동 하는지 확인 하려면 앱을 테스트 해야 합니다. 예를 들어 앱을 시작 하 고 사용한 다음 MDM에서 장치 등록을 취소 합니다. 그런 다음 앱을 다시 시작할 수 있는지 확인 합니다. 앱의 작업에 중요 한 파일이 암호화 되어 있으면 앱이 시작 되지 않을 수 있습니다. 또한 앱이 사용자에 게 개인 인 파일을 실수로 암호화 하지 않도록 앱이 상호 작용 하는 파일을 검토 합니다. 여기에는 메타 데이터 파일, 이미지 및 기타 항목이 포함 될 수 있습니다.

앱을 테스트 한 후이 플래그를 리소스 파일이 나 프로젝트에 추가 하 고 앱을 다시 컴파일합니다.

```cpp
MICROSOFTEDPAUTOPROTECTIONALLOWEDAPPINFO EDPAUTOPROTECTIONALLOWEDAPPINFOID
BEGIN
    0x0001
END
```
MDM 정책에는 플래그가 필요 하지 않지만 MAM 정책은이를 수행 합니다.

### <a name="uwp-apps"></a>UWP 앱

앱이 MAM 정책에 포함 되는 것으로 간주 되는 경우이를 밝게 해야 합니다. MDM의 장치에 배포 되는 정책에는이 기능이 필요 하지 않지만, 조직 소비자에 게 앱을 배포 하는 경우 사용할 정책 관리 시스템의 유형을 결정할 수 없는 경우에는 어렵습니다. 앱이 두 가지 유형의 정책 관리 시스템 (MDM 및 MAM)에서 작동 하는지 확인 하려면 앱을 밝게 지정 해야 합니다.






 