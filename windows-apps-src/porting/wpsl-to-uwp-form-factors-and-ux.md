---
author: stevewhims
description: Windows 앱은 PC, 모바일 디바이스 및 기타 여러 종류의 디바이스에서 일반적인 모양과 느낌을 공유합니다. 사용자 인터페이스, 입력 및 조작 패턴이 매우 비슷하고, 디바이스 간에 이동하면 친숙한 환경이 제공됩니다.
title: 폼 팩터 및 UX에 대 한 WindowsPhone Silverlight를 UWP로 포팅
ms.assetid: 96244516-dd2c-494d-ab5a-14b7dcd2edbd
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 809cf2691a2bc7b7c72d4ba031fa4c6b45335dde
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6658700"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-form-factor-and-ux"></a>폼 팩터 및 UX에 대 한 WindowsPhone Silverlight를 UWP로 포팅


이전 항목은 [비즈니스 및 데이터 계층 포팅](wpsl-to-uwp-business-and-data.md)입니다.

Windows 앱은 PC, 모바일 장치 및 기타 여러 종류의 장치에서 일반적인 모양과 느낌을 공유합니다. 사용자 인터페이스, 입력 및 조작 패턴이 매우 비슷하고, 디바이스 간에 이동하면 친숙한 환경이 제공됩니다. 실제 크기, 기본 방향 및 유니버설 Windows 플랫폼 (UWP) 앱 Windows10에 의해 렌더링 되는 방식에 유효 픽셀 해상도 요인과 같은 장치 간 차이점이 있습니다. 좋은 소식은 시스템에서 유효 픽셀과 같은 스마트 개념을 사용하여 대부분의 번거로운 작업을 처리합니다.

## <a name="different-form-factors-and-user-experience"></a>다른 폼 팩터 및 사용자 환경

다양한 장치가 다양한 가로 세로 비율로, 가능한 여러 세로 및 가로 해상도를 사용할 수 있습니다. UWP 앱의 해당 인터페이스, 텍스트 및 자산의 시각적 측면은 어떻게 확장되나요? 터치, 마우스 및 키보드 입력을 어떻게 지원할 수 있나요? 시청 거리가 서로 다른 다양한 크기의 장치에서 터치를 지원하는 앱에서 하나의 컨트롤을 픽셀 밀도가 서로 다른 올바른 크기의 터치 대상으로 사용*하고* 다양한 거리에서 콘텐츠를 읽을 수 있도록 하려면 어떻게 해야 하나요? 다음 섹션에서는 알아야 할 사항에 대해 설명합니다.

## <a name="what-is-the-size-of-a-screen-really"></a>실제 화면 크기는 어떻게 되나요?

간단하게 대답하면 화면 크기는 디스플레이의 객관적인 크기와 디스플레이까지의 거리에 따라 다르기 때문에 주관적입니다. 주관적이라는 것은 각자 자신의 입장에서 생각하므로 앱 개발자가 어떻게 하든 마찬가지라는 의미입니다.

객관적으로 화면은 인치 단위 및 실제(원시) 픽셀로 측정됩니다. 이 두 메트릭을 모두 알면 적합한 인치당 픽셀 수를 알 수 있습니다. 이것이 바로 픽셀 밀도이며, DPI(인치당 도트 수) 또는 PPI(인치당 픽셀 수)라고도 합니다. DPI의 역은 픽셀의 실제 크기로 1인치의 몇 분의 1에 불과합니다. 픽셀 밀도를 *해상도*라고도 하지만, 해상도는 막연히 픽셀 수를 나타내는 데 주로 사용됩니다.

