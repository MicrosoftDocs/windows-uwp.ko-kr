---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: 공용 컨트롤 시작
title: 공용 컨트롤 시작
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ebba5abe0de8014a21d2e651534dacc118705fff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610628"
---
# <a name="getting-started-common-controls"></a>시작 하기: 공용 컨트롤


## <a name="common-controls-list"></a>공용 컨트롤 목록

이전 섹션에서는 두 개의 컨트롤 즉, 단추와 텍스트 블록만 사용했습니다. 가 물론를 사용할 수 있는 많은 자세한 컨트롤입니다. 앱에서 사용하는 몇 가지 일반적인 컨트롤과 이와 동일한 iOS 컨트롤은 다음과 같습니다. iOS 컨트롤은 사전순으로 나열되어 있고 그 옆에 가장 유사한 UWP(유니버설 Windows 플랫폼) 컨트롤이 나열되어 있습니다.

UWP 컨트롤은 실행 중인 장치 유형을 인식하여 그에 따라 모양과 기능을 변경할 수 있습니다. 예를 들어 프로젝트에서 [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681) 컨트롤을 사용하는 경우 데스크톱 컴퓨터와 휴대폰에서 서로 다르게 보이고 동작하도록 자체적으로 최적화할 수 있습니다. 아무 작업도 수행할 필요가 없습니다. 컨트롤이 런타임에 자동으로 조정됩니다.

