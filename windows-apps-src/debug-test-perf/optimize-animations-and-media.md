---
author: jwmsft
ms.assetid: DE5B084C-DAC1-430B-A15B-5B3D5FB698F7
title: 애니메이션, 미디어 및 이미지 최적화
description: 매끄러운 애니메이션, 높은 프레임 속도 및 고성능 미디어 캡처 및 재생을 지원하는 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 66704daaca67dae2ba4f5a3882f5885ff333d2ce
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7290785"
---
# <a name="optimize-animations-media-and-images"></a>애니메이션, 미디어 및 이미지 최적화


매끄러운 애니메이션, 높은 프레임 속도 및 고성능 미디어 캡처 및 재생을 지원하는 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다.

## <a name="make-animations-smooth"></a>애니메이션 매끄럽게 만들기

UWP 앱의 주요 측면은 매끄러운 조작입니다. 여기에는 "손가락을 고정하는" 터치 조작, 매끄러운 전환/애니메이션 및 입력 피드백을 제공하는 작은 이동이 포함됩니다. XAML 프레임워크에는 앱의 시각적 요소에 컴퍼지션 및 애니메이션 효과를 주는 데 사용되는 컴퍼지션 스레드가 있습니다. 컴퍼지션 스레드는 프레임워크와 개발자 코드를 실행하는 UI 스레드와 구분되므로 앱이 복잡한 레이아웃 단계나 확장된 계산에 관계없이 일관된 프레임 속도와 매끄러운 애니메이션을 달성할 수 있습니다. 이 섹션에서는 컴퍼지션 스레드를 사용하여 앱 애니메이션을 아주 매끄럽게 유지하는 방법을 보여 줍니다. 애니메이션에 대한 자세한 내용은 [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/Mt187350)를 참조하세요. 계산량이 많은 작업을 수행하는 동안 앱의 응답성을 높이는 방법에 대한 자세한 내용은 [UI 스레드 응답 유지](keep-the-ui-thread-responsive.md)를 참조하세요.

### <a name="use-independent-instead-of-dependent-animations"></a>종속 애니메이션이 대신에 독립 애니메이션 사용

독립 애니메이션은 애니메이션할 속성을 변경해도 장면의 나머지 개체가 영향을 받지 않기 때문에 만드는 당시에 처음부터 끝까지 계산할 수 있습니다. 따라서 독립 애니메이션은 UI 스레드가 아니라 컴퍼지션 스레드에서 실행될 수 있습니다. 이에 따라 컴퍼지션 스레드가 일관된 간격으로 업데이트되므로 애니메이션이 매끄럽게 유지됩니다.

이 유형의 애니메이션은 모두 독립 애니메이션이 됩니다.

