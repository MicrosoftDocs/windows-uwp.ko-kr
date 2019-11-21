---
Description: The first step in creating a new app in Partner Center is reserving an app name. 앱 이름을 예약하는 방법을 살펴보고 적합한 앱 이름을 선택하기 위한 제안을 확인하세요.
title: 이름을 예약하여 앱 만들기
keywords: Windows 10, UWP, 이름 예약, 앱 이름, 앱 이름들, 이름들, 제품 이름, 이름 지정, 예약 이름, 제목, 이름들, 제목들
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 79831e1e14ddf8c525f1697074e6a435513f5ea1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259024"
---
# <a name="create-your-app-by-reserving-a-name"></a>이름을 예약하여 앱 만들기

The first step in creating a new app in [Partner Center](https://partner.microsoft.com/dashboard) is reserving an app name. 각 예약 이름 (앱의 *제목*이라고도 불림)은 Microsoft Store 전반에서 고유해야 합니다.

앱 빌드를 아직 시작하지 않은 경우에 앱에 대한 이름을 예약할 수 있습니다. We recommend doing so as soon as possible, so that nobody else can use the name. 이름의 사용 예약을 유지하려면 3개월 내에 앱을 제출해야 합니다.

[앱 패키지를 업로드](upload-app-packages.md) 할 때 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 값은 앱용으로 예약한 이름과 일치해야 합니다. Microsoft Visual Studio를 사용하여 앱 패키지를 만들 경우 이 특성은 자동으로 채워집니다.

> [!IMPORTANT]
> You can reserve additional names for an app, and you may choose to use one of those in the published version of your app instead of the one you reserve when you first create your app in Partner Center. However, be aware that the first name you enter here will be used in some of your app's [identity details](view-app-identity-details.md), such as the **Package Family Name (PFN)** . These values may be visible to some users, and cannot be changed, so make sure that the name you reserve is appropriate for this use.


## <a name="create-your-app-by-reserving-a-new-name"></a>새 이름을 예약하여 앱 만들기

Reserving a name is the first step in creating an app in Partner Center. 

1.  **개요** 페이지에서 **새 앱 만들기**를 클릭합니다.
2.  텍스트 상자에 사용할 이름을 입력하고 **가용성 확인**을 선택합니다. 이름을 사용할 수 있으면 녹색 확인 표시가 보입니다. 입력한 이름이 다른 개발자에 의해 이미 예약되었거나 사용 중이면 이름을 사용할 수 없다는 메시지가 표시됩니다.
3.  **앱 이름 예약**을 클릭합니다.

이제 이름이 예약되었고 필요할 때마다 [제출](app-submissions.md) 작업을 시작할 수 있습니다. 

> [!NOTE]
> Microsoft Store에 특정 이름으로 나열된 앱이 표시되지 않아도 이름을 예약하지 못할 수 있습니다. 이는 대개 다른 개발자가 앱용으로 해당 이름을 예약했지만 아직 앱을 제출하지 않았기 때문입니다. 자신이 상표 또는 다른 법적 권한을 가진 이름을 예약할 수 없는 경우 또는 Microsoft Store에 해당 이름을 가진 다른 앱이 표시되는 경우 [Microsoft에 문의](https://www.microsoft.com/info/cpyrtInfrg.html)하세요.

이름을 예약한 후 3개월 이내에 해당 앱을 제출해야 합니다. 3개월 이내에 앱을 제출하지 않으면 이름 예약이 만료되고 다른 개발자가 앱에 해당 이름을 사용할 수 있습니다. 만료된 이름으로 앱을 제출하려고 하면 오류가 발생할 수 있습니다.


## <a name="choosing-your-apps-name"></a>앱 이름 선택

올바른 앱 이름 선택은 중요한 작업입니다. 고객의 흥미를 끌고 앱을 자세히 알아보도록 유도하는 이름을 선택하세요. 다음은 유용한 앱 이름을 선택하기 위한 몇 가지 팁입니다.

-   **Keep it short.** 대부분의 경우 앱 이름을 표시할 공간이 제한되어 있으므로 이름을 최대한 짧게 사용하는 것이 좋습니다. 앱 이름은 최대 256자까지 가능하면 이름이 길면 이름 끝부분이 고객에게 표시되지 않을 수도 있습니다.
    > [!NOTE]
    > 다양한 위치에 표시되는 실제 문자 수는 할당된 길이와 앱 이름에 사용된 문자 유형에 따라 달라집니다. 예를 들어 Windows에서 사용되는 맑은 고딕 글꼴의 경우 "I"자 약 30개가 "W"자 10개와 동일한 공간을 차지합니다. Because of this variation, be sure to test your app and verify how its name appears on its tiles (if you choose to overlay the app name), in search results, and within the app itself. 앱을 제공하는 각 언어도 고려하세요. 동아시아 문자는 라틴 문자보다 폭이 더 넓은 경향이 있으므로 더 적은 수의 문자가 표시된다는 점을 유의하세요.
-   **Be original.** 앱 이름이 기존 앱과 쉽게 혼동되지 않고 구별되도록 하세요.
-   **Don't use names trademarked by others.** 예약하는 이름을 사용할 권한을 가지고 있어야 합니다. 이름의 상표권이 다른 사람에게 있는 경우 위반을 보고할 수 있으며, 이 경우 해당 이름을 계속해서 사용할 수 없습니다. 앱이 게시된 후 이런 문제가 발생하면 앱이 Microsoft Store에서 제거됩니다. 이 경우 앱과 해당 콘텐츠 전체에서 앱의 이름을 모두 변경해야 다시 인증을 받기 위해 [앱을 제출](app-submissions.md)할 수 있습니다.
-   **Avoid adding differentiating info at the end of the name.** 여러 앱을 구분하는 정보를 이름의 끝에 추가할 경우, 특히 이름이 긴 경우에는 고객이 앱을 찾지 못할 수 있습니다. 모든 앱이 동일한 이름을 갖는 것처럼 보일 수 있습니다. If this is unavoidable, use different logos and app images to make it easier to differentiate one app from another.
-   **Don't include emojis in your name.** 이모지 또는 지원되지 않는 기타 문자가 포함된 이름은 예약할 수 없습니다.


## <a name="manage-additional-app-names"></a>추가 앱 이름 관리

You can add and manage additional names on the **Manage app names** page in the **App management** section for each of your apps in Partner Center.

경우에 따라 동일한 앱에 사용할 여러 이름을 예약할 수도 있습니다. 예를 들어 앱을 여러 언어로 제공하고 각 언어에 대해 서로 다른 이름을 사용하려는 경우가 여기에 해당합니다. 앱 이름을 완전히 변경하려면 추가 이름을 예약해야 합니다.

이 페이지에서는 예약했으나 더 이상 사용하지 않으려는 이름을 삭제할 수도 있습니다.

자세한 내용은 [앱 이름 관리](manage-app-names.md)를 참조하세요.

 

 




