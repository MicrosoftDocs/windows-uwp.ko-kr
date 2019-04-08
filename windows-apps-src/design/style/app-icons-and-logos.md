---
Description: 앱 아이콘/로고는 시작 메뉴, 앱 타일, 작업 표시줄, Microsoft Store 앱을 나타내는 만드는 방법입니다.
title: 앱 아이콘 및 로고
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b755e8d165d58ce4303d9fefe6d051abce6c9765
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622458"
---
# <a name="app-icons-and-logos"></a>앱 아이콘 및 로고 

모든 앱을 나타내는 아이콘/로고 있고 Windows shell의 여러 위치에서 해당 아이콘이 표시 됩니다. 

:::row:::
    :::column:::
        * 앱 창의 제목 표시줄
        * 시작 메뉴의 앱 목록
        * 작업 표시줄 및 작업 관리자
        * 앱의 타일
        * 앱의 시작 화면
        * Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

이 문서에서는 앱 아이콘을 만드는 기본 사항을 설명 해야 할 관리 하 고 수동으로 관리 하는 방법에 Visual Studio를 사용 하는 방법입니다.
 
(이 문서는 특히 앱 자체를 나타내는 일반 아이콘 지침을 참조 하는 아이콘을 [아이콘](icons.md) 문서입니다.)

## <a name="icon-types-locations-and-scale-factors"></a>아이콘 형식, 위치 및 배율

기본적으로 Visual Studio는 자산 하위 디렉터리의 아이콘 자산을 저장합니다. 표시 되는 위치, 아이콘 및 호출 하는 다른 형식의 목록은 다음과 같습니다. 

| 아이콘 이름 | 에 표시 됩니다. | 자산 파일 이름 |
| ---      | ---        | --- |
| 작은 타일 | 시작 메뉴 |  SmallTile.png  |
| 중간 크기 타일 |시작 메뉴에서 Microsoft Store 목록\*  |  Square150x150Logo.png |
| 넓은 타일  | 시작 메뉴   | Wide310x150Logo.png |
| 큰 타일   | 시작 메뉴에서 Microsoft Store 목록\* |  LargeTile.png  |
| 앱 아이콘 | 시작 메뉴, 작업 표시줄, 작업 관리자의 앱 목록 | Square44x44Logo.png |
| 시작 화면 | 앱의 시작 화면 | SplashScreen.png  |
| 배지 로고 | 앱의 타일 | BadgeLogo.png  |
| 패키지 로고/스토어 로고 | 앱 설치 관리자, 파트너 센터 "리뷰 쓰기" 옵션이 저장소의 저장소에 "앱 보고" 옵션 | StoreLogo.png  |

\* 사용 하도록 선택 하지 않으면 [표시에만 저장소에 이미지 업로드](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)합니다. 

이러한 아이콘 모든 화면에서 정확한 확인을 위해 다른 표시 배율에 대 한 동일한 아이콘의 여러 버전을 만들 수 있습니다. 

배율 인수는 텍스트와 같은 UI 요소의 크기를 결정합니다. 요소 범위 100%에서 400% 확장 합니다. 더 큰 값을 쉽게 높은 DPI 디스플레이에 큰 UI 요소를 만듭니다. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


앱 아이콘 자산은 비트맵 이므로 비트맵은 잘 확장 되지 않는 각 배율에 대 한 각 아이콘 자산 버전을 제공 하는 것이 좋습니다. 100%, 125%, 150%, 200%, 및 400%입니다. 많은 아이콘입니다. 다행히 Visual Studio는 쉽게 생성 하 고 이러한 아이콘을 업데이트 하는 도구를 제공 합니다. 

## <a name="microsoft-store-listing-image"></a>Microsoft Store 목록 이미지

"지정 하는 방법 이미지 내 앱 목록에 대 한 Microsoft Store"?