-   키 프레임을 사용하는 개체 애니메이션
-   제로 기간 애니메이션
-   [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) 및 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772) 속성에 대한 애니메이션
-   [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 속성에 대한 애니메이션
-   [**SolidColorBrush.Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 하위 속성을 대상으로 할 때 유형 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)의 속성에 대한 애니메이션
-   반환 값 유형의 하위 속성을 대상으로 할 때 다음 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 속성에 대한 애니메이션

    -   [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.rendertransform)
    -   [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d)
    -   [**프로젝션**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection)
    -   [**클립**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip)

종속 애니메이션은 레이아웃에 영향을 미치므로 UI 스레드에서 추가 입력이 없으면 계산할 수 없습니다. 종속 애니메이션에는 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 같은 속성의 수정이 포함됩니다. 기본적으로 종속 애니메이션은 실행되지 않으며 앱 개발자의 옵트인(opt in)이 필요합니다. 종속 애니메이션을 사용하는 경우 UI 스레드가 잠금 해제되어 있으면 매끄럽게 실행되지만 프레임워크나 앱이 UI 스레드에서 다른 작업을 많이 수행할 경우 잠시 멈추는 현상이 시작됩니다.

XAML 프레임워크의 거의 모든 애니메이션은 기본적으로 독립적이므로 몇 가지 작업으로 이 최적화를 비활성화할 수 있습니다. 특히 다음과 같은 경우에 주의하세요.

-   종속 애니메이션이 UI 스레드에서 실행될 수 있도록 [**EnableDependentAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210356) 속성을 설정할 경우. 이 애니메이션을 독립 버전으로 변환합니다. 예를 들어 개체의 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 및 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)가 아니라 [**ScaleTransform.ScaleX**](https://msdn.microsoft.com/library/windows/apps/BR242946) 및 [**ScaleTransform.ScaleY**](https://msdn.microsoft.com/library/windows/apps/BR242948)를 애니메이션합니다. 이미지, 텍스트와 같은 개체의 크기를 주저 없이 조정하세요. 프레임워크에서는 [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940)이 애니메이션되는 동안 쌍선형 크기 조정을 적용합니다. 이미지/텍스트는 최종 크기로 다시 변환되므로 항상 분명하게 표시됩니다.
-   종속 애니메이션에 효과적인 프레임당 업데이트를 만드는 경우. 이와 같은 예는 [**CompositonTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127) 이벤트 처리기의 변형에 적용됩니다.
-   [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.cachemode) 속성이 **BitmapCache**로 설정된 요소에서 독립적으로 간주된 애니메이션을 실행하는 경우. 각 프레임에 대한 캐시가 다시 변환되어야 하므로 이 애니메이션은 종속으로 간주됩니다.

### <a name="dont-animate-a-webview-or-mediaplayerelement"></a>WebView 또는 MediaPlayerElementWebView에 애니메이션 효과를 주지 않음

[**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) 컨트롤 내의 웹 콘텐츠는 XAML 프레임워크에서 직접 렌더링되지 않으므로 나머지 장면을 구성하기 위한 추가 작업이 필요합니다. 이 추가 작업은, 화면 주위에서 컨트롤을 애니메이션으로 만들 때 추가되며 잠재적으로 동기화 문제를 발생시킬 수 있습니다(예: HTML 콘텐츠가 페이지의 나머지 XAML 콘텐츠와 동기화하여 이동하지 않을 수 있음). **WebView** 컨트롤에 애니메이션 효과를 주어야 할 경우 애니메이션 중 컨트롤을 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webviewbrush.aspx)로 전환하세요.

마찬가지로 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 애니메이션도 바람직한 방법이 아닙니다. 성능이 손상될 뿐만 아니라 재생할 동영상 콘텐츠에서 작은 흠이나 기타 아티팩트가 생길 수 있습니다.

> **참고**  **MediaPlayerElement** 에 대 한이 문서의 권장 사항은 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)에 적용 됩니다. **MediaPlayerElement**는 Windows 10 버전 1607에서만 사용할 수 있으므로 이전 버전의 Windows용 앱을 만들려는 경우 **MediaElement**를 사용해야 합니다.

### <a name="use-infinite-animations-sparingly"></a>꼭 필요한 경우에만 무한 애니메이션 사용

