---
author: Xansky
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: 이 섹션의 Java 코드 예제를 사용하여 Microsoft Store 제출 API를 사용하는 방법에 대해 자세히 알아봅니다.
title: Java 샘플 - 앱, 추가 기능, 플라이트 제출
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 코드 예제, Java
ms.localizationpriority: medium
ms.openlocfilehash: 5a3280b6b9c0f012f36588d6eb0297b415e07f78
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5825397"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Java 샘플: 앱, 추가 기능, 플라이트 제출

이 문서는 이런 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md)를 사용하는 방법을 설명하는 Java 코드 예제를 제공합니다.

* [Azure AD 액세스 토큰 가져오기](#token)
* [추가 기능 만들기](#create-add-on)
* [패키지 플라이트 만들기](#create-package-flight)
* [앱 제출 만들기](#create-app-submission)
* [추가 기능 제출 만들기](#create-add-on-submission)
* [패키지 플라이트 제출 만들기](#create-flight-submission)

각 예제를 검토하여 각 예제에서 보여 주는 작업에 대해 자세히 알아보거나 이 문서의 모든 코드 예제를 콘솔 응용 프로그램으로 빌드할 수 있습니다. 전체 코드 목록은 이 문서의 끝 부분에 있는 [코드 목록](java-code-examples-for-the-windows-store-submission-api.md#code-listing)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

이러한 예제는 다음 라이브러리를 사용합니다.

* [Apache Commons Logging 1.2](http://commons.apache.org/proper/commons-logging)(commons-logging-1.2.jar).
* [Apache HttpComponents Core 4.4.5 및 Apache HttpComponents Client 4.5.2](https://hc.apache.org/)(httpcore-4.4.5.jar 및 httpclient-4.5.2.jar).
* [JSR 353 JSON Processing API 1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) 및 [JSR 353 JSON Processing Default Provider API 1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4)(javax.json-api-1.0.jar 및 javax.json-1.0.4.jar).

## <a name="main-program-and-imports"></a>주 프로그램 및 가져오기

다음 예제에서는 모든 코드 예제에 사용되는 가져오기 문을 보여 주고 다른 예제 메서드를 호출하는 명령줄 프로그램을 구현합니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/MainExample.java#L1-L64)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Azure AD 액세스 토큰 가져오기

다음 예제는 Microsoft Store 제출 API에서 메서드를 호출하는 데 사용할 수 있도록 [Azure AD 액세스 토큰을 가져오는](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) 방법을 보여 줍니다. 토큰을 가져온 후 만료되기 전에 이 토큰을 Microsoft Store 제출 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L65-L95)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>추가 기능 만들기

다음 예제에서는 추가 기능 [만들기](create-an-add-on.md) 및 [삭제](delete-an-add-on.md) 방법을 보여 줍니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L310-L345)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>패키지 플라이트 만들기

다음 예제에서는 패키지 플라이트 [만들기](create-a-flight.md) 및 [삭제](delete-a-flight.md) 방법을 보여 줍니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L185-L221)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

다음 예제는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 앱 제출을 만드는 방법을 보여 줍니다. 이를 위해 ```SubmitNewApplicationSubmission``` 메서드를 사용하여 마지막으로 게시된 제출의 복제본으로 새 제출을 만든 다음, 복제한 제출을 Windows 개발자 센터에 업데이트하고 커밋합니다. 특히 ```SubmitNewApplicationSubmission``` 메서드는 다음과 같은 작업을 수행합니다.

1. 먼저 해당 메서드를 통해 [지정된 앱의 데이터 가져오기](get-an-app.md) 작업을 수행합니다.
2. 다음으로 [앱의 현재 보류 중인 제출을 삭제](delete-an-app-submission.md)합니다(보류 중인 제출이 있을 경우).
3. 그런 다음 [앱에 대한 새 제출 만들기](create-an-app-submission.md) 작업을 수행합니다(새 제출은 마지막으로 게시된 제출의 사본).
4. 새 제출에 대한 세부 정보를 변경하고 제출할 새 패키지를 Azure Blob Storage에 업로드합니다.
5. 다음으로 Windows 개발자 센터에 새 제출을 [업데이트](update-an-app-submission.md)한 다음 [커밋](commit-an-app-submission.md)합니다.
6. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태 확인](get-status-for-an-app-submission.md)을 수행합니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L97-L183)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>추가 기능 제출 만들기

다음 예제는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 추가 기능 제출을 만드는 방법을 보여 줍니다. 이를 위해 ```SubmitNewInAppProductSubmission``` 메서드를 사용하여 마지막으로 게시된 제출의 복제본으로 새 제출을 만든 다음, 복제한 제출을 Windows 개발자 센터에 업데이트하고 커밋합니다. ```SubmitNewInAppProductSubmission``` 메서드는 구체적으로 다음과 같은 작업을 수행합니다.

1. 먼저 해당 메서드를 통해 [지정된 추가 기능의 데이터 가져오기](get-an-add-on.md) 작업을 수행합니다.
2. 다음으로 [추가 기능의 현재 보류 중인 제출을 삭제](delete-an-add-on-submission.md)합니다(보류 중인 제출이 있을 경우).
3. 그런 다음 [추가 기능에 대한 새 제출 만들기](create-an-add-on-submission.md)를 진행합니다(새 제출은 마지막으로 게시된 제출의 사본).
4. 제출할 아이콘이 포함된 ZIP 보관 파일을 Azure Blob Storage에 업로드합니다.
5. 다음으로 Windows 개발자 센터에 새 제출을 [업데이트](update-an-add-on-submission.md)한 다음 [커밋](commit-an-add-on-submission.md)합니다.
6. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태 확인](get-status-for-an-add-on-submission.md)합니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L347-L431)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>패키지 플라이트 제출 만들기

다음 예제에서는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 패키지 플라이트 제출을 만드는 방법을 보여 줍니다. 이를 위해 ```SubmitNewFlightSubmission``` 메서드를 사용하여 마지막으로 게시된 제출의 복제본으로 새 제출을 만든 다음, 복제한 제출을 Windows 개발자 센터에 업데이트하고 커밋합니다. ```SubmitNewFlightSubmission``` 메서드는 구체적으로 다음과 같은 작업을 수행합니다.

1. 먼저 이 메서드는 [지정된 패키지 플라이트의 데이터 가져오기](get-a-flight.md) 작업을 수행합니다.
2. 다음으로 [패키지 플라이트의 현재 보류 중인 제출을 삭제](delete-a-flight-submission.md)합니다(보류 중인 제출이 있을 경우).
3. 그런 다음 [패키지 플라이트에 대한 새 제출 만들기](create-a-flight-submission.md) 작업을 수행합니다(새 제출은 마지막으로 게시된 제출의 사본).
4. 제출할 새 패키지를 Azure Blob Storage에 업로드합니다.
5. 다음으로 Windows 개발자 센터에 새 제출을 [업데이트](update-a-flight-submission.md)한 다음 [커밋](commit-a-flight-submission.md)합니다.
6. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태를 확인](get-status-for-a-flight-submission.md)합니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L223-L308)]

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>제출 파일을 업로드하고 요청 응답을 처리하는 유틸리티 메서드

다음 유틸리티 메서드는 이러한 작업을 설명합니다.

* 앱 또는 추가 기능 제출을 위해 새 자산이 포함된 ZIP 보관 파일을 Azure Blob Storage에 업로드하는 방법. 앱 및 추가 기능 제출을 위해 Azure Blob Storage에 ZIP 보관 파일을 업로드하는 방법에 대한 자세한 내용은 [앱 제출 만들기](manage-app-submissions.md#create-an-app-submission), [추가 기능 제출 만들기](manage-add-on-submissions.md#create-an-add-on-submission), [패키지 플라이트 제출 만들기](manage-flight-submissions.md#create-a-package-flight-submission)의 관련 지침을 참조하세요.
* 요청 응답을 처리하는 방법

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L433-L490)]

<span id="code-listing" />

## <a name="complete-code-listing"></a>전체 코드 목록

다음 코드 목록에는 하나의 원본 파일로 구성된 이전의 모든 예제가 포함되어 있습니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L1-L491)]

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
