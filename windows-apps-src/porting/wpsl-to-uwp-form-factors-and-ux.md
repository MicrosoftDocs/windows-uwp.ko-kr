---
description: Windows 앱은 Pc, 모바일 장치 및 기타 여러 종류의 장치에서 공통적인 모양과 느낌을 공유 합니다. 사용자 인터페이스, 입력 및 상호 작용 패턴은 매우 비슷하며 장치 간에 이동 하는 사용자는 친숙 한 환경을 시작 합니다.
title: 폼 팩터 및 UX를 위해 UWP에 Windows Phone Silverlight 포팅
ms.assetid: 96244516-dd2c-494d-ab5a-14b7dcd2edbd
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e4471d7cfb682906b4f340fdbdc294723116855f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164358"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-form-factor-and-ux"></a>폼 팩터 및 UX를 위해 UWP에 Windows Phone Silverlight 포팅


이전 항목에서는 [비즈니스 및 데이터 계층을 이식](wpsl-to-uwp-business-and-data.md)했습니다.

Windows 앱은 Pc, 모바일 장치 및 기타 여러 종류의 장치에서 공통적인 모양과 느낌을 공유 합니다. 사용자 인터페이스, 입력 및 상호 작용 패턴은 매우 비슷하며 장치 간에 이동 하는 사용자는 친숙 한 환경을 시작 합니다. Windows 10에서 유니버설 Windows 플랫폼 (UWP) 앱이 렌더링 되는 방식에 대 한 실제 크기, 기본 방향 및 유효 픽셀 해상도 요소와 같은 장치 간의 차이점입니다. 좋은 소식은 효과적 픽셀 같은 스마트 개념을 사용 하 여 시스템에서 많은 많은 작업이 처리 되는 것입니다.

## <a name="different-form-factors-and-user-experience"></a>다양 한 폼 팩터 및 사용자 환경

다양 한 측면에서 다양 한 가로 및 세로 해상도의 장치를 사용할 수 있습니다. UWP 앱 확장의 인터페이스, 텍스트 및 자산의 시각적 측면은 어떻게 되나요? 마우스 및 키보드 입력 뿐만 아니라 터치를 어떻게 지원할 수 있나요? 다른 보기 거리를 사용 하는 다양 한 크기의 장치에 터치를 지 원하는 앱을 사용 하는 경우 컨트롤은 서로 다른 픽셀 밀도에서 오른쪽 크기의 터치 대상이 되 *고* 다른 거리에서 콘텐츠를 읽을 수 있나요? 다음 섹션에서는 알아야 할 사항에 대해 설명 합니다.

## <a name="what-is-the-size-of-a-screen-really"></a>화면 크기는 정말 인가요?

간단한 대답은 표시의 목표 크기 뿐만 아니라 얼마나 멀리 떨어져 있는지에 따라 달라 지므로 주관적인 것입니다. Mpqa은 사용자의 신발에 직접 배치 해야 하며,이는 좋은 앱 개발자가 어떤 경우에도이 작업을 수행 한다는 것을 의미 합니다.

객관적으로 화면을 측정 하 고, 인치 단위 및 실제 (raw) 픽셀 단위로 측정 합니다. 이러한 메트릭을 모두 알면 1 인치에 맞는 픽셀 수를 알 수 있습니다. 이는 픽셀 밀도 이며 DPI (인치당 도트 수) 라고도 합니다. PPI (인치당 픽셀 수) 라고도 합니다. DPI의 역 수는 픽셀의 실제 크기 (인치)입니다. 픽셀 밀도는 보통 픽셀 수를 의미 하기 위해 느슨하게 사용 되더라도 *해상도*라고도 합니다.

