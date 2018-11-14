---
author: mijacobs
Description: This article describes best practices for creating and displaying app settings.
title: 앱 설정에 대한 지침
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 56a952950fa9f2d9d57d5beaed397dd72f64ea54
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6158226"
---
# <a name="guidelines-for-app-settings"></a>앱 설정에 대한 지침



앱 설정은 앱의 사용자 지정 가능한 부분이고 앱 설정 페이지 내에 있습니다. 예를 들어 사용자는 뉴스 뷰어 앱의 앱 설정을 사용하여 표시할 뉴스 소스를 지정하거나 화면에 표시할 칼럼 수를 지정할 수 있지만, 날씨 앱의 설정을 사용하여 섭씨 및 화씨 중에서 기본 측정 단위를 선택할 수 있습니다. 이 문서에서는 앱 설정을 만들고 표시하기 위한 모범 사례에 대해 설명합니다.


## <a name="should-i-include-a-settings-page-in-my-app"></a>앱에 설정 페이지를 포함해야 하나요?

다음은 앱 설정 페이지에 속한 앱 옵션의 예입니다.

-   날씨 앱에서 온도 기본 단위로 섭씨 또는 화씨 선택, 메일 앱의 계정 설정 변경, 알림 설정 또는 내게 필요한 옵션 등 앱 동작에 영향을 주고 자주 재조정할 필요가 없는 구성 옵션
-   음악, 사운드 효과 또는 색 테마와 같이 사용자의 기본 설정에 따라 달라지는 옵션
-   자주 액세스하지 않는 앱 정보(예: 개인 정보 취급 방침, 도움말, 앱 버전 또는 저작권 정보)

