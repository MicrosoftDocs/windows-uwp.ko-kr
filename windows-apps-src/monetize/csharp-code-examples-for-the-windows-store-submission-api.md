---
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: 'Microsoft Store 제출 API를 사용 하는 방법에 대 한 자세한 내용은이 섹션의 c # 코드 예제를 사용 합니다.'
title: 'C # 샘플-앱, 추가 기능 및 항공편에 대 한 전송'
ms.date: 08/03/2017
ms.topic: article
keywords: 'windows 10, uwp, Microsoft Store 제출 API, 코드 예제, C #'
ms.localizationpriority: medium
ms.openlocfilehash: c1f5704963dd1d6d6ad786a48c63ecfcd789aff9
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811303"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>C \# 샘플: 앱, 추가 기능 및 항공편에 대 한 전송

이 문서에서는 이러한 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md) 를 사용 하는 방법을 보여 주는 c # 코드 예제를 제공 합니다.

* [앱 제출 만들기](#create-app-submission)
* [추가 기능 제출 만들기](#create-add-on-submission)
* [추가 기능 제출 업데이트](#update-add-on-submission)
* [패키지 플라이트 제출 만들기](#create-flight-submission)

각 예제를 검토 하 여 보여 주는 작업에 대 한 자세한 내용을 보거나이 문서의 모든 코드 예제를 콘솔 응용 프로그램에 빌드할 수 있습니다. 예제를 빌드하려면 Visual Studio에서 **DeveloperApiCSharpSample** 라는 c # 콘솔 응용 프로그램을 만들고, 각 예제를 프로젝트의 개별 코드 파일에 복사 하 고, 프로젝트를 빌드합니다.

## <a name="prerequisites"></a>필수 조건

이러한 예제에서는 다음 라이브러리를 사용 합니다.

* Microsoft.WindowsAzure.Storage.dll. 이 라이브러리는 [.net 용 AZURE SDK](https://azure.microsoft.com/downloads/)에서 사용할 수 있으며, [windowsazure.servicebus NuGet 패키지](https://www.nuget.org/packages/WindowsAzure.Storage)를 설치 하 여 가져올 수 있습니다.
* [Newtonsoft.Js](https://www.newtonsoft.com/json) Newtonsoft.json의 NuGet 패키지입니다.

## <a name="main-program"></a>주 프로그램

다음 예제에서는 Microsoft Store 제출 API를 사용 하는 다양 한 방법을 보여 주기 위해이 문서의 다른 예제 메서드를 호출 하는 명령줄 프로그램을 구현 합니다. 이 프로그램을 사용자의 고유한 용도에 맞게 조정 하려면 다음을 수행 합니다.

* ```ApplicationId```, ```InAppProductId``` 및 ```FlightId``` 속성을 관리 하려는 앱, 추가 기능 및 패키지 항공편의 ID에 할당 합니다.
* ```ClientId``` ```ClientSecret``` 앱의 클라이언트 ID 및 키에 및 속성을 할당 하 고 URL의 *tenantid* 문자열을 ```TokenEndpoint``` 앱의 테 넌 트 ID로 바꿉니다. 자세한 내용은 [파트너 센터 계정에 AZURE AD 응용 프로그램을 연결 하는 방법](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) 을 참조 하세요.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/Program.cs" id="Main":::

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>ClientConfiguration helper 클래스

샘플 앱은 도우미 클래스를 사용 하 여 ```ClientConfiguration``` Microsoft Store 제출 API를 사용 하는 각 예제 메서드에 Azure Active Directory 데이터와 앱 데이터를 전달 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/ClientConfiguration.cs" id="ClientConfiguration":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

다음 예제에서는 Microsoft Store 제출 API의 여러 메서드를 사용 하 여 앱 제출을 업데이트 하는 클래스를 구현 합니다. ```RunAppSubmissionUpdateSample```클래스의 메서드는 마지막으로 게시 된 제출의 복제본으로 새 제출을 만든 다음 파트너 센터에 복제 된 제출을 업데이트 하 고 커밋합니다. 특히 메서드는 ```RunAppSubmissionUpdateSample``` 다음 작업을 수행 합니다.

1. 시작 하기 위해 메서드는 [지정 된 앱에 대 한 데이터를 가져옵니다](get-an-app.md).
2. 그런 다음 [앱에 대 한 보류 중인 제출](delete-an-app-submission.md)(있는 경우)을 삭제 합니다.
3. 그런 다음 [앱에 대 한 새 제출을 만듭니다](create-an-app-submission.md) . 새 제출은 마지막으로 게시 된 제출의 복사본입니다.
4. 새 전송에 대 한 일부 세부 정보를 변경 하 고 Azure Blob Storage 전송에 대 한 새 패키지를 업로드 합니다.
5. 그런 다음 [업데이트를 업데이트](update-an-app-submission.md) 한 후 파트너 센터에 새 제출을 [커밋합니다](commit-an-app-submission.md) .
6. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-an-app-submission.md) 를 주기적으로 확인 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs" id="AppSubmissionUpdateSample":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>추가 기능 제출 만들기

다음 예제에서는 Microsoft Store 제출 API의 여러 메서드를 사용 하 여 새 추가 기능 제출을 만드는 클래스를 구현 합니다. ```RunInAppProductSubmissionCreateSample```클래스의 메서드는 다음 작업을 수행 합니다.

1. 시작 하기 위해 메서드는 [새 추가 기능을 만듭니다](create-an-add-on.md).
2. 그런 다음 [추가 기능에 대 한 새 제출을 만듭니다](create-an-add-on-submission.md).
3. Azure Blob Storage 전송에 대 한 아이콘을 포함 하는 ZIP 보관 파일을 업로드 합니다.
4. 그런 다음 [파트너 센터에 새 제출을 커밋합니다](commit-an-add-on-submission.md).
5. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-an-add-on-submission.md) 를 주기적으로 확인 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs" id="InAppProductSubmissionCreateSample":::

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>추가 기능 제출 업데이트

다음 예제에서는 Microsoft Store 제출 API의 여러 메서드를 사용 하 여 기존 추가 기능 제출을 업데이트 하는 클래스를 구현 합니다. ```RunInAppProductSubmissionUpdateSample```클래스의 메서드는 마지막으로 게시 된 제출의 복제본으로 새 제출을 만든 다음 파트너 센터에 복제 된 제출을 업데이트 하 고 커밋합니다. 특히 메서드는 ```RunInAppProductSubmissionUpdateSample``` 다음 작업을 수행 합니다.

1. 시작 하기 위해 메서드는 [지정 된 추가 기능에 대 한 데이터를 가져옵니다](get-an-add-on.md).
2. 그런 다음 [추가 기능에 대 한 보류 중인 제출을 삭제](delete-an-add-on-submission.md)합니다 (있는 경우).
3. 그런 다음 [추가 기능에 대 한 새 제출을 만듭니다](create-an-add-on-submission.md) . 새 제출은 마지막으로 게시 된 제출의 복사본입니다.
5. 그런 다음 [업데이트를 업데이트](update-an-add-on-submission.md) 한 후 파트너 센터에 새 제출을 [커밋합니다](commit-an-add-on-submission.md) .
6. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-an-add-on-submission.md) 를 주기적으로 확인 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs" id="InAppProductSubmissionUpdateSample":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>패키지 플라이트 제출 만들기

다음 예제에서는 Microsoft Store 제출 API의 여러 메서드를 사용 하 여 패키지 비행 전송을 업데이트 하는 클래스를 구현 합니다. ```RunFlightSubmissionUpdateSample```클래스의 메서드는 마지막으로 게시 된 제출의 복제본으로 새 제출을 만든 다음 파트너 센터에 복제 된 제출을 업데이트 하 고 커밋합니다. 특히 메서드는 ```RunFlightSubmissionUpdateSample``` 다음 작업을 수행 합니다.

1. 시작 하기 위해 메서드는 [지정 된 패키지 항공편에 대 한 데이터를 가져옵니다](get-a-flight.md).
2. 그런 다음, [패키지 비행에 대해 보류 중인 제출](delete-a-flight-submission.md)(있는 경우)을 삭제 합니다.
3. 그런 다음 [패키지 항공편에 대 한 새 제출을 만듭니다](create-a-flight-submission.md) (새 제출은 마지막으로 게시 된 제출의 복사본).
4. Azure Blob Storage 전송에 대 한 새 패키지를 업로드 합니다.
5. 그런 다음 [업데이트를 업데이트](update-a-flight-submission.md) 한 후 파트너 센터에 새 제출을 [커밋합니다](commit-a-flight-submission.md) .
6. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-a-flight-submission.md) 를 주기적으로 확인 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs" id="FlightSubmissionUpdateSample":::

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>IngestionClient helper 클래스

```IngestionClient```클래스는 샘플 앱의 다른 메서드에서 다음 작업을 수행 하는 데 사용 하는 도우미 메서드를 제공 합니다.

* Microsoft Store 제출 API에서 메서드를 호출 하는 데 사용할 수 있는 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 토큰을 가져온 후에는 토큰이 만료 되기 전에 Microsoft Store 제출 API에 대 한 호출에서이 토큰을 사용 하는 데 60 분이 소요 됩니다. 토큰이 만료 된 후 새 토큰을 생성할 수 있습니다.
* Azure Blob Storage에 대 한 앱 또는 추가 기능 제출에 대 한 새 자산을 포함 하는 ZIP 보관 파일을 업로드 합니다. 앱 및 추가 기능 제출에 대 한 Azure Blob Storage에 ZIP 보관 파일을 업로드 하는 방법에 대 한 자세한 내용은 [앱 제출 만들기](manage-app-submissions.md#create-an-app-submission) 및 [추가 기능 등록 만들기](manage-add-on-submissions.md#create-an-add-on-submission)의 관련 지침을 참조 하세요.
* Microsoft Store 제출 API에 대 한 HTTP 요청을 처리 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/IngestionClient.cs" id="IngestionClient":::

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
