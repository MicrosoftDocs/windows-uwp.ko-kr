---
description: Microsoft Store 제출 API를 사용 하 여 게임 옵션 및 트레일러를 제출 하는 방법에 대 한 자세한 내용은이 섹션의 Java 코드 예제를 사용 합니다.
title: Java 샘플-게임 옵션 및 트레일러를 사용 하는 앱 제출
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 코드 예제, 게임 옵션, 트레일러, 고급 목록, java
ms.localizationpriority: medium
ms.openlocfilehash: 38351c06fd680a16eaf54b0f2f8c84460a275028
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363076"
---
# <a name="java-sample-app-submission-with-game-options-and-trailers"></a>Java 샘플: 게임 옵션과 예고편이 포함된 앱 제출

이 문서에서는 이러한 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md) 를 사용 하는 방법을 보여 주는 Java 코드 예제를 제공 합니다.

* Microsoft Store 제출 API와 함께 사용할 Azure AD 액세스 토큰을 가져옵니다.
* 앱 제출 만들기
* [게임](manage-app-submissions.md#gaming-options-object) 및 [트레일러](manage-app-submissions.md#trailer-object) 고급 목록 옵션을 포함 하 여 앱 전송에 대 한 저장소 목록 데이터를 구성 합니다.
* 앱 전송용 패키지, 목록 이미지 및 트레일러 파일이 포함 된 ZIP 파일을 업로드 합니다.
* 앱 제출을 커밋합니다.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

`CreateAndSubmitSubmissionExample`클래스는 `main` MICROSOFT STORE 제출 API를 사용 하 여 게임 옵션 및 트레일러가 포함 된 앱 제출을 만들고 커밋하는 다른 예제 메서드를 호출 하는 프로그램을 구현 합니다. 이 코드를 사용자 고유의 용도에 맞게 조정 하려면 다음을 수행 합니다.

* `tenantId`앱에 대 한 테 넌 트 ID에 변수를 할당 하 고 `clientId` `clientSecret` 앱의 클라이언트 ID 및 키에 및 변수를 할당 합니다. 자세한 내용은 [파트너 센터 계정에 AZURE AD 응용 프로그램을 연결 하는 방법](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) 을 참조 하세요.
* `applicationId`제출을 만들려는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 에 변수를 할당 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/java/CreateAndSubmitSubmissionExample.java" range="1-313":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Azure AD 액세스 토큰 가져오기

`DevCenterAccessTokenClient`클래스는, 및 값을 사용 하 여 `tenantId` `clientId` `clientSecret` Microsoft Store 제출 API와 함께 사용할 Azure AD 액세스 토큰을 만드는 도우미 메서드를 정의 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterAccessTokenClient.java" range="1-69":::


<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>제출 API를 호출 하 고 제출 파일을 업로드 하는 도우미 메서드

`DevCenterClient`클래스는 Microsoft Store 제출 API에서 다양 한 메서드를 호출 하 고 앱 전송용 패키지, 목록 이미지 및 트레일러 파일이 포함 된 ZIP 파일을 업로드 하는 도우미 메서드를 정의 합니다.

> [!div class="tabbedCodeSnippets"]
:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterClient.java" range="1-224":::

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