시청 거리가 증가하면 모든 객관적 메트릭은 더 작아지는 것처럼 *보이고* 화면의 *유효 크기*와 *유효 해상도*로 확인됩니다. 가장 가까운 위치에서 사용되는 것은 휴대폰이고 그다음이 태블릿, PC 모니터의 순이며, [Surface Hub](http://www.microsoft.com/microsoft-surface-hub) 장치와 TV가 가장 멀리서 사용됩니다. 이를 보완하기 위해 디바이스가 시청 거리에서 객관적으로 더 커지는 경향이 있습니다. UI 요소에서 크기를 설정할 때 유효 픽셀(epx)이라는 단위로 해당 크기를 설정합니다. 및 UI 요소의 최상의 보기 환경을 제공 하기 위해 실제 픽셀로 최상의 크기를 계산 하는 장치에서 일반적인 가시 거리와 DPI 계정에 Windows10 걸립니다. [보기/유효 픽셀, 가시거리 및 배율 인수](wpsl-to-uwp-porting-xaml-and-ui.md)를 참조하세요.

그렇다고 해도 각 환경을 확인할 수 있도록 여러 디바이스에서 앱을 테스트하는 것이 좋습니다.

## <a name="touch-resolution-and-viewing-resolution"></a>터치 해상도 및 보기 해상도

어포던스(UI 위젯)를 위해 터치 크기가 적정해야 합니다. 따라서 픽셀 밀도가 다른 디바이스 간에 터치 대상이 실제 크기에 가깝게 유지되어야 합니다. 여기에서는 유효 픽셀도 유용합니다. 터치 대상에 적합한 거의 일정한 실제 크기를 유지하기 위해 다양한 디바이스에서 픽셀 밀도를 고려하여 유효 픽셀의 크기를 조정합니다.

시청 거리에 대해 텍스트의 크기가 읽기에 적합하고(20인치 시청 거리에서 12포인트 텍스트가 가장 적합함) 이미지의 크기가 적절하고 해상도가 유효해야 합니다. 다양한 장치에서 유효 픽셀 크기를 동일하게 조정하여 UI를 읽기 쉽고 적절한 크기로 유지합니다. 텍스트와 다른 벡터 기반 그래픽 크기는 자동으로 적절하게 조정됩니다. 개발자가 단일의 큰 크기 자산을 제공한 경우 래스터(비트맵) 기반 그래픽의 크기도 자동으로 조정됩니다. 개발자는 대상 장치의 배율 인수에 적절한 자산을 자동으로 로드할 수 있도록 각 자산을 크기 범위로 제공하는 것이 좋습니다. 자세한 내용은 [보기/유효 픽셀, 가시거리 및 배율 인수](wpsl-to-uwp-porting-xaml-and-ui.md)를 참조하세요.

## <a name="layout-and-adaptive-visual-state-manager"></a>레이아웃 및 적응 Visual State Manager

화면 크기의 의미를 이해하는 것과 관련된 요소에 관해 설명했습니다. 이제 앱의 레이아웃과 추가 화면 영역(사용 가능한 경우)을 활용하는 방법에 대해 생각해 보겠습니다. 좁은 모바일 장치에서 실행하도록 설계된 매우 간단한 앱에서 이 페이지를 고려하세요. 이 페이지가 더 큰 화면에서는 어떻게 보이나요?

![포팅된 Windows Phone 스토어 앱](images/wpsl-to-uwp-case-studies/c01-04-uni-phone-app-ported.png)

책 목록에 가장 적합한 가로 세로 비율이 세로 방향이므로 모바일 버전이 세로 전용 방향으로 제약되며, 텍스트 페이지에 대해서도 모바일 디바이스에서 단일 열로 유지하는 것이 가장 좋으므로 세로 전용 방향으로 제약합니다. 하지만 PC 및 태블릿 화면은 방향과 상관없이 모두 크므로 큰 장치에서는 모바일 장치처럼 제약할 필요가 없습니다.

모바일 버전처럼 보이도록 앱을 광학적으로 확대하면 장치 및 추가 공간을 활용할 수 없을 뿐만 아니라 사용자에게도 도움이 되지 않습니다. 동일한 콘텐츠를 더 크게 표시하는 것보다 더 많은 콘텐츠를 표시하는 것을 고려해야 합니다. 패블릿에서도 더 많은 행의 콘텐츠를 표시할 수 있습니다. 추가 공간을 사용하여 광고 등과 같은 다른 콘텐츠를 표시하거나, 목록 상자를 목록 보기로 변경하고 항목을 여러 열로 줄 바꿈(가능한 경우)하여 공간을 그런 식으로 사용할 수 있습니다. [목록 및 그리드 보기 컨트롤에 대한 지침](https://msdn.microsoft.com/library/windows/apps/mt186889)을 참조하세요.

목록 보기 및 그리드 보기 같은 새 컨트롤 외에도 WindowsPhone Silverlight에서 설정 된 레이아웃 형식의 대부분 유니버설 Windows 플랫폼 (UWP)에서 해당 항목을 갖고 있습니다. 예를 들어 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 및 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)입니다. 이러한 형식을 사용하는 대부분의 UI는 쉽게 포팅할 수 있지만, 항상 다른 크기의 장치에서 자동으로 크기를 조정하고 다시 배치하도록 이러한 레이아웃 패널의 동적 레이아웃 기능 활용 방법을 찾아야 합니다.

시스템 컨트롤 및 레이아웃 패널에 내장 된 동적 레이아웃 하는 것을 [적응 Visual State Manager](wpsl-to-uwp-porting-xaml-and-ui.md)이라는 새로운 Windows10 기능을 사용할 수 있습니다.

## <a name="input-modalities"></a>입력 형식

WindowsPhone Silverlight 인터페이스는 터치와 관련이 있습니다. 포팅된 앱의 인터페이스도 터치를 지원해야 하지만 마우스, 키보드 등과 같은 다른 입력 형식을 지원하도록 선택할 수도 있습니다. UWP에서는 마우스, 펜 및 터치식 입력이 *포인터 입력*으로 통합됩니다. 자세한 내용은 [포인터 입력 처리](https://msdn.microsoft.com/library/windows/apps/mt404610) 및 [키보드 조작](https://msdn.microsoft.com/library/windows/apps/mt185607)을 참조하세요.

## <a name="maximizing-markup-and-code-re-use"></a>태그 및 코드 재사용 최대화

다양한 폼 팩터가 있는 대상 장치와 UI를 공유하는 기술은 [태그 및 코드 재사용 최대화](wpsl-to-uwp-porting-to-a-uwp-project.md)를 참조하세요.

## <a name="more-info-and-design-guidelines"></a>추가 정보 및 디자인 지침

-   [UWP 앱 디자인](http://dev.windows.com/design)
-   [글꼴에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh700394)
-   [다양한 양식 요소 계획](https://msdn.microsoft.com/library/windows/apps/dn958435)

## <a name="related-topics"></a>관련 항목

* [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)

