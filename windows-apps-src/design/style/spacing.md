---
title: 간격 및 크기
description: 장치 및 입력 방법에 관계 없이 편리한 사용자 환경을 보장 하는 새로운 Fluent 표준 및 Compact 컨트롤 스타일입니다.
keywords: UWP, Windows 10, 컨트롤, 크기, 밀도, standard, compact
ms.date: 4/4/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 7b74e3dc2ad047d9e52509b71ef00b829ad63a0d
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249455"
---
# <a name="control-size-and-density"></a>컨트롤 크기와 밀도

유니버설 Windows 플랫폼 (UWP) 응용 프로그램을 최적화 하 고 앱의 기능 및 상호 작용 요구 사항에 가장 적합 한 사용자 환경을 제공 하려면 컨트롤 크기와 밀도 조합해 서 사용 합니다.

기본적으로 UWP 앱을 저밀도 사용 하 여 렌더링 됩니다 (또는 `Standard`) 레이아웃 합니다. 그러나는 고밀도 WinUI 2.1부터 (또는 `Compact`) 레이아웃 옵션 정보에 대 한 풍부한 UI와 유사한 특수 시나리오도 지원 됩니다. 기본 스타일 리소스를 통해 지정할 수 있습니다 (아래 예제 참조).

둘 간에 일관성을 유지 하 고 기능 및 동작 하는 동안 변경 되지 않은 크기와 밀도 옵션을 기본 본문 글꼴 크기는 이러한 두 밀도 옵션을 지원 하기 위해 모든 컨트롤에 대 한 14px 하도록 업데이트 되었습니다. 이 글꼴 크기는 지역 및 장치에서 작동 하 고 응용 프로그램을 부하가 분산 되 고 사용자가 편리 하 게 계속 되도록 합니다.

## <a name="fluent-standard-sizing"></a>Fluent 표준 크기 조정

*Fluent 표준 크기 조정* 정보 밀도 및 사용자와 쾌적 간의 균형을 이룰 위해 만들어졌습니다. 효과적으로 화면에 있는 모든 항목 UI 요소 표 형식으로 정렬 하 고 적절 하 게 확장할 수 있습니다. 시스템 수준 크기 조정에 기반 하는 40 x 40 효과적인 픽셀 (epx) 대상에 맞춥니다.

**표준 크기 조정은 터치와 입력 포인터에 맞게 설계 되었습니다.**

> [!NOTE]
>효과적인 픽셀 및 크기 조정에 대 한 자세한 내용은 참조 하세요. [UWP 앱 디자인 소개](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 시스템 수준 크기 조정에 대 한 자세한 내용은 참조 하세요. [맞춤, 여백, 안쪽 여백](../layout/alignment-margin-padding.md)합니다.

Windows 10 년 10 월에 대 한 모든 사용 시나리오에서 유용성을 높이려면 2018 업데이트 (버전 1809), 표준, 모든 UWP 컨트롤에 대 한 기본 크기 감소 했습니다.

다음 이미지는 Windows를 사용 하 여 도입 된 레이아웃 변경 내용을 컨트롤의 일부 표시 10 년 10 월 2018 업데이트 합니다. 특히 4epx, 하는 헤더와 컨트롤의 위쪽 사이의 여백을 8epx에서 감소 및 44epx 표 40epx 표 형식으로 변경 되었습니다.

![표준 컨트롤 레이아웃 예제](images/standarddensity.png)

*표준 컨트롤 레이아웃 예제*

이 다음 이미지는 Windows에 대 한 크기를 제어 하는 변경 10 년 10 월 2018 업데이트 합니다. 40epx 눈금에 맞춤 특히입니다.

![표준 명령 예제](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Fluent Compact 크기 조정

Compact 크기 조정 조밀한 정보가 풍부한 그룹 컨트롤을 사용 하도록 설정 및 다음 유용할 수 있습니다.

- 많은 양의 콘텐츠를 검색 합니다.
- 페이지에 표시 되는 콘텐츠의 최대화 합니다.
- 컨트롤 및 콘텐츠를 사용 하 여 상호 작용 및 탐색

**Compact 크기는 주로 포인터 입력에 맞게 설계 되었습니다.**

### <a name="examples"></a>예

Compact 크기 조정 페이지 수준에서 또는 특정 레이아웃에 응용 프로그램에서 지정할 수 있는 특별 한 리소스 사전을 통해 구현 됩니다. 리소스 사전에서 사용할 수는 [WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/) Nuget 패키지.

다음 예제에 나온 방법을 `Compact` 페이지 및 개별 표 컨트롤에 대 한 스타일을 적용할 수 있습니다.

#### <a name="page-level"></a>페이지 수준

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>표 수준

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="related-articles"></a>관련 문서

- [터치 대상에 대 한 지침](../input/guidelines-for-targeting.md)
- [ResourceDictionary 및 XAML 리소스 참조](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [리소스 사전](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [XAML 스타일](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