대부분의 애니메이션은 지정된 시간 동안 실행되지만 [**Timeline.Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 속성을 Forever로 설정하면 애니메이션이 무한 실행됩니다. 무한 애니메이션의 사용은 최소화하는 것이 좋습니다. CPU 리소스를 계속 소비하고 CPU가 절전 또는 유휴 상태로 전환하는 것을 막아 더 일찍 전원 부족 상태가 될 수도 있기 때문입니다.

[**CompositionTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127) 처리기 추가는 무한 애니메이션 실행과 비슷합니다. 보통 UI 스레드는 수행할 작업이 있는 경우에만 활성화되지만 이 이벤트용 처리기를 추가하면 모든 프레임을 강제 실행합니다. 수행할 작업이 없는 경우 처리기를 제거하고 필요할 경우 다시 등록하세요.

### <a name="use-the-animation-library"></a>애니메이션 라이브러리 사용

[**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/BR243232) 네임스페이스에는 다른 Windows 애니메이션과 모양과 느낌이 일치하는 고성능의, 자연스러운 애니메이션 라이브러리가 포함되어 있습니다. 관련 클래스에는 해당 이름에 "테마"가 있으며 [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/Mt187350)에 설명되어 있습니다. 이 라이브러리는 앱의 첫 화면을 애니메이션하거나 상태 및 콘텐츠 전환을 만드는 것과 같은 여러 일반적인 애니메이션 시나리오를 지원합니다. UWP UI에서 성능과 일관성을 높이기 위해 가능하면 이 애니메이션 라이브러리를 사용하는 것이 좋습니다.

> **참고**  애니메이션 라이브러리는 가능한 모든 속성을 애니메이션할 수는 없습니다. 애니메이션 라이브러리가 적용되지 않는 XAML 시나리오의 경우 [스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/Mt187354)을 참조하세요.


### <a name="animate-compositetransform3d-properties-independently"></a>CompositeTransform3D 속성을 독립적으로 애니메이션

[**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/Dn914714)의 각 속성을 독립적으로 애니메이션할 수 있으므로 필요한 애니메이션만 적용합니다. 예제 및 자세한 내용은 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d)를 참조하세요. 변형을 애니메이션하는 방법에 대한 자세한 내용은 [스토리보드 애니메이션](https://msdn.microsoft.com/library/windows/apps/Mt187354) 및 [키 프레임 및 감속/가속 함수 애니메이션](https://msdn.microsoft.com/library/windows/apps/Mt187352)을 참조하세요.

## <a name="optimize-media-resources"></a>미디어 리소스 최적화

오디오, 동영상 및 이미지는 대부분의 앱이 사용하는 매력적인 콘텐츠 형식입니다. 미디어 캡처 속도가 높아지고 콘텐츠가 표준 화질에서 고화질로 이동함에 따라 이 콘텐츠를 저장, 디코드 및 재생하는 데 필요한 리소스가 증가합니다. XAML 프레임워크는 UWP 미디어 엔진에 추가된 최신 기능을 기반으로 빌드되므로 앱이 이러한 향상된 기능을 무료로 이용합니다. 여기에서는 UWP 앱에서 미디어를 최대한 활용할 수 있는 몇 가지 추가적인 트릭을 설명합니다.

### <a name="release-media-streams"></a>미디어 스트림 해제

미디어 파일은 앱에서 자주 사용하는 부담이 큰 리소스입니다. 미디어 파일 리소스 때문에 앱의 메모리 사용 공간이 크게 증가할 수 있으므로, 앱에서 더 이상 사용하지 않을 때 미디어에 대한 핸들을 해제해야 합니다.

예를 들어 앱에서 [**RandomAccessStream**](/uwp/api/Windows.Storage.Streams.RandomAccessStream) 또는 [**IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718) 개체를 사용하는 경우 더 이상 그 개체를 사용하지 않을 때 close 메서드를 호출하여 기본 개체를 릴리스해야 합니다.

### <a name="display-full-screen-video-playback-when-possible"></a>가능한 경우 전체 화면 동영상 재생 표시

UWP 앱에서는 항상 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)에서 [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.isfullwindow.aspx) 속성을 사용하여 전체 창 렌더링을 사용하거나 사용하지 않습니다. 그러면 미디어를 재생하는 동안 시스템 수준 최적화가 사용됩니다.

XAML 프레임워크는 단독으로 렌더링되는 동영상 콘텐츠의 표시를 최적화하므로 전원이 덜 사용되고 프레임 속도가 높아진 환경이 이루어집니다. 가장 효율적인 미디어 재생을 위해 **MediaPlayerElement** 크기를 화면 너비 및 높이로 설정하며 기타 XAML 요소를 표시하지 않습니다.

선택 자막 또는 순간적인 전송 컨트롤과 같이 화면의 전체 너비 및 높이를 차지하는 **MediaPlayerElement**에 XAML 요소를 오버레이하는 데는 정당한 이유가 있습니다. 미디어 재생을 다시 가장 효율적인 상태로 설정하는 데 이러한 요소가 필요하지 않는 경우이러한 요소를 숨겨야 합니다(`Visibility="Collapsed"` 설정).

### <a name="display-deactivation-and-conserving-power"></a>디스플레이 비활성화 및 전원 절약

앱이 동영상을 재생하고 있는 때와 같이 사용자 작업이 더 이상 검색되지 않을 때 디스플레이가 비활성화되지 않도록 하려면 [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/BR241818)를 호출할 수 있습니다.

전원과 배터리 수명을 절약하려면 [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/BR241819)를 호출하여 더 이상 필요하지 않은 디스플레이 요청을 해제해야 합니다.

