---
author: DelfCo
Description: "RTL(오른쪽에서 왼쪽) 방향으로 읽는 것을 포함하여 여러 언어의 레이아웃과 글꼴을 지원하기 위해 앱을 개발합니다."
title: "레이아웃 및 글꼴 조정, RTL 지원"
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 9c700928d2ec0da21b518528289034296637eeff
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>레이아웃 및 글꼴 조정, RTL 지원
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

RTL(오른쪽에서 왼쪽) 방향으로 읽는 것을 포함하여 여러 언어의 레이아웃과 글꼴을 지원하기 위해 앱을 개발합니다. 

## <a name="layout-guidelines"></a>레이아웃 지침


일부 언어(예: 독일어 및 핀란드어)에는 영어보다 더 많은 텍스트 공간이 필요합니다. 일본어 같은 일부 언어의 글꼴은 높이가 더 높아야 합니다. 그리고 아랍어 및 히브리어와 같은 일부 언어에서는 텍스트 레이아웃과 앱 레이아웃이 RTL(오른쪽에서 왼쪽) 읽는 순서를 따라야 합니다.

절대 위치, 고정 너비 또는 고정 높이 대신 유연한 레이아웃 메커니즘을 사용하세요. 필요한 경우 언어에 따라 특정 UI 요소를 조정할 수 있습니다.

다음과 같이 요소에 대한 **Uid**를 지정합니다.

```XML
<TextBlock x:Uid="Block1">
```

앱의 ResW 파일에 Block1.Width에 대한 리소스가 있는지 확인합니다. 이 리소스는 지역화하는 언어별로 설정할 수 있습니다.

C++, C\# 또는 Visual Basic으로 작성한 Windows 스토어 앱의 경우 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 속성을 대칭 안쪽 여백 및 여백과 함께 사용하여 다른 레이아웃 방향에 대한 지역화를 사용하도록 설정합니다.

XAML 레이아웃 컨트롤(예: [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704))은 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 속성을 사용하여 자동으로 배율 조정 및 대칭 이동합니다. 앱의 고유한 **FlowDirection** 속성을 지역화 도구를 위한 리소스로 공개합니다.

앱의 기본 페이지에 대한 **Uid**를 지정합니다.

```XML
<Page x:Uid="MainPage">
```

앱의 **ResW** 파일에 MainPage.FlowDirection에 대한 리소스가 있는지 확인합니다. 이 리소스는 지역화하는 언어별로 설정할 수 있습니다.


## <a name="mirroring-images"></a>이미지 미러링

앱에 RTL을 위해 미러링해야 하는 이미지가 있는 경우(즉, 동일한 이미지가 대칭될 수 있음) [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 속성을 적용할 수 있습니다.

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```


앱에서 이미지를 올바르게 대칭 이동하기 위해 다른 이미지가 필요한 경우 리소스 관리 시스템을 [layoutdir 한정자](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)와 함께 사용할 수 있습니다. [응용 프로그램 언어](manage-language-and-region.md)가 RTL 언어로 설정되어 있는 경우 file.layoutdir-rtl.png 이미지가 선택됩니다. 이 방법은 이미지의 일부만 대칭 이동되는 경우에 사용할 수 있습니다.

## <a name="fonts"></a>글꼴

특정 언어에 대한 권장 글꼴 패밀리, 크기, 두께 및 스타일에 프로그래밍 방식으로 액세스하려면 [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) 글꼴 매핑 API를 사용합니다. **LanguageFont** 개체는 UI 헤더, 알림, 본문, 사용자 편집 가능한 문서 본문 글꼴을 비롯하여 콘텐츠의 다양한 범주에 대한 올바른 글꼴 정보에 액세스합니다.

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>RTL(오른쪽에서 왼쪽) 언어를 처리하기 위한 모범 사례

앱이 RTL(오른쪽에서 왼쪽) 언어로 지역화되어 있는 경우 API를 사용하여 RootFrame에 대한 기본 텍스트 방향을 설정합니다. 이렇게 하면 RootFrame 내에 포함된 모든 컨트롤이 기본 텍스트 방향에 적절히 응답합니다.  둘 이상의 언어가 지원되는 경우 기본 설정 언어에 대한 LayoutDirection을 사용하여 FlowDirection 속성을 설정합니다. Windows에 포함된 대부분의 컨트롤은 이미 FlowDirection을 사용합니다. 사용자 지정 컨트롤을 구현하는 경우 FlowDirection을 사용하여 RTL 및 LTR 언어에 대해 적절하게 레이아웃을 변경해야 합니다.

C#
```csharp    
// For bidirectional languages, determine flow direction for RootFrame and all derived UI.

    string resourceFlowDirection = ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
    if (resourceFlowDirection == "LTR")
    {
       RootFrame.FlowDirection = FlowDirection.LeftToRight;
    }
    else
    {
       RootFrame.FlowDirection = FlowDirection.RightToLeft;
    }
```

C++:
```
    // Get preferred app language
    m_language = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);
     
    // Set flow direction accordingly
    m_flowDirection = ResourceManager::Current->DefaultContext->QualifierValues->Lookup("LayoutDirection") != "LTR" ? 
       FlowDirection::RightToLeft : FlowDirection::LeftToRight;
```


### <a name="rtl-faq"></a>RTL FAQ 

<dl>
  <dt> <p><b>Q:</b> <b>FlowDirection</b>은 현재 언어 선택에 따라 자동으로 설정되나요? 예를 들어 영어를 선택하는 경우 왼쪽에서 오른쪽으로 표시되고 아랍어를 선택하는 경우 오른쪽에서 왼쪽으로 표시되나요?</p></dt>

  <dd><p><b>A:</b> <b>FlowDirection</b>은 언어를 고려하지 않습니다. 현재 표시 중인 언어에 맞게 <b>FlowDirection</b>을 설정합니다. 위의 샘플 코드를 참조하세요.</p></dd> 

  <dt> <p><b>Q:</b> 지역화에 익숙하지 않습니다. 리소스에 흐름 방향이 이미 포함되어 있나요? 현재 언어의 흐름 방향을 결정할 수 있나요?</p></dt>

  <dd> <p><b>A:</b> 현재 모범 사례를 사용하는 경우 리소스에 흐름 방향이 직접 포함되어 있지는 않습니다. 현재 언어에 대한 흐름 방향을 결정해야 합니다. 두 가지 방법으로 이 작업을 수행할 수 있습니다. </p>
   <p>좋은 방법은 기본 설정 언어에 대한 LayoutDirection을 사용하여 RootFrame의 FlowDirection 속성을 설정하는 것입니다. RootFrame의 모든 컨트롤은 RootFrame의 FlowDirection을 상속받습니다.</p>
   <p>또 다른 방법은 지역화하는 RTL 언어의 resw 파일에서 FlowDirection을 설정하는 것입니다. 예를 들어 아랍어 resw 파일과 히브리어 resw 파일이 있을 수 있습니다. 이러한 파일에서 x:UID를 사용하여 FlowDirection을 설정할 수 있습니다. 그러나 이 메서드는 프로그래밍 방식보다 오류 발생 가능성이 더 큽니다.</p></dd>
</dl>


## <a name="related-topics"></a>관련 항목
[FlowDirection](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.flowdirection.aspx)
