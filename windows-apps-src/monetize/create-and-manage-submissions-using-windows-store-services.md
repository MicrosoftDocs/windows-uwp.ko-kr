---
author: mcleanbyron
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: "Windows 스토어 제출 API를 사용하여 Windows 개발자 센터 계정에 등록된 앱에 대한 제출을 프로그래밍 방식으로 만들고 관리합니다."
title: "Windows 스토어 서비스를 사용하여 제출 만들기 및 관리"
translationtype: Human Translation
ms.sourcegitcommit: ccc7cfea885cc9c8803cfc70d2e043192a7fee84
ms.openlocfilehash: 8467cddd5eec2348cd35f4f5dc1564b47813a6ca

---

# <a name="create-and-manage-submissions-using-windows-store-services"></a>Windows 스토어 서비스를 사용하여 제출 만들기 및 관리


*Windows 스토어 제출 API*를 사용하여 사용자 또는 조직의 Windows 개발자 센터 계정에 대한 앱, 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 및 패키지 플라이트를 프로그래밍 방식으로 쿼리하고 이에 대한 제출을 만듭니다. 이 API는 계정에서 많은 앱 또는 추가 기능을 관리하고 이러한 자산의 제출 프로세스를 자동화 및 최적화하려는 경우 유용합니다. 이 API는 Azure AD(Azure Active Directory)를 사용하여 앱 또는 서비스의 호출을 인증합니다.

다음 단계에서는 Windows 스토어 제출 API를 사용하는 종단 간 프로세스를 설명합니다.