다음은 디스플레이 요청을 해제해야 하는 몇 가지 상황입니다.

-   사용자 작업, 버퍼링 또는 제한된 대역폭으로 인한 조정 등으로 비디오 재생이 일시 중지되었습니다.
-   재생이 중지되었습니다. 예를 들어 비디오 재생이 완료되거나 프레젠테이션이 끝난 경우입니다.
-   재생 오류가 발생했습니다. 예를 들어 네트워크 연결 문제 또는 손상된 파일의 경우입니다.

### <a name="put-other-elements-to-the-side-of-embedded-video"></a>기타 요소를 포함된 동영상에 넣기

종종 앱은 페이지에서 동영상이 재생되는 포함된 보기를 제공합니다. 이제 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)가 페이지 크기가 아니고 다른 XAML 개체가 그려졌으므로 분명히 전체 화면 최적화 상태가 아닙니다. **MediaPlayerElement** 주위에 테두리를 그리면 실수로 이 모드로 전환되므로 유의하세요.

포함 모드인 경우 동영상의 맨 위에 XAML 요소를 그리지 마세요. 그릴 경우 프레임워크에서 장면을 구성하는 추가 작업을 수행해야 합니다. 동영상 맨 위가 아니라 포함된 미디어 요소 아래에 전송 컨트롤을 배치하는 것이 이 상황에 대한 좋은 최적화 예입니다. 이 이미지에서 빨간색 막대는 일련의 전송 컨트롤을 나타냅니다(재생, 일시 정지, 중지 등).

![오버레이 요소가 있는 MediaPlayerElement](images/videowithoverlay.png)

이러한 컨트롤을 전체 화면이 아닌 미디어의 맨 위에 배치하지 마십시오. 대신에 전송 컨트롤을 미디어가 렌더링되는 영역의 외부에 배치합니다. 다음 이미지에서 컨트롤은 미디어 아래에 배치됩니다.

![인접 요소가 있는 MediaPlayerElement](images/videowithneighbors.png)

### <a name="delay-setting-the-source-for-a-mediaplayerelement"></a>MediaPlayerElement에 대한 소스 설정 지연

미디어 엔진은 부담이 큰 개체이고 XAML 프레임워크에서는 dll 로드 및 큰 개체 만들기를 최대한 오래 지연합니다. [**소스**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 속성을 통해 소스가 설정된 후 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)가 이 작업을 수행해야 합니다. 사용자가 실제로 미디어를 재생할 준비가 될 때 이 항목을 설정하면 **MediaPlayerElement**와 관련된 대부분의 부담이 최대한 오래 지연됩니다.

### <a name="set-mediaplayerelementpostersource"></a>MediaPlayerElement.PosterSource 설정

[**MediaPlayerElement.PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.postersource.aspx)를 설정하면 XAML이 다른 경우에는 사용되었을 수 있는 일부 GPU 리소스를 해제할 수 있습니다. 이 API를 사용하면 앱이 최대한 적은 메모리를 사용할 수 있습니다.

### <a name="improve-media-scrubbing"></a>미디어 스크러빙 개선

언제나 스크러빙은 미디어 플랫폼의 응답 성능을 실현하기 위한 힘든 작업입니다. 일반적으로 사람들은 슬라이더 값을 변경하여 이 목적을 달성합니다. 다음은 스크러빙을 가능한 효율적으로 만드는 방법에 대한 몇 가지 팁입니다.

-   [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx)의 [**Position**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.position.aspx)을 쿼리하는 타이머에 따라 [**Slider**](https://msdn.microsoft.com/library/windows/apps/BR209614) 값을 업데이트합니다. 타이머에 대한 적절한 업데이트 빈도를 사용해야 합니다. **Position** 속성만 재생하는 동안 모든 250밀리초마다 업데이트합니다.
-   슬라이더에서 단계 빈도 크기는 동영상 길이를 통해 조정되어야 합니다.
-   슬라이더에서 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointermoved.aspx), [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerreleased.aspx) 이벤트를 구독하여 사용자가 슬라이더의 위치 조정 컨트롤을 끌 때 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackrate.aspx) 속성을 0으로 설정합니다.
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerreleased.aspx) 이벤트 처리기에서 수동으로 미디어 위치를 슬라이더 위치 값으로 설정하여 스크러빙하는 동안 최적의 위치 조정 컨트롤 끌기를 제공합니다.

