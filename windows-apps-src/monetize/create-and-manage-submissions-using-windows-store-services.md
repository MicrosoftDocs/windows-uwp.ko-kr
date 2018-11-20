---
author: Xansky
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Microsoft Store 제출 API를 사용 하 여 프로그래밍 방식으로 만들고 파트너 센터 계정에 등록 된 앱 제출을 관리 합니다.
title: 제출 만들기 및 관리
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API
ms.localizationpriority: medium
ms.openlocfilehash: c91c7b42642df9a03aab1324f074799b63157e62
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7421191"
---
# <a name="create-and-manage-submissions"></a>제출 만들기 및 관리


*Microsoft Store 제출 API* 사용 하 여 프로그래밍 방식으로 쿼리하고 앱, 추가 기능 및 사용자 또는 사용자 조직의 파트너 센터 계정에 대 한 패키지 플라이트에 대 한 제출을 만듭니다. 이 API는 계정에서 많은 앱 또는 추가 기능을 관리하고 이러한 자산의 제출 프로세스를 자동화 및 최적화하려는 경우 유용합니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

다음 단계에서는 Microsoft Store 제출 API를 사용하는 종단 간 프로세스를 설명합니다.

1.  [필수 조건](#prerequisites)을 모두 완료했는지 확인합니다.
3.  Microsoft Store 제출 API에서 메서드를 호출하기 전에 [Azure AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후 만료되기 전에 이 토큰을 Microsoft Store 제출 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.
4.  [Microsoft Store 제출 API 호출](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> 이 API를 사용 하 여 앱, 패키지 플라이트 또는 추가 기능에 대 한 제출을 생성 하는 경우에 파트너 센터에 있는 것이 아니라만 API를 사용 하 여 제출을 추가 변경 하려면 해야 합니다. 파트너 센터를 사용 하 여 원래 API를 사용 하 여 만든 제출을 변경을 변경 하거나 해당 제출 API를 사용 하 여 커밋할 수 없게 됩니다. 경우에 따라 제출이 제출 프로세스를 더 이상 진행할 수 없는 오류 상태로 남을 수 있습니다. 이러한 문제가 발생하는 경우, 해당 제출을 삭제하고 새 제출을 생성해야 합니다.

> [!IMPORTANT]
> [비즈니스용 Microsoft Store 및 교육용 Microsoft Store를 통해 대량 구매](../publish/organizational-licensing.md)하기 위한 제출을 게시하거나 [LOB 앱](../publish/distribute-lob-apps-to-enterprises.md)에 대한 제출을 기업에 직접 게시하는 데 이 API를 사용할 수 없습니다. 이러한 시나리오 모두 사용 해야 파트너 센터에서 제출을 게시 합니다.

> [!NOTE]
> 필수 앱 업데이트 및 Microsoft Store 관리 소모성 추가 기능을 사용하는 앱 또는 추가 기능에 이 API를 사용할 수 없습니다. 이러한 기능 중 하나를 사용하는 앱 또는 추가 기능에서 Microsoft Store 제출 API를 사용하는 경우 이 API는 409 오류 코드를 반환합니다. 이 경우 앱 또는 추가 기능에 대 한 제출을 관리 하려면 파트너 센터를 사용 해야 합니다.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>1단계: Microsoft Store 제출 API를 사용하기 위한 필수 조건 완료

Microsoft Store 제출 API를 호출하는 코드 작성을 시작하기 전에 다음 필수 조건을 완료했는지 확인합니다.

* 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](http://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다. 그렇지 않으면 추가 요금 없이 [파트너 센터에서 Azure AD를 새로 만들](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) 수 있습니다.

* [파트너 센터 계정과 Azure AD 응용 프로그램 연결](#associate-an-azure-ad-application-with-your-windows-dev-center-account) 해야 하 고 ID, 클라이언트 ID 및 키 테 넌 트를 가져와야 합니다. 이러한 값은 Microsoft Store 제출 API에 대한 호출에 사용하는 Azure AD 액세스 토큰을 가져오는 데 필요합니다.

* Microsoft Store 제출 API에서 사용하도록 앱을 준비합니다.

  * 파트너 센터에서 앱 아직 존재 하지 않는 경우 [파트너 센터에서 해당 이름을 예약 하 여 앱 만들기](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name)해야 합니다. Microsoft Store 제출 API를 사용 하 여; 파트너 센터에서 앱을 만들 수 없습니다. 을 만드는 파트너 센터에서 작업 해야 있으며 다음 그 후 사용할 수 있습니다 API를 앱에 액세스 하 고 그에 대 한 제출을 프로그래밍 방식으로 만들 합니다. 그러나 해당 제출을 만들기 전에 API를 사용하여 프로그래밍 방식으로 추가 기능 및 패키지 플라이트를 만들 수 있습니다.

  * 이 API를 사용 하 여 지정된 된 앱에 대 한 제출을 만들기 전에 첫 번째 [파트너 센터에서 앱에 대 한 제출을 만들려면](https://msdn.microsoft.com/windows/uwp/publish/app-submissions), [연령별 등급](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) 설문지에 답변 하기를 포함 해야 합니다. 이 작업을 수행한 후 API를 사용하여 프로그래밍 방식으로 이 앱에 대한 새 제출을 만들 수 있습니다. 이러한 유형의 제출에 대해 API를 사용하기 전에 추가 기능 제출 또는 패키지 플라이트 제출을 만들 필요는 없습니다.

  * 앱 제출을 만들거나 업데이트하는 중이고 앱 패키지를 포함해야 하는 경우 [앱 패키지를 준비합니다](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * 앱 제출을 만들거나 업데이트하는 중이고 스토어 목록에 대한 스크린샷 또는 이미지를 포함해야 할 경우 [앱 스크린샷 및 이미지를 준비합니다](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * 추가 기능 제출을 만들거나 업데이트하는 중이고 아이콘을 포함해야 할 경우 [아이콘을 준비합니다](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하는 방법

Microsoft Store 제출 API를 사용 하려면 먼저 Azure AD 응용 프로그램을 파트너 센터 계정에 연결 해야, ID 및 클라이언트 ID는 응용 프로그램에 대 한 테 넌 트 검색 및 키를 생성 합니다. Azure AD 응용 프로그램은 Microsoft Store 제출 API를 호출할 앱 또는 서비스입니다. API에 전달하는 Azure AD 액세스 토큰을 가져오려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.

> [!NOTE]
> 이 작업은 한 번만 수행하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키는 Azure AD 액세스 토큰을 새로 만들 때마다 다시 사용할 수 있습니다.

1.  파트너 센터에서는 [조직의 파트너 센터 계정을 조직의 Azure AD 디렉터리와 연결](../publish/associate-azure-ad-with-dev-center.md)합니다.

2.  파트너 센터, [Azure AD 응용 프로그램을 추가](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) 앱을 나타내는 또는 파트너 센터 계정에 대 한 제출에 액세스 하는 데 사용할 서비스의 **계정 설정** 섹션에서 **사용자가** 페이지에서 다음으로. 이 응용 프로그램에 **관리자** 역할을 할당하도록 합니다. 응용 프로그램이 아직 Azure AD 디렉터리에 수 있는 경우 [새 파트너 센터에서 Azure AD 응용 프로그램](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)합니다.  

3.  **사용자** 페이지로 돌아가서 Azure AD 응용 프로그램의 이름을 클릭하여 응용 프로그램 설정으로 이동하고 **테넌트 ID** 및 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 벗어난 후에는 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [AD 응용 프로그램 키 관리](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)를 참조하세요.

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>2단계: Azure AD 액세스 토큰 가져오기

Microsoft Store 제출 API에서 메서드를 호출하기 전에 먼저 API에 있는 각 메서드의 **Authorization** 헤더에 전달하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 API에 대한 추가 호출에 계속 사용할 수 있도록 해당 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용한 서비스 간 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)의 지침에 따라 HTTP POST를 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 끝점에 보냅니다. 다음은 샘플 요청입니다.

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

POST URI 및 *client\_id* 및 *client\_secret* 매개 변수에서 *tenant\_id* 값에 대 한 테 넌 트 ID, 클라이언트 ID 및 이전 섹션에서 파트너 센터에서 검색 하는 응용 프로그램에 대 한 키를 지정 합니다. *resource* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다.

C#, Java 또는 Python 코드를 사용하여 액세스 토큰을 가져오는 방법을 보여 주는 예제는 Microsoft Store 제출 API [코드 예제](#code-examples)를 참조하세요.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>3단계: Microsoft Store 제출 API 사용

Azure AD 액세스 토큰이 있으면 Microsoft Store 제출 API에서 메서드를 호출할 수 있습니다. API에는 앱, 추가 기능 및 패키지 플라이트에 대한 시나리오로 그룹화된 많은 메서드가 포함되어 있습니다. 제출을 만들거나 업데이트하려면 일반적으로 Microsoft Store 제출 API에서 특정 순서로 여러 메서드를 호출합니다. 각 시나리오와 각 메서드의 구문에 대한 자세한 내용은 다음 표의 문서를 참조하세요.

> [!NOTE]
> 액세스 토큰을 가져온 후 만료되기 전에 Microsoft Store 제출 API에서 메서드를 호출할 수 있는 시간은 60분입니다.

| 시나리오       | 설명                                                                 |
|---------------|----------------------------------------------------------------------|
| 앱 |  앱에 대 한 제출을 만들고 파트너 센터 계정에 등록 된 모든 앱에 대 한 데이터를 검색 합니다. 이러한 메서드에 대한 자세한 내용은 다음 문서를 참조하세요. <ul><li>[앱 데이터 가져오기](get-app-data.md)</li><li>[앱 제출 관리](manage-app-submissions.md)</li></ul> |
| 추가 기능 | 앱에 대한 추가 기능을 가져오거나 만들거나 삭제한 다음 추가 기능에 대한 제출을 가져오거나 만들거나 삭제합니다. 이러한 메서드에 대한 자세한 내용은 다음 문서를 참조하세요. <ul><li>[추가 기능 관리](manage-add-ons.md)</li><li>[추가 기능 제출 관리](manage-add-on-submissions.md)</li></ul> |
| 패키지 플라이트 | 앱에 대한 패키지 플라이트를 가져오거나 만들거나 삭제한 다음 패키지 플라이트에 대한 제출을 가져오거나 만들거나 삭제합니다. 이러한 메서드에 대한 자세한 내용은 다음 문서를 참조하세요. <ul><li>[패키지 플라이트 관리](manage-flights.md)</li><li>[패키지 플라이트 제출 관리](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>코드 예제

다음 문서는 여러 다른 프로그래밍 언어로 Microsoft Store 제출 API를 사용하는 방법을 보여 주는 자세한 코드 예제를 제공합니다.

* [C# 샘플: 앱, 추가 기능, 플라이트 제출](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C# 샘플: 앱의 게임 옵션과 예고편 제출](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java 샘플: 앱, 추가 기능, 플라이트 제출](java-code-examples-for-the-windows-store-submission-api.md)
* [Java 샘플: 앱의 게임 옵션과 예고편 제출](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python 샘플: 앱, 추가 기능, 플라이트 제출](python-code-examples-for-the-windows-store-submission-api.md)
* [Python 샘플: 게임 옵션과 예고편이 포함된 앱 제출](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 모듈

Microsoft Store 제출 API를 직접 호출하는 대신 이 API 위에 명령줄 인터페이스를 구현하는 오픈 소스 PowerShell 모듈도 제공합니다. 이 모듈을 [StoreBroker](https://aka.ms/storebroker)라고 합니다. 이 모듈을 사용하여 Microsoft Store 제출 API를 직접 호출하는 대신 명령줄에서 앱, 플라이트 및 추가 기능 제출을 관리할 수 있습니다. 또는 소스에서 이 API를 호출하는 방법에 대한 예제를 더 찾아볼 수 있습니다. StoreBroker 모듈은 많은 자사 응용 프로그램이 스토어에 제출되는 기본 방식으로 Microsoft 내에서 많이 사용됩니다.

자세한 내용은 [GitHub의 StoreBroker 페이지](https://aka.ms/storebroker)를 참조하세요.

## <a name="troubleshooting"></a>문제 해결

| 문제      | 해결 방법                                          |
|---------------|---------------------------------------------|
| PowerShell에서 Microsoft Store 제출 API를 호출한 후 API에 대한 응답 데이터를 [ConvertFrom Json](https://technet.microsoft.com/library/hh849898.aspx) cmdlet을 사용하여 JSON 형식에서 PowerShell 개체로 변환하고 [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx) cmdlet을 사용하여 다시 JSON 형식으로 변환하면 API에 대한 응답 데이터가 손상됩니다. |  기본적으로 [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx) cmdlet에 대한 *-Depth* 매개 변수는 개체의 2개 수준으로 설정되며 이는 Microsoft Store 제출 API에 에서 반환하는 대부분의 JSON 개체에는 너무 얕습니다. [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx) cmdlet을 호출할 때 *-Depth* 매개 변수를 20과 같이 큰 수로 설정합니다. |

## <a name="additional-help"></a>추가 도움말

Microsoft Store 제출 API에 대한 질문이 있거나 이 API의 제출을 관리하는 데 도움이 필요한 경우 다음 리소스를 사용합니다.

* [포럼](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit)에서 궁금한 사항을 질문합니다.
* 우리의 [지원 페이지를](https://developer.microsoft.com/windows/support) 방문 하 고 파트너 센터에 대 한 보조 지원 옵션 중 하나를 요청 합니다. 문제 유형 및 범주를 선택하라는 메시지가 표시되면 **앱 제출 및 인증**과 **앱 제출**을 각각 선택합니다.  

## <a name="related-topics"></a>관련 항목

* [앱 데이터 가져오기](get-app-data.md)
* [앱 제출 관리](manage-app-submissions.md)
* [추가 기능 관리](manage-add-ons.md)
* [추가 기능 제출 관리](manage-add-on-submissions.md)
* [패키지 플라이트 관리](manage-flights.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)
