---
description: 이 섹션의 Python 코드 예제를 사용하여 Microsoft Store 제출 API를 사용해 게임 옵션과 예고편을 제출하는 방법에 대해 자세히 알아봅니다.
title: Python 샘플 - 앱의 게임 옵션과 예고편 제출
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 코드 예제, 게임 옵션, 예고편, 고급 목록, python
ms.localizationpriority: medium
ms.openlocfilehash: 59306e32fe1fcc68978c977b89934e64d85b8cc8
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7984377"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Python 샘플: 앱의 게임 옵션과 예고편 제출

이 문서는 이런 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md)를 사용하는 방법을 설명하는 Python 코드 예제를 제공합니다.

* Microsoft Store 제출 API와 함께 사용할 Azure AD 액세스 토큰을 가져옵니다.
* 앱 제출 만들기
* [게임](manage-app-submissions.md#gaming-options-object) 및 [예고편](manage-app-submissions.md#trailer-object) 고급 목록 옵션을 포함, 앱 제출용 스토어 목록 데이터를 구성합니다.
* 앱 제출을 위해 패키지, 목록 이미지, 예고편 파일 등이 포함된 ZIM 파일을 업로드 합니다.
* 앱 제출을 커밋합니다.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

이 코드는 Microsoft Store 제출 API를 사용, 게임 옵션과 예고편이 포함된 앱 제출을 만들어 커밋하는 다른 예제 클래스와 기능을 호출합니다. 이 코드를 필요에 따라 조정하려면,

* 앱 테넌트 ID에 ```tenant```를, 앱의 클라이언트 ID 및 키에 ```client``` 및 ```secret``` 변수를 할당합니다. 자세한 내용은 [파트너 센터 계정과 Azure AD 응용 프로그램을 연결 하는 방법을](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) 참조 하세요.
* ```application_id``` 변수를 제출을 만들고 싶은 앱의 [Store ID](in-app-purchases-and-trials.md#store-ids)에 할당합니다.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py#L1-L74)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Azure AD 액세스 토큰을 가져와, 제출 API를 호출

다음은 다음 클래스를 정의하는 예제입니다.

* ```DevCenterAccessTokenClient```클래스는 ```tenantId```, ```clientId```및 ```clientSecret``` 값을 사용하여 Microsoft Store 제출 API에 사용할 Azure AD 액세스 토큰을 생성하는 도우미 메서드를 정의합니다.
* ```DevCenterClient```클래스는 Microsoft Store 제출 API의 다양한 메서드를 호출하고, 앱 제출을 위해 패키지, 목록 이미지, 예고편 파일이 포함된 ZIP 파일을 업로드하는 도우미 메서드를 정의합니다.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py#L1-L126)]

<span id="token" />

## <a name="get-app-submission-listing-data"></a>앱 제출 목록 데이터 가져오기

다음은 새 샘플 앱 제출을 위해 JSON 형식 목록 데이터를 반환하는 도우미 기능을 정의하는 예제입니다.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py#L1-L170)]

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
