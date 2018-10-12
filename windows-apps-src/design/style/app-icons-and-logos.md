---
author: mijacobs
Description: How to create app icons/logos that represent your app in the Start menu, app tiles, the taskbar, the Microsoft Store, and more.
title: 앱 아이콘 및 로고
template: detail.hbs
ms.author: mijacobs
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 04263122c1a96aadc5e4d0ad8f804730d3a2a20f
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4563669"
---
# <a name="app-icons-and-logos"></a>앱 아이콘 및 로고 

모든 앱을 나타내는 아이콘/로고 있으며 Windows 셸과에서 여러 위치에서 해당 아이콘이 표시 됩니다. 

:::row:::
    :::column:::
        * 앱 창의 제목 표시줄
        * 시작 메뉴에서 앱 목록
        * 작업 표시줄 및 작업 관리자
        * 앱의 타일
        * 앱의 시작 화면
        * Microsoft 스토어에서
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

이 문서에서는 앱 아이콘 만들기의 기본 사항이 필요할 경우 Visual Studio, 관리 하 고 어떻게 수동으로 관리를 사용 하는 방법입니다.
 
(이 문서는 앱 자체를 나타내는, 일반 아이콘 지침에 대 한 [아이콘](icons.md) 문서를 참조 하는 아이콘에 특별히 임)

## <a name="icon-types-locations-and-scale-factors"></a>아이콘 유형, 위치 및 배율 인수

기본적으로 Visual Studio는 자산 하위 디렉터리에서 아이콘 자산을 저장합니다. 다음은 나타나는, 아이콘 및 라고 어떻게 여러 종류의 목록입니다. 

| 아이콘 이름 | 에 표시 됩니다. | 자산 파일 이름 |
| ---      | ---        | --- |
| 작은 타일 | 시작 메뉴 |  SmallTile.png  |
| 중간 크기 타일 |시작 메뉴, Microsoft Store listing\ *  |  Square150x150Logo.png |
| 와이드 타일  | 시작 메뉴   | Wide310x150Logo.png |
| 큰 타일   | 시작 메뉴, Microsoft Store listing\ * |  LargeTile.png  |
| 앱 아이콘 | 작업 관리자, 시작 메뉴 및 작업 표시줄에서 앱 목록 | Square44x44Logo.png |
| 시작 화면 | 앱의 시작 화면 | SplashScreen.png  |
| 배지 로고 | 앱의 타일 | BadgeLogo.png  |
| 패키지 로고/스토어 로고 | 앱 설치 관리자, 개발자 센터, 스토어에서 "리뷰 를" 옵션을 스토어에서 "앱을 보고" 옵션 | StoreLogo.png  |

\ * [디스플레이 에서만 스토어에서 이미지를](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)업로드 선택 하지 않는 한 사용. 

이러한 아이콘이 모든 화면에 대해 선명 되도록 다양 한 디스플레이 배율 인수에 대 한 동일한 아이콘의 여러 버전을 만들 수 있습니다. 

배율 인수는 텍스트와 같은 UI 요소의 크기를 결정합니다. 400%에서 100% 요소 범위를 조정 합니다. 값이 클수록 더 큰 UI 요소를 쉽게 높은 DPI 디스플레이에서 볼 수 있도록 만듭니다. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


앱 아이콘 자산은 비트맵 비트맵 원활 하 게 조정 되므로 각 배율 인수에 대 한 각 아이콘 자산 버전을 제공 하는 것이 좋습니다: 100%, 125%, 150%, 200% 및 400%입니다. 아이콘의이! Visual Studio Fortunatly 간편 하 게 생성 하 고이 아이콘을 업데이트 하는 도구를 제공 합니다. 

## <a name="microsoft-store-listing-image"></a>Microsoft Store 목록 이미지

"지정 하는 방법 이미지 내 앱 목록에 대 한 Microsoft Store에서"?