### <a name="match-video-resolution-to-device-resolution"></a>동영상 해상도를 장치 해상도와 일치

동영상 디코딩에는 많은 메모리와 GPU 주기가 사용되므로 표시되는 해상도에 가까운 동영상 형식을 선택하세요. 훨씬 더 작은 크기로 축소되는 경우 1080 동영상을 디코딩하는 리소스를 사용하는 것은 아무 의미가 없습니다. 대부분의 앱에서는 동일한 동영상이 서로 다른 해상도로 인코딩되지 않지만 사용 가능한 경우 표시되는 해상도에 가까운 인코딩을 사용하세요.

### <a name="choose-recommended-formats"></a>권장 형식 선택

미디어 형식 선택은 민감한 주제가 될 수 있으며 주로 비즈니스 의사 결정에 좌우됩니다. UWP 성능의 관점에서 보면 H.264 비디오를 주 비디오 형식으로, AAC 및 MP3를 기본 오디오 형식으로 사용하는 것이 좋습니다. 로컬 파일 재생의 경우 MP4가 동영상 콘텐츠의 기본 파일 컨테이너입니다. H.264 디코딩은 가장 최신 그래픽 하드웨어를 통해 가속됩니다. 또한 VC-1 디코딩을 위한 하드웨어 가속이 현재 판매 중인 그래픽 하드웨어의 상당수에서 폭넓게 지원되지만, 전체 스트림 수준 하드웨어 오프로드(예: VLD 모드)가 아닌 부분적인 가속 수준(즉 IDCT 수준)으로 제한되는 경우가 많습니다.

동영상 콘텐츠 생성 프로세스를 완벽하게 제어할 수 있는 경우에는 압축 효율성과 GOP 구조 사이의 적절한 균형을 유지하는 방법을 파악해야 합니다. B 사진은 상대적으로 GOP 크기가 작기 때문에 검색 또는 트릭 모드에서 성능이 향상될 수 있습니다.

짧은 대기 시간의 짤막한 오디오 효과를 포함할 경우(예: 게임) 압축된 오디오 형식에서 흔히 나타나는 처리 오버헤드를 줄이기 위해 압축되지 않은 PCM 데이터의 WAV 파일을 사용합니다.


## <a name="optimize-image-resources"></a>이미지 리소스 최적화

### <a name="scale-images-to-the-appropriate-size"></a>이미지 크기를 적절하게 조정