거리가 늘어나면 이러한 모든 *목표 메트릭은 더* 작은 것으로 보이고 화면의 *유효 크기*와 *유효한 해상도*로 확인 됩니다. 휴대폰은 일반적으로 눈에 가까운 곳에 저장 됩니다. 그러면 태블릿 PC 모니터가 다음에 Surface Hub 장치 및 Tv가 [Surface Hub](https://www.microsoft.com/surface/devices/surface-hub) 됩니다. 보정을 위해 장치는 거리가 표시 되는 객관적으로 더 큰 경향이 있습니다. UI 요소에 크기를 설정 하는 경우 해당 크기를 유효 픽셀 (epx) 이라는 단위로 설정 합니다. 및 Windows 10은 가장 적합 한 보기 환경을 제공 하기 위해 실제 픽셀의 UI 요소에 가장 적합 한 크기를 계산 하는 데 사용 하는 DPI 및 일반적인 보기 거리를 고려 합니다. [보기/효과적인 픽셀, 거리 보기 및 배율 인수](wpsl-to-uwp-porting-xaml-and-ui.md)를 참조 하세요.

따라서 각 환경을 확인할 수 있도록 다양 한 장치를 사용 하 여 앱을 테스트 하는 것이 좋습니다.

## <a name="touch-resolution-and-viewing-resolution"></a>터치 해상도 및 해상도 보기

어포던스(UI 위젯)를 위해 터치 크기가 적정해야 합니다. 따라서 터치 대상은 픽셀 밀도가 다를 수 있는 여러 장치에서 실제 크기를 더 많이 또는 더 작게 유지 해야 합니다. 효과적인 픽셀은이를 도와 드리겠습니다 .이는 터치 목표에 적합 한 더 크거나 작은 크기의 실제 크기를 얻기 위해 픽셀 밀도를 고려 하 여 다양 한 장치에서 크기를 조정 하 고 있습니다.

텍스트는 읽을 수 있는 올바른 크기 여야 합니다 (20 인치 보기 거리의 12 포인트 텍스트는 엄지 단추 임). 그리고 이미지는 보기 거리의 올바른 크기와 유효 해상도를 필요로 합니다. 서로 다른 장치에서 동일 하 게 적용 되는 픽셀 크기 조정은 UI 요소를 오른쪽 크기와 읽기 쉽게 유지 합니다. 텍스트 및 기타 벡터 기반 그래픽은 자동으로 확장 됩니다. 개발자가 단일 크기의 자산을 제공 하는 경우 래스터 (비트맵) 기반 그래픽도 자동으로 크기가 조정 됩니다. 그러나 개발자는 대상 장치의 배율 인수에 대 한 적절 한 항목을 자동으로 로드할 수 있도록 각 자산의 크기를 제공 하는 것이 좋습니다. 이에 대 한 자세한 내용은 [보기/유효 픽셀, 거리 보기 및 배율 인수](wpsl-to-uwp-porting-xaml-and-ui.md)를 참조 하세요.

## <a name="layout-and-adaptive-visual-state-manager"></a>레이아웃 및 적응 시각적 상태 관리자

화면 크기를 의미 있게 이해 하는 데 관련 된 요인을 설명 했습니다. 이제 앱의 레이아웃 및 사용 가능한 추가 화면 부동산을 사용 하는 방법에 대해 생각해 보겠습니다. 좁은 모바일 장치에서 실행 되도록 설계 된 매우 간단한 앱에서이 페이지를 살펴보겠습니다. 큰 화면에서이 페이지가 표시 되는 모양

![포팅 된 windows phone 스토어 앱](images/wpsl-to-uwp-case-studies/c01-04-uni-phone-app-ported.png)

모바일 버전은 책 목록에 대 한 최상의 가로 세로 비율 이므로 세로 전용 방향으로 제한 됩니다. 그리고 모바일 장치에서 단일 열에 가장 잘 유지 되는 텍스트 페이지에 대해 동일한 작업을 수행 합니다. 하지만 PC와 태블릿 화면은 어느 방향에서 든 모바일 장치 제약 조건이 더 큰 장치에서 불필요 한 제한 사항 처럼 보입니다.

앱을 확대/축소 하는 것이 더 Optically, 장치 및 추가 공간을 활용 하지 않으며, 사용자가 잘 서비스를 제공 하지 않습니다. 동일한 콘텐츠가 더 많이 표시 되지 않고 더 많은 콘텐츠를 표시 하는 것을 고려해 야 합니다. Phablet 에서도 몇 가지 콘텐츠 행을 표시할 수 있습니다. 추가 공간을 사용 하 여 광고와 같은 다른 콘텐츠를 표시할 수 있습니다. 또는 목록 상자를 목록 보기로 변경 하 고 가능한 경우 여러 열에 항목을 래핑하여 해당 방식으로 공간을 사용할 수 있습니다. [목록 및 그리드 뷰 컨트롤에 대 한 지침](../design/controls-and-patterns/lists.md)을 참조 하세요.

목록 보기 및 그리드 보기와 같은 새 컨트롤 외에도 Windows Phone Silverlight에서 설정 된 대부분의 레이아웃 형식은 UWP (유니버설 Windows 플랫폼)에 해당 합니다. 예: [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas), [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid)및 [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel). 이러한 형식을 사용 하는 대부분의 UI를 이식 하는 것은 간단 하지만 항상 이러한 레이아웃 패널의 동적 레이아웃 기능을 활용 하 여 다양 한 크기의 장치에서 자동으로 크기를 조정 하 고 레이아웃을 조정 하는 방법을 찾아야 합니다.

시스템 컨트롤 및 레이아웃 패널에 기본 제공 되는 동적 레이아웃 외에도 [적응형 시각적 상태 관리자](wpsl-to-uwp-porting-xaml-and-ui.md)라는 새로운 Windows 10 기능을 사용할 수 있습니다.

## <a name="input-modalities"></a>입력 소프트웨어나

Windows Phone Silverlight 인터페이스는 터치 전용입니다. 또한 이식 된 앱의 인터페이스에서 터치를 지원 하지만 마우스 및 키보드와 같은 다른 입력 소프트웨어나 지원 하도록 선택할 수 있습니다. UWP에서 마우스, 펜 및 터치식 입력은 *포인터 입력*으로 통합 됩니다. 자세한 내용은 [포인터 입력 처리](../design/input/handle-pointer-input.md)및 [키보드 상호 작용](../design/input/keyboard-interactions.md)을 참조 하세요.

## <a name="maximizing-markup-and-code-re-use"></a>태그 및 코드 다시 사용 최대화

다양 한 폼 팩터를 사용 하 여 UI를 대상 장치에 공유 하는 방법에 대해서는 [태그 및 코드 재사용 극대화](wpsl-to-uwp-porting-to-a-uwp-project.md) 목록을 다시 참조 하세요.

## <a name="more-info-and-design-guidelines"></a>추가 정보 및 디자인 지침

-   [UWP 앱 디자인](https://developer.microsoft.com/windows/apps/design)
-   [글꼴에 대 한 지침](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)
-   [다양 한 폼 팩터 계획](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)

## <a name="related-topics"></a>관련 항목

* [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)