---
author: jnHs
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: 앱 이름 관리
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 8/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 응용 프로그램 이름, 응용 프로그램 이름, 업데이트 응용 프로그램 이름, 게임 이름, 제품 이름 변경
ms.localizationpriority: medium
ms.openlocfilehash: f0d2c6f72e2f69f0b768af55f9bddeb9bb008027
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2861882"
---
# <a name="manage-app-names"></a>앱 이름 관리

추가 이름 (다른 언어 또는 응용 프로그램의 이름을 변경 하려면), 예약 하 고 하지 않아도 이름을 삭제 하는 모든 응용 프로그램에 대 한 예약 했을 때 이름을 보려면 **관리 응용 프로그램 이름** 수 있습니다. 앱의 왼쪽된 탐색 메뉴에서 **응용 프로그램 관리** 섹션을 확장 하 여 [Windows 개발자 센터 대시보드](https://partner.microsoft.com/dashboard) 이 페이지를 찾을 수 있습니다.


## <a name="reserve-additional-names-for-your-app"></a>앱 이름을 추가로 예약

같은 앱에 사용할 여러 앱 이름을 예약할 수 있습니다. 이 기능은 특히 앱을 여러 언어로 제공 중이고 언어에 따라 다른 이름을 사용하려는 경우 유용합니다. 또한 아래 설명에 따라 응용 프로그램의 이름을 변경 하려면 새 이름의 예약할 수 있습니다.

새 응용 프로그램 이름, 예약 하는 **관리 응용 프로그램 이름** 페이지의 **더 많은 이름 예약** 섹션에서 텍스트 상자를 찾습니다. 예약할 이름을 입력하고 **가용성 확인**을 클릭합니다. 이름을 사용할 수 있으면 **제품 이름 예약**을 클릭합니다. 원하는 경우 다음이 단계를 반복 하 여 여러 응용 프로그램 이름을 예약할 수 있습니다.

> [!NOTE]
> 앱 이름 예약에 대한 자세한 내용과 특정 이름을 사용할 수 없는 이유는 [이름을 예약하여 앱 만들기](create-your-app-by-reserving-a-name.md)를 참조하세요.


## <a name="delete-app-names"></a>앱 이름 삭제

이전에 예약한 이름을 더 이상 사용하지 않으려면 여기에서 이름을 삭제하여 해제할 수 있습니다. 이름을 삭제하기 전에 확실한지 확인하세요. 이렇게 하면 다른 사용자가 즉시 이 이름을 예약하고 사용할 수 있기 때문입니다.

앱의 예약 이름을 삭제하려면 더 이상 사용하지 않을 이름을 찾고 **삭제**를 클릭합니다. 확인 대화 상자에서 **삭제**를 다시 클릭하여 확인합니다.

참고 앱 하나 이상의 예약 된 이름이 해 놓아야 합니다. 완전히 대시보드에에서 앱 제거 (및 해당 앱에 대 한 예약 했을 때 모든 이름을 해제), **응용 프로그램 개요 (영문)** 페이지에서 **이 응용 프로그램 삭제** 를 클릭 합니다. 앱 제출이 진행 중인 경우에는 먼저 해당 제출을 삭제해야 합니다. 메모는 저장소에 앱을 이미 게시 하는 경우 삭제할 수 없습니다 것 대시보드에에서 (상황이 **개요 (영문)** 페이지에서 **표시/숨기기 제품** 기능을 사용 하 여 숨길 수 수)입니다. 


## <a name="rename-an-app-that-has-already-been-published"></a>이미 게시된 앱의 이름 바꾸기

앱이 이미 스토어에 있는데 이름을 바꾸려면 위에 설명된 단계에 따라 앱용으로 새 이름을 예약하고 앱에 대한 새 제출을 만듭니다. 

새 데이터베이스로 이전 이름으로 바꿉니다 제출을에 업데이트 된 패키지를 업로드 하 여 앱 패키지를 업데이트 해야 합니다.
- 첫째, 수동으로 또는 Visual Studio를 사용 하 여 새 이름으로 사용 하 여 Package.StoreAssociation.xml 파일을 업데이트 (**프로젝트 > 저장소 >... 저장소와 연결 App**). 자세한 정보에 대 한 [Visual Studio를 사용한 UWP 응용 프로그램 패키지](../packaging/packaging-uwp-apps.md)를 참조 하십시오.
- 앱 매니페스트의 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 요소를 업데이트하고 앱 이름을 포함하는 그래픽이나 텍스트를 업데이트해야 합니다. 
  > [!IMPORTANT]
  > 앱 매니페스트에서 **Package/Properties/DisplayName**을 변경하기에 앞서 Package.StoreAssociation.xml 파일을 업데이트해야 합니다. 그렇지 않으면 오류가 발생할 수 있습니다.

새 이름을 사용 하 여 저장소 나열을 업데이트 하려면는 [페이지를 나열 하는 저장소](create-app-store-listings.md) 해당 언어에 대 한로 이동 하 고 **제품 이름** 드롭다운 목록에서 이름을 선택 합니다. 입력 한 설명 및 모든 멘 이름에 대 한 목록의 다른 부분을 검토 하 고 필요한 경우 업데이트를 확인 해야 합니다.

> [!NOTE]
> 있으면 앱 패키지 및/또는 저장소 목록에서 여러 언어를 업데이트 된 패키지 및/또는 이름을 업데이트 해야 하는 모든 언어에 대 한 목록을 저장 필요 합니다.

새 이름으로 응용 프로그램에 게시 되 면 더이상 필요 없는 사용 하는 모든 오래 된 이름은 삭제할 수 없습니다.

> [!TIP]
> 각 응용 프로그램에 대 한 예약 하는 이름을 사용 하 여 대시보드에 표시 됩니다. 응용 프로그램의 이름을 바꾸려면 위의 단계를 따르면 구매할 새 이름을 사용 하 여 대시보드에 표시 되 게 하는 경우에 ( **관리 응용 프로그램 이름** 페이지에서 **삭제** 를 클릭) 하 여 원래 이름을 삭제 해야 합니다. 

 

 




