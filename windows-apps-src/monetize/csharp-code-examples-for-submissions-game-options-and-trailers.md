---
author: Xansky
description: 이 섹션의 C# 코드 예제를 사용하여 Microsoft Store 제출 API를 사용해 게임 옵션과 예고편을 제출하는 방법에 대해 자세히 알아봅니다.
title: C# 샘플 - 앱 게임 옵션과 예고편 제출
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 제출 API, 코드 예제, 게임 옵션, 예고편, 고급 목록, C#
ms.localizationpriority: medium
ms.openlocfilehash: e22081435bea8c73f509719aec1ce31d9157a315
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "5013211"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>C\# 샘플: 앱 게임 옵션과 예고편 제출

이 문서는 이런 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md)를 사용하는 방법을 설명하는 C# 코드 예제를 제공합니다.

* Microsoft Store 제출 API와 함께 사용할 Azure AD 액세스 토큰을 가져옵니다.
* 앱 제출 만들기
* [게임](manage-app-submissions.md#gaming-options-object) 및 [예고편](manage-app-submissions.md#trailer-object) 고급 목록 옵션을 포함, 앱 제출용 스토어 목록 데이터를 구성합니다.
* 앱 제출을 위해 패키지, 목록 이미지, 예고편 파일 등이 포함된 ZIM 파일을 업로드 합니다.
* 앱 제출을 커밋합니다.

각 예제를 검토하여 각 예제에서 보여 주는 작업에 대해 자세히 알아보거나 이 문서의 모든 코드 예제를 콘솔 응용 프로그램으로 빌드할 수 있습니다. 예제를 빌드하려면 Visual Studio에서 **DevCenterApiSample**이라는 C# 콘솔 응용 프로그램을 만들고 각 예제를 프로젝트의 별도 코드 파일에 복사하여 프로젝트를 빌드합니다.

## <a name="prerequisites"></a>필수 구성 요소

이러한 예제의 필수 조건은 다음과 같습니다.

* 프로젝트에 System.Web 어셈블리 참조를 추가합니다.
* 프로젝트에 Newtonsoft의 [Newtonsoft.Json](http://www.newtonsoft.com/json) NuGet 패키지를 설치합니다.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

```CreateAndSubmitSubmissionExample```클래스는 Microsoft Store 제출 API를 사용, 게임 옵션과 예고편이 포함된 앱 제출을 만들어 커밋하는 예제 메서드를 호출하는 공용 ```Execute``` 메서드를 정의합니다. 이 코드를 필요에 따라 조정하려면,

* 앱 테넌트 ID에 ```tenantId```를, 앱의 클라이언트 ID 및 키에 ```clientId``` 및 ```clientSecret``` 변수를 할당합니다. 자세한 내용은 [Azure AD 응용 프로그램을 Windows 개발자 센터 계정과 연결하는 방법](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)을 참조하세요.
* ```applicationId``` 변수를 제출을 만들고 싶은 앱의 [Store ID](in-app-purchases-and-trials.md#store-ids)에 할당합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs#CreateAndSubmitSubmissionExample)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Azure AD 액세스 토큰 가져오기

```DevCenterAccessTokenClient```클래스는 ```tenantId```, ```clientId```및 ```clientSecret``` 값을 사용하여 Microsoft Store 제출 API에 사용할 Azure AD 액세스 토큰을 생성하는 도우미 메서드를 정의합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs#DevCenterAccessTokenClient)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>제출 API를 호출하고, 제출 파일을 업로드 하는 도우미 메서드

```DevCenterClient```클래스는 Microsoft Store 제출 API의 다양한 메서드를 호출하고, 앱 제출을 위해 패키지, 목록 이미지, 예고편 파일이 포함된 ZIP 파일을 업로드하는 도우미 메서드를 정의합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs#DevCenterClient)]

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
