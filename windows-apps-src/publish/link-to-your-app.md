---
author: jnHs
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: 앱에 대한 링크
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 10/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 링크, windows 스토어 프로토콜, 앱 연결, 앱 링크
ms.localizationpriority: medium
ms.openlocfilehash: 0025321aa73a66cc0a976bd347e613de3c3c4765
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2815633"
---
# <a name="link-to-your-app"></a>앱에 대한 링크


Microsoft Store에서 앱의 목록에 연결 하 여 앱을 검색 하는 고객을 할 수 있습니다.

## <a name="getting-the-link-to-your-apps-store-listing"></a>앱의 스토어 목록에 대한 링크 가져오기

앱의 스토어 목록에 대한 URL를 가져오려면 **앱 관리** 섹션의 [앱 ID](view-app-identity-details.md) 페이지를 탐색합니다. URL은 **`https://www.microsoft.com/store/apps/<your app's Store ID>`** 형식으로 되어 있습니다.

고객이 이 링크를 클릭하면 앱에 대한 웹 기반 목록 페이지가 열립니다. 또한 Windows 장치에서 스토어 앱이 시작되고 앱 목록을 표시합니다.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Microsoft 저장소 배지 있는 앱의 저장소 목록에 연결

앱 Microsoft 저장소에는 고객에 게 알려주는 사용자 지정 배지 있는 앱의 목록에 직접 연결할 수 있습니다.

프로그램 배지를 만들려면 [Microsoft 저장소 배지](http://go.microsoft.com/fwlink/p/?LinkID=534236) 페이지를 방문 합니다. 배지 및 링크를 만들려면 앱의 12문자 **Store ID**가 있어야 합니다. **앱 관리** 섹션의 [앱 ID](view-app-identity-details.md) 페이지에서 앱의 **스토어 ID**를 찾을 수 있습니다.

> [!NOTE]
> 정보 및 Microsoft 저장소 배지의 사용에 관련 된 요구 사항에 대 한 [앱 마케팅 지침](app-marketing-guidelines.md) 을 참조 하십시오.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Microsoft Store에서 앱에 직접 연결

Microsoft 저장소를 시작 하 고 사용 하 여 브라우저를 열지 않고도 앱의 목록 페이지에 직접 이동 하는 링크를 만들 수는 **ms windows 저장소:** URI 스키마입니다.

이러한 링크는 사용자가 Windows 장치에 있으며 스토어의 목록 페이지에 직접 도달하고자 하는 경우에 유용합니다. 예를 들어, 브라우저에서 사용자 에이전트 문자열을 확인하여 사용자의 운영 체제가 스토어를 지원한다는 것을 확인한 후나 UWP 앱을 통해 이미 통신을 하고 있을 때 이 링크를 사용하고 싶을 수 있습니다.

이 URI 체계를 사용 하 여 앱의 저장소를 나열 하는 중에 직접 연결할, 하려면이 링크를 앱의 저장소 ID를 추가 합니다.

`ms-windows-store://pdp/?ProductId=`

Microsoft 저장소 프로토콜을 사용 하는 방법에 대 한 자세한 참조 [Microsoft 응용 프로그램을 실행](../launch-resume/launch-store-app.md)합니다.

 

 




