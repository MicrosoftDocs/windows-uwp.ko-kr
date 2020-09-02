---
description: 이 섹션의 Python 코드 예제를 사용 하 여 Microsoft Store 제출 API를 사용 하 여 게임 옵션 및 트레일러를 제출 하는 방법에 대해 자세히 알아보세요.
title: Python 샘플-게임 옵션 및 트레일러를 사용 하는 앱 제출
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 코드 예제, 게임 옵션, 트레일러, 고급 목록, python
ms.localizationpriority: medium
ms.openlocfilehash: 26a2062ff1d8e0f03ff8507cab89bed942012143
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364076"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Python 샘플: 게임 옵션과 예고편이 포함된 앱 제출

이 문서에서는 이러한 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md) 를 사용 하는 방법을 보여 주는 Python 코드 예제를 제공 합니다.

* Microsoft Store 제출 API와 함께 사용할 Azure AD 액세스 토큰을 가져옵니다.
* 앱 제출 만들기
* [게임](manage-app-submissions.md#gaming-options-object) 및 [트레일러](manage-app-submissions.md#trailer-object) 고급 목록 옵션을 포함 하 여 앱 전송에 대 한 저장소 목록 데이터를 구성 합니다.
* 앱 전송용 패키지, 목록 이미지 및 트레일러 파일이 포함 된 ZIP 파일을 업로드 합니다.
* 앱 제출을 커밋합니다.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

이 코드는 Microsoft Store 제출 API를 사용 하 여 게임 옵션 및 트레일러가 포함 된 앱 제출을 만들고 커밋하는 다른 예제 클래스 및 함수를 호출 합니다. 이 코드를 사용자 고유의 용도에 맞게 조정 하려면 다음을 수행 합니다.

* `tenant`앱에 대 한 테 넌 트 ID에 변수를 할당 하 고 `client` `secret` 앱의 클라이언트 ID 및 키에 및 변수를 할당 합니다. 자세한 내용은 [파트너 센터 계정에 AZURE AD 응용 프로그램을 연결 하는 방법](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) 을 참조 하세요.
* `application_id`제출을 만들려는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 에 변수를 할당 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py" range="1-74":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Azure AD 액세스 토큰을 가져오고 제출 API를 호출 합니다.

다음 예제에서는 다음 클래스를 정의 합니다.

* `DevCenterAccessTokenClient`클래스는, 및 값을 사용 하 여 `tenantId` `clientId` `clientSecret` Microsoft Store 제출 API와 함께 사용할 Azure AD 액세스 토큰을 만드는 도우미 메서드를 정의 합니다.
* `DevCenterClient`클래스는 Microsoft Store 제출 API에서 다양 한 메서드를 호출 하 고 앱 전송용 패키지, 목록 이미지 및 트레일러 파일이 포함 된 ZIP 파일을 업로드 하는 도우미 메서드를 정의 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py" range="1-126":::

<span id="token" />

## <a name="get-app-submission-listing-data"></a>앱 제출 목록 데이터 가져오기

다음 예제에서는 새 샘플 앱 제출을 위한 JSON 형식 목록 데이터를 반환 하는 도우미 함수를 정의 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py" range="1-170":::

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