1.  [필수 조건](#prerequisites)을 모두 완료했는지 확인합니다.
3.  Windows 스토어 제출 API에서 메서드를 호출하기 전에 [Azure AD 액세스 토큰을 가져옵니다](#obtain-an-azure-ad-access-token). 토큰을 가져온 후 만료되기 전에 이 토큰을 Windows 스토어 제출 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.
4.  [Windows 스토어 제출 API를 호출합니다](#call-the-windows-store-submission-api).


<span id="not_supported" />
>**중요**

> * 이 API는 API를 사용할 권한이 부여된 Windows 개발자 센터 계정에만 사용할 수 있습니다. 이 권한은 단계별로 개발자 계정에서 사용할 수 있으며 이때 모든 계정에서 이 권한을 사용할 수 있는 것은 아닙니다. 이전 액세스를 요청하려면 개발자 센터 대시보드에 로그온하고 대시보드의 아래쪽에서 **피드백**을 클릭하여 피드백 영역의 **제출 API**를 선택한 다음 요청을 제출합니다. 계정에 대해 이 권한을 사용할 수 있는 경우 메일을 받게 됩니다.
<br/><br/>
> * 이 API는 필수 앱 업데이트와 스토어 관리 소모성 추가 기능을 포함하여(하지만 여기에 국한되지는 않음) 2016년 8월에 개발자 센터 대시보드에 도입된 특정 기능을 사용하는 앱 또는 추가 기능에는 사용할 수 없습니다. 이러한 기능 중 하나를 사용하는 앱 또는 추가 기능에서 Windows 스토어 제출 API를 사용하는 경우 이 API는 409 오류 코드를 반환합니다. 이 경우 대시보드를 사용하여 앱 또는 추가 기능에 대한 제출을 관리해야 합니다.


<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-submission-api"></a>1단계: Windows 스토어 제출 API를 사용하기 위한 필수 조건 완료

Windows 스토어 제출 API를 호출하는 코드 작성을 시작하기 전에 다음 필수 조건을 완료했는지 확인합니다.

* 사용자(또는 조직)에게 Azure AD 디렉터리와 해당 디렉터리에 대한 [전역 관리자](http://go.microsoft.com/fwlink/?LinkId=746654) 권한이 있어야 합니다. 이미 Office 365 또는 Microsoft의 다른 비즈니스 서비스를 사용하는 경우 이미 Azure AD 디렉터리가 있습니다. 아니면 추가 요금 없이 [개발자 센터 내에서 Azure AD를 새로 만들 수 있습니다](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

* [Azure AD 응용 프로그램을 Windows 개발자 센터 계정에 연결](#associate-an-azure-ad-application-with-your-windows-dev-center-account)해야 하며 테넌트 ID, 클라이언트 ID 및 키를 가져와야 합니다. 이러한 값은 Windows 스토어 제출 API에 대한 호출에 사용하는 Azure AD 액세스 토큰을 가져오는 데 필요합니다.

* Windows 스토어 제출 API에서 사용하도록 앱을 준비합니다.

  * 개발자 센터에 아직 앱이 없는 경우 [개발자 센터 대시보드에서 앱을 만듭니다](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). 개발자 센터에서 앱을 만드는 데 Windows 스토어 제출 API를 사용할 수 없습니다. 대시보드를 사용하여 앱을 만들어야 하며 해당 API를 사용하여 앱에 액세스하고 프로그래밍 방식으로 앱용 제출을 만들 수 있습니다. 그러나 해당 제출을 만들기 전에 API를 사용하여 프로그래밍 방식으로 추가 기능 및 패키지 플라이트를 만들 수 있습니다.

  * 이 API를 사용하여 지정된 앱에 대한 제출을 만들기 전에 먼저 [연령별 등급](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) 질문에 대한 대답을 포함하여 [개발자 센터 대시보드에서 앱에 대한 한 개의 제출을 만들어야](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) 합니다. 이 작업을 수행한 후 API를 사용하여 프로그래밍 방식으로 이 앱에 대한 새 제출을 만들 수 있습니다. 이러한 유형의 제출에 대해 API를 사용하기 전에 추가 기능 제출 또는 패키지 플라이트 제출을 만들 필요는 없습니다.

  * 앱 제출을 만들거나 업데이트하는 중이고 앱 패키지를 포함해야 하는 경우 [앱 패키지를 준비합니다](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * 앱 제출을 만들거나 업데이트하는 중이고 스토어 목록에 대한 스크린샷 또는 이미지를 포함해야 할 경우 [앱 스크린샷 및 이미지를 준비합니다](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * 추가 기능 제출을 만들거나 업데이트하는 중이고 아이콘을 포함해야 할 경우 [아이콘을 준비합니다](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />
### <a name="how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account"></a>Azure AD 응용 프로그램을 Windows 개발자 센터 계정과 연결하는 방법

Windows 스토어 제출 API를 사용하려면 먼저 Azure AD 응용 프로그램을 개발자 센터 계정에 연결하고 해당 응용 프로그램에 대한 테넌트 ID 및 클라이언트 ID를 검색하고 키를 생성합니다. Azure AD 응용 프로그램은 Windows 스토어 제출 API를 호출할 앱 또는 서비스입니다. API에 전달하는 Azure AD 액세스 토큰을 가져오려면 테넌트 ID, 클라이언트 ID 및 키가 필요합니다.

>**참고**&nbsp;&nbsp;이 작업은 한 번만 수행하면 됩니다. 테넌트 ID, 클라이언트 ID 및 키는 Azure AD 액세스 토큰을 새로 만들 때마다 다시 사용할 수 있습니다.

1.  개발자 센터에서 **계정 설정**으로 이동하여 **사용자 관리**를 클릭하고 조직의 개발자 센터 계정을 조직의 Azure AD 디렉터리와 연결합니다. 자세한 내용은 [계정 사용자 관리](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)를 참조하세요.

2.  **사용자 관리** 페이지에서 **Azure AD 응용 프로그램 추가**를 클릭하여 개발자 센터 계정에 대한 제출에 액세스하는 데 사용할 앱 또는 서비스를 나타내는 Azure AD 응용 프로그램을 추가하고 **관리자** 역할에 할당합니다. 이 응용 프로그램이 Azure AD 디렉터리에 이미 존재하는 경우 **Azure AD 응용 프로그램 추가** 페이지에서 선택하여 개발자 센터 계정에 추가할 수 있습니다. 그러지 않으면 **Azure AD 응용 프로그램 추가 페이지**에서 새 Azure AD 응용 프로그램을 만들 수 있습니다. 자세한 내용은 [Azure AD 응용 프로그램 추가 및 관리](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)를 참조하세요.

3.  **사용자 관리** 페이지로 돌아가서 Azure AD 응용 프로그램의 이름을 클릭하여 응용 프로그램 설정으로 이동하고 **테넌트 ID** 및 **클라이언트 ID** 값을 복사합니다.

4. **새 키 추가**를 클릭합니다. 다음 화면에서 **키** 값을 복사합니다. 이 페이지를 벗어난 후에는 이 정보에 다시 액세스할 수 없습니다. 자세한 내용은 [Azure AD 응용 프로그램 추가 및 관리](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)에서 키 관리에 대한 내용을 참조하세요.

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>단계 2: Azure AD 액세스 토큰 가져오기

Windows 스토어 제출 API에서 메서드를 호출하기 전에 먼저 API에 있는 각 메서드의 **Authorization** 헤더에 전달하는 Azure AD 액세스 토큰을 가져와야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 API에 대한 추가 호출에 계속 사용할 수 있도록 해당 토큰을 새로 고칠 수 있습니다.

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

POST URI의 *tenant\_id* 값, *client\_id* 및 *client\_secret* 매개 변수에는 이전 섹션의 개발자 센터에서 검색한 응용 프로그램의 테넌트 ID, 클라이언트 ID 및 키를 지정합니다. *resource* 매개 변수에는 ```https://manage.devcenter.microsoft.com```을 지정해야 합니다.

만료된 액세스 토큰은 [여기](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)의 지침에 따라 새로 고칠 수 있습니다.

C#, Java 또는 Python 코드를 사용하여 액세스 토큰을 가져오는 방법을 보여 주는 예제는 Windows 스토어 제출 API [코드 예제](#code-examples)를 참조하세요.

<span id="call-the-windows-store-submission-api">
## <a name="step-3-use-the-windows-store-submission-api"></a>3단계: Windows 스토어 제출 API 사용

Azure AD 액세스 토큰이 있으면 Windows 스토어 제출 API에서 메서드를 호출할 수 있습니다. API에는 앱, 추가 기능 및 패키지 플라이트에 대한 시나리오로 그룹화된 많은 메서드가 포함되어 있습니다. 제출을 만들거나 업데이트하려면 일반적으로 Windows 스토어 제출 API에서 특정 순서로 여러 메서드를 호출합니다. 각 시나리오와 각 메서드의 구문에 대한 자세한 내용은 다음 표의 문서를 참조하세요.

>**참고**&nbsp;&nbsp;액세스 토큰을 가져온 후 만료되기 전에 Windows 스토어 제출 API에서 메서드를 호출할 수 있는 시간은 60분입니다.

| 시나리오       | 설명                                                                 |
|---------------|----------------------------------------------------------------------|
| 앱 |  Windows 개발자 센터 계정에 등록된 모든 앱에 대한 데이터를 검색하고 앱에 대한 제출을 만듭니다. 이러한 메서드에 대한 자세한 내용은 다음 문서를 참조하세요. <ul><li>[앱 데이터 가져오기](get-app-data.md)</li><li>[앱 제출 관리](manage-app-submissions.md)</li></ul> |
| 추가 기능 | 앱에 대한 추가 기능을 가져오거나 만들거나 삭제한 다음 추가 기능에 대한 제출을 가져오거나 만들거나 삭제합니다. 이러한 메서드에 대한 자세한 내용은 다음 문서를 참조하세요. <ul><li>[추가 기능 관리](manage-add-ons.md)</li><li>[추가 기능 제출 관리](manage-add-on-submissions.md)</li></ul> |
| 패키지 플라이트 | 앱에 대한 패키지 플라이트를 가져오거나 만들거나 삭제한 다음 패키지 플라이트에 대한 제출을 가져오거나 만들거나 삭제합니다. 이러한 메서드에 대한 자세한 내용은 다음 문서를 참조하세요. <ul><li>[패키지 플라이트 관리](manage-flights.md)</li><li>[패키지 플라이트 제출 관리](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>
## <a name="code-examples"></a>코드 예제

다음 문서는 여러 다른 프로그래밍 언어로 Windows 스토어 제출 API를 사용하는 방법을 보여 주는 자세한 코드 예제를 제공합니다.

* [C# 코드 예제](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 코드 예제](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 코드 예제](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="troubleshooting"></a>문제 해결

| 문제      | 해결 방법                                          |
|---------------|---------------------------------------------|
| PowerShell에서 Windows 스토어 제출 API를 호출한 후 API에 대한 응답 데이터를 [ConvertFrom Json](https://technet.microsoft.com/en-us/library/hh849898.aspx) cmdlet을 사용하여 JSON 형식에서 PowerShell 개체로 변환하고 [ConvertTo Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) cmdlet을 사용하여 다시 JSON 형식으로 변환하면 API에 대한 응답 데이터가 손상됩니다. |  기본적으로 [ConvertTo Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) cmdlet에 대한 *-Depth* 매개 변수는 개체의 2개 수준으로 설정되며 이는 Windows 스토어 제출 API에 에서 반환하는 대부분의 JSON 개체에는 너무 얕습니다. [ConvertTo Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) cmdlet을 호출할 때 *-Depth* 매개 변수를 20과 같이 큰 수로 설정합니다. |

## <a name="additional-help"></a>추가 도움말

Windows 스토어 제출 API에 대한 질문이 있거나 이 API의 제출을 관리하는 데 도움이 필요한 경우 다음 리소스를 사용합니다.

* [포럼](https://social.msdn.microsoft.com/Forums/windowsapps/en-us/home?forum=wpsubmit)에서 궁금한 사항을 질문합니다.
* [지원 페이지](https://developer.microsoft.com/windows/support)를 방문하여 개발자 센터 대시보드에 대한 보조 지원 옵션 중 하나를 요청합니다. 문제 유형 및 범주를 선택하라는 메시지가 표시되면 **앱 제출 및 인증**과 **앱 제출**을 각각 선택합니다.  

## <a name="related-topics"></a>관련 항목

* [앱 데이터 가져오기](get-app-data.md)
* [앱 제출 관리](manage-app-submissions.md)
* [추가 기능 관리](manage-add-ons.md)
* [추가 기능 제출 관리](manage-add-on-submissions.md)
* [패키지 플라이트 관리](manage-flights.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)
 



<!--HONumber=Dec16_HO3-->


