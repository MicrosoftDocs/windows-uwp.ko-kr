---
author: jnHs
Description: "앱의 스토어 목록에 연결하면 고객이 앱을 검색하는 데 도움을 줄 수 있습니다."
title: "앱에 대한 링크"
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 59f19dbf0cd66679805a9fcf3054427a22fb0e8f
ms.lasthandoff: 02/07/2017

---

# <a name="link-to-your-app"></a>앱에 대한 링크


앱의 스토어 목록에 연결하면 고객이 앱을 검색하는 데 도움을 줄 수 있습니다.

## <a name="getting-the-link-to-your-apps-store-listing"></a>앱의 스토어 목록에 대한 링크 가져오기


대시보드 내 각 앱에 대한 **앱 관리** 섹션의 [앱 ID](view-app-identity-details.md) 페이지에서 앱의 스토어 목록에 대한 링크를 찾을 수 있습니다.

이 링크는 **`https://www.microsoft.com/store/apps/<your app's Store ID>`** 형식입니다.

고객이 이 링크를 클릭하면 앱에 대한 웹 기반 목록 페이지가 열립니다. 고객의 디바이스에 앱을 사용할 수 있는 경우 스토어 앱에서 앱의 목록을 시작하고 표시합니다.

> **참고**  대상으로 하는 OS 버전에 따라 둘 이상의 링크가 있을 수도 있습니다. 모든 앱에는 모든 OS에 작동하는 Windows 10용 URL이 표시됩니다. Windows 8.1 이하 및/또는 Windows Phone 8.1 이하에 대한 추가 링크도 있지만 이러한 링크는 지정된 OS 버전에서만 작동합니다.

 

## <a name="linking-to-your-apps-store-listing-with-the-windows-store-badge"></a>Windows 스토어 배지를 사용하여 앱의 스토어 목록에 연결


고객이 Windows 스토어에서 앱을 확인할 수 있도록 사용자 지정 배지를 사용하여 앱의 목록에 직접 연결할 수 있습니다.

배지를 만들려면 [Windows 스토어 배지](http://go.microsoft.com/fwlink/p/?LinkID=534236) 페이지를 방문하세요. 이 양식을 사용하여 배지 및 링크를 생성하는 데 사용하려면 앱의 스토어 ID가 있어야 합니다. 이 ID는 **앱 관리** 섹션의 [앱 ID](view-app-identity-details.md) 페이지에 표시된 **Windows 10용 URL**의 마지막 12자입니다.

> **참고**  Windows 스토어 배지 사용에 대한 자세한 내용은 [앱 마케팅 지침](app-marketing-guidelines.md)을 참조하세요.

 

## <a name="linking-directly-to-your-app-in-the-windows-store"></a>Windows 스토어에서 앱에 직접 연결


**ms-windows-store:** URI 스키마를 사용하여 브라우저를 열지 않고도 Windows 스토어를 시작하고 앱의 목록 페이지로 직접 이동하는 링크를 만들 수 있습니다.

사용자가 Windows 디바이스에 있으며 스토어의 목록 페이지에 직접 연결되도록 설정하려는 경우에 이러한 링크가 유용합니다. 예를 들어 브라우저에서 사용자 에이전트 문자열을 검사하여 사용자의 운영 체제를 확인한 후 또는 UWP 앱을 통해 이미 통신하고 있는 경우 이 프로토콜을 적용할 수 있습니다.

Windows 스토어 프로토콜을 사용하여 앱의 스토어 목록에 직접 연결하려면 앱의 스토어 ID를 이 링크에 추가합니다.

`ms-windows-store://pdp/?ProductId=`

Windows 스토어 프로토콜을 사용하는 방법에 대한 자세한 내용은 [Windows 스토어 앱 시작](../launch-resume/launch-store-app.md)을 참조하세요.

 

 





