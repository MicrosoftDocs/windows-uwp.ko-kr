---
author: JnHs
Description: Learn how to create known user groups to use for package flighting and more.
title: 알려진 사용자 그룹 만들기
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 대상 그룹, 고객, 플라이트 그룹, 사용자 그룹, 알려진 사용자
ms.localizationpriority: medium
ms.openlocfilehash: e15b5a4a2f76cbfc33db593c3110ac6f2d054b5b
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4535633"
---
# <a name="create-known-user-groups"></a>알려진 사용자 그룹 만들기

알려진 사용자 그룹에서는 Microsoft 계정과 연결된 메일 주소를 사용하여 그룹에 특정인을 추가할 수 있습니다. [패키지 플라이팅](package-flights.md)에서 선택한 그룹의 고객에게 특정 패키지를 배포하거나 [개인 대상](choose-visibility-options.md#audience)에 제출을 배포할 수 있도록 이러한 알려진 사용자 그룹이 가장 자주 사용됩니다. 또한 특정 고객 그룹에 [대상이 지정된 알림](send-push-notifications-to-your-apps-customers.md) 또는 [대상 제품](use-targeted-offers-to-maximize-engagement-and-conversions.md)을 보내는 등의 참여 캠페인에서도 사용할 수 있습니다.

그룹의 회원으로 계산이 되도록 제공하는 메일 주소와 연결된 Microsoft 계정을 사용하여 스토어에서 각 사용자를 인증해야 합니다. 앱 플라이팅을 통해 앱을 다운로드하려면 그룹 구성원이 패키지 플라이트(Windows.Desktop build 10586 or later; Windows.Mobile 빌드 10586.63 이상이나 Xbox One)를 지원하는 Windows 10 버전을 사용해야 합니다. 개인 대상 제출에서는 그룹 구성원이 Windows 10, 버전 1607 이상(Xbox One 포함)을 사용하고 있어야 합니다.

## <a name="to-create-a-known-user-group"></a>알려진 사용자 그룹을 만들려면

1. Windows 개발자 센터 대시보드의 왼쪽 탐색 메뉴에서 **참여**를 확장하고 **고객 그룹**을 선택합니다. 
2. **내 고객 그룹** 섹션에서 **새 그룹 만들기**를 선택합니다.
3. 다음 페이지에서 **그룹 이름** 상자에 그룹 이름을 입력합니다.
4. **알려진 사용자 그룹** 라디오 단추가 선택되었는지 확인합니다.
5. 그룹에 추가하려는 사람의 메일 주소를 입력합니다. 하나 이상의 메일 주소(최대 10,000개)를 포함해야 합니다. 필드에 직접 메일 주소를 입력하거나(공백, 쉼표 또는 세미콜론으로 구분) **.csv 가져오기** 링크를 클릭하여 .csv 파일로 메일 주소 목록에서 플라이트 그룹을 만들 수 있습니다.
6. **저장**을 선택합니다.

이제 그룹을 사용할 수 있습니다.

[패키지 플라이트](package-flights.md) 생성 페이지에서 **플라이트 그룹 만들기**를 선택하여 알려진 사용자 그룹을 생성할 수도 있습니다. 이 경우 패키지 플라이트 생성 페이지에 이미 제공한 정보가 있다면 다시 입력해야 합니다.

> [!IMPORTANT]
> 패키지 플라이팅에서 알려진 사용자 그룹을 사용할 때는 그룹에 추가한 사용자로부터 필요한 동의를 얻어야 하며, 이러한 사용자가 플라이트 외 제출과는 다른 패키지가 다운로드된다는 사실을 이해하고 있는지 확인해야 합니다. 

## <a name="to-edit-a-known-user-group"></a>알려진 사용자 그룹을 편집하려면

생성된 이후에는 대시보드에서 알려진 사용자 그룹을 제거하거나 이름을 변경할 수 없지만, 언제라도 구성원을 편집할 수 있습니다.

알려진 사용자 그룹을 검토 및 편집하려면 왼쪽 탐색 메뉴에서 **참여**를 확장하고 **고객 그룹**을 선택합니다. **내 고객 그룹**에서 편집하려는 그룹의 이름을 선택합니다. 새 플라이트를 만들 때 **기존 그룹 보기 및 관리**를 선택하거나 패키지 플라이트의 개요 페이지에서 그룹 이름을 선택하여 패키지 플라이트 생성 페이지에서 알려진 사용자 그룹을 편집할 수도 있습니다. 

편집하려는 그룹을 선택한 후 필드에서 직접 메일 주소를 추가 또는 제거할 수 있습니다.

대대적인 변경이 필요할 때는 **.csv 내보내기**를 선택하여 .csv 파일로 그룹 구성원을 저장합니다. 새 버전을 사용하여 그룹 구성원을 업데이트하려면 이 파일을 변경한 다음 **.csv 가져오기**를 클릭합니다.

구성원 변경이 구현되는 데는 최대 30분이 소요될 수 있습니다. 새 그룹 구성원이 패키지 플라이트 또는 개인 대상을 통해 제출에 액세스(변경을 수행하는 즉시 액세스)할 수 있도록 하려면 새 제출을 게시해서는 안 됩니다. 