이미지는 매우 높은 해상도로 캡처되므로 디스크에서 로드된 후 이미지 데이터 및 더 많은 메모리를 디코딩하는 경우 앱에서 CPU를 더 많이 사용할 수 있습니다. 하지만 단지 원본 크기보다 작게 표시하기 위해 고해상도 이미지를 디코딩하고 메모리에 저장하는 것은 아무 의미가 없습니다. 대신, [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 및 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 속성을 사용하여 화면에 정확한 크기로 그려지는 이미지 버전을 만듭니다.

금지 사항:

```xaml
<Image Source="ms-appx:///Assets/highresCar.jpg"
       Width="300" Height="200"/>    <!-- BAD CODE DO NOT USE.-->
```

대신 수행할 작업:

```xaml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg"
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

[**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 및 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241)에 대한 단위는 기본적으로 실제 픽셀입니다. [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 속성을 사용하면 이 동작을 변경할 수 있습니다. 즉 **DecodePixelType**을 **Logical**로 설정하면 디코드 크기가 다른 XAML 콘텐츠와 유사하게 시스템의 현재 배율을 자동으로 고려합니다. 그러므로 예를 들어 **DecodePixelWidth** 및 **DecodePixelHeight**를 이미지가 표시되는 이미지 컨트롤의 Height 및 Width 속성과 일치시키려는 경우 **DecodePixelType**을 **Logical**로 설정하는 것이 일반적으로 적절합니다. 실제 픽셀을 사용하는 기본 동작에서는 직접 시스템의 현재 배율을 고려해야 하므로 사용자가 디스플레이 기본 설정을 변경하는 경우 배율 변경 알림을 수신하게 됩니다.

DecodePixelWidth/Height가 이미지가 화면에 표시되는 것보다 명시적으로 크게 설정되면 앱에서 픽셀 당 최대 4바이트의 추가 메모리를 불필요하게 사용하게 되므로 큰 이미지의 경우 순식간에 과도하게 사용됩니다. 또한 이미지가 쌍선형 배율을 사용하면서 축소되어 큰 배율의 경우 흐리게 표시될 수 있습니다.

DecodePixelWidth/DecodePixelHeight가 이미지가 화면에 표시되는 것보다 명시적으로 작게 설정되면 이미지가 확대되어 모자이크처럼 나타날 수 있습니다.

적절한 디코드 크기를 미리 결정할 수 없는 일부 경우, 명시적인 DecodePixelWidth/DecodePixelHeight가 지정되지 않았을 때 XAML의 적절한 크기로 자동 디코딩에 맡기는 것이 좋습니다. 이를 사용하면 가장 최선의 노력을 들여서 적절한 크기로 이미지를 디코딩할 수 있습니다.

미리 이미지 콘텐츠 크기를 알고 있는 경우 명시적인 디코드 크기를 설정하는 것이 좋습니다. 제공된 디코드 크기가 다른 XAML 요소 크기와 관련되어 있는 경우 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545)을 **Logical**로 함께 설정해야 합니다. 예를 들어 Image.Width와 Image.Height를 가진 콘텐츠 크기를 명시적으로 설정하는 경우 DecodePixelType을 DecodePixelType.Logical로 설정하여 동일한 논리적 픽셀 치수를 이미지 컨트롤로 사용한 다음 명시적으로 BitmapImage.DecodePixelWidth 및/또는 BitmapImage.DecodePixelHeight를 사용하여 잠재적으로 메모리가 많이 절약되도록 이미지의 크기를 제어할 수 있습니다.

디코딩된 콘텐츠의 크기를 결정할 때 Image.Stretch를 고려해야 합니다.

### <a name="right-sized-decoding"></a>적절한 크기로 디코딩

명시적인 디코드 크기를 설정하지 않은 경우 XAML은 포함하는 페이지의 초기 레이아웃에 따라 화면에 표시하는 정확한 크기로 이미지를 디코딩함으로써 메모리를 절약하기 위한 최선의 노력을 합니다. 가능한 경우 이 기능을 사용하는 방식으로 응용 프로그램을 작성하는 것이 좋습니다. 다음 조건 중 하나가 충족되는 경우에는 이 기능이 사용되지 않습니다.

-   [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235)가 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) 또는 [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx)를 사용하여 콘텐츠를 설정한 다음 라이브 XAML 트리에 연결되었습니다.
-   이미지가 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255)와 같은 동기 디코딩을 사용하여 디코드됩니다.
-   호스트 이미지 요소 또는 브러시 또는 부모 요소에서 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)를 0으로 설정하거나 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility)를 **Collapsed**로 설정해서 이미지가 숨겨졌습니다.
-   이미지 컨트롤 또는 브러시가 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242968) **None**을 사용합니다.
-   이미지가 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756)로 사용됩니다.
-   `CacheMode="BitmapCache"` (이)가 부모 요소 또는 이미지 요소에서 설정되었습니다.
-   이미지 브러시가 사각형이 아닙니다(예: 모양 또는 텍스트에 적용되는 경우).

위의 시나리오에서는 명시적인 디코드 크기를 설정하는 것이 유일하게 메모리를 절약할 수 있는 방법입니다.

원본을 설정하기 전에 항상 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235)를 라이브 트리에 연결해야 합니다. 이미지 요소 또는 브러시가 태그에서 지정되면 항상 자동으로 그렇게 됩니다. "라이브 트리 예제"라는 제목으로 예제가 제공됩니다. 스트림 원본을 설정할 때 항상 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255)를 사용하지 말고 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522)를 대신 사용해야 합니다. 또한 [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx) 이벤트가 발생되는 것을 기다리는 동안 이미지 콘텐츠를 숨기지(불투명도가 0이거나 축소된 표시 유형) 않는 것도 좋은 방법입니다. 이렇게 하는 것은 판단 호출입니다. 즉, 이 호출을 수행하면 적절한 크기로 자동 디코딩 혜택을 이용할 수 없게 됩니다. 앱에서 이미지 콘텐츠를 처음에 숨겨야 하는 경우에도, 가능하다면 디코드 크기를 명시적으로 설정해야 합니다.

**라이브 트리 예제**

예제 1(좋음) - 태그에 지정된 URI(Uniform Resource Identifier)

```xaml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

예제 2 태그 - 코드 숨김에 지정된 URI

```xaml
<Image x:Name="myImage"/>
```

예제 2 코드 숨김(좋음) - UriSource를 설정하기 전에 BitmapImage를 트리에 연결

```csharp
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

예제 2 코드 숨김 (나쁨)-트리에 연결 하기 전에 BitmapImage의 UriSource를 설정 합니다.

```csharp
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

### <a name="caching-optimizations"></a>캐싱 최적화

캐싱 최적화는 [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx)를 사용하여 앱 패키지 또는 웹에서 콘텐츠를 로드하는 이미지에 효과적입니다. URI는 기본 콘텐츠를 고유하게 식별하는 데 사용되므로 내부적으로 XAML 프레임워크가 콘텐츠를 여러 번 다운로드하거나 디코드하지 않습니다. 대신 캐시된 소프트웨어 또는 하드웨어 리소스를 사용하여 콘텐츠를 여러 번 표시합니다.

이 최적화의 예외는 이미지가 다른 해상도(명시적으로 또는 적절한 크기로 자동 디코딩을 통해 지정될 수 있음)로 여러 번 표시되는 경우입니다. 또한 각 캐시 항목은 이미지의 해상도를 저장하고 XAML이 필요한 해상도와 일치하는 원본 URI를 가진 이미지를 찾을 수 없는 경우 해당 크기로 새 버전을 디코딩합니다. 그러나 인코딩된 이미지 데이터를 다시 다운로드하지는 않습니다.

결과적으로 앱 패키지에서 이미지를 로드할 때 [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx)를 사용하는 것을 받아들이고, 요구하지 않는 한 파일 스트림 및 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522)를 사용하는 것을 피해야 합니다.

