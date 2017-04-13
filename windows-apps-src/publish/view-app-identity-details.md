---
author: jnHs
Description: "Windows 개발자 센터 대시보드에서 앱 작업을 할 때 Windows 스토어에서 앱에 할당된 고유 ID와 관련된 세부 정보를 보고 앱의 스토어 목록 링크를 가져올 수 있습니다."
title: "앱 ID 세부 정보 보기"
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: b48f99d4146bfa5e4d9b2af3184e1ce3c1aea491
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="view-app-identity-details"></a>앱 ID 세부 정보 보기


Windows 개발자 센터 대시보드에서 앱 작업을 할 때 Windows 스토어에서 앱에 할당된 고유 ID와 관련된 세부 정보를 보고 앱의 스토어 목록 링크를 가져올 수 있습니다.

이 정보를 찾으려면 앱의 하나로 이동하고 왼쪽 탐색 메뉴에서 **앱 관리**를 확장합니다. **앱 ID** 클릭하여 이들 세부 정보를 표시합니다.

> **참고** 대부분의 경우 ID 세부 정보를 표시하려면 앱용으로 [예약된 이름](create-your-app-by-reserving-a-name.md)이 있어야 합니다.

## <a name="values-to-include-in-your-appx-manifest"></a>appx 매니페스트에 포함할 값


다음 값을 appx 매니페스트에 포함해야 합니다. Microsoft Visual Studio를 사용하여 패키지를 빌드하고 개발자 계정과 연결된 같은 Microsoft 계정으로 로그인하면 이들 세부 정보는 자동으로 포함됩니다. 패키지를 수동으로 빌드할 경우에는 직접 세부 정보를 추가해야 합니다.

-   **패키지/ID/이름**
-   **패키지/ID/게시자**

자세한 내용은 [패키지 매니페스트 스키마 참조](https://msdn.microsoft.com/library/windows/apps/br211473)에서 [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441)를 참조하세요.

이들 요소는 앱의 ID를 선언하여 패키지가 속한 "패키지 패밀리"를 설정합니다. 개별 패키지에는 아키텍처 및 버전과 같은 추가 세부 정보가 포함됩니다.

## <a name="additional-values-for-package-family"></a>패키지 패밀리에 대한 추가 값


다음 값은 앱의 패키지 패밀리를 나타내는 추가적인 값이지만 매니페스트에 포함되지 않습니다.

-   **PFN(패키지 패밀리 이름)**: 이 값은 특정 Windows API와 함께 사용합니다.
-   **패키지 SID**: WNS 알림을 앱에 보내려면 이 값이 필요합니다. 자세한 내용은 [WNS(Windows 푸시 알림 서비스) 개요](https://msdn.microsoft.com/library/windows/apps/mt187203)를 참조하세요.

## <a name="link-to-your-apps-listing"></a>앱 목록 링크

고객이 스토어에서 앱을 찾는 데 도움이 되도록 앱 페이지 링크를 공유할 수 있습니다. 이 링크는 **`https://www.microsoft.com/store/apps/<your app's Store ID>`** 형식입니다.

> **참고** 이 URL은 앱을 사용할 수 있는 모든 OS 버전에 적용됩니다. Windows 8.1 이하 및/또는 Windows Phone 8.1 이하에 대한 추가 링크도 있지만 이러한 링크는 지정된 OS 버전에서만 작동합니다.

고객이 이 링크를 클릭하면 앱에 대한 웹 기반 목록 페이지가 열립니다. 고객의 Windows 디바이스에 앱을 사용할 수 있는 경우 스토어 앱에서 앱의 목록을 시작하고 표시합니다.

앱의 **스토어 ID**도 이 섹션에 표시됩니다. 이 스토어 ID는 [스토어 배지를 생성](http://go.microsoft.com/fwlink/p/?LinkId=534236)하거나 그렇지 않으면 앱을 식별하는 데 사용할 수 있습니다.

 

 




