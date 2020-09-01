---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
title: 공용 컨트롤 시작
description: UWP (common 유니버설 Windows 플랫폼) 컨트롤 및 해당 하는 iOS 컨트롤에 대 한 항목의 링크 목록을 봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50678f0bd5eb3cb48c6bcd915fd88bdfb460118b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174877"
---
# <a name="getting-started-common-controls"></a>시작: 공용 컨트롤


## <a name="common-controls-list"></a>공용 컨트롤 목록

이전 섹션에서는 button 및 textblock의 두 가지 컨트롤만 작업 했습니다. 물론, 많은 컨트롤을 사용할 수 있습니다. 앱에서 사용 하는 몇 가지 일반적인 컨트롤 및 해당 iOS가 여기에 해당 합니다. IOS 컨트롤은 가장 유사한 유니버설 Windows 플랫폼 (UWP) 컨트롤 옆에 사전순으로 나열 됩니다.

UWP 컨트롤에 대 한 자세한 내용은 실행 중인 장치 유형을 적절 하 게 변경 하 고 그에 따라 모양 및 기능을 변경 하는 것입니다. 예를 들어, 프로젝트에서 [**DatePicker**](/previous-versions/windows/apps/br211681(v=win.10)) 컨트롤을 사용 하는 경우 휴대폰과 비교 하 여 데스크톱 컴퓨터에서 다르게 표시 하 고 다르게 동작 하도록 최적화할 수 있습니다. 아무것도 수행할 필요가 없습니다. 컨트롤은 런타임에 자체적으로 조정 됩니다.