### <a name="images-in-virtualized-panels-listview-for-instance"></a>가상화 패널의 이미지(예: ListView)

앱이 명시적으로 제거하거나 최신 가상화된 패널에 있어서 보기에서 스크롤될 때 암시적으로 제거되었기 때문에 이미지가 트리에서 제거된 경우 XAML은 하드웨어 리소스가 더 이상 필요하지 않게 된 이후 해당 이미지에 대한 하드웨어 리소스를 릴리스함으로써 메모리 사용을 최적화합니다. 메모리는 즉시 릴리스되지 않고 이미지 요소가 트리에 더 이상 존재하지 않게 되고 나서 1초가 지난 후 발생하는 프레임 업데이트 동안 릴리스됩니다.

따라서 이미지 콘텐츠 목록을 호스트하려면 최신 가상화된 패널을 사용해야 합니다.

### <a name="software-rasterized-images"></a>소프트웨어-래스터화된 이미지

이미지가 사각형이 아닌 브러시 또는 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756)에 사용되는 경우 해당 이미지는 소프트웨어 래스터화 경로를 사용하므로 전혀 이미지의 배율을 조정하지 않습니다. 또한 소프트웨어 및 하드웨어 메모리 모두에 이미지의 복사본을 저장해야 합니다. 예를 들어 이미지가 타원에 대한 브러시로 사용되는 경우 잠재적으로 큰 전체 이미지는 내부적으로 두 번 저장됩니다. **NineGrid** 또는 사각형이 아닌 브러시를 사용하는 경우 앱은 해당 이미지의 배율을 렌더링되는 크기로 대략 미리 조정합니다.

### <a name="background-thread-image-loading"></a>백그라운드 스레드 이미지 로딩

