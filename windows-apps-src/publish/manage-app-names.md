---
author: jnHs
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: 앱 이름 관리
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 앱 이름, 앱 이름, 업데이트 앱 이름, 게임, 제품 이름 변경
ms.localizationpriority: medium
ms.openlocfilehash: 878b105541691834dbbe35b5210f33045afdc47b
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5164769"
---
# <a name="manage-app-names"></a>앱 이름 관리

모든 앱에 대 한 예약 된 이름을 볼 **앱 이름 관리** 수 있으며 (다른 언어 또는 앱의 이름을 변경 하려면), 추가 이름을 예약 하 고 필요 없는 이름을 삭제 합니다. [Windows 개발자 센터 대시보드에서](https://partner.microsoft.com/dashboard) 앱 중 하나에 대 한 왼쪽된 탐색 메뉴에서 **앱 관리** 섹션을 확장 하 여이 페이지를 찾을 수 있습니다.

> [!IMPORTANT]
> 앱에 대 한 추가 이름을 예약할 수 있습니다 및 대시보드에서 앱을 처음 만들어질 때 예약한 목록 대신 앱의 게시 된 버전 중 하나를 사용 하도록 선택할 수 있습니다. 그러나 [id 세부 정보](view-app-identity-details.md), 같은 **패키지 패밀리 이름 (PFN)** 의 제품에 대해 예약한 이름을 it 중 일부에 사용 됩니다. 이러한 값을 일부 사용자에 게 표시 될 수 있습니다 하 고 여야 먼저 예약한 이름을 사용 하이 여가에 대 한 적절 한 인지 확인 하므로 변경 합니다.


## <a name="reserve-additional-names-for-your-app"></a>앱 이름을 추가로 예약

같은 앱에 사용할 여러 앱 이름을 예약할 수 있습니다. 이 기능은 특히 앱을 여러 언어로 제공 중이고 언어에 따라 다른 이름을 사용하려는 경우 유용합니다. 또한 아래 설명 된 대로 앱의 이름을 변경 하기 위해 새 이름을 예약할 수 있습니다.

새 앱 이름을 예약 하려면 **앱 이름 관리** 페이지의 **더 많은 이름 예약** 섹션에서 텍스트 상자를 찾습니다. 예약할 이름을 입력하고 **가용성 확인**을 클릭합니다. 이름을 사용할 수 있으면 **제품 이름 예약**을 클릭합니다. 원하는 경우 다음이 단계를 반복 하 여 여러 앱 이름을 예약할 수 있습니다.

> [!NOTE]
> 앱 이름 예약에 대한 자세한 내용과 특정 이름을 사용할 수 없는 이유는 [이름을 예약하여 앱 만들기](create-your-app-by-reserving-a-name.md)를 참조하세요.


## <a name="delete-app-names"></a>앱 이름 삭제

이전에 예약한 이름을 더 이상 사용하지 않으려면 여기에서 이름을 삭제하여 해제할 수 있습니다. 이름을 삭제하기 전에 확실한지 확인하세요. 이렇게 하면 다른 사용자가 즉시 이 이름을 예약하고 사용할 수 있기 때문입니다.

앱의 예약 이름을 삭제하려면 더 이상 사용하지 않을 이름을 찾고 **삭제**를 클릭합니다. 확인 대화 상자에서 **삭제**를 다시 클릭하여 확인합니다.

참고 앱이 하나 이상의 예약 된 이름은 수 있어야 합니다. 완전히 대시보드에서 앱을 제거 (및 해당 앱에 대해 예약한 모든 이름도 해제) 하려면 **앱 개요** 페이지에서 **이 앱 삭제** 를 클릭 합니다. 앱 제출이 진행 중인 경우에는 먼저 해당 제출을 삭제해야 합니다. 참고는 스토어에 앱을 이미 게시 한를 삭제할 수 없습니다 것 대시보드에서 (하지만 **개요** 페이지에서 **제품 표시/숨기기** 기능을 사용 하 여를 숨길 수 있습니다). 


## <a name="rename-an-app-that-has-already-been-published"></a>이미 게시된 앱의 이름 바꾸기

앱이 이미 스토어에 있는데 이름을 바꾸려면 위에 설명된 단계에 따라 앱용으로 새 이름을 예약하고 앱에 대한 새 제출을 만듭니다. 

새 패키지로 이전 이름을 바꾸고 제출에 업데이트 된 패키지를 업로드 하려면 앱의 패키지를 업데이트 해야 합니다.
- 먼저, 수동으로 또는 Visual Studio를 사용 하 여 새 이름을 사용 하는 Package.StoreAssociation.xml 파일을 업데이트 (**프로젝트 > 스토어 > 스토어... 앱 연결**). 자세한 내용은 [Visual Studio를 사용 하 여 UWP 앱 패키지](../packaging/packaging-uwp-apps.md)를 참조 하세요.
- 앱 매니페스트의 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 요소를 업데이트하고 앱 이름을 포함하는 그래픽이나 텍스트를 업데이트해야 합니다. 
  > [!IMPORTANT]
  > 앱 매니페스트에서 **Package/Properties/DisplayName**을 변경하기에 앞서 Package.StoreAssociation.xml 파일을 업데이트해야 합니다. 그렇지 않으면 오류가 발생할 수 있습니다.

새 이름을 사용 하 여 스토어 목록을 업데이트 하 고 [스토어 목록 페이지](create-app-store-listings.md) 해당 언어에 대 한 이동한 **제품 이름** 드롭다운 목록에서 이름을 선택 합니다. 설명 및 이름의 모든 멘 션에 대 한 목록의 다른 부분을 검토 하 고 필요한 경우 업데이트를 확인 해야 합니다.

> [!NOTE]
> 앱을 여러 언어로 패키지 및/또는 스토어 목록에 있으면 패키지 업데이트 및/또는 이름을 업데이트 해야 하는 모든 언어에 대 한 목록을 저장 해야 합니다.

새 이름으로 앱을 게시 한 후에 더 이상 사용할 필요가 없는 이전 이름을 삭제할 수 없습니다.

> [!TIP]
> 이 대 한 예약 된 이름을 사용 하 여 대시보드에서 각 앱이 나타납니다. 앱 이름을 바꾸려면 위의 단계에 따라 새 이름을 사용 하 여 대시보드에 표시 되 게 하려는 경우 ( **앱 이름 관리** 페이지에서 **삭제** 를 클릭) 하 여 원래 이름을 삭제 해야 합니다. 

 

 




