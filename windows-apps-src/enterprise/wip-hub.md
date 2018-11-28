---
Description: This is a hub topic covering the full developer picture of how Windows Information Protection (WIP) relates to files, buffers, clipboard, networking, background tasks, and data protection under lock.
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: WIP(Windows Information Protection)
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Windows Information Protection, 엔터프라이즈 데이터, 엔터프라이즈 데이터 보호, edp, 인식 앱
ms.assetid: 08f0cfad-f15d-46f7-ae7c-824a8b1c44ea
ms.localizationpriority: medium
ms.openlocfilehash: b65da20c8931f74800f817ecba0139b14d0447ad
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7846310"
---
# <a name="windows-information-protection-wip"></a>WIP(Windows Information Protection)

__참고__ WIP(Windows Information Protection) 정책을 Windows10 버전 1607에 적용할 수 있습니다.

WIP는 조직에서 정의한 정책을 적용하여 조직에 속한 데이터를 보호합니다. 앱이 이러한 정책에 포함된 경우 앱에 의해 생성된 모든 데이터에는 정책 제한이 적용됩니다. 이 항목은 사용자의 개인 데이터에 영향을 주지 않고 더욱 원활하게 이러한 정책을 적용하는 앱을 빌드하는 데 도움이 됩니다.
<iframe src="https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Securing-Enterprise-Data-with-Windows-Information-Protection/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="first-what-is-wip"></a>WIP란?

WIP는 조직의 MDM(모바일 디바이스 관리) 및 MAM(모바일 응용 프로그램 관리) 시스템을 지원하는 데스크톱, 랩톱, 태블릿 및 휴대폰의 기능 모음입니다.

WIP와 MDM을 함께 사용하면 조직이 관리하는 디바이스에서 데이터가 처리되는 방식을 훨씬 잘 제어할 수 있습니다. 사용자가 회사로 디바이스를 가지고 와서 조직의 MDM에 등록하지 않는 경우가 있습니다.  이 경우 조직에서 MAM을 사용하면 사용자가 디바이스에 설치하는 특정 기간 업무 앱에서 데이터가 처리되는 방식을 훨씬 잘 제어할 수 있습니다.

관리자는 MDM 또는 MAM을 사용하여 조직에 속한 파일에 액세스할 수 있는 앱을 식별할 수 있으며, 사용자가 해당 파일에서 데이터를 복사한 다음 그 데이터를 개인 문서에 붙여 넣을 수 있는지 식별할 수 있습니다.

작동 방식은 다음과 같습니다. 사용자는 조직의 MDM(모바일 디바이스 관리) 시스템에 디바이스를 등록합니다. 관리 조직에서 관리자는 Microsoft Intune 또는 SCCM(System Center Configuration Manager)을 사용하여 정책을 정의한 다음 등록된 디바이스에 배포합니다.

사용자가 디바이스를 등록할 필요가 없는 경우에는 관리자가 MAM 시스템을 사용하여 특정 앱에 적용되는 정책을 정의하고 배포합니다. 사용자는 이러한 앱을 설치할 때 관련 정책을 수신하게 됩니다.

해당 정책은 엔터프라이즈 데이터에 액세스할 수 있는 앱을 식별합니다(정책의 *허용 목록*이라고 함). 이러한 앱은 보호된 엔터프라이즈 파일, VPN(가상 사설망) 및 클립보드에 있거나 공유 계약을 통한 엔터프라이즈 데이터에 액세스할 수 있습니다. 또한 정책은 데이터를 제어하는 규칙을 정의합니다. 예를 들어 엔터프라이즈 소유 파일에서 데이터를 복사한 다음 엔터프라이즈 소유가 아닌 파일에 붙여넣을 수 있는지 여부입니다.

사용자가 조직의 MDM 시스템에서 디바이스 등록을 해제하거나 조직의 MAM 시스템에서 식별된 앱을 제거할 경우 관리자는 디바이스에서 엔터프라이즈 데이터를 원격으로 지울 수 있습니다.

![WIP 수명 주기](images/wip-lifecycle.png)