XAML은 소프트웨어 메모리에서 중간 표면을 요구하지 않고 하드웨어 메모리의 표면에 비동기적으로 이미지 콘텐츠를 디코딩할 수 있는 내부 최적화가 있습니다. 그러므로 최고 메모리 사용 및 렌더링 대기 시간이 줄어듭니다. 다음 조건 중 하나가 충족되는 경우에는 이 기능이 사용되지 않습니다.

-   이미지가 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756)로 사용됩니다.
-   `CacheMode="BitmapCache"` (이)가 부모 요소 또는 이미지 요소에서 설정되었습니다.
-   이미지 브러시가 사각형이 아닙니다(예: 모양 또는 텍스트에 적용되는 경우).

### <a name="softwarebitmapsource"></a>SoftwareBitmapSource

[**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854) 클래스는 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/BR226176), 카메라 API 및 XAML과 같이 서로 다른 WinRT 네임스페이스 간에 상호 운용이 가능한 압축되지 않은 이미지를 교환합니다. 이 클래스는 [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259)을 사용하여 일반적으로 필요한 추가 복사본을 제거합니다. 이를 통해 최고 메모리 및 원본 대 화면 대기 시간을 줄일 수 있도록 합니다.

또한 원본 정보를 제공하는 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358)은 사용자 지정 [**IWICBitmap**](https://msdn.microsoft.com/library/windows/desktop/Ee719675)을 사용할 수 있도록 구성하여 필요할 때마다 앱에서 메모리를 다시 매핑할 수 있는 재로드 가능 백업 저장소를 제공할 수 있습니다. 이는 고급 C++ 사용 사례입니다.

이미지를 생성하고 사용하는 다른 WinRT API와 상호 작용하려면 앱에서 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358) 및 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854)를 사용해야 합니다. 그리고 [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259)을 사용하는 대신 압축되지 않은 이미지 데이터를 로드할 때 앱에서 **SoftwareBitmapSource**를 사용해야 합니다.

### <a name="use-getthumbnailasync-for-thumbnails"></a>미리 보기에 GetThumbnailAsync 사용

이미지 크기 조정을 위한 하나의 사용 사례는 미리 보기를 만드는 것입니다. [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 및 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241)를 사용하여 작은 이미지 버전을 제공할 수 있지만 UWP에서는 미리 보기 검색을 위한 훨씬 더 효율적인 API를 제공합니다. [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210)는 파일 시스템이 이미 캐시된 이미지의 미리 보기를 제공합니다. 이미지를 열거나 디코딩할 필요가 없으므로 XAML API보다 훨씬 더 나은 성능을 제공합니다.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> FileOpenPicker picker = new FileOpenPicker();
> picker.FileTypeFilter.Add(".bmp");
> picker.FileTypeFilter.Add(".jpg");
> picker.FileTypeFilter.Add(".jpeg");
> picker.FileTypeFilter.Add(".png");
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary;
>
> StorageFile file = await picker.PickSingleFileAsync();
>
> StorageItemThumbnail fileThumbnail = await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64);
>
> BitmapImage bmp = new BitmapImage();
> bmp.SetSource(fileThumbnail);
>
> Image img = new Image();
> img.Source = bmp;
> ```
> ```vb
> Dim picker As New FileOpenPicker()
> picker.FileTypeFilter.Add(".bmp")
> picker.FileTypeFilter.Add(".jpg")
> picker.FileTypeFilter.Add(".jpeg")
> picker.FileTypeFilter.Add(".png")
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary
>
> Dim file As StorageFile = Await picker.PickSingleFileAsync()
>
> Dim fileThumbnail As StorageItemThumbnail = Await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64)
>
> Dim bmp As New BitmapImage()
> bmp.SetSource(fileThumbnail)
>
> Dim img As New Image()
> img.Source = bmp
> ```

### <a name="decode-images-once"></a>이미지를 한 번 디코딩

이미지를 두 번 이상 디코딩하지 않으려면 메모리 스트림을 사용하지 않고 URI에서 [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760) 속성을 할당합니다. XAML 프레임워크는 여러 위치에서 동일한 URI를 단일 디코딩 이미지와 연결할 수 있지만 동일한 데이터가 포함되고 메모리 스트림별로 다른 디코딩 이미지를 만드는 여러 메모리 스트림에 대해 동일 작업을 수행할 수 없습니다.
