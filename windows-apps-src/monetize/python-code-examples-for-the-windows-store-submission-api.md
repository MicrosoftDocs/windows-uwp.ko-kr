---
author: Xansky
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: 이 섹션의 Python 코드 예제를 사용하여 Microsoft Store 제출 API를 사용하는 방법에 대해 자세히 알아봅니다.
title: Python 샘플 - 앱, 추가 기능, 플라이트 제출
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 코드 예제, python
ms.localizationpriority: medium
ms.openlocfilehash: 34d686b8e20d384da4a3db1ea3805ad082d5f8a8
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6196050"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Python 샘플: 앱, 추가 기능, 플라이트 제출

이 문서는 이런 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md)를 사용하는 방법을 설명하는 Python 코드 예제를 제공합니다.

* [Azure AD 액세스 토큰 가져오기](#token)
* [추가 기능 만들기](#create-add-on)
* [패키지 플라이트 만들기](#create-package-flight)
* [앱 제출 만들기](#create-app-submission)
* [추가 기능 제출 만들기](#create-add-on-submission)
* [패키지 플라이트 제출 만들기](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Azure AD 액세스 토큰 가져오기

다음 예제는 Microsoft Store 제출 API에서 메서드를 호출하는 데 사용할 수 있도록 [Azure AD 액세스 토큰을 가져오는](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) 방법을 보여 줍니다. 토큰을 가져온 후 만료되기 전에 이 토큰을 Microsoft Store 제출 API에 대한 호출에 사용할 수 있는 시간은 60분입니다. 토큰이 만료된 후 새 토큰을 생성할 수 있습니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L1-L20)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>추가 기능 만들기

다음 예제에서는 추가 기능 [만들기](create-an-add-on.md) 및 [삭제](delete-an-add-on.md) 방법을 보여 줍니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L26-L52)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>패키지 플라이트 만들기

다음 예제에서는 패키지 플라이트 [만들기](create-a-flight.md) 및 [삭제](delete-a-flight.md) 방법을 보여 줍니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L58-L87)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

다음 예제는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 앱 제출을 만드는 방법을 보여 줍니다. 이렇게 하려면 코드 마지막으로 게시 된 제출의 복제본으로 새 제출을 만든 다음 업데이트 및 파트너 센터에 복제 한 제출을 커밋합니다. 이 예제는 특히 다음과 같은 작업을 수행합니다.

1. 먼저 해당 예제를 통해 [지정된 앱의 데이터 가져오기](get-an-app.md) 작업을 수행합니다.
2. 다음으로 [앱의 현재 보류 중인 제출을 삭제](delete-an-app-submission.md)합니다(보류 중인 제출이 있을 경우).
3. 그런 다음 [앱에 대한 새 제출 만들기](create-an-app-submission.md) 작업을 수행합니다(새 제출은 마지막으로 게시된 제출의 사본).
4. 새 제출에 대한 세부 정보를 변경하고 제출할 새 패키지를 Azure Blob Storage에 업로드합니다.
5. 그런 다음이 [업데이트](update-an-app-submission.md) 한 다음 파트너 센터에 새 제출 [을 커밋합니다](commit-an-app-submission.md) .
6. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태 확인](get-status-for-an-app-submission.md)을 수행합니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L93-L166)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>추가 기능 제출 만들기

다음 예제는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 추가 기능 제출을 만드는 방법을 보여 줍니다. 이렇게 하려면 코드 마지막으로 게시 된 제출의 복제본으로 새 제출을 만든 다음 업데이트 및 파트너 센터에 복제 한 제출을 커밋합니다. 이 예제는 특히 다음과 같은 작업을 수행합니다.

1. 먼저 예제를 통해 [지정된 추가 기능의 데이터 가져오기](get-an-add-on.md) 작업을 수행합니다.
2. 다음으로 [추가 기능의 현재 보류 중인 제출을 삭제](delete-an-add-on-submission.md)합니다(보류 중인 제출이 있을 경우).
3. 그런 다음 [추가 기능에 대한 새 제출 만들기](create-an-add-on-submission.md)를 진행합니다(새 제출은 마지막으로 게시된 제출의 사본).
4. 제출할 아이콘이 포함된 ZIP 보관 파일을 Azure Blob Storage에 업로드합니다. 자세한 내용은 [추가 기능 제출 만들기](manage-add-on-submissions.md#create-an-add-on-submission)에서 Azure Blob Storage에 ZIP 보관 파일을 업로드하는 작업에 대한 관련 지침을 참조하세요.
5. 그런 다음이 [업데이트](update-an-add-on-submission.md) 한 다음 파트너 센터에 새 제출 [을 커밋합니다](commit-an-add-on-submission.md) .
6. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태 확인](get-status-for-an-add-on-submission.md)합니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L172-L245)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>패키지 플라이트 제출 만들기

다음 예제에서는 Microsoft Store 제출 API에서 여러 메서드를 사용하여 패키지 플라이트 제출을 만드는 방법을 보여 줍니다. 이렇게 하려면 코드 마지막으로 게시 된 제출의 복제본으로 새 제출을 만든 다음 업데이트 및 파트너 센터에 복제 한 제출을 커밋합니다. 이 예제는 특히 다음과 같은 작업을 수행합니다.

1. 먼저 예제를 통해 [지정된 패키지 플라이트의 데이터 가져오기](get-a-flight.md) 작업을 수행합니다.
2. 다음으로 [패키지 플라이트의 현재 보류 중인 제출을 삭제](delete-a-flight-submission.md)합니다(보류 중인 제출이 있을 경우).
3. 그런 다음 [패키지 플라이트에 대한 새 제출 만들기](create-a-flight-submission.md) 작업을 수행합니다(새 제출은 마지막으로 게시된 제출의 사본).
4. 제출할 새 패키지를 Azure Blob Storage에 업로드합니다. 자세한 내용은 [패키지 플라이트 제출 만들기](manage-flight-submissions.md#create-a-package-flight-submission)에서 Azure Blob Storage에 ZIP 보관 파일을 업로드하는 작업에 대한 관련 지침을 참조하세요.
5. 그런 다음이 [업데이트](update-a-flight-submission.md) 한 다음 파트너 센터에 새 제출 [을 커밋합니다](commit-a-flight-submission.md) .
6. 마지막으로 제출이 성공적으로 커밋될 때까지 정기적으로 [새 제출의 상태 확인](get-status-for-a-flight-submission.md)합니다.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L251-L325)]

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
