---
description: 지도 스타일 시트의 항목 및 속성
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 지도 스타일시트 참조
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, 지도, 지도 스타일 시트
ms.localizationpriority: medium
ms.openlocfilehash: ec30fd5d63dfa522ef721d2d8a2e4950e6e8e854
ms.sourcegitcommit: c9edb164356c0843d8a9b19ab43707d486e392e1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85377934"
---
# <a name="map-style-sheet-reference"></a>지도 스타일시트 참조

Microsoft 매핑 기술은 [지도 스타일 시트](https://docs.microsoft.com/BingMaps/styling/map-style-sheets) 를 사용 하 여 Windows 스토어 응용 프로그램의 [없습니다](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)표시 된 지도를 포함 하 여 지도 모양을 정의 합니다.  지도 스타일 시트는 [Mapstylesheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) 메서드를 통해 JAVASCRIPT OBJECT NOTATION (JSON)를 사용 하 여 정의 됩니다.

스타일 시트는 맵의 마지막 모양을 변경 하기 위해 속성을 재정의 하는 [항목](https://docs.microsoft.com/BingMaps/styling/map-style-sheet-entries) 으로 구성 됩니다.

예를 들어, 다음 JSON을 사용 하 여 워터 마크를 빨간색으로 표시 하 고, 워터 마크 레이블을 녹색으로 표시 하 고, 육지 영역을 파란색으로 표시할 수 있습니다.

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

스타일 시트는 [지도 스타일 시트 편집기](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) 응용 프로그램을 사용 하 여 대화형으로 만들 수 있습니다.