> **WIP에 대해 자세히 알아보기** <br>
* [Windows Information Protection 소개](https://blogs.technet.microsoft.com/windowsitpro/2016/06/29/introducing-windows-information-protection/)
* [WIP(Windows Information Protection)를 사용하여 엔터프라이즈 데이터 보호](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)

앱이 허용 목록에 있는 경우 앱에 의해 생성된 모든 데이터에는 정책 제한이 적용됩니다. 즉, 관리자가 엔터프라이즈 데이터에 대한 사용자의 액세스를 해지하면 해당 사용자는 앱에서 생성한 모든 데이터에 액세스할 수 없습니다.

엔터프라이즈 용도로만 설계된 앱일 경우에 좋습니다. 사용자의 개인적인 데이터가 생성될 경우 앱이 엔터프라이즈와 개인 데이터를 지능적으로 구별하여 *인식*할 수 있으면 좋을 것입니다. 이러한 앱은 사용자 개인 데이터의 무결성을 유지하면서 엔터프라이즈 정책을 적절하게 적용할 수 있기 때문에 이런 유형의 앱을 *엔터프라이즈 인식*이라고 합니다.

## <a name="create-an-enterprise-enlightened-app"></a>엔터프라이즈 인식 앱 만들기

WIP API를 사용하여 앱을 지원한 다음 엔터프라이즈 인식 앱으로 선언합니다.

조직 및 개인 용도로 모두 사용되는 경우 앱을 인식합니다.

정책 요소의 적용을 적절하게 처리하려는 경우 앱을 인식합니다.

예를 들어 정책에서 사용자가 개인 문서에 엔터프라이즈 데이터를 붙여넣는 것을 허용할 경우에는 사용자가 데이터를 붙여넣기 전에 동의 대화 상자에 응답할 필요가 없습니다. 마찬가지로, 이러한 종류의 이벤트에 대한 응답에 사용자 지정 정보 대화 상자를 표시할 수 있습니다.

앱을 인식할 준비가 되면 다음 가이드 중 하나를 참조하세요.

**C#을 사용 하 여 빌드하는 유니버설 Windows 플랫폼 (UWP) 앱에 대 한**

[WIP(Windows Information Protection) 개발자 가이드](wip-dev-guide.md).

**C++를 사용하여 빌드하는 데스크톱 앱의 경우**

[WIP(Windows Information Protection) 개발자 가이드(C++)](http://go.microsoft.com/fwlink/?LinkId=822192).


## <a name="create-non-enlightened-enterprise-app"></a>미인식 엔터프라이즈 앱 만들기

개인용이 아닌 LOB(기간 업무) 앱을 개발하는 경우에는 앱을 인식할 필요가 없습니다.

### <a name="windows-desktop-apps"></a>Windows 데스크톱 앱
Windows 데스크톱 앱을 인식할 필요는 없지만 앱이 정책에 따라 올바르게 작동하는지 테스트를 통해 확인해야 합니다. 예를 들어 앱을 시작하고 사용한 후 MDM에서 디바이스의 등록을 해제합니다. 그런 다음 앱이 다시 시작되는지 확인합니다. 앱 작동에 필수적인 파일이 암호화되면 앱이 시작되지 않습니다. 또한 앱이 상호 작용하는 파일을 검토하여 앱이 사용자의 개인적인 파일을 실수로 암호화하지 않도록 하세요. 여기에는 메타데이터 파일, 이미지 등이 포함됩니다.

앱을 테스트한 후 리소스 파일이나 프로젝트에 이 플래그를 추가한 후 앱을 다시 컴파일하세요.

```cpp
MICROSOFTEDPAUTOPROTECTIONALLOWEDAPPINFO EDPAUTOPROTECTIONALLOWEDAPPINFOID
BEGIN
    0x0001
END
```
MDM 정책은 플래그를 요구하지 않지만 MAM 정책은 플래그를 요구합니다.

### <a name="uwp-apps"></a>UWP 앱

앱이 MAM 정책에 포함되기를 바란다면 앱을 인식해야 합니다. MDM하에서 디바이스에 배포되는 정책은 이를 요구하지 않겠지만 조직의 소비자에게 앱을 배포할 경우 어떤 유형의 정책 관리 시스템을 사용할지 파악하기 어렵습니다. 앱이 두 가지 유형의 정책 관리 시스템(MDM과 MAM) 모두에서 작동하도록 하려면 앱을 인식해야 합니다.






 