기본적으로 사용 하 여 일부 패키지를 이미지 저장소에이 페이지의 맨 위에 있는 표에 설명 된 대로 (다른와 함께 [제출 프로세스 중에 제공 하는 이미지](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). 그러나 저장소 (Xbox 포함), Windows 10에서 고객에 게 목록을 표시 하는 경우 앱의 패키지에 로고 이미지를 사용 하지 않도록 있고 대신 업로드 하는 이미지에만 사용 하 여 저장소를 수가 있습니다. 이렇게 하면 다양한 디스플레이에서 스토어의 앱 표시 모양을 훨씬 더 많이 제어할 수 있습니다. (참고는 제품에서 이전 OS 버전을 지 원하는 경우 해당 고객에 게 표시 될 수 계속 이미지에 패키지에서이 옵션을 사용 하는 경우에 합니다.) 이 수행할 수 있는 **로고를 저장** 섹션을 **스토어 목록** 제출 프로세스의 단계입니다.

![앱 제출 프로세스 중 지정 스토어 로고](images/app-icons/storelogodisplay.png)

이 확인란을 선택 하면 새 섹션을 호출 **이미지를 표시 하는 저장소** 나타납니다. 여기에서 스토어 앱의 패키지에서 로고 이미지 대신 사용 하는 3 개의 이미지 크기를 업로드할 수 있습니다. 300 x 300 고 71x71 정사각형 150 x 150 픽셀입니다. 300 x 300 크기는 필수 이지만 모든 3 가지 크기를 제공 하는 것이 좋습니다.

자세한 내용은 참조 하세요. [표시만 업로드 된 로고 이미지 저장소에](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)입니다.

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Visual Studio 매니페스트 디자이너를 사용 하 여 관리 앱 아이콘

Visual Studio 이라는 앱 아이콘을 관리 하는 매우 유용한 도구를 제공 합니다 **매니페스트 디자이너**합니다. 

> Visual Studio 2017이 아직 없는 경우 여러 가지 버전이 있습니다 (Visual Studio 2017 Community Edition), 무료 버전을 비롯 하 여 및 다른 버전 평가판을 제공 합니다. 여기서 다운로드할 수 있습니다. [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


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
        1. Visual Studio를 사용 하 여 UWP 프로젝트를 열려고 합니다.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. 에 **솔루션 탐색기**, Package.appmxanifest 파일을 두 번 클릭 합니다.
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
        3. 클릭 합니다 **시각적 자산** 탭 합니다.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>모든 자산을 한 번에 생성

첫 번째 메뉴 항목에 **시각적 자산** 탭 **모든 시각적 자산**, 정확히 어떤 이름과 제안지 않습니다: 앱에서 단추를 눌러를 사용 하 여 필요한 모든 시각적 자산을 생성 합니다.

![Visual Studio에서 모든 시각적 자산을 생성 합니다.](images/app-icons/all-visual-assets.png)

단일 이미지를 제공 하기만 하면 및 Visual Studio을 작은 타일, 중간 크기 타일, 큰 타일, 넓은 타일, 큰 타일을 앱 아이콘, 시작 화면을 생성 하 고 모든 배율에 대 한 로고 자산을 패키지 합니다.

한 번에 모든 자산을 생성 합니다.
1. 클릭 된 **...**  옆에 **소스** 필드를 사용 하려는 이미지를 선택 합니다. 비트맵 이미지를 사용 하는 경우 정확한 결과 받을 수 있도록 400 픽셀로 400 이상 인지 확인 합니다. 벡터 기반 이미지에 가장 적합 합니다. Visual Studio를 사용 하면 AI (Adobe Illustrator) 및 PDF 파일을 사용할 수 있습니다. 
2. (선택 사항). 에 **디스플레이 설정** 섹션에서 이러한 옵션을 구성 합니다.

    a.  **짧은 이름**:  앱에 대 한 짧은 이름을 지정 합니다.

    b.  **이름 표시**: 짧은 이름 보통, 넓게 또는 대규모 타일에 표시할 것인지 여부를 나타냅니다. 

    c. **타일 배경**: 16 진수 값 이거나 타일 배경 색상에 대 한 색 이름을 지정 합니다. 예를 들면 `#464646`입니다. 기본값은 `transparent`입니다.

    d. **시작 화면 배경**: 시작 화면 배경에 대 한 16 진수 값 또는 색 이름을 지정 합니다. 

3. 클릭 **생성**합니다. 

Visual Studio 이미지 파일을 생성 하 고 프로젝트에 추가 합니다. 자산을 변경 하려는 경우에 프로세스를 반복 하면 됩니다. 

확장된 아이콘 자산 파일 명명 규칙을 따릅니다.

*filename*크기 조정-*배율을*.png

예를 들면

Square150x150Logo-확장-100.png, Square150x150Logo-확장-200.png Square150x150Logo-확장-400.png

Visual Studio 배지 로고를 기본적으로 생성 하지는 알 수 있습니다. 배지 로고는 고유 하며 아마도 다른 앱 아이콘이 일치 해서는 안 됩니다 때문입니다. 자세한 내용은 참조는 [UWP 앱 문서에 대 한 알림 배지](/windows/uwp/design/shell/tiles-and-notifications/badges)합니다. 


## <a name="more-about-app-icon-assets"></a>앱 아이콘 자산에 대 한 자세한 정보
Visual Studio에서 프로젝트에 필요한 모든 앱 아이콘 자산을 생성 하지만 어떻게 다른 앱 자산에서 다른 지 알아두면 이해 하는 것을 사용자 지정 하려는 경우. 

앱 아이콘 자산 많은 부분에 표시 됩니다: Windows 작업 표시줄, 작업 보기, ALT + TAB 및 시작 타일의 오른쪽 아래 모퉁이. 몇 가지 추가 크기 조정 및 기타 자산 없는 옵션 plating에 너무 많은 위치에서 앱 아이콘 자산 나타나므로: 자산 "대상 크기" 및 "플레이트 되지 않은" 자산입니다. 

### <a name="target-size-app-icon-assets"></a>대상 크기 앱 아이콘 자산
표준 확장 비율 크기 ("400.png Square44x44Logo.scale") 하는 것 외에도 "대상 크기" 자산을 만드는 것이 좋습니다. 400 등의 특정 배율 인수 대신 16 픽셀 같은 특정 크기를 대상으로 하기 때문에 이러한 자산 대상 크기 라고 합니다. 대상 크기 자산 크기 조정 고 원으로 시스템을 사용 하지 않는 표면에 됩니다.

* 점프 목록 시작(데스크톱)
* 타일의 하단 모서리 시작(데스크톱)
* 바로 가기 키(데스크톱)
* 제어판(데스크톱)

대상 크기 자산 목록을 다음과 같습니다.


| 자산 크기 | 파일 이름 예제                  |
|------------|------------------------------------|
| 16 x 16\*    | Square44x44Logo.targetsize-16.png  |
| 24 x 24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256 x 256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

\* 여기에 최소한 이러한 크기를 제공 하는 것이 좋습니다. 

이러한 자산에 안쪽 여백을 추가하지 않아도 됩니다. 필요한 경우 Windows에서 자동으로 안쪽 여백을 추가합니다. 이러한 자산에서는 16픽셀의 최소 공간을 고려해야 합니다. 

다음은 Windows 작업 표시줄의 아이콘에 나타나는 이러한 자산의 예입니다.

![Windows 작업 표시줄의 자산](images/assetguidance21.png)

### <a name="unplated-assets"></a>플레이트 되지 않은 자산
기본적으로 Windows는 기본적으로 색이 지정 된 백플레이트 위에 대상 기반 자산을 사용합니다. 원하는 경우 대상 기반 플레이트 되지 않은 자산을 제공할 수 있습니다. "플레이트 되지 않은" 투명 한 배경을에 표시할 자산을 의미 합니다. 이러한 자산은 다양 한 배경색을 통해 표시 되는 점을 염두에 두십시오. 

![판이 없는 자산 및 판이 있는 자산](images/assetguidance22.png)

플레이트 되지 않은 앱 아이콘 자산을 사용 하는 화면은 다음과 같습니다.
* 작업 표시줄 및 작업 표시줄 미리 보기(데스크톱)
* 작업 표시줄 점프 목록
* 작업 보기
* Alt+Tab


### <a name="target-and-unplated-sizing"></a>대상 및 플레이트 되지 않은 크기 조정

100% 대규모로 대상 기반 자산에 대 한 권장 크기는 다음과 같습니다.

![100% 배율의 대상 기반 자산 크기 조정](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>시작 화면 자산에 대 한 자세한 내용
시작 화면에 대 한 자세한 내용은 참조는 [UWP 시작 화면 문서의](/windows/uwp/launch-resume/splash-screens)합니다.

## <a name="more-about-badge-logo-assets"></a>배지 로고 자산에 대 한 자세한 내용

배지 로고 기본적으로 생성 하지 이유는 이유는 자산 생성기를 사용 하 여 필요한 모든 자산을 생성: 기타 앱 자산와에서 매우 다릅니다. 배지 로고는 알림 및 앱의 타일에 표시 되는 상태 이미지입니다. 

자세한 내용은 참조는 [UWP 앱 문서에 대 한 알림 배지](/windows/uwp/design/shell/tiles-and-notifications/badges)합니다.


## <a name="customizing-asset-padding"></a>사용자 지정 자산 안쪽 여백

기본적으로 Visual Studio 자산 생성기 권장된 안쪽 여백 모든 이미지에 적용 됩니다. 이미지에는 이미 안쪽 여백을 포함 하거나 확장 타일의 끝에 있는 전체 화상 물림 재단 이미지를 해제할 수 있습니다이 기능 선택을 취소 하 여 합니다 **권장 안쪽 여백 적용** 확인란 합니다. 

### <a name="tile-padding-recommendations"></a>안쪽 여백 권장 사항 타일
사용자 고유의 패딩 제공 하려는 경우 타일에는 권장 사항이 다음과 같습니다. 

4 개의 타일 크기가: 작은 (71x71 정사각형), 중간 (150 x 150), 와이드 (310 x 150) 및 큰 (310x310 정사각형). 

각 타일 자산은 해당 자산이 배치되는 타일 크기와 동일합니다.

![화상 물림 재단 전체를 보여 주는 타일](images/app-icons/tile-assets1.png)

타일의에 지로 확장 하 여 아이콘을 사용 하지 않으려는 경우에 안쪽 여백을 만드는 자산에서 투명 한 픽셀을 사용할 수 있습니다. 

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









