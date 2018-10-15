---
author: Xansky
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: 이 섹션의 C# 코드 예제를 사용하여 Microsoft Store 제출 API를 사용하는 방법에 대해 자세히 알아봅니다.
title: C# 샘플 - 앱, 추가 기능, 플라이트 제출
ms.author: mhopkins
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 제출 API, 코드 예제, C#
ms.localizationpriority: medium
ms.openlocfilehash: f5e508bd89c06841009576a0a69cb960a20faa83
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4612540"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>C\# 샘플: 앱, 추가 기능, 플라이트 제출

이 문서는 이런 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md)를 사용하는 방법을 설명하는 C# 코드 예제를 제공합니다.

* [앱 제출 만들기](#create-app-submission)
* [추가 기능 제출 만들기](#create-add-on-submission)
* [추가 기능 제출 업데이트](#update-add-on-submission)
* [패키지 플라이트 제출 만들기](#create-flight-submission)

각 예제를 검토하여 각 예제에서 보여 주는 작업에 대해 자세히 알아보거나 이 문서의 모든 코드 예제를 콘솔 응용 프로그램으로 빌드할 수 있습니다. 예제를 빌드하려면 Visual Studio에서 **DeveloperApiCSharpSample**이라는 C# 콘솔 응용 프로그램을 만들고 각 예제를 프로젝트의 별도 코드 파일에 복사하여 프로젝트를 빌드합니다.

## <a name="prerequisites"></a>필수 조건

이러한 예제는 다음 라이브러리를 사용합니다.

* Microsoft.WindowsAzure.Storage.dll. 이 라이브러리는 [.NET용 Azure SDK](https://azure.microsoft.com/downloads/)에서 사용할 수 있으며 또는 [WindowsAzure.Storage NuGet 패키지](https://www.nuget.org/packages/WindowsAzure.Storage)를 설치하여 가져올 수 있습니다.
* Newtonsoft의 [Newtonsoft.Json](http://www.newtonsoft.com/json) NuGet 패키지.

## <a name="main-program"></a>기본 프로그램

다음 예제에서는 Microsoft Store 제출 API를 사용하는 여러 가지 방법을 보여 주기 위해 이 문서의 다른 예제 메서드를 호출하는 명령줄 프로그램을 구현합니다. 이 프로그램을 필요에 따라 조정하려면,

* 관리하려는 앱, 추가 기능, 패키지 플라이트의 ID에 ```ApplicationId```, ```InAppProductId``` 및 ```FlightId``` 속성을 할당합니다.
* 앱의 클라이언트 ID 및 키에 ```ClientId``` 및 ```ClientSecret``` 속성을 할당하고 ```TokenEndpoint``` URL의 *tenantid* 문자열을 앱의 테넌트 ID로 바꿉니다. 자세한 내용은 [Azure AD 응용 프로그램을 Windows 개발자 센터 계정과 연결하는 방법](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)을 참조하세요.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/Program.cs#Main)]

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>ClientConfiguration 도우미 클래스

이 샘플 앱에서는 ```ClientConfiguration``` 도우미 클래스를 사용하여 Microsoft Store 제출 API를 사용하는 예제 메서드 각각에 Azure Active Directory 데이터 및 앱 데이터를 전달합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/ClientConfiguration.cs#ClientConfiguration)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

다음 예제는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 앱 제출을 업데이트하는 클래스를 구현합니다. 이 클래스의 ```RunAppSubmissionUpdateSample``` 메서드는 마지막으로 게시된 제출의 복제본으로 새 제출을 만든 다음 복제한 제출을 Windows 개발자 센터에 업데이트하고 커밋합니다. ```RunAppSubmissionUpdateSample``` 메서드는 구체적으로 다음과 같은 작업을 수행합니다.

1. 먼저 해당 메서드를 통해 [지정된 앱의 데이터 가져오기](get-an-app.md) 작업을 수행합니다.
2. 다음으로 [앱의 현재 보류 중인 제출을 삭제](delete-an-app-submission.md)합니다(보류 중인 제출이 있을 경우).
3. 그런 다음 [앱에 대한 새 제출 만들기](create-an-app-submission.md) 작업을 수행합니다(새 제출은 마지막으로 게시된 제출의 사본).
4. 새 제출에 대한 세부 정보를 변경하고 제출할 새 패키지를 Azure Blob Storage에 업로드합니다.
5. 다음으로 Windows 개발자 센터에 새 제출을 [업데이트](update-an-app-submission.md)한 다음 [커밋](commit-an-app-submission.md)합니다.
6. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태 확인](get-status-for-an-app-submission.md)을 수행합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs#AppSubmissionUpdateSample)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>추가 기능 제출 만들기

다음 예제는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 새 추가 기능 제출을 만드는 클래스를 구현합니다. 이 클래스의 ```RunInAppProductSubmissionCreateSample``` 메서드는 다음과 같은 작업을 수행합니다.

1. 먼저 이 메서드는 [새 추가 기능 만들기](create-an-add-on.md) 작업을 수행합니다.
2. 다음으로 [추가 기능의 새 제출 만들기](create-an-add-on-submission.md) 작업을 수행합니다.
3. 제출할 아이콘이 포함된 ZIP 보관 파일을 Azure Blob Storage에 업로드합니다.
4. 다음으로 [Windows 개발자 센터에 새 제출을 커밋](commit-an-add-on-submission.md)합니다.
5. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태를 확인](get-status-for-an-add-on-submission.md)합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs#InAppProductSubmissionCreateSample)]

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>추가 기능 제출 업데이트

다음 예제는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 기존의 추가 기능 제출을 업데이트하는 클래스를 구현합니다. 이 클래스의 ```RunInAppProductSubmissionUpdateSample``` 메서드는 마지막으로 게시된 제출의 복제본으로 새 제출을 만든 다음 복제한 제출을 Windows 개발자 센터에 업데이트하고 커밋합니다. ```RunInAppProductSubmissionUpdateSample``` 메서드는 구체적으로 다음과 같은 작업을 수행합니다.

1. 먼저 해당 메서드를 통해 [지정된 추가 기능의 데이터 가져오기](get-an-add-on.md) 작업을 수행합니다.
2. 다음으로 [추가 기능의 현재 보류 중인 제출을 삭제](delete-an-add-on-submission.md)합니다(보류 중인 제출이 있을 경우).
3. 그런 다음 [추가 기능에 대한 새 제출 만들기](create-an-add-on-submission.md) 작업을 수행합니다(새 제출은 마지막으로 게시된 제출의 사본).
5. 다음으로 Windows 개발자 센터에 새 제출을 [업데이트](update-an-add-on-submission.md)한 다음 [커밋](commit-an-add-on-submission.md)합니다.
6. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태 확인](get-status-for-an-add-on-submission.md)합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs#InAppProductSubmissionUpdateSample)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>패키지 플라이트 제출 만들기

다음 예제는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 패키지 플라이트 제출을 업데이트하는 클래스를 구현합니다. 이 클래스의 ```RunFlightSubmissionUpdateSample``` 메서드는 마지막으로 게시된 제출의 복제본으로 새 제출을 만든 다음 복제한 제출을 Windows 개발자 센터에 업데이트하고 커밋합니다. ```RunFlightSubmissionUpdateSample``` 메서드는 구체적으로 다음과 같은 작업을 수행합니다.

1. 먼저 이 메서드는 [지정된 패키지 플라이트의 데이터 가져오기](get-a-flight.md) 작업을 수행합니다.
2. 다음으로 [패키지 플라이트의 현재 보류 중인 제출을 삭제](delete-a-flight-submission.md)합니다(보류 중인 제출이 있을 경우).
3. 그런 다음 [패키지 플라이트에 대한 새 제출 만들기](create-a-flight-submission.md) 작업을 수행합니다(새 제출은 마지막으로 게시된 제출의 사본).
4. 제출할 새 패키지를 Azure Blob Storage에 업로드합니다.
5. 다음으로 Windows 개발자 센터에 새 제출을 [업데이트](update-a-flight-submission.md)한 다음 [커밋](commit-a-flight-submission.md)합니다.
6. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태를 확인](get-status-for-a-flight-submission.md)합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs#FlightSubmissionUpdateSample)]

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>IngestionClient 도우미 클래스

```IngestionClient``` 클래스는 샘플 앱의 다른 메서드에서 다음 작업을 수행하는 데 사용하는 도우미 메서드를 제공합니다.

* Microsoft Store 제출 API에서 메서드를 호출하는 데 사용할 수 있는 [Azure AD 액세스 토큰 가져오기](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) 작업을 수행합니다. 토큰을 가져온 후 만료되기 전에 이 토큰을 Microsoft Store 제출 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.
* 앱 또는 추가 기능 제출을 위해 새 자산이 포함된 ZIP 보관 파일을 Azure Blob Storage에 업로드합니다. 앱 및 추가 기능 제출을 위해 ZIP 보관 파일을 Azure Blob Storage에 업로드하는 방법에 대한 자세한 내용은 [앱 제출 만들기](manage-app-submissions.md#create-an-app-submission) 및 [추가 기능 제출 만들기](manage-add-on-submissions.md#create-an-add-on-submission)의 관련 지침을 참조하세요.
* Microsoft Store 제출 API에 대한 HTTP 요청을 처리합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/IngestionClient.cs#IngestionClient)]

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
