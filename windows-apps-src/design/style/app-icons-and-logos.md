---
Description: 시작 메뉴, 앱 타일, 작업 표시줄, Microsoft Store 등에서 앱을 나타내는 앱 아이콘/로고를 만드는 방법을 알아봅니다.
title: 앱 아이콘 및 로고
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 31b90866a0f612fb8f488d11e7d989380f14da99
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63784879"
---
# <a name="app-icons-and-logos"></a>앱 아이콘 및 로고 

모든 앱은 자신을 나타내는 아이콘/로고가 있으며, 아이콘은 Windows 셸의 여러 위치에서 표시됩니다. 

:::row:::
    :::column:::
        * 앱 창의 제목 표시줄
        * 시작 메뉴의 앱 목록
        * 작업 표시줄 및 작업 관리자
        * 앱의 타일
        * 앱의 시작 화면
        * Microsoft Store에서
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

이 문서에서는 앱 아이콘 만들기의 기본 사항, Visual Studio를 사용하여 앱 아이콘을 관리하는 방법, 필요한 경우 수동으로 앱 아이콘을 관리하는 방법을 다룹니다.
 
(이 문서는 앱 자체를 나타내는 아이콘을 위해 특별히 작성되었으며, 일반 아이콘 지침은 [아이콘](icons.md) 문서를 참조하세요.)

## <a name="icon-types-locations-and-scale-factors"></a>아이콘 유형, 위치 및 배율

기본적으로 Visual Studio는 아이콘 자산을 자산 하위 디렉터리에 저장합니다. 다음은 여러 유형의 아이콘, 표시되는 위치 및 이름 목록입니다. 

| 아이콘 이름 | 표시 위치 | 자산 파일 이름 |
| ---      | ---        | --- |
| 작은 타일 | 시작 메뉴 |  SmallTile.png  |
| 중간 타일 |시작 메뉴, Microsoft Store 목록\*  |  Square150x150Logo.png |
| 와이드 타일  | 시작 메뉴   | Wide310x150Logo.png |
| 큰 타일   | 시작 메뉴, Microsoft Store 목록\* |  LargeTile.png  |
| 앱 아이콘 | 시작 메뉴, 작업 표시줄, 작업 관리자의 앱 목록 | Square44x44Logo.png |
| 시작 화면 | 앱의 시작 화면 | SplashScreen.png  |
| 배지 로고 | 앱의 타일 | BadgeLogo.png  |
| 패키지 로고/Microsoft Store 로고 | 앱 설치 관리자, 파트너 센터, Microsoft Store의 "앱 보고" 옵션, Microsoft Store의 "리뷰 작성" 옵션 | StoreLogo.png  |

\* [업로드된 이미지만 Microsoft Store에 표시](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)하도록 선택하지 않는 이상 이러한 아이콘이 사용됩니다. 

이러한 아이콘이 모든 화면에서 선명하게 보이게 하려면 여러 표시 배율을 대비하여 동일한 아이콘의 여러 버전을 만들면 됩니다. 

배율에 따라 텍스트 같은 UI 요소의 크기가 결정됩니다. 배율 범위는 100%~400%입니다. 값이 클수록 더 큰 UI 요소가 생성되므로 DPI가 높은 디스플레이에서 잘 보입니다. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


앱 아이콘 자산은 비트맵이고 비트맵은 확장이 어렵기 때문에 아이콘 자산마다 각 배율 100%, 125%, 150%, 200% 및 400%에 대한 버전을 제공하는 것이 좋습니다. 그러면 아이콘 수가 많아집니다! 다행히도 Visual Studio는 이러한 아이콘을 쉽게 생성하고 업데이트할 수 있는 도구를 제공합니다. 

## <a name="microsoft-store-listing-image"></a>Microsoft Store 목록 이미지

"Microsoft Store에서 앱 목록의 이미지를 지정하려면 어떻게 해야 하나요"?

기본적으로 이 페이지의 맨 위에 있는 표에 설명된 것처럼 Microsoft Store의 패키지에 있는 이미지를 사용합니다([제출 프로세스 중에 사용자가 제공하는 다른 이미지](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)와 함께). 하지만 Windows 10(Xbox 포함)의 고객에게 목록을 표시할 때 Microsoft Store에서 앱 패키지의 로고 이미지를 사용하지 못하도록 하고, 그 대신 사용자가 업로드한 이미지만 사용하게 할 수도 있습니다. 이렇게 하면 Microsoft Store의 다양한 디스플레이에서 앱 모양을 훨씬 정교하게 제어할 수 있습니다. (제품에서 이전 OS 버전을 지원하는 경우 이 옵션을 사용하더라도 패키지의 이미지가 계속 표시될 수 있습니다.) 이 작업은 제출 프로세스 중에 **Microsoft Store 목록** 단계의 **Microsoft Store 로고** 섹션에서 수행할 수 있습니다.

