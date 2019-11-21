---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: 공용 컨트롤 시작
title: 공용 컨트롤 시작
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bc45f21acf5b9a485cf6bd5ead18482f1920bf99
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260146"
---
# <a name="getting-started-common-controls"></a>시작: 공용 컨트롤


## <a name="common-controls-list"></a>공용 컨트롤 목록

이전 섹션에서는 두 개의 컨트롤 즉, 단추와 텍스트 블록만 사용했습니다. There are, of course, many more controls that are available to you. 앱에서 사용하는 몇 가지 일반적인 컨트롤과 이와 동일한 iOS 컨트롤은 다음과 같습니다. iOS 컨트롤은 사전순으로 나열되어 있고 그 옆에 가장 유사한 UWP(유니버설 Windows 플랫폼) 컨트롤이 나열되어 있습니다.

UWP 컨트롤은 실행 중인 장치 유형을 인식하여 그에 따라 모양과 기능을 변경할 수 있습니다. 예를 들어 프로젝트에서 [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) 컨트롤을 사용하는 경우 데스크톱 컴퓨터와 휴대폰에서 서로 다르게 보이고 동작하도록 자체적으로 최적화할 수 있습니다. 아무 작업도 수행할 필요가 없습니다. 컨트롤이 런타임에 자동으로 조정됩니다.

| iOS 컨트롤(클래스/프로토콜) | Equivalent UWP control |
|------------------------------|--------------------------------------|
| 활동 표시기(**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> [빠른 시작: 진행률 컨트롤 추가](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) 참조 |
| 광고 배너 보기(**ADBannerView**) 및 광고 배너 보기 대리자(**ADBannerViewDelegate**) | [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> [앱에서 광고 표시](../monetize/display-ads-in-your-app.md) 참조 |
| 단추(UIButton) | [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> [빠른 시작: 단추 컨트롤 추가](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) 참조 |
| 날짜 선택기(UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| 이미지 보기(UIImageView) | [이미지](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> [Image 및 ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) 참조 |
| 레이블(UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> [빠른 시작: 텍스트 표시](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) 참조 |
| 지도 보기(MKMapView) 및 지도 보기 대리자(MKMapViewDelegate) | See [Bing Maps for UWP apps](https://msdn.microsoft.com/library/hh846481) |
| 탐색 컨트롤러(UINavigationController) 및 탐색 컨트롤러 대리자(UINavigationControllerDelegate) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> [탐색](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) 참조 |
| 페이지 컨트롤(UIPageControl) | [페이지](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> [탐색](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) 참조 |
| 선택기 보기(UIPickerView) 및 선택기 보기 대리자(UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> [콤보 상자 및 목록 상자 추가](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10))도 참조 |
| 진행률 표시줄(UIProgressView) | [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> [빠른 시작: 진행률 컨트롤 추가](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) 참조 |
| 스크롤 보기(UIScrollView) 및 스크롤 보기 대리자(UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  [XAML(Extensible Application Markup Language) 스크롤, 이동 및 확대/축소 샘플](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)(영문) 참조 |
| 검색 창(UISearchBar) 및 검색 창 대리자(UISearchBarDelegate) | [앱에 검색 추가](https://docs.microsoft.com/previous-versions/windows/apps/jj130767(v=win.10)) 참조 <br/>  [빠른 시작: 앱에 검색 추가](https://docs.microsoft.com/previous-versions/windows/apps/hh868180(v=win.10)) 참조 |
| 세그먼트 컨트롤(UISegmentedControl) | 없음 |
| 슬라이더(UISlider) | [슬라이더](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  [슬라이더를 추가하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh868197(v=win.10))도 참조 |
| 분할 보기 컨트롤러(UISplitViewController) 및 분할 보기 컨트롤러 대리자(UISplitViewControllerDelegate) | 없음 |
| 스위치(UISwitch) | [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  [설정/해제 스위치를 추가하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh868198(v=win.10)) 참조 |
| 탭 바 컨트롤러(UITabBarController) 및 탭 바 컨트롤러 대리자(UITabBarControllerDelegate) | 없음 |
| 테이블 보기 컨트롤러(UITableViewController), 테이블 보기(UITableView), 테이블 보기 대리자(UITableViewDelegate) 및 테이블 셀(UITableViewCell) | [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  [빠른 시작: ListView 및 GridView 컨트롤 추가](https://docs.microsoft.com/previous-versions/windows/apps/hh780650(v=win.10))도 참조 |
| 텍스트 필드(UITextField) 및 텍스트 필드 대리자(UITextFieldDelegate) | [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  [텍스트 표시 및 편집](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-controls)참조 |
| 텍스트 보기(UITextView) 및 텍스트 보기 대리자(UITextViewDelegate) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  [빠른 시작: 텍스트 표시](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) 참조 |
| 보기(UIView) 및 보기 컨트롤러(UIViewController) | [페이지](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  [탐색](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) 참조 |
| 웹 보기(UIWebView) 및 웹 보기 대리자(UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  [XAML WebView 컨트롤 샘플](https://code.msdn.microsoft.com/windowsapps/XAML-WebView-control-sample-58ad63f7) 참조 |
| 창(UIWindow) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  [탐색](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) 참조 |

기타 컨트롤에 대해서는 [컨트롤 목록](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)을 참조하세요.

**Note**  For a list of controls for UWP apps using JavaScript and HTML, see [Controls list](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10)).

### <a name="next-step"></a>다음 단계

[Getting Started: Navigation](getting-started-navigation.md)

## <a name="related-topics"></a>관련 항목

* [build 2014: What about XAML UI and Controls?](https://channel9.msdn.com/Events/Build/2014/2-516)
* [build 2014: Developing Apps using the Common XAML UI Framework](https://channel9.msdn.com/Events/Build/2014/2-507)
* [build 2014: Using Visual Studio to Build XAML Converged Apps](https://channel9.msdn.com/Events/Build/2014/3-591)
