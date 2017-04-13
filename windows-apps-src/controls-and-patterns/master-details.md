---
author: Jwmsft
Description: "마스터/세부 정보 패턴은 현재 선택한 항목에 대한 마스터 목록과 세부 정보를 표시합니다. 이 패턴은 메일 및 연락처 목록/주소록에 자주 사용됩니다."
title: "마스터/세부"
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: d5933eec7b2f34b2c5939bb083113dfd3a1f965d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="masterdetails-pattern"></a>마스터/세부 정보 패턴

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

마스터/세부 정보 패턴에는 마스터 창(일반적으로 [목록 보기](lists.md)와 함께) 및 콘텐츠의 세부 정보 창이 있습니다. 마스터 목록의 항목을 선택하면 세부 정보 창이 업데이트됩니다. 이 패턴은 메일 및 주소록에 자주 사용됩니다.

![마스터/세부 정보 패턴의 예](images/HIGSecOne_MasterDetail.png)

## <a name="is-this-the-right-pattern"></a>올바른 패턴인가요?

다음을 하려는 경우에 마스터/세부 정보 패턴이 제대로 작동합니다.

-   메일 앱, 주소록 또는 목록 세부 정보 레이아웃을 기반으로 하는 앱을 빌드합니다.
-   큰 콘텐츠 컬렉션을 찾아 우선 순위를 지정합니다.
-   컨텍스트 간 앞과 뒤에서 작업하는 동안 목록에 항목을 빠르게 추가하고 목록에서 항목을 제거하도록 허용합니다.

## <a name="choose-the-right-style"></a>올바른 스타일 선택

마스터/세부 정보 패턴을 구현할 때 사용 가능한 화면 공간의 크기에 따라 누적 스타일 또는 가로 정렬 스타일을 사용하는 것이 좋습니다.

| 사용 가능한 창 너비 | 권장 스타일 |
|------------------------|-------------------|
| 320epx - 719epx        | 누적           |
| 720epx 이상       | 가로 정렬      |

 
## <a name="stacked-style"></a>누적 스타일

누적 스타일에서는 마스터 또는 세부 정보 창이 한 번에 하나만 표시됩니다.

![누적 모드의 마스터 세부 정보](images/patterns-md-stacked.png)

사용자는 마스터 창에서 시작하여 마스터 목록에서 항목을 선택함으로써 세부 정보 창으로 "드릴다운"합니다. 사용자에게는 마스터 및 세부 정보 보기가 별도의 두 페이지에 있는 것처럼 나타납니다.

### <a name="create-a-stacked-masterdetails-pattern"></a>누적 마스터/세부 정보 패턴 만들기

누적 마스터/세부 정보 패턴을 만드는 한 가지 방법은 마스터 창 및 세부 정보 창에 별도의 페이지를 사용하는 것입니다. 마스터 목록을 제공하는 목록 보기를 한 페이지에 배치하고 세부 정보 창의 콘텐츠 요소를 별도의 페이지에 배치합니다.

![누적 스타일 마스터 세부 정보에 대한 부분](images/patterns-md-stacked-parts.png)

마스터 창의 경우 이미지와 텍스트를 포함할 수 있는 목록을 표시하는 데 [목록 보기](lists.md) 컨트롤이 적합합니다.

세부 정보 창의 경우 가장 적합한 콘텐츠 요소를 사용합니다. 별도의 필드가 많은 경우 그리드 레이아웃을 사용하여 요소를 양식에 정렬하는 것이 좋습니다.

## <a name="side-by-side-style"></a>가로 정렬 스타일

가로 정렬 스타일에서는 마스터 창과 세부 정보 창이 동시에 표시될 수 있습니다.

![마스터/세부 정보 패턴](images/patterns-masterdetail-400x227.png)

마스터 창의 목록에는 현재 선택한 항목을 나타내는 선택 시각 효과가 있습니다. 마스터 목록에서 새 항목을 선택하면 세부 정보 창이 업데이트됩니다.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>가로 정렬 마스터/세부 정보 패턴 만들기

마스터 창의 경우 이미지와 텍스트를 포함할 수 있는 목록을 표시하는 데 [목록 보기](lists.md) 컨트롤이 적합합니다.

세부 정보 창의 경우 가장 적합한 콘텐츠 요소를 사용합니다. 별도의 필드가 많은 경우 그리드 레이아웃을 사용하여 요소를 양식에 정렬하는 것이 좋습니다.

## <a name="examples"></a>예제

주식 시장을 추적하는 앱의 이 디자인은 마스터/세부 정보 패턴을 사용합니다. 앱의 이 예제에서는 앱이 휴대폰에 표시될 때 마스터 창/목록이 왼쪽에, 세부 정보 창이 오른쪽에 표시됩니다.

![휴대폰에서 마스터 세부 정보 패턴을 사용하는 앱의 예](images/uap-finance-phone-masterdetails-600.png)

주식 시장을 추적하는 앱의 이 디자인은 마스터/세부 정보 패턴을 사용합니다. 앱의 이 예제에서는 앱이 데스크톱에 표시될 때 마스터 창/목록과 세부 정보 창이 둘 다 전체 화면에 표시됩니다. 마스터 창에는 위쪽에 검색 상자가, 아래쪽에 명령 모음이 표시됩니다.

![데스크톱에서 마스터 세부 정보 패턴을 사용하는 앱의 예](images/uap-finance-desktop700.png)

마스터/세부 정보 패턴을 보여 주는 샘플 코드는 다음을 참조하세요.
- [ListView 및 GridView 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619900)
- [RSS 수집기 샘플](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>관련 문서

- [목록](lists.md)
- [검색](search.md)
- [앱 및 명령 모음](app-bars.md)
- [**ListView 클래스(XAML)**](https://msdn.microsoft.com/library/windows/apps/br242878)
