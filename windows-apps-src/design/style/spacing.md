---
title: 간격 및 크기
description: 새로운 Fluent 표준 및 컴팩트 컨트롤 스타일은 디바이스 및 입력 방법에 관계 없이 편리한 사용자 환경을 보장합니다.
keywords: UWP, Windows 10, 컨트롤, 크기, 밀도, 표준, 컴팩트
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 634393e700538dc5db43b2d4065c6742fd7673f1
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234331"
---
# <a name="control-size-and-density"></a>컨트롤 크기 및 밀도

컨트롤 크기와 밀도 조합을 사용하여 Windows 애플리케이션을 최적화하고 앱의 기능과 상호 작용 요구 사항에 가장 적합한 사용자 환경을 제공할 수 있습니다.

기본적으로 UWP 앱은 저밀도(또는 `Standard`) 레이아웃을 사용하여 렌더링됩니다. 그러나 WinUI 2.1부터는 정보가 많은 UI와 비슷한 특수 시나리오를 위한 고밀도(또는 `Compact`) 레이아웃 옵션도 지원됩니다. 기본 스타일 리소스를 통해 지정할 수 있습니다(아래 예제 참조).

두 크기 및 밀도 옵션의 기능과 동작은 변경되지 않고 그대로 유지되지만, 두 가지 밀도 옵션을 지원하기 위해 모든 컨트롤의 기본 본문 글꼴 크기가 14픽셀로 업데이트되었습니다. 이 글꼴 크기는 지역과 디바이스에 관계 없이 작동하며 사용자를 위해 애플리케이션의 균형과 사용 편의를 유지합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Compact Sizing">앱을 열고 컴팩트 크기 조정이 실제로 작동하는 모습을 확인하세요</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-standard-sizing"></a>Fluent 표준 크기

*Fluent 표준 크기*는 정보 밀도와 사용자 편의 간에 균형을 유지하기 위해 개발되었습니다. 화면에 있는 모든 항목을 40x40 유효 픽셀(epx) 대상에 맞추기 때문에 UI 요소를 그리드에 맞추고 시스템 수준 스케일링에 따라 적절하게 크기를 조정할 수 있습니다.

**표준 크기는 터치와 포인터 입력을 모두 수용하도록 설계되었습니다.**

> [!NOTE]
>유효 픽셀과 크기 조정에 대한 자세한 내용은 [Windows 앱 디자인 소개](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)를 참조하세요.
>
> 시스템 수준 크기 조정에 대한 자세한 내용은 [맞춤, 여백, 안쪽 여백](../layout/alignment-margin-padding.md)을 참조하세요.

Windows 10 2018년 10월 업데이트(버전 1809)의 경우 모든 사용 시나리오의 유용성을 높이기 위해 모든 UWP 컨트롤의 표준 기본 크기를 줄였습니다.

다음 이미지는 Windows 10 2018년 10월 업데이트에 도입된 컨트롤 레이아웃 변경 내용의 일부를 보여줍니다. 특히 컨트롤의 헤더와 상단 사이의 여백이 8epx에서 4epx로 줄고, 44epx 그리드가 40epx 그리드로 변경되었습니다.

![표준 컨트롤 레이아웃 예제](images/standarddensity.png)

*표준 컨트롤 레이아웃 예제*

다음 이미지는 Windows 10 2018년 10월 업데이트의 컨트롤 크기 변경 내용을 보여줍니다. 특히 40epx 그리드에 맞추도록 변경되었습니다.

![표준 명령 예제](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Fluent 컴팩트 크기

컴팩트 크기는 정보가 많은 고밀도 컨트롤 그룹을 사용할 수 있으며 다음 용도에 유용할 수 있습니다.

- 대량의 콘텐츠를 검색합니다.
- 페이지에 표시되는 콘텐츠를 최대화합니다.
- 컨트롤과 콘텐츠의 탐색 및 상호 작용

**컴팩트 크기는 주로 포인터 입력에 맞게 설계되었습니다.**

### <a name="examples"></a>예

컴팩트 크기는 페이지 수준에서 또는 특정 레이아웃에서 애플리케이션을 사용하여 지정할 수 있는 특수 리소스 사전을 통해 구현됩니다. 리소스 사전은 [WinUI](https://docs.microsoft.com/uwp/toolkits/winui/) Nuget 패키지에서 사용할 수 있습니다.

다음 예제에서는 페이지 및 개별 그리드 컨트롤에 `Compact` 스타일을 적용하는 방법을 보여줍니다.

#### <a name="page-level"></a>페이지 수준

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>그리드 수준

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련된 문서

- [터치 대상에 대한 지침](../input/guidelines-for-targeting.md)
- [ResourceDictionary 및 XAML 리소스 참조](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [리소스 사전](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary)
- [XAML 스타일](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles) 
