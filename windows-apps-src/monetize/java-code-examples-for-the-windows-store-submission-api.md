---
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: Microsoft Store 제출 API를 사용 하는 방법에 대 한 자세한 내용은이 섹션의 Java 코드 예제를 사용 합니다.
title: Java 샘플-앱, 추가 기능 및 항공편에 대 한 전송
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 코드 예제, java
ms.localizationpriority: medium
ms.openlocfilehash: d10390dbb5364ff4f05de211167551d91dfab858
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363936"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Java 샘플: 앱, 추가 기능 및 플라이트 제출

이 문서에서는 이러한 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md) 를 사용 하는 방법을 보여 주는 Java 코드 예제를 제공 합니다.

* [Azure AD 액세스 토큰 가져오기](#token)
* [추가 기능 만들기](#create-add-on)
* [패키지 플라이트 만들기](#create-package-flight)
* [앱 제출 만들기](#create-app-submission)
* [추가 기능 제출 만들기](#create-add-on-submission)
* [패키지 플라이트 제출 만들기](#create-flight-submission)

각 예제를 검토 하 여 보여 주는 작업에 대 한 자세한 내용을 보거나이 문서의 모든 코드 예제를 콘솔 응용 프로그램에 빌드할 수 있습니다. 전체 코드 목록은이 문서의 끝에 있는 [코드 목록](java-code-examples-for-the-windows-store-submission-api.md#code-listing) 섹션을 참조 하세요.

## <a name="prerequisites"></a>사전 요구 사항

이러한 예제에서는 다음 라이브러리를 사용 합니다.

* [Apache Commons Logging 1.2](https://commons.apache.org/proper/commons-logging/)  (commons-logging-1.2).
* [Apache Httpcomponents Core 4.4.5 및 Apache Httpcomponents 클라이언트 4.5.2](https://hc.apache.org/) (httpcore-4.4.5 및 httpclient-4.5.2).
* [Jsr 353 Json 처리 API 1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) 및 [JSR 353 Json 처리 기본 공급자 API 1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4) (javax.json-api-1.0 .jar 및 javax.json-1.0.4).

## <a name="main-program-and-imports"></a>주 프로그램 및 가져오기

다음 예제에서는 모든 코드 예제에서 사용 된 imports 문을 보여 주고 다른 예제 메서드를 호출 하는 명령줄 프로그램을 구현 합니다.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/MainExample.java" range="1-64":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Azure AD 액세스 토큰 가져오기

다음 예제에서는 Microsoft Store 제출 API에서 메서드를 호출 하는 데 사용할 수 있는 [AZURE AD 액세스 토큰을 가져오는](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) 방법을 보여 줍니다. 토큰을 가져온 후에는 토큰이 만료 되기 전에 Microsoft Store 제출 API에 대 한 호출에서이 토큰을 사용 하는 데 60 분이 소요 됩니다. 토큰이 만료 된 후 새 토큰을 생성할 수 있습니다.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="65-95":::

<span id="create-add-on" />

## <a name="create-an-add-on"></a>추가 기능 만들기

다음 예제에서는 추가 기능을 [만든](create-an-add-on.md) 다음 [삭제](delete-an-add-on.md) 하는 방법을 보여 줍니다.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="310-345":::

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>패키지 플라이트 만들기

다음 예에서는 패키지를 [만들고](create-a-flight.md) [삭제](delete-a-flight.md) 하는 방법을 보여 줍니다.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="185-221":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

다음 예제에서는 Microsoft Store 제출 API에서 여러 메서드를 사용 하 여 앱 제출을 만드는 방법을 보여 줍니다. 이 작업을 수행 하기 위해 `SubmitNewApplicationSubmission` 메서드는 마지막으로 게시 된 제출의 복제본으로 새 제출을 만든 다음, 파트너 센터에 복제 된 제출을 업데이트 하 고 커밋합니다. 특히 메서드는 `SubmitNewApplicationSubmission` 다음 작업을 수행 합니다.

1. 시작 하기 위해 메서드는 [지정 된 앱에 대 한 데이터를 가져옵니다](get-an-app.md).
2. 그런 다음 [앱에 대 한 보류 중인 제출](delete-an-app-submission.md)(있는 경우)을 삭제 합니다.
3. 그런 다음 [앱에 대 한 새 제출을 만듭니다](create-an-app-submission.md) . 새 제출은 마지막으로 게시 된 제출의 복사본입니다.
4. 새 제출에 대 한 일부 세부 정보를 변경 하 고 Azure Blob storage에 제출할 새 패키지를 업로드 합니다.
5. 그런 다음 [업데이트를 업데이트](update-an-app-submission.md) 한 후 파트너 센터에 새 제출을 [커밋합니다](commit-an-app-submission.md) .
6. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-an-app-submission.md) 를 주기적으로 확인 합니다.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="97-183":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>추가 기능 제출 만들기

다음 예제에서는 Microsoft Store 제출 API에서 여러 메서드를 사용 하 여 추가 기능 제출을 만드는 방법을 보여 줍니다. 이 작업을 수행 하기 위해 `SubmitNewInAppProductSubmission` 메서드는 마지막으로 게시 된 전송의 클론으로 새 제출을 만든 다음, 복제 된 제출을 업데이트 하 고 파트너 센터에 커밋합니다. 특히 메서드는 `SubmitNewInAppProductSubmission` 다음 작업을 수행 합니다.

1. 시작 하기 위해 메서드는 [지정 된 추가 기능에 대 한 데이터를 가져옵니다](get-an-add-on.md).
2. 그런 다음 [추가 기능에 대 한 보류 중인 제출을 삭제](delete-an-add-on-submission.md)합니다 (있는 경우).
3. 그런 다음 [추가 기능에 대 한 새 제출을 만듭니다](create-an-add-on-submission.md) . 새 제출은 마지막으로 게시 된 제출의 복사본입니다.
4. Azure Blob storage에 제출할 수 있는 아이콘을 포함 하는 ZIP 보관 파일을 업로드 합니다.
5. 그런 다음 [업데이트를 업데이트](update-an-add-on-submission.md) 한 후 파트너 센터에 새 제출을 [커밋합니다](commit-an-add-on-submission.md) .
6. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-an-add-on-submission.md) 를 주기적으로 확인 합니다.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="347-431":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>패키지 플라이트 제출 만들기

다음 예제에서는 Microsoft Store 제출 API에서 여러 메서드를 사용 하 여 패키지 비행 제출을 만드는 방법을 보여 줍니다. 이 작업을 수행 하기 위해 `SubmitNewFlightSubmission` 메서드는 마지막으로 게시 된 전송의 클론으로 새 제출을 만든 다음, 복제 된 제출을 업데이트 하 고 파트너 센터에 커밋합니다. 특히 메서드는 `SubmitNewFlightSubmission` 다음 작업을 수행 합니다.

1. 시작 하기 위해 메서드는 [지정 된 패키지 항공편에 대 한 데이터를 가져옵니다](get-a-flight.md).
2. 그런 다음, [패키지 비행에 대해 보류 중인 제출](delete-a-flight-submission.md)(있는 경우)을 삭제 합니다.
3. 그런 다음 [패키지 항공편에 대 한 새 제출을 만듭니다](create-a-flight-submission.md) (새 제출은 마지막으로 게시 된 제출의 복사본).
4. Azure Blob storage에 제출할 새 패키지를 업로드 합니다.
5. 그런 다음 [업데이트를 업데이트](update-a-flight-submission.md) 한 후 새 제출을 새 제출물에 [커밋합니다](commit-a-flight-submission.md) .
6. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-a-flight-submission.md) 를 주기적으로 확인 합니다.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="223-308":::

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>제출 파일을 업로드 하 고 요청 응답을 처리 하는 유틸리티 메서드

다음 유틸리티 메서드는 이러한 작업을 보여 줍니다.

* Azure Blob storage에 앱 또는 추가 기능 제출에 대 한 새 자산을 포함 하는 ZIP 보관 파일을 업로드 하는 방법입니다. 앱 및 추가 기능 제출을 위해 Azure Blob storage에 ZIP 보관 파일을 업로드 하는 방법에 대 한 자세한 내용은 [앱 제출 만들기](manage-app-submissions.md#create-an-app-submission), [추가 기능 제출 만들기](manage-add-on-submissions.md#create-an-add-on-submission)및 [패키지 비행 전송 만들기](manage-flight-submissions.md#create-a-package-flight-submission)에서 관련 지침을 참조 하세요.
* 요청 응답을 처리 하는 방법입니다.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="433-490":::

<span id="code-listing" />

## <a name="complete-code-listing"></a>전체 코드 목록

다음 코드 목록에는 하나의 소스 파일로 구성 된 이전 예제가 모두 포함 되어 있습니다.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="1-491":::

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