![앱 제출 프로세스 중에 Microsoft Store 로고 지정](images/app-icons/storelogodisplay.png)

이 확인란을 선택하면 **Microsoft Store 디스플레이 이미지**라는 새 섹션이 나타납니다. 여기서 Microsoft Store가 앱 패키지의 로고 이미지 자리에 사용할 3가지 이미지 크기 300 x 300, 150 x 150 및 71 x 71 픽셀을 업로드할 수 있습니다. 이 중에서 300 x 300 크기만 필수이지만, 3가지 크기를 모두 제공하는 것이 좋습니다.

자세한 내용은 [업로드된 로고 이미지만 Microsoft Store에 표시](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)를 참조하세요.

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Visual Studio 매니페스트 디자이너를 사용하여 앱 아이콘 관리

Visual Studio는 앱 아이콘을 관리할 수 있는 **매니페스트 디자이너**라는 매우 유용한 도구를 제공합니다. 

> 아직 Visual Studio 2017이 없는 경우 체험 버전(Visual Studio 2017 Community Edition)을 비롯한 여러 버전이 제공되며, 다른 버전은 평가판을 제공합니다. [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)에서 다운로드할 수 있습니다.


매니페스트 디자이너를 시작하려면 다음을 수행합니다.
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Visual Studio를 사용하여 UWP 프로젝트를 엽니다.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. **솔루션 탐색기**에서 Package.appmxanifest 파일을 두 번 클릭합니다.
    :::column-end:::
    :::column:::
        ![The Visual Studio 2017 Manifest Designer](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio displays the Manifest Designer.
    :::column-end:::
    :::column:::
            ![The Visual Assets tab](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. **시각적 자산** 탭을 클릭합니다.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>모든 자산을 한 번에 생성

**시각적 자산** 탭의 첫 번째 메뉴 항목인 **모든 시각적 자산**은 그 이름에서 추측할 수 있듯이 단추를 한 번 누르는 것만으로 앱에 필요한 모든 시각적 자산을 생성합니다.

![Visual Studio의 모든 시각적 자산을 생성](images/app-icons/all-visual-assets.png)

사용자는 이미지 하나만 제공하면 됩니다. 그러면 Visual Studio가 모든 배율에 맞는 작은 타일, 중간 타일, 큰 타일, 와이드 타일, 앱 아이콘, 시작 화면 및 패키지 로고 자산을 생성합니다.

모든 자산을 한 번에 생성하려면 다음을 수행합니다.
1. **원본** 필드 옆에 있는 **...** 를 클릭하고 사용하려는 이미지를 선택합니다. 비트맵 이미지를 사용하는 경우 이미지가 선명하게 표시되려면 적어도 400 x 400 픽셀 이상이어야 합니다. 벡터 기반 이미지가 가장 좋습니다. Visual Studio는 AI(Adobe Illustrator) 및 PDF 파일을 사용할 수 있습니다. 
2. (선택 사항) **디스플레이 설정** 섹션에서 다음 옵션을 구성합니다.

    a.  **짧은 이름**:  앱의 짧은 이름을 지정합니다.

    b.  **이름 표시**: 중간 타일, 와이드 타일 또는 큰 타일에 짧은 이름을 표시할 것인지 여부를 나타냅니다. 

    c. **타일 배경**: 타일 배경색의 16진수 값 또는 색 이름을 지정합니다. 예를 들면 `#464646`입니다. 기본값은 `transparent`입니다.

    d. **시작 화면 배경**: 시작 화면 배경의 16진수 값 또는 색 이름을 지정합니다. 

3. **생성**을 클릭합니다. 

Visual Studio가 이미지 파일을 생성하여 프로젝트에 추가합니다. 자산을 변경하려면 프로세스를 반복하면 됩니다. 

배율이 조정된 자산 아이콘은 다음과 같은 파일 명명 규칙을 따릅니다.

*파일 이름*-scale-*배율*.png

예를 들면

Square150x150Logo-scale-100.png, Square150x150Logo-scale-200.png, Square150x150Logo-scale-400.png

Visual Studio는 기본적으로 배지 로고를 생성하지 않습니다. 배지 로고는 고유하며 다른 앱 아이콘과 일치하면 안 되기 때문입니다. 자세한 내용은 [UWP 앱에 대한 배지 알림](/windows/uwp/design/shell/tiles-and-notifications/badges) 문서를 참조하세요. 


## <a name="more-about-app-icon-assets"></a>앱 아이콘 자산에 대한 자세한 정보
Visual Studio는 프로젝트에 필요한 모든 앱 아이콘 자산을 생성하지만, 앱 아이콘 자산을 사용자 지정하려면 앱 아이콘 자산이 다른 앱 자산과 어떻게 다른지 알아두는 것이 좋습니다. 

앱 아이콘 자산은 Windows 작업 표시줄, 작업 보기, ALT+TAB, 시작 타일의 오른쪽 아래 모서리 등 다양한 장소에 표시됩니다. 앱 아이콘 자산은 정말 많은 장소에 표시되므로 다른 자산에는 없는 크기 조정 옵션인 "대상 크기" 자산과 "플레이트되지 않은" 자산이 있습니다. 

### <a name="target-size-app-icon-assets"></a>대상 크기 앱 아이콘 자산
표준 배율 크기("Square44x44Logo.scale-400.png") 외에도 "대상 크기" 자산을 만드는 것이 좋습니다. 이러한 자산 대상 크기를 호출하는 이유는 400 같은 특정 배율이 아닌 16픽셀 같은 특정 크기를 대상으로 하기 때문입니다. 대상 크기 자산은 배율 수준 시스템을 사용하지 않는 화면에 사용됩니다.

* 점프 목록 시작(데스크톱)
* 타일의 하단 모서리 시작(데스크톱)
* 바로 가기 키(데스크톱)
* 제어판(데스크톱)

다음은 대상 크기 자산 목록입니다.


| 자산 크기 | 파일 이름 예제                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

\* 적어도 이러한 크기를 제공하는 것이 좋습니다. 

이러한 자산에 안쪽 여백을 추가하지 않아도 됩니다. 필요한 경우 Windows에서 자동으로 안쪽 여백을 추가합니다. 이러한 자산에서는 16픽셀의 최소 공간을 고려해야 합니다. 

다음은 Windows 작업 표시줄의 아이콘에 나타나는 이러한 자산의 예입니다.

![Windows 작업 표시줄의 자산](images/assetguidance21.png)

### <a name="unplated-assets"></a>플레이트되지 않은 자산
기본적으로 Windows는 색이 지정된 백플레이트 위에 대상 기반 자산을 사용합니다. 원한다면 대상 기반의 플레이트되지 않은 자산을 제공할 수 있습니다. "플레이트되지 않은"이란 자산이 투명한 배경에 표시된다는 것을 의미합니다. 이러한 자산은 다양한 배경색 위에 표시된다는 것을 기억해 두세요. 

![판이 없는 자산 및 판이 있는 자산](images/assetguidance22.png)

다음은 플레이트되지 않은 앱 아이콘 자산을 사용하는 화면입니다.
* 작업 표시줄 및 작업 표시줄 미리 보기(데스크톱)
* 작업 표시줄 점프 목록
* 작업 보기
* Alt+Tab

### <a name="unplated-assets-and-themes"></a>플레이트되지 않은 자산 및 테마

사용자가 선택한 테마에 따라 작업 표시줄의 색이 결정됩니다. 플레이트되지 않은 자산이 현재 테마로 한정되지 않은 경우 시스템에서는 자산의 대비를 확인합니다. 현재 자산과 작업 표시줄의 대비가 충분한 경우 시스템에서는 현재 자산을 사용합니다. 충분하지 않으면 시스템에서는 고대비 버전의 자산을 찾습니다. 고대비 자산을 찾을 수 없으면 시스템에서는 플레이트된 형식의 자산을 그립니다. 


### <a name="target-and-unplated-sizing"></a>대상 및 플레이트되지 않은 크기 조정

100% 배율에서 대상 기반 자산에 권장하는 크기는 다음과 같습니다.

![100% 배율의 대상 기반 자산 크기 조정](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>시작 화면 자산에 대한 자세한 정보
시작 화면에 대한 자세한 내용은 [UWP 시작 화면](/windows/uwp/launch-resume/splash-screens) 문서를 참조하세요.

## <a name="more-about-badge-logo-assets"></a>배지 로고 자산에 대한 자세한 정보

자산 생성기를 사용하여 필요한 모든 자산을 생성하는 경우 자산 생성기가 기본적으로 배지 로고를 생성하지 않는 이유가 있습니다. 배지 로고는 다른 앱 자산과 매우 다릅니다. 배지 로고는 알림 및 앱의 타일에 표시되는 상태 이미지입니다. 

자세한 내용은 [UWP 앱에 대한 배지 알림](/windows/uwp/design/shell/tiles-and-notifications/badges) 문서를 참조하세요.


## <a name="customizing-asset-padding"></a>자산 안쪽 여백 사용자 지정

기본적으로 Visual Studio 자산 생성기는 모든 이미지에 권장 안쪽 여백을 적용합니다. 이미지에 이미 안쪽 여백이 있거나 타일의 끝까지 확장되는 전체 화상 물림 재단 이미지를 원하는 경우 **권장 안쪽 여백 적용** 확인란을 선택 취소하여 이 기능을 해제할 수 있습니다. 

### <a name="tile-padding-recommendations"></a>타일 안쪽 여백 권장 사항
사용자 고유의 안쪽 여백을 제공하려는 경우 타일의 권장 사항은 다음과 같습니다. 

작은 타일(71 x 71), 중간 타일(150 x 150), 와이드 타일(310 x 150), 큰 타일(310 x 310)의 4가지 타일 크기가 있습니다. 

각 타일 자산은 해당 자산이 배치되는 타일 크기와 동일합니다.

![전체 화상 물림 재단을 보여주는 타일](images/app-icons/tile-assets1.png)

아이콘이 타일의 가장자리까지 확장되는 것을 원하지 않으면 자산에 투명 픽셀을 사용하여 안쪽 여백을 만들면 됩니다. 

![타일 및 뒷판](images/assetguidance05.png)

작은 타일의 경우 아이콘 너비와 높이를 타일 크기의 66%로 제한합니다.

![작은 타일 크기 조정 비율](images/assetguidance09.png)

중간 크기 타일의 경우 타일 크기의 66%로 아이콘 너비를 제한하고 50%로 높이를 제한합니다. 그러면 브랜딩 표시줄에서 요소가 겹치지 않게 됩니다.

![중간 크기 타일 크기 조정 비율](images/assetguidance10.png)

와이드 타일의 경우 타일 크기의 66%로 아이콘 너비를 제한하고 높이를 50%로 제한합니다. 그러면 브랜딩 표시줄에서 요소가 겹치지 않게 됩니다.

![와이드 타일 크기 조정 비율](images/assetguidance11.png)

큰 타일의 경우 아이콘 너비를 타일 크기의 66%로 제한하고 높이를 50%로 제한합니다.

![큰 타일 크기 비율](images/assetguidance12.png)

일부 아이콘은 가로 또는 세로 방향이 되도록 디자인되었지만 또 다른 아이콘은 대상 크기 내에서 정사각형으로 배치할 수 없는 더 복잡한 모양을 가집니다. 가운데에 표시되는 아이콘이 한쪽으로 기울 수 있습니다. 이런 경우 정사각형에 맞는 아이콘과 동일한 시각적 공간을 차지하면 아이콘의 일부가 권장 공간 외부에 표시될 수 있습니다.

![가운데 배치된 세 개의 아이콘](images/assetguidance13.png)

풀 블리드 자산을 사용할 경우 여백 및 타일의 가장자리 내에서 조작하는 계정 요소를 고려합니다. 타일의 너비 또는 높이의 최소 16%로 여백을 유지 관리합니다. 이 백분율은 가장 작은 타일 크기에서는 여백 너비의 두 배를 나타냅니다.

![여백이 있는 풀 블리드 타일](images/assetguidance14.png)

이 예제에서는 여백이 너무 작습니다.

![여백이 너무 적은 풀 블리드 타일](images/assetguidance15.png)


## <a name="optimizing-for-specific-themes-languages-and-other-conditions"></a>특정 테마, 언어 및 기타 조건의 최적화 

이 문서에서는 특정 배율에 맞는 자산을 만드는 방법을 설명했지만, 다양한 조건 및 조건 조합에 맞는 자산을 만들 수도 있습니다. 예를 들어 고대비 디스플레이나 밝은 테마 및 어두운 테마의 아이콘을 만들 수 있습니다. 심지어 특정 언어에 대한 자산도 만들 수 있습니다.

자세한 지침은 [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)을 참조하세요.