| iOS 컨트롤 (클래스/프로토콜) | 해당 하는 UWP 컨트롤 |
|------------------------------|--------------------------------------|
| 활동 표시기 (**UIActivityIndicatorView**) | [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> 참고 항목 [: 빠른 시작: 진행률 컨트롤 추가](/previous-versions/windows/apps/hh780651(v=win.10)) |
| 광고 배너 보기 (**ADBannerView**) 및 ad 배너 뷰 대리자 (**ADBannerViewDelegate**) | [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> 또한 [앱에서 광고 표시를](../monetize/display-ads-in-your-app.md) 참조 하세요. |
| 단추 (UIButton) | [Button](/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> 참고 항목 [: 빠른 시작: 단추 컨트롤 추가](/previous-versions/windows/apps/jj153346(v=win.10)) |
| 날짜 선택기 (UIDatePicker) | [DatePicker](/previous-versions/windows/apps/br211681(v=win.10)) |
| 이미지 뷰 (UIImageView) | [이미지](/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> 참고 항목 [이미지 및 ImageBrush](../design/controls-and-patterns/images-imagebrushes.md) |
| 레이블 (UILabel) | [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> 참고 항목 [: 빠른 시작: 텍스트 표시](/previous-versions/windows/apps/hh700392(v=win.10)) |
| 지도 보기 (MKMapView) 및 지도 보기 대리자 (MKMapViewDelegate) | [UWP 앱에 대 한 Bing Maps](/previous-versions/windows/apps/dn642089(v=win.10)) 참조 |
| 탐색 컨트롤러 (UINavigationController) 및 탐색 컨트롤러 대리자 (UINavigationControllerDelegate) | [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> 참고 항목 [탐색](../design/basics/navigation-basics.md) |
| 페이지 컨트롤 (UIPageControl) | [호출](/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> 참고 항목 [탐색](../design/basics/navigation-basics.md) |
| 선택 보기 (UIPickerView) 및 선택기 뷰 대리자 (UIPickerViewDelegate) | [ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> [콤보 상자 및 목록 상자 추가](/previous-versions/windows/apps/hh780616(v=win.10)) 도 참조 하세요. |
| 진행률 표시줄 (UIProgressView) | [ProgressBar](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> 참고 항목 [: 빠른 시작: 진행률 컨트롤 추가](/previous-versions/windows/apps/hh780651(v=win.10)) |
| 스크롤 뷰 (UIScrollView) 및 scroll view 대리자 (UIScrollViewDelegate) | [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  [Extensible Application Markup Language (XAML) 스크롤, 패닝 및 확대/축소 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample%20(Windows%208)) 도 참조 하세요. |
| 검색 창(UISearchBar) 및 검색 창 대리자(UISearchBarDelegate) | [앱에 검색 추가를](/previous-versions/windows/apps/jj130767(v=win.10)) 참조 하세요. <br/>  참고 항목 [: 빠른 시작: 앱에 검색 추가](/previous-versions/windows/apps/hh868180(v=win.10)) |
| 분할 컨트롤 (UISegmentedControl) | 없음 |
| 슬라이더 (UISlider) | [슬라이더](/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  또한 [슬라이더를 추가 하는 방법](/previous-versions/windows/apps/hh868197(v=win.10)) 을 참조 하세요. |
| 분할 뷰 컨트롤러 (UISplitViewController) 및 분할 뷰 컨트롤러 대리자 (UISplitViewControllerDelegate) | 없음 |
| 스위치 (UISwitch) | [ToggleSwitch](/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  또한 [토글 스위치를 추가 하는 방법을](/previous-versions/windows/apps/hh868198(v=win.10)) 참조 하세요. |
| 탭 표시줄 컨트롤러 (Uitabbar Controller) 및 탭 모음 컨트롤러 대리자 (Uitab바 Controllerdelegate) | 없음 |
| 테이블 뷰 컨트롤러 (UITableViewController), 테이블 뷰 (UITableView), 테이블 뷰 대리자 (Uitableviewcontroller) 및 테이블 셀 (Uitableviewcontroller) | [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  참고 항목 [: 빠른 시작: ListView 및 GridView 컨트롤 추가](/previous-versions/windows/apps/hh780650(v=win.10)) |
| 텍스트 필드 (UITextField) 및 텍스트 필드 대리자 (UITextFieldDelegate) | [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  또한 [텍스트 표시 및 편집을](../design/controls-and-patterns/text-controls.md) 참조 하세요. |
| 텍스트 뷰 (UITextView) 및 텍스트 뷰 대리자 (UITextViewDelegate) | [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  참고 항목 [: 빠른 시작: 텍스트 표시](/previous-versions/windows/apps/hh700392(v=win.10)) |
| 보기 (UIView) 및 뷰 컨트롤러 (UIViewController) | [호출](/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  참고 항목 [탐색](../design/basics/navigation-basics.md) |
| 웹 보기 (UIWebView 보기) 및 웹 보기 대리자 (UIWebViewDelegate) | [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  [XAML 웹 보기 컨트롤 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20WebView%20control%20sample%20(Windows%208)) 도 참조 하세요. |
| 창 (UIWindow) | [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  참고 항목 [탐색](../design/basics/navigation-basics.md) |

더 많은 컨트롤은 [컨트롤 목록](../design/controls-and-patterns/index.md)을 참조 하세요.

**참고**    JavaScript 및 HTML을 사용 하는 UWP 앱에 대 한 컨트롤 목록은 [controls list](/previous-versions/windows/apps/hh465453(v=win.10))를 참조 하세요.

### <a name="next-step"></a>다음 단계

[시작 하기: 탐색](getting-started-navigation.md)

## <a name="related-topics"></a>관련 항목

* [빌드 2014: XAML UI 및 컨트롤은 무엇 인가요?](https://channel9.msdn.com/Events/Build/2014/2-516)
* [빌드 2014: 공통 XAML UI 프레임 워크를 사용 하 여 앱 개발](https://channel9.msdn.com/Events/Build/2014/2-507)
* [빌드 2014: Visual Studio를 사용 하 여 XAML 수렴 형 앱 빌드](https://channel9.msdn.com/Events/Build/2014/3-591)