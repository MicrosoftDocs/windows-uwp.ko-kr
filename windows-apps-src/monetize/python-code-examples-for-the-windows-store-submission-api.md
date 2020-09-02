---
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: Microsoft Store 제출 API를 사용 하는 방법에 대 한 자세한 내용은이 섹션의 Python 코드 예제를 사용 합니다.
title: 앱, 추가 기능 및 항공편을 제출 하는 Python 코드
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 코드 예제, python
ms.localizationpriority: medium
ms.openlocfilehash: f551a7de85e493f4fbc1a027fb3ab9c3ca2dd598
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363866"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Python 샘플: 앱, 추가 기능 및 플라이트 제출

이 문서에서는 이러한 작업에 [Microsoft Store 제출 API](create-and-manage-submissions-using-windows-store-services.md) 를 사용 하는 방법을 보여 주는 Python 코드 예제를 제공 합니다.

* [Azure AD 액세스 토큰 가져오기](#token)
* [추가 기능 만들기](#create-add-on)
* [패키지 플라이트 만들기](#create-package-flight)
* [앱 제출 만들기](#create-app-submission)
* [추가 기능 제출 만들기](#create-add-on-submission)
* [패키지 플라이트 제출 만들기](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Azure AD 액세스 토큰 가져오기

다음 예제에서는 Microsoft Store 제출 API에서 메서드를 호출 하는 데 사용할 수 있는 [AZURE AD 액세스 토큰을 가져오는](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) 방법을 보여 줍니다. 토큰을 가져온 후에는 토큰이 만료 되기 전에 Microsoft Store 제출 API에 대 한 호출에서이 토큰을 사용 하는 데 60 분이 소요 됩니다. 토큰이 만료 된 후 새 토큰을 생성할 수 있습니다.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="1-20":::

<span id="create-add-on" />

## <a name="create-an-add-on"></a>추가 기능 만들기

다음 예제에서는 추가 기능을 [만든](create-an-add-on.md) 다음 [삭제](delete-an-add-on.md) 하는 방법을 보여 줍니다.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="26-52":::

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>패키지 플라이트 만들기

다음 예에서는 패키지를 [만들고](create-a-flight.md) [삭제](delete-a-flight.md) 하는 방법을 보여 줍니다.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="58-87":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>앱 제출 만들기

다음 예제에서는 Microsoft Store 제출 API에서 여러 메서드를 사용 하 여 앱 제출을 만드는 방법을 보여 줍니다. 이렇게 하기 위해이 코드는 마지막으로 게시 된 전송의 복제본으로 새 제출을 만든 다음, 복제 된 제출을 업데이트 하 고 파트너 센터에 커밋합니다. 특히이 예에서는 다음 작업을 수행 합니다.

1. 시작 하기 위해이 예제는 [지정 된 앱에 대 한 데이터를 가져옵니다](get-an-app.md).
2. 그런 다음 [앱에 대 한 보류 중인 제출](delete-an-app-submission.md)(있는 경우)을 삭제 합니다.
3. 그런 다음 [앱에 대 한 새 제출을 만듭니다](create-an-app-submission.md) . 새 제출은 마지막으로 게시 된 제출의 복사본입니다.
4. 새 제출에 대 한 일부 세부 정보를 변경 하 고 Azure Blob storage에 제출할 새 패키지를 업로드 합니다.
5. 그런 다음 [업데이트를 업데이트](update-an-app-submission.md) 한 후 파트너 센터에 새 제출을 [커밋합니다](commit-an-app-submission.md) .
6. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-an-app-submission.md) 를 주기적으로 확인 합니다.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="93-166":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>추가 기능 제출 만들기

다음 예제에서는 Microsoft Store 제출 API에서 여러 메서드를 사용 하 여 추가 기능 제출을 만드는 방법을 보여 줍니다. 이렇게 하기 위해이 코드는 마지막으로 게시 된 전송의 복제본으로 새 제출을 만든 다음, 복제 된 제출을 업데이트 하 고 파트너 센터에 커밋합니다. 특히이 예에서는 다음 작업을 수행 합니다.

1. 시작 하기 위해이 예제에서는 [지정 된 추가 기능에 대 한 데이터를 가져옵니다](get-an-add-on.md).
2. 그런 다음 [추가 기능에 대 한 보류 중인 제출을 삭제](delete-an-add-on-submission.md)합니다 (있는 경우).
3. 그런 다음 [추가 기능에 대 한 새 제출을 만듭니다](create-an-add-on-submission.md) . 새 제출은 마지막으로 게시 된 제출의 복사본입니다.
4. Azure Blob storage에 제출할 수 있는 아이콘을 포함 하는 ZIP 보관 파일을 업로드 합니다. 자세한 내용은 [추가 기능 제출 만들기](manage-add-on-submissions.md#create-an-add-on-submission)에서 ZIP 보관 파일을 Azure Blob 저장소에 업로드 하는 방법에 대 한 관련 지침을 참조 하세요.
5. 그런 다음 [업데이트를 업데이트](update-an-add-on-submission.md) 한 후 파트너 센터에 새 제출을 [커밋합니다](commit-an-add-on-submission.md) .
6. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-an-add-on-submission.md) 를 주기적으로 확인 합니다.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="172-245":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>패키지 플라이트 제출 만들기

다음 예제에서는 Microsoft Store 제출 API에서 여러 메서드를 사용 하 여 패키지 비행 제출을 만드는 방법을 보여 줍니다. 이렇게 하기 위해이 코드는 마지막으로 게시 된 전송의 복제본으로 새 제출을 만든 다음, 복제 된 제출을 업데이트 하 고 파트너 센터에 커밋합니다. 특히이 예에서는 다음 작업을 수행 합니다.

1. 시작 하기 위해이 예제에서는 [지정 된 패키지 항공편에 대 한 데이터를 가져옵니다](get-a-flight.md).
2. 그런 다음, [패키지 비행에 대해 보류 중인 제출](delete-a-flight-submission.md)(있는 경우)을 삭제 합니다.
3. 그런 다음 [패키지 항공편에 대 한 새 제출을 만듭니다](create-a-flight-submission.md) (새 제출은 마지막으로 게시 된 제출의 복사본).
4. Azure Blob storage에 제출할 새 패키지를 업로드 합니다. 자세한 내용은 [패키지 비행 전송 만들기](manage-flight-submissions.md#create-a-package-flight-submission)에서 ZIP 보관 파일을 Azure Blob 저장소에 업로드 하는 방법에 대 한 관련 지침을 참조 하세요.
5. 그런 다음 [업데이트를 업데이트](update-a-flight-submission.md) 한 후 파트너 센터에 새 제출을 [커밋합니다](commit-a-flight-submission.md) .
6. 마지막으로, 전송이 성공적으로 커밋될 때까지 [새 제출의 상태](get-status-for-a-flight-submission.md) 를 주기적으로 확인 합니다.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="251-325":::

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