기본적으로 사용 하 여 패키지에서 이미지의 일부가 스토어에서 다른 [제출 프로세스 중에 제공 된 이미지](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)) (함께이 페이지의 맨 표에 설명 된 대로 합니다. 그러나 스토어 (Xbox 포함), Windows 10의 고객에 게 목록을 표시할 때 앱의 패키지의 로고 이미지를 사용 하는 것을 방지 하 고 대신 업로드 한 이미지만 사용 스토어가 있습니다. 이 스토어에서 다양 한 디스플레이에 앱의 모양 보다 잘 제어를 제공 합니다. (단 제품 이전 OS 버전을 지 원하는 경우 해당 고객 수 여전히 이미지, 패키지에서이 옵션을 사용 하는 경우에.) **스토어 로고** 섹션 제출 프로세스의 **스토어 목록** 단계에서이 수행할 수 있습니다.

![앱 제출 프로세스 동안 스토어 로고를 지정합니다.](images/app-icons/storelogodisplay.png)

이 확인란을 선택 하면 **이미지를 표시 하는 저장소** 라는 섹션이 새로 나타납니다. 여기에서 스토어 앱의 패키지의 로고 이미지 대신 사용 하는 3 가지 이미지 크기를 업로드할 수 있습니다: 300 x 300, 150 x 150, 71 x 71 픽셀입니다. 300 x 300 크기에만 필요 하지만 3 크기를 제공 하는 것이 좋습니다.

자세한 내용은 [디스플레이 저장소의 로고 이미지를 업로드](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)를 참조 하세요.

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Visual Studio 매니페스트 디자이너를 사용 하 여 앱 아이콘 관리

Visual Studio **매니페스트 디자이너**호출 앱 아이콘을 관리 하기 위한 유용한 도구를 제공 합니다. 

> Visual Studio 2017 없는 경우는 사용할 수 있는 무료 버전 (Visual Studio 2017 Community Edition)을 포함 하 여 여러 버전 및 다른 버전 무료 평가판을 제공 합니다. 여기서 다운로드할 수 있습니다.[https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


매니페스트 디자이너를 시작 합니다.
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Visual Studio를 사용 하 여 UWP 프로젝트를 엽니다.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. **솔루션 탐색기**Package.appmxanifest 파일을 두 번 클릭 합니다.
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
        3. **시각적 자산** 탭을 클릭 합니다.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>한 번에 모든 자산을 생성

**모든 시각적 자산**, **시각적 자산** 탭의 첫 번째 메뉴 항목은 정확히 어떤 이름이 제안: 단추 누르기를 사용 하 여 앱에 필요한 모든 시각적 자산을 생성 합니다.

![Visual Studio에서 모든 시각적 자산을 생성 합니다.](images/app-icons/all-visual-assets.png)

단일 이미지를 제공 하기만 하면 및 Visual Studio에서 작은 타일, 중간 크기 타일, 큰 타일, 와이드 타일, 큰 타일, 앱 아이콘, 시작 화면을 생성 하 고 모든 배율 인수에 대 한 로고 자산 패키지 됩니다.

한 번에 모든 자산을 생성 합니다.
1. **원본** 필드 옆 **...** 를 클릭 하 고 사용 하려는 이미지를 선택 합니다. 비트맵 이미지를 사용 하는 경우 선명 하 게 결과 얻을 수 있도록 400 픽셀 400 이상 인지 확인 합니다. 벡터 기반 이미지에 가장 적합 합니다. Visual Studio를 사용 하면 AI (Adobe Illustrator) 및 PDF 파일을 사용할 수 있습니다. 
2. (선택 사항). **디스플레이 설정** 섹션에서 이러한 옵션을 구성 합니다.

    a.  **짧은 이름**: 앱에 대 한 짧은 이름을 지정 합니다.

    b.  **표시 이름**: 짧은 이름 중간, 넓 음 또는 큰 타일에 표시할 것인지 여부를 나타냅니다. 

    c. **배경 타일**: 16 진수 값 또는 타일 배경색에 대 한 색 이름을 지정 합니다. 예를 들면 `#464646`입니다. 기본값은 `transparent`입니다.

    d. **Spash 화면 배경**: spash 화면 배경 16 진수 값 이나 색 이름을 지정 합니다. 

3. **생성**을 클릭 합니다. 

Visual Studio에서는 이미지 파일을 생성 하 고 프로젝트에 추가 합니다. 자산을 변경 하려는 경우 프로세스를 반복 하면 됩니다. 

크기 조정 된 아이콘 자산이 파일 명명 규칙을 따릅니다.

*파일 이름*-scale-*배율 인수*.png

예를 들면

Square150x150Logo-scale-100.png, Square150x150Logo-scale-200.png, Square150x150Logo-scale-400.png

Visual Studio 기본적으로 배지 로고를 생성 하지 않는 것입니다. 배지 로고는를 가장 다른 앱 아이콘 일치 하지 않아야 하기 때문입니다. 자세한 내용은 [배지 알림은 UWP 앱 문서](/windows/uwp/design/shell/tiles-and-notifications/badges)를 참조 하세요. 


## <a name="more-about-app-icon-assets"></a>앱 아이콘 자산에 대 한 자세한 내용
Visual Studio에서 프로젝트에 필요한 모든 앱 아이콘 자산을 생성 하지만 다른 응용 프로그램 자산 다는 방법을 이해 하는 사용자 지정 하려는 경우. 

다양 한 위치에에서 표시 되는 앱 아이콘 자산: Windows 작업 표시줄, 작업 보기, ALT + TAB 및 시작 타일의 오른쪽 아래 모서리입니다. 몇 가지 추가 크기 조정 및 기타 자산 없는 옵션 plating에 너무 많은 장소에서 앱 아이콘 자산 나타나므로: 자산 "대상 크기" 및 "판이 없는" 자산입니다. 

### <a name="target-size-app-icon-assets"></a>대상 크기 앱 아이콘 자산
표준 배율 인수 크기 ("400.png Square44x44Logo.scale") 뿐 아니라 "대상 크기" 자산을 만드는 것이 좋습니다. 400 등의 특정 배율 인수 보다는 16 픽셀 등의 특정 크기를 대상으로 하기 때문에 이러한 자산 대상 크기 호출 합니다. 대상 크기 자산은 배율 플라토 시스템을 사용 하지 않는 표면의:

* 점프 목록 시작(데스크톱)
* 타일의 하단 모서리 시작(데스크톱)
* 바로 가기 키(데스크톱)
* 제어판(데스크톱)

대상 크기 자산 목록은 다음과 같습니다.


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

\ * 최소한이 크기를 제공 권장 합니다. 

이러한 자산에 안쪽 여백을 추가하지 않아도 됩니다. 필요한 경우 Windows에서 자동으로 안쪽 여백을 추가합니다. 이러한 자산에서는 16픽셀의 최소 공간을 고려해야 합니다. 

다음은 Windows 작업 표시줄의 아이콘에 나타나는 이러한 자산의 예입니다.

![Windows 작업 표시줄의 자산](images/assetguidance21.png)

### <a name="unplated-assets"></a>판이 없는 자산
기본적으로 Windows는 기본적으로 색이 있는 뒷판 대상 기반 자산을 사용합니다. 원하는 경우 대상 기반의 판이 없는 자산을 제공할 수 있습니다. "판이 없는" 의미 자산 투명 배경에서 표시 됩니다. 이러한 자산은 다양 한 배경색을 통해 표시 되는 점을 염두에 두십시오. 

![판이 없는 자산 및 판이 있는 자산](images/assetguidance22.png)

표면 판이 없는 앱 아이콘 자산을 사용 하는 다음과 같습니다.
* 작업 표시줄 및 작업 표시줄 미리 보기(데스크톱)
* 작업 표시줄 점프 목록
* 작업 보기
* Alt+Tab


### <a name="target-and-unplated-sizing"></a>대상 및 판이 없는 크기 조정

다음은 100% 배율로 대상 기반 자산에 대 한 크기 권장 사항입니다.

![100% 배율의 대상 기반 자산 크기 조정](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>시작 화면 자산에 대 한 자세한 내용
시작 화면에 대 한 자세한 내용은 [UWP 시작 화면 문서](/windows/uwp/launch-resume/splash-screens)를 참조 하세요.

## <a name="more-about-badge-logo-assets"></a>배지 로고 자산에 대 한 자세한 내용

왜 그렇지 기본적으로 배지 로고를 생성 하지 않는 이유는 자산 생성기를 사용 하 여 필요한 모든 자산을 생성 하는 경우: 다른 응용 프로그램 자산와에서 매우 다릅니다. 배지 로고는 앱의 타일 및 알림에 표시 되는 상태 이미지. 

자세한 내용은 [배지 알림은 UWP 앱 문서](/windows/uwp/design/shell/tiles-and-notifications/badges)를 참조 하세요.


## <a name="customizing-asset-padding"></a>사용자 지정 자산 안쪽 여백

기본적으로 Visual Studio 자산 생성기 권장된 안쪽 여백 어떤 이미지에 적용 됩니다. 이미지 여백에 이미 포함 하거나 타일의 끝까지 블리드 이미지, **안쪽 여백 권장 적용** 확인란 선택을 취소 하 여이 기능을 끌 수 있습니다. 

### <a name="tile-padding-recommendations"></a>타일 여백 권장 사항
자신의 안쪽 여백 제공 하려는 경우 타일에 대 한 권장 사항이 다음과 같습니다. 

타일 크기를 4 가지: (71 x 71) 작은 크기 (150 x 150), (150 x 310) 와이드 및 큰 (310 x 310). 

각 타일 자산은 해당 자산이 배치되는 타일 크기와 동일합니다.

![타일 표시 블리드](images/app-icons/tile-assets1.png)

아이콘 타일의 가장자리까지 확장을 사용 하지 않으려는 경우 자산에 안쪽 여백 만드는 투명 픽셀을 사용할 수 있습니다. 

![타일 및 뒷판](images/assetguidance05.png)

작은 타일의 경우 아이콘 너비와 높이를 타일 크기의 66%로 제한합니다.

![작은 타일 크기 조정 비율](images/assetguidance09.png)

중간 크기 타일의 경우 타일 크기의 66%로 아이콘 너비를 제한하고 50%로 높이를 제한합니다. 그러면 브랜딩 표시줄에서 요소가 겹치지 않게 됩니다.

![중간 크기 타일 크기 조정 비율](images/assetguidance10.png)

와이드 타일의 경우 타일 크기의 66%로 아이콘 너비를 제한하고 높이를 50%로 제한합니다. 그러면 브랜딩 표시줄에서 요소가 겹치지 않게 됩니다.

![와이드 타일 크기 조정 비율](images/assetguidance11.png)

큰 타일의 경우 타일 크기의 66%로 아이콘 너비를 제한하고 50%로 높이를 제한합니다.

![큰 타일 크기 비율](images/assetguidance12.png)

일부 아이콘은 가로 또는 세로 방향이 되도록 디자인되었지만 또 다른 아이콘은 대상 크기 내에서 정사각형으로 배치할 수 없는 더 복잡한 모양을 가집니다. 가운데에 표시되는 아이콘이 한쪽으로 기울 수 있습니다. 이런 경우 정사각형에 맞는 아이콘과 동일한 시각적 공간을 차지하면 아이콘의 일부가 권장 공간 외부에 표시될 수 있습니다.

![가운데 배치된 세 개의 아이콘](images/assetguidance13.png)

풀 블리드 자산을 사용할 경우 여백 및 타일의 가장자리 내에서 조작하는 계정 요소를 고려합니다. 타일의 너비 또는 높이의 최소 16%로 여백을 유지 관리합니다. 이 백분율은 가장 작은 타일 크기에서는 여백 너비의 두 배를 나타냅니다.

![여백이 있는 풀 블리드 타일](images/assetguidance14.png)

이 예제에서는 여백이 너무 작습니다.

![여백이 너무 적은 풀 블리드 타일](images/assetguidance15.png)









