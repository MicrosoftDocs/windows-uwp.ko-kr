---
author: Jwmsft
Description: "사진 뷰어/편집기, 문서 뷰어, 지도, 그리기 또는 자유 스크롤 보기를 사용하는 기타 앱과 같은 단일 보기 앱 또는 모달 환경에 대한 콘텐츠 영역 및 명령 영역이 있는 패턴입니다."
title: "활성 캔버스 레이아웃 패턴"
ms.assetid: 4D768472-64D6-406C-9E87-F750F6B981A0
label: TBD
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 56b2a2c5a00ee81da296e724ed883e974f5796c1
ms.lasthandoff: 02/07/2017

---
# <a name="active-canvas-layout-pattern"></a>활성 캔버스 레이아웃 패턴

활성 캔버스는 콘텐츠 영역 및 명령 영역이 있는 패턴입니다. 활성 캔버스는 사진 뷰어/편집기, 문서 뷰어, 지도, 그리기 또는 자유 스크롤 보기를 사용하는 기타 앱과 같은 단일 보기 앱 또는 모달 환경용입니다. 작업 수행의 경우 필요한 작업 수와 유형에 따라 활성 캔버스를 명령 모음과 연결하거나 단추와만 연결할 수도 있습니다.

## <a name="examples"></a>예제

사진 편집 앱의 이 디자인에서는 왼쪽에 모바일 예가 있고 오른쪽에 데스크톱 예가 있는 활성 캔버스 패턴을 갖추고 있습니다. 이미지 편집 화면은 캔버스이며, 아래쪽의 명령 모음에는 앱에 대한 상황별 작업이 모두 포함되어 있습니다.

![활성 캔버스 패턴을 사용하는 사진 편집기의 예](images/uap-photo-pc-phone-700.png)

이 지하철 지도 앱 디자인에서는 단 두 개의 작업과 검색 상자 하나만 있는 간단한 UI 스트립이 위쪽에 있는 활성 캔버스를 사용합니다. 상황별 작업은 오른쪽 이미지와 같이 상황에 맞는 메뉴에 표시됩니다.

![활성 캔버스 패턴을 사용하는 지도 앱의 예](images/uap-subway-pc-phone-700.png)


## <a name="implementing-this-pattern"></a>이 패턴 구현

활성 캔버스 패턴은 콘텐츠 영역 및 명령 영역으로 구성됩니다.

**콘텐츠 영역입니다.**  콘텐츠 영역은 일반적으로 자유 스크롤 캔버스입니다. 한 앱 내에 여러 콘텐츠 영역이 있을 수 있습니다.

**명령 영역입니다.**  많은 명령을 배치하는 경우 화면 크기에 따라 반응하는 명령 모음이 최상의 옵션이 될 수 있습니다. 그렇게 많은 명령을 배치하지 않고 반응형 UI에 관심이 없는 경우 공간을 절약하는 단추가 잘 작동합니다.



## <a name="related-articles"></a>관련 문서

-   [**앱 바 및 명령 모음**](../controls-and-patterns/app-bars.md)