일반적인 앱 워크플로에 속하는 명령(예: 미술 앱에서 브러시 크기 변경)은 설정 페이지에 있으면 안 됩니다. 명령 배치에 대한 자세한 내용은 [명령 디자인 기본 사항](https://msdn.microsoft.com/library/windows/apps/dn958433)을 참조하세요.

## <a name="general-recommendations"></a>일반 권장 사항


-   설정 페이지를 간단하게 유지하고 이진(켜기/끄기) 컨트롤을 사용합니다. [토글 스위치](../controls-and-patterns/toggles.md)는 일반적으로 이진 설정에 가장 적합한 컨트롤입니다.
-   사용자가 함께 사용할 수 없는 최대 5개의 관련 옵션으로 이루어진 집합에서 한 항목을 선택할 수 있는 설정에는 [라디오 단추](../controls-and-patterns/radio-button.md)를 사용합니다.
-   앱 설정 페이지에서 모든 앱 설정에 대한 진입점을 만듭니다.
-   설정을 간단하게 유지합니다. 스마트 기본값을 정의하고 설정 수를 최소한으로 유지합니다.
-   사용자가 설정을 변경할 경우 변경 내용이 앱에 즉시 반영되어야 합니다.
-   일반적인 앱 워크플로에 포함된 명령은 포함하지 마세요.

## <a name="entry-point"></a>진입점


사용자가 앱 설정 페이지에 액세스하는 방법은 앱 레이아웃에 기반을 두어야 합니다.

**탐색 창**

탐색 창 레이아웃에서는 앱 설정이 탐색 선택 항목 목록의 마지막 항목이고 맨 아래에 고정되어야 합니다.

![탐색 창의 앱 설정 진입점](images/appsettings-entrypoint-navpane.png)

**앱 바**

[앱 바](../controls-and-patterns/app-bars.md) 또는 도구 모음을 사용하는 경우, "자세히" 오버플로 메뉴에서 설정 진입점을 마지막 항목으로 배치합니다. 설정 진입점이 눈에 잘 띄어야 하는 앱의 경우에는 오버플로가 아니라 앱 바에 직접 진입점을 배치하세요.

![앱 바의 앱 설정 진입점](images/appsettings-entrypoint-tabs.png)

**허브**

허브 레이아웃을 사용하고 있으면 앱 설정 진입점을 앱 바의 "자세히" 오버플로 메뉴 내부에 배치해야 합니다.

**탭/피벗**

탭 또는 피벗 레이아웃에서는 앱 설정 진입점을 탐색 창 내의 최상위 항목 중 하나로 배치하지 않는 것이 좋습니다. 대신 앱 설정 진입점을 앱 바의 "자세히" 오버플로 메뉴 내부에 배치해야 합니다.

**마스터-세부 정보**

앱 설정 진입점을 마스터-세부 창 내에 깊이 포함하는 대신 마스터 창의 최상위에 마지막 고정 항목으로 설정합니다.

## <a name="layout"></a>레이아웃


데스크톱 및 모바일 모두에서 앱 설정 창은 전체 화면으로 열리고 전체 창을 채워야 합니다. 앱 설정 메뉴가 최대 4개의 최상위 그룹 사이에 있으면 이들 그룹은 한 열씩 아래로 계단식 배열되어야 합니다.

데스크톱:

![데스크톱의 앱 설정 페이지 레이아웃](images/appsettings-layout-navpane-desktop.png)

모바일:

![휴대폰의 앱 설정 페이지 레이아웃](images/appsettings-layout-navpane-mobile.png)

## <a name="color-mode-settings"></a>"컬러 모드" 설정


앱에서 사용자가 앱의 색 모드를 선택하도록 허용하는 경우, "앱 모드 선택" 헤더와 함께 [라디오 단추](../controls-and-patterns/radio-button.md) 또는 [콤보 상자](../controls-and-patterns/lists.md#drop-down-lists)를 사용하여 이러한 옵션을 표시합니다. 포함할 수 있는 옵션
- 밝게
- 어둡게
- Windows 기본값

또 사용자가 현재 기본 앱 모드에 액세스 해 수정할 수 있도록 Windows 설정 앱의 색 페이지에 하이퍼링크를 추가하는 것이 좋습니다. 하이퍼링크 스트링에 "Windows 색 설정"이라는 문자열을 사용하십시오.

!["모드 선택" 섹션](images/appsettings_mode.png)

<!--
<div class="microsoft-internal-note">
Detailed redlines showing preferred text strings for the "Choose a mode" section are available on [UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav/2543/0/dv/?t=Windows%7CControls%7CColorMode&f=RS2).
</div>
-->

## <a name="about-section-and-feedback-button"></a>섹션과 피드백 단추


앱에 전용 페이지나 고유의 섹션으로 "이 앱에 대한 정보" 섹션을 배치하는 것이 좋습니다. "피드백 보내기" 단추가 필요하면 "이 앱에 대한 정보" 페이지의 맨 아래쪽으로 배치합니다.

"법"과 관련된 하위 머리글 아래 "서비스 계약"과 "개인정보 처리 방침"(텍스트 줄 바꿈이 있는 [하이퍼링크 단추](../controls-and-patterns/hyperlinks.md)과 저작권 같은 법과 관련된 추가 정보를 배치합니다.

!["피드백 보내기" 단추가 있는 "이 앱에 대한 정보" 섹션](images/appsettings-about.png)


## <a name="recommended-page-content"></a>권장되는 페이지 콘텐츠


앱 설정 페이지에 포함할 항목 목록이 있으면 다음 지침을 고려해 보세요.

-   유사한 설정이나 관련된 설정을 하나의 설정 레이블 아래에 그룹화합니다.
-   총 설정 수를 최대 4 또는 5로 유지해 보세요.
-   앱 컨텍스트에 관계없이 동일한 설정을 표시합니다. 특정 컨텍스트에서 일부 설정이 관련이 없을 경우 앱 설정 플라이아웃에서 사용하지 않도록 설정합니다.
-   설정에 한 단어로 된 설명 레이블을 사용합니다. 예를 들어 계정 관련 설정의 경우 설정 이름을 "계정 설정" 대신 "계정"으로 지정합니다. 설정에 대해 하나의 옵션만 사용하려는 경우 설정이 설명 레이블에 적합하지 않으면 "옵션" 또는 "기본값"을 사용합니다.
-   설정이 플라이아웃 대신 웹에 직접 연결된 경우 사용자가 시각 신호를 알 수 있도록 합니다(예: [하이퍼링크](../controls-and-patterns/hyperlinks.md)로 스타일이 지정된 "도움말(온라인)" 또는 "웹 포럼"). 웹에 대한 여러 링크를 단일 설정이 있는 플라이아웃으로 그룹화합니다. 예를 들어 "정보" 설정은 서비스 계약, 개인 정보 취급 방침 및 앱 지원에 대한 링크로 플라이아웃을 엽니다.
-   더 일반적인 설정이 각각 자체 진입점을 가질 수 있도록 덜 사용하는 설정을 단일 진입점으로 결합합니다. "정보" 설정에는 정보만 포함된 콘텐츠나 링크를 배치합니다.
-   "사용 권한" 창에서 기능을 중복하지 마세요. 이 창은 Windows에서 기본적으로 제공되며 수정할 수 없습니다.

-   설정 플라이아웃에 설정 콘텐츠 추가
-   필요한 경우 스크롤 가능한 단일 열에 위쪽에서 아래쪽으로 콘텐츠를 제공합니다. 스크롤을 화면 높이의 최대 두 배로 제한합니다.
-   앱 설정에 다음 컨트롤을 사용합니다.

    -   [토글 스위치](../controls-and-patterns/toggles.md): 사용자가 값을 설정하거나 해제할 수 있습니다.
    -   [라디오 단추](../controls-and-patterns/radio-button.md): 사용자가 최대 5개의 상호 배타적인 관련 옵션으로 이루어진 집합에서 한 항목을 선택할 수 있습니다.
    -   [텍스트 입력 상자](../controls-and-patterns/text-block.md): 사용자가 텍스트를 입력할 수 있습니다. 메일이나 암호와 같이, 사용자로부터 얻는 텍스트 형식에 해당하는 텍스트 입력 상자 형식을 사용합니다.
    -   [하이퍼링크](../controls-and-patterns/hyperlinks.md): 사용자를 앱 내의 다른 페이지나 외부 웹 사이트로 이동합니다. 사용자가 하이퍼링크를 클릭하면 설정 플라이아웃이 해제됩니다.
    -   [단추](../controls-and-patterns/buttons.md): 사용자가 현재 설정 플라이아웃을 해제하지 않고 즉각적인 작업을 시작할 수 있습니다.
-   컨트롤 중 하나가 비활성화되면 설명 메시지를 추가합니다. 이 메시지를 비활성화된 컨트롤 위에 배치합니다.
-   설정 플라이아웃 및 헤더에 애니메이션 효과를 준 후 단일 블록으로 콘텐츠 및 컨트롤에 애니메이션 효과를 줍니다. 100px 왼쪽 오프셋과 함께 [**enterPage**](https://msdn.microsoft.com/library/windows/apps/br212672) 또는 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288) 애니메이션을 사용하여 콘텐츠에 애니메이션 효과를 줍니다.
-   필요한 경우 구역 머리글, 단락 및 레이블을 사용하여 콘텐츠를 구성하고 명확하게 표시합니다.
-   설정을 반복해야 하는 경우 추가 UI 수준이나 확장/축소 모델을 사용하되 두 수준보다 더 깊은 계층 구조는 사용하지 않습니다. 예를 들어 도시별 설정을 제공하는 날씨 앱은 도시를 나열하고 사용자가 도시를 탭하여 새 플라이아웃을 열거나 설정 옵션을 확장하여 표시하도록 할 수 있습니다.
-   컨트롤 또는 웹 콘텐츠를 로드하는 데 시간이 걸리는 경우 확정되지 않은 진행률 컨트롤을 사용하여 정보가 로드되고 있음을 사용자에게 표시합니다. 자세한 내용은 [진행률 컨트롤에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465469)을 참조하세요.
-   탐색이나 변경 내용 커밋에 단추를 사용하지 마세요. 하이퍼링크를 사용하여 다른 페이지를 탐색하고, 단추를 사용하여 변경 내용을 커밋하는 대신 사용자가 설정 플라이아웃을 해제할 때 변경 내용을 앱 설정에 자동으로 저장합니다.



## <a name="related-articles"></a>관련 문서

* [명령 디자인 기본 사항](https://msdn.microsoft.com/library/windows/apps/dn958433)
* [진행률 컨트롤에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465469)
* [앱 데이터 저장 및 검색](https://msdn.microsoft.com/library/windows/apps/mt299098)
* [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288)