| iOS 컨트롤(클래스/프로토콜) | 해당 하는 UWP 컨트롤 |
|------------------------------|--------------------------------------|
| 활동 표시기(**UIActivityIndicatorView**) | [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) <br/> [빠른 시작: 진행률 컨트롤 추가](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) 참조 |
| 광고 배너 보기(**ADBannerView**) 및 광고 배너 보기 대리자(**ADBannerViewDelegate**) | [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) <br/> [앱에서 광고 표시](../monetize/display-ads-in-your-app.md) 참조 |
| 단추(UIButton) | [Button](https://msdn.microsoft.com/library/windows/apps/br209265) <br/> 참고 [빠른 시작: 단추 컨트롤 추가](https://msdn.microsoft.com/library/windows/apps/xaml/jj153346) |
| 날짜 선택기(UIDatePicker) | [DatePicker](https://msdn.microsoft.com/library/windows/apps/br211681) |
| 이미지 보기(UIImageView) | [이미지](https://msdn.microsoft.com/library/windows/apps/br242752) <br/> [Image 및 ImageBrush](https://msdn.microsoft.com/library/windows/apps/mt280382) 참조 |
| 레이블(UILabel) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/> [빠른 시작: 텍스트 표시](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) 참조 |
| 지도 보기(MKMapView) 및 지도 보기 대리자(MKMapViewDelegate) | 참조 [UWP 앱 용 Bing 지도](https://go.microsoft.com/fwlink/p/?LinkId=263496) |
| 탐색 컨트롤러(UINavigationController) 및 탐색 컨트롤러 대리자(UINavigationControllerDelegate) | [프레임](https://msdn.microsoft.com/library/windows/apps/br242682) <br/> [탐색](https://msdn.microsoft.com/library/windows/apps/mt187344) 참조 |
| 페이지 컨트롤(UIPageControl) | [페이지](https://msdn.microsoft.com/library/windows/apps/br227503) <br/> [탐색](https://msdn.microsoft.com/library/windows/apps/mt187344) 참조 |
| 선택기 보기(UIPickerView) 및 선택기 보기 대리자(UIPickerViewDelegate) | [ComboBox](https://msdn.microsoft.com/library/windows/apps/br209348) <br/> [콤보 상자 및 목록 상자 추가](https://msdn.microsoft.com/library/windows/apps/xaml/hh780616)도 참조 |
| 진행률 표시줄(UIProgressView) | [ProgressBar](https://msdn.microsoft.com/library/windows/apps/br227529) <br/> [빠른 시작: 진행률 컨트롤 추가](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) 참조 |
| 스크롤 보기(UIScrollView) 및 스크롤 보기 대리자(UIScrollViewDelegate) | [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/br209527) <br/>  [XAML(Extensible Application Markup Language) 스크롤, 이동 및 확대/축소 샘플](https://go.microsoft.com/fwlink/p/?LinkId=238577)(영문) 참조 |
| 검색 창(UISearchBar) 및 검색 창 대리자(UISearchBarDelegate) | [앱에 검색 추가](https://msdn.microsoft.com/library/windows/apps/xaml/jj130767) 참조 <br/>  참고 [빠른 시작: 앱에 검색 추가](https://msdn.microsoft.com/library/windows/apps/xaml/hh868180) |
| 세그먼트 컨트롤(UISegmentedControl) | 없음 |
| 슬라이더(UISlider) | [슬라이더](https://msdn.microsoft.com/library/windows/apps/br209614) <br/>  [슬라이더를 추가하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh868197)도 참조 |
| 분할 보기 컨트롤러(UISplitViewController) 및 분할 보기 컨트롤러 대리자(UISplitViewControllerDelegate) | 없음 |
| 스위치(UISwitch) | [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/br209712) <br/>  [설정/해제 스위치를 추가하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh868198) 참조 |
| 탭 바 컨트롤러(UITabBarController) 및 탭 바 컨트롤러 대리자(UITabBarControllerDelegate) | 없음 |
| 테이블 보기 컨트롤러(UITableViewController), 테이블 보기(UITableView), 테이블 보기 대리자(UITableViewDelegate) 및 테이블 셀(UITableViewCell) | [ListView](https://msdn.microsoft.com/library/windows/apps/br242878) <br/>  [빠른 시작: ListView 및 GridView 컨트롤 추가](https://msdn.microsoft.com/library/windows/apps/xaml/hh780650)도 참조 |
| 텍스트 필드(UITextField) 및 텍스트 필드 대리자(UITextFieldDelegate) | [TextBox](https://msdn.microsoft.com/library/windows/apps/br209683) <br/>  [텍스트 표시 및 편집](https://msdn.microsoft.com/library/windows/apps/mt280218)참조 |
| 텍스트 보기(UITextView) 및 텍스트 보기 대리자(UITextViewDelegate) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/>  [빠른 시작: 텍스트 표시](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) 참조 |
| 보기(UIView) 및 보기 컨트롤러(UIViewController) | [페이지](https://msdn.microsoft.com/library/windows/apps/br227503) <br/>  [탐색](https://msdn.microsoft.com/library/windows/apps/mt187344) 참조 |
| 웹 보기(UIWebView) 및 웹 보기 대리자(UIWebViewDelegate) | [WebView](https://msdn.microsoft.com/library/windows/apps/br227702) <br/>  [XAML WebView 컨트롤 샘플](https://go.microsoft.com/fwlink/p/?LinkId=238582) 참조 |
| 창(UIWindow) | [프레임](https://msdn.microsoft.com/library/windows/apps/br242682) <br/>  [탐색](https://msdn.microsoft.com/library/windows/apps/mt187344) 참조 |

기타 컨트롤에 대해서는 [컨트롤 목록](https://msdn.microsoft.com/library/windows/apps/mt185406)을 참조하세요.

**참고**  JavaScript 및 HTML을 사용 하 여 UWP 앱에 대 한 컨트롤의 목록은 참조 하세요. [컨트롤 목록](https://msdn.microsoft.com/library/windows/apps/hh465453)합니다.

### <a name="next-step"></a>다음 단계

[시작 하기: 탐색](getting-started-navigation.md)

## <a name="related-topics"></a>관련 항목

* [build 2014: XAML UI 및 컨트롤에 대 한 무엇입니까?](https://go.microsoft.com/fwlink/p/?LinkID=397897)
* [build 2014: 공용 XAML UI 프레임 워크를 사용 하 여 앱 개발](https://go.microsoft.com/fwlink/p/?LinkID=397898)
* [build 2014: XAML 빌드를 Visual Studio를 사용 하 여 수렴 형 앱](https://go.microsoft.com/fwlink/p/?LinkID=397876)
