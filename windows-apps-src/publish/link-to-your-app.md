---
Description: 고객은 Microsoft Store 앱의 목록에 연결 하 여 앱을 검색할 수 있습니다.
title: 앱에 대한 링크
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, 링크, windows 스토어 프로토콜, 앱 연결, 앱 링크
ms.localizationpriority: medium
ms.openlocfilehash: 56bc051c3c5a935f3b6b26e478731fcde9c06902
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661538"
---
# <a name="link-to-your-app"></a>앱에 대한 링크


고객은 Microsoft Store 앱의 목록에 연결 하 여 앱을 검색할 수 있습니다.

## <a name="getting-the-link-to-your-apps-store-listing"></a>앱의 스토어 목록에 대한 링크 가져오기

앱의 스토어 목록에 대한 URL를 가져오려면 **앱 관리** 섹션의 [앱 ID](view-app-identity-details.md) 페이지를 탐색합니다. URL은 **`https://www.microsoft.com/store/apps/<your app's Store ID>`** 형식으로 되어 있습니다.

고객이 이 링크를 클릭하면 앱에 대한 웹 기반 목록 페이지가 열립니다. 또한 Windows 장치에서 스토어 앱이 시작되고 앱 목록을 표시합니다.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Microsoft Store 배지를 사용 하 여 앱의 스토어 목록에 연결

Microsoft Store 앱이 고객에 게 알려주는 배지를 사용자 지정을 사용 하 여 앱의 목록에 직접 연결할 수 있습니다.

배지를 만들려면 다음을 방문 합니다 [Microsoft Store 배지](https://go.microsoft.com/fwlink/p/?LinkID=534236) 페이지입니다. 배지 및 링크를 만들려면 앱의 12문자 **Store ID**가 있어야 합니다. **앱 관리** 섹션의 [앱 ID](view-app-identity-details.md) 페이지에서 앱의 **스토어 ID**를 찾을 수 있습니다.

> [!NOTE]
> 참조 [지침을 마케팅 하는 앱](app-marketing-guidelines.md) 정보 및 Microsoft Store 배지 사용에 관련 된 요구 사항에 대 한 합니다.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Microsoft Store 앱에 직접 연결

Microsoft Store 시작 하 고 사용 하 여 브라우저를 열지 않고 앱의 목록 페이지로 직접 이동 하는 링크를 만들 수는 **ms-windows-스토어:** URI 체계입니다.

이러한 링크는 사용자가 Windows 장치에 있으며 스토어의 목록 페이지에 직접 도달하고자 하는 경우에 유용합니다. 예를 들어, 브라우저에서 사용자 에이전트 문자열을 확인하여 사용자의 운영 체제가 스토어를 지원한다는 것을 확인한 후나 UWP 앱을 통해 이미 통신을 하고 있을 때 이 링크를 사용하고 싶을 수 있습니다.

이 URI 체계를 사용 하 여 나열 되는 앱의 저장소에 직접 링크를 앱의 Store ID이 링크를 추가 합니다.

`ms-windows-store://pdp/?ProductId=`

Microsoft Store 프로토콜을 사용 하는 방법에 대 한 자세한 내용은 참조 하세요 [Microsoft 앱을 시작](../launch-resume/launch-store-app.md)합니다.

 

 




