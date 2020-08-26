---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Microsoft Store 제출 API를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 제출을 프로그래밍 방식으로 만들고 관리 합니다.
title: 제출 만들기 및 관리
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API
ms.localizationpriority: medium
ms.openlocfilehash: 38a59db4115332a374c96c8a4400dbaccff9cd82
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846823"
---
# <a name="create-and-manage-submissions"></a>제출 만들기 및 관리

*Microsoft Store 제출 API* 를 사용 하 여 또는 조직의 파트너 센터 계정에 대 한 앱, 추가 기능 및 패키지에 대 한 제출을 프로그래밍 방식으로 쿼리하고 만들 수 있습니다. 이 API는 계정이 많은 앱 또는 추가 기능을 관리 하 고 이러한 자산에 대 한 제출 프로세스를 자동화 하 고 최적화 하려는 경우에 유용 합니다. 이 API는 Azure Active Directory (Azure AD)를 사용 하 여 앱 또는 서비스에서 호출을 인증 합니다.

다음 단계에서는 Microsoft Store 제출 API를 사용 하는 종단 간 프로세스를 설명 합니다.

1.  모든 [필수 구성 요소](#prerequisites)를 완료 했는지 확인 합니다.
3.  Microsoft Store 제출 API에서 메서드를 호출 하기 전에 [AZURE AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후에는 토큰이 만료 되기 전에 Microsoft Store 제출 API에 대 한 호출에서이 토큰을 사용 하는 데 60 분이 소요 됩니다. 토큰이 만료 된 후 새 토큰을 생성할 수 있습니다.
4.  [Microsoft Store 제출 API를 호출](#call-the-windows-store-submission-api)합니다.

<span id="not_supported" />

> [!IMPORTANT]
> 이 API를 사용 하 여 앱, 패키지 비행 또는 추가 기능에 대 한 제출을 만드는 경우 파트너 센터가 아닌 API를 사용 하 여 제출을 추가로 변경 해야 합니다. 파트너 센터를 사용 하 여 원래 API를 사용 하 여 만든 제출을 변경 하는 경우 API를 사용 하 여 해당 제출을 변경 하거나 커밋할 수 없습니다. 경우에 따라 제출 프로세스에서 계속할 수 없는 오류 상태로 제출 될 수 있습니다. 이 문제가 발생 하는 경우 제출을 삭제 하 고 새 제출을 만들어야 합니다.

> [!IMPORTANT]
> 이 API를 사용 하 여 비즈니스에 대 한 [Microsoft Store 및 교육용 Microsoft Store를 통해 대량 구매](../publish/organizational-licensing.md) 를 위한 제출을 게시 하거나 [LOB 앱](../publish/distribute-lob-apps-to-enterprises.md) 에 대 한 제출을 엔터프라이즈에 직접 게시할 수 없습니다. 이러한 두 시나리오 모두 파트너 센터에서 제출 게시를 사용 해야 합니다.

> [!NOTE]
> 이 API는 필수 앱 업데이트 및 저장소 관리 사용 가능 추가 기능을 사용 하는 앱 또는 추가 기능과 함께 사용할 수 없습니다. 이러한 기능 중 하나를 사용 하는 앱 또는 추가 기능과 함께 Microsoft Store 제출 API를 사용 하는 경우 API가 409 오류 코드를 반환 합니다. 이 경우 파트너 센터를 사용 하 여 앱 또는 추가 기능에 대 한 제출을 관리 해야 합니다.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>1 단계: Microsoft Store 제출 API를 사용 하기 위한 필수 구성 요소 완료

Microsoft Store 제출 API를 호출 하는 코드 작성을 시작 하기 전에 다음 필수 구성 요소를 완료 했는지 확인 합니다.

* 사용자(또는 조직)는 Azure AD 디렉터리가 있어야 하고 디렉터리에 대한 [전역 관리자](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 권한이 있어야 합니다. Microsoft에서 이미 Microsoft 365 또는 다른 비즈니스 서비스를 사용 하는 경우 Azure AD 디렉터리가 이미 있습니다. 그렇지 않으면 추가 비용 없이 [파트너 센터에서 새 AZURE AD를 만들](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) 수 있습니다.

* [Azure AD 애플리케이션을 파트너 센터 계정에 연결](#associate-an-azure-ad-application-with-your-windows-partner-center-account)하고 테넌트 ID, 클라이언트 ID 및 키를 가져와야 합니다. Microsoft Store 제출 API를 호출하는 데 사용할 Azure AD 액세스 토큰을 얻으려면 이러한 값이 필요합니다.

* Microsoft Store 제출 API에서 사용할 앱을 준비 합니다.

  * 파트너가 파트너 센터에 아직 없는 경우 [파트너 센터에서 해당 이름을 예약 하 여 앱을 만들어야](https://docs.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name)합니다. Microsoft Store 제출 API를 사용 하 여 파트너 센터에서 앱을 만들 수 없습니다. 파트너 센터에서 작업 하 여 앱을 만든 후 API를 사용 하 여 앱에 액세스 하 고 프로그래밍 방식으로 해당 앱에 대 한 제출을 만들 수 있습니다. 그러나 API를 사용 하 여 프로그래밍 방식으로 추가 기능과 패키지를 만들 수 있습니다.

  * 이 API를 사용 하 여 지정 된 앱에 대 한 제출을 만들려면 먼저 [연령 등급](https://docs.microsoft.com/windows/uwp/publish/age-ratings) 질문에 응답 하는 것을 포함 하 여 [파트너 센터에서 앱에 대 한 제출을 하나 만들어야](https://docs.microsoft.com/windows/uwp/publish/app-submissions)합니다. 이렇게 하면 API를 사용 하 여 프로그래밍 방식으로이 앱에 대 한 새 제출을 만들 수 있습니다. 이러한 유형의 제출에 API를 사용 하기 전에 추가 기능 제출 또는 패키지 비행 제출을 만들 필요가 없습니다.

  * 앱 제출을 만들거나 업데이트 하는 중 이며 앱 패키지를 포함 해야 하는 경우 [앱 패키지를 준비](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements)합니다.

  * 앱 제출을 만들거나 업데이트 하는 경우 매장 목록에 대 한 스크린샷 또는 이미지를 포함 해야 하는 경우 [앱 스크린샷 및 이미지를 준비](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images)합니다.

  * 추가 기능 제출을 만들거나 업데이트 하는 중에 아이콘을 포함 해야 하는 경우 [아이콘을 준비](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions)합니다.

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>파트너 센터 계정에 Azure AD 애플리케이션을 연결하는 방법

Microsoft Store 제출 API를 사용 하려면 먼저 Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고, 응용 프로그램에 대 한 테 넌 트 ID와 클라이언트 ID를 검색 하 고, 키를 생성 해야 합니다. Azure AD 응용 프로그램은 Microsoft Store 제출 API를 호출 하려는 앱 또는 서비스를 나타냅니다. API에 전달하는 Azure AD 액세스 토큰을 얻으려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.

> [!NOTE]
> 이 작업은 한 번만 수행 하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키가 있으면 새 Azure AD 액세스 토큰을 만들어야 할 때마다 다시 사용할 수 있습니다.

1.  파트너 센터에서 [조직의 파트너 센터 계정을 조직의 Azure AD 디렉터리에 연결합니다](../publish/associate-azure-ad-with-partner-center.md).

2.  그런 다음 파트너 센터의 **계정 설정** 섹션에 있는 **사용자** 페이지에서 파트너 센터 계정에 대한 전송에 액세스하는 데 사용할 앱 또는 서비스를 나타내는 [Azure AD 애플리케이션을 추가합니다](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account). 이 애플리케이션을 **관리자** 역할에 할당해야 합니다. 애플리케이션이 아직 Azure AD 디렉터리에 존재하지 않는 경우 [파트너 센터에서 새 Azure AD 애플리케이션 만들 수 있습니다](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

3.  **사용자** 페이지로 돌아가 Azure AD 애플리케이션의 이름을 클릭하여 애플리케이션 설정으로 이동한 다음 **테넌트 ID**와 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 나가면 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [Azure AD 애플리케이션 키 관리](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)를 참조하세요.

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>2단계: Azure AD 액세스 토큰 가져오기

Microsoft Store 제출 API에서 메서드를 호출 하기 전에 먼저 API의 각 메서드에 대 한 **인증** 헤더에 전달 하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후에는 API에 대 한 추가 호출에서 토큰을 계속 사용할 수 있도록 토큰을 새로 고칠 수 있습니다.

액세스 토큰을 가져오려면 [클라이언트 자격 증명을 사용 하 여 서비스 호출](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) 에 대 한 지침에 따라 HTTP POST를 끝점으로 보냅니다 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` . 샘플 요청은 다음과 같습니다.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

POST URI와 클라이언트 * \_ id* 및 *클라이언트 \_ 암호* 매개 변수의 *테 넌 트 \_ Id* 값에 대해 이전 섹션의 파트너 센터에서 검색 한 응용 프로그램의 테 넌 트 id, 클라이언트 id 및 키를 지정 합니다. *리소스* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

액세스 토큰이 만료 되 면 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)에 설명 된 지침에 따라 새로 고칠 수 있습니다.

C #, Java 또는 Python 코드를 사용 하 여 액세스 토큰을 가져오는 방법을 보여 주는 예제는 Microsoft Store 제출 API [코드 예제](#code-examples)를 참조 하세요.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>3단계: Microsoft Store 제출 API 사용

Azure AD 액세스 토큰을 만든 후 Microsoft Store 제출 API에서 메서드를 호출할 수 있습니다. API에는 앱, 추가 기능 및 패키지 항공편에 대 한 시나리오로 그룹화 된 많은 메서드가 포함 되어 있습니다. 제출을 만들거나 업데이트 하려면 일반적으로 Microsoft Store 제출 API에서 특정 순서로 여러 메서드를 호출 합니다. 각 시나리오와 각 메서드의 구문에 대 한 자세한 내용은 다음 표의 문서를 참조 하세요.

> [!NOTE]
> 액세스 토큰을 가져온 후에는 토큰이 만료 되기 전에 Microsoft Store 제출 API에서 메서드를 호출 하는 데 60 분이 소요 됩니다.

| 시나리오       | 설명                                                                 |
|---------------|----------------------------------------------------------------------|
| 의 |  파트너 센터 계정에 등록 된 모든 앱에 대 한 데이터를 검색 하 고 앱에 대 한 제출을 만듭니다. 이러한 메서드에 대 한 자세한 내용은 다음 문서를 참조 하세요. <ul><li>[앱 데이터 가져오기](get-app-data.md)</li><li>[앱 제출 관리](manage-app-submissions.md)</li></ul> |
| 추가 기능 | 앱에 대 한 추가 기능을 가져오거나 만들거나 삭제 한 다음 추가 기능에 대 한 제출을 가져오거나 만들거나 삭제 합니다. 이러한 메서드에 대 한 자세한 내용은 다음 문서를 참조 하세요. <ul><li>[추가 기능 관리](manage-add-ons.md)</li><li>[추가 기능 제출 관리](manage-add-on-submissions.md)</li></ul> |
| 패키지 플라이트 | 앱에 대 한 패키지를 시작, 생성 또는 삭제 한 다음, 패키지에 대해 항공편을 가져오거나 만들거나 삭제 합니다. 이러한 메서드에 대 한 자세한 내용은 다음 문서를 참조 하세요. <ul><li>[패키지 플라이트 관리](manage-flights.md)</li><li>[패키지 플라이트 제출 관리](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>코드 예제

다음 문서에서는 다양 한 프로그래밍 언어로 Microsoft Store 제출 API를 사용 하는 방법을 보여 주는 자세한 코드 예제를 제공 합니다.

* [C# 샘플: 앱, 추가 기능 및 플라이트 제출](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C# 샘플: 게임 옵션과 예고편이 포함된 앱 제출](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java 샘플: 앱, 추가 기능 및 플라이트 제출](java-code-examples-for-the-windows-store-submission-api.md)
* [Java 샘플: 게임 옵션과 예고편이 포함된 앱 제출](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python 샘플: 앱, 추가 기능 및 플라이트 제출](python-code-examples-for-the-windows-store-submission-api.md)
* [Python 샘플: 게임 옵션과 예고편이 포함된 앱 제출](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>을 (를) Broker PowerShell 모듈

Microsoft Store 제출 API를 직접 호출 하는 대신 API를 기반으로 명령줄 인터페이스를 구현 하는 오픈 소스 PowerShell 모듈을 제공 합니다. 이 모듈을이 모듈을이 모듈 [이라고 합니다.](https://github.com/Microsoft/StoreBroker) 이 모듈을 사용 하 여 Microsoft Store 제출 API를 직접 호출 하는 대신 명령줄에서 앱, 비행 및 추가 기능 제출을 관리 하거나 단순히 소스를 검색 하 여이 API를 호출 하는 방법에 대 한 더 많은 예제를 볼 수 있습니다. 여러 자사 응용 프로그램을 저장소에 제출 하는 기본 방법으로 Microsoft 내에서이 기능을 사용 합니다.

자세한 내용은 [GitHub의 microsoft 컴퓨터 브로커 페이지](https://github.com/Microsoft/StoreBroker)를 참조 하세요.

## <a name="troubleshooting"></a>문제 해결

| 문제      | 해결 방법                                          |
|---------------|---------------------------------------------|
| PowerShell에서 Microsoft Store 제출 API를 호출한 후 [convertfrom-csv](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertFrom-Json) cmdlet을 사용 하 여 json 형식에서 PowerShell 개체로 변환한 다음 [convertto-html](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) cmdlet을 사용 하 여 json 형식으로 변환 하면 API에 대 한 응답 데이터가 손상 됩니다. |  기본적으로 [convertto-html](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) cmdlet에 대 한 *-Depth* 매개 변수는 Microsoft Store 제출 API에서 반환 된 대부분의 Json 개체에 대해 너무 얕은 두 수준으로 설정 됩니다. [Convertto-html](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) cmdlet을 호출 하는 경우 *-Depth* 매개 변수를 20 등의 더 큰 값으로 설정 합니다. |

## <a name="additional-help"></a>추가 도움말

Microsoft Store 제출 API에 대 한 질문이 있거나이 API를 사용 하 여 제출을 관리 하는 데 도움이 필요한 경우 다음 리소스를 사용 하세요.

* [포럼](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit)에 질문 하세요.
* [지원 페이지](https://developer.microsoft.com/windows/support) 를 방문 하 여 파트너 센터에 대 한 보조 지원 옵션 중 하나를 요청 하세요. 문제 유형 및 범주를 선택 하 라는 메시지가 표시 되 면 **앱 제출 및 인증** 을 선택 하 고 앱을 각각 **제출**합니다.  

## <a name="related-topics"></a>관련 항목

* [앱 데이터 가져오기](get-app-data.md)
* [앱 제출 관리](manage-app-submissions.md)
* [추가 기능 관리](manage-add-ons.md)
* [추가 기능 제출 관리](manage-add-on-submissions.md)
* [패키지 플라이트 관리](manage-flights.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)
