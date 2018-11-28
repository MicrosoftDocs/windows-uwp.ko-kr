---
Description: Learn to develop accessible Windows 10 UWP apps that include keyboard navigation, color and contrast settings, and support for assistive technologies.
ms.assetid: 9311D23A-B340-42F0-BEFE-9261442AF108
title: 포괄 Windows 10 앱 개발
label: Developing inclusive Windows 10 apps
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e38b57deb7101dcf0476bd3d952fc01ffd605db
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7835027"
---
# <a name="developing-inclusive-windows-apps"></a>포괄 Windows 앱 개발  

이 문서에서는 접근성 있는 UWP(유니버설 Windows 플랫폼) 앱을 개발하는 방법에 대해 설명합니다. 특히 앱에 대한 논리 계층 구조 디자인 방법에 대해 이해하고 있다고 가정합니다. 키보드 탐색, 색 및 대비 설정, 그리고 보조 기술에 대한 지원 기능을 포함하는 액세스 가능한 Windows 10 UWP 앱 개발 방법을 알아봅니다.

아직 이해하지 못했으면 [포괄 소프트웨어 디자인](designing-inclusive-software.md)을 먼저 읽고 시작하세요.

앱에 접근성이 있도록 하려면 다음 세 가지를 수행해야 합니다.

1. [프로그래밍 방식 액세스](#programmatic-access)에 UI 요소를 표시합니다.
2. 앱이 마우스 또는 터치 스크린을 사용할 수 없는 사용자를 위해 [키보드 탐색](#keyboard-navigation)을 지원하는지 확인합니다.
3. 앱이 접근성 있는 [색 및 대비](#color-and-contrast) 설정을 지원하는지 확인합니다.

## <a name="programmatic-access"></a>프로그래밍 방식 액세스  
프로그래밍 방식 액세스는 앱의 접근성을 만드는 데 중요합니다. 이렇게 하려면 앱에 있는 콘텐츠 및 대화형 UI 요소의 접근성 있는 이름(필수) 및 설명(선택)을 설정합니다. 이렇게 하면 UI 컨트롤이 화면 읽기 프로그램(예: 내레이터) 또는 대체 출력 디바이스(예: 점자 표시)와 같은 AT(보조 기술)에 표시됩니다. 프로그래밍 방식으로 액세스하지 않고는 보조 기술에 대한 API가 정보를 정확하게 해석할 수 없어 제품을 충분히 사용할 수 없습니다. 또는 AT가 문서화되지 않은 프로그래밍 인터페이스나 접근성 인터페이스로 사용하지 않는 기술을 강제로 사용하도록 합니다. UI 컨트롤이 보조 기술에 표시되면 AT가 사용자에게 어떤 작업 및 옵션이 사용 가능한지 확인할 수 있습니다.  

앱 UI 요소를 AT(보조 기술)에 사용할 수 있도록 하는 방법에 대한 자세한 내용은 [기본적인 접근성 정보 표시](basic-accessibility-information.md)를 참조하세요.

## <a name="keyboard-navigation"></a>키보드 탐색  
시각 장애가 있거나 움직임이 자유롭지 않은 사용자의 경우 키보드를 이용하여 UI를 탐색하는 것이 매우 중요합니다. 그러나 작동에 사용자 조작이 필요한 UI 컨트롤만 키보드 포커스로 지정되어야 합니다. 정적 이미지와 같이 작업이 필요하지 않은 구성 요소는 키보드 포커스가 필요하지 않습니다.  

마우스 또는 터치를 사용하여 탐색하는 것과는 다르게 키보드 탐색은 선형이라는 점에 주의해야 합니다. 키보드 탐색을 고려할 때는 사용자의 제품 조작 방법 및 적용되는 논리적 탐색에 대해 생각합니다. 서양 문화권 사용자는 왼쪽에서 오른쪽, 위에서 아래로 읽습니다. 따라서 키보드 탐색에 이 패턴을 따르도록 하는 것이 일반적입니다.  

키보드 탐색을 디자인할 때 UI를 검사하고 다음 질문에 대해 생각해봅니다.
* 어떻게 컨트롤을 UI 내에 배치 또는 그룹화할 것인가요?
* 몇 가지 주요 컨트롤 그룹이 있나요?
    * 그렇다면, 이러한 그룹에 다른 수준의 그룹이 포함되나요?
*   피어 컨트롤 간을 탐색하는 데 탭 이동, 특수 탐색(예: 화살표 키), 아니면 둘 다 사용해야 하나요?

목표는 사용자가 UI 배치 방법을 이해하고 작동 가능한 컨트롤을 식별할 수 있도록 하는 것입니다. 사용자가 탐색 루프를 완료하기 전에 탭 정지가 너무 많다고 생각되면 관련된 컨트롤을 함께 그룹화하는 것이 좋습니다. 하이브리드 컨트롤과 같은 관련된 일부 컨트롤은 초기 탐색 단계에서 다뤄져야 할 수 있습니다. 제품 개발을 시작한 후에는 키보드 탐색 재작업이 어려우므로 신중하게 일찍 계획해야 합니다.  

UI 요소 간 키보드 탐색에 대한 자세한 내용은 [키보드 접근성](keyboard-accessibility.md)을 참조하세요.  

또한 [Engineering Software for Accessibility(접근성을 위해 소프트웨어 엔지니어링)](https://www.microsoft.com/download/details.aspx?id=19262) 전자책의 _Designing the Logical Hierarchy(논리 계층 디자인)_ 장에 이 주제에 대해 자세히 설명되어 있습니다.

## <a name="color-and-contrast"></a>색상 및 대비  
Windows에서 기본 제공되는 접근성 기능 중 하나는 컴퓨터 화면에서 텍스트와 이미지의 색상 대비를 강조하는 고대비 모드입니다. 일부 사용자의 경우 색상 대비를 높이면 눈의 피로를 줄이고 더 읽기 쉽게 만들어 줍니다. 고대비로 UI를 확인할 때 컨트롤이 일관되고 시스템 색(하드 코드된 색이 아님)으로 코딩되었는지 확인하여 고대비를 사용하지 않는 사용자가 보는 화면에서 모든 컨트롤을 볼 수 있도록 합니다.  

XAML
```xaml
<Button Background="{ThemeResource ButtonBackgroundThemeBrush}">OK</Button>
```
시스템 색 및 리소스 사용에 대한 자세한 내용은 [XAML 테마 리소스](../controls-and-patterns/xaml-theme-resources.md)를 참조하세요.

시스템 색을 재정의하지 않는 한 UWP 앱은 기본적으로 고대비 테마를 지원합니다. 사용자가 시스템에서 시스템 설정 또는 접근성 도구의 고대비 테마를 사용하도록 선택한 경우 이 프레임워크에서는 UI의 컨트롤 및 구성 요소에 대해 고대비 레이아웃 및 렌더링을 생성하는 색상 및 스타일 설정을 자동으로 사용합니다.   

자세한 내용은 [고대비 테마](high-contrast-themes.md)를 참조하세요.  

시스템 색 대신 사용자 고유의 색 테마를 사용하고자 하는 경우 다음 지침을 고려합니다.  

**색상 대비 비율** – 다른 입법뿐만 아니라 미국 장애인 복지법의 업데이트된 섹션 508에서 요구하는 텍스트와 배경 간 기본 색상 대비는 5:1입니다. 큰 텍스트(18포인트 글꼴 크기 또는 14포인트 및 굵은 글씨)의 경우 필요한 기본 대비는 3:1입니다.  

**색 조합** – 약 7%의 남성(및 1% 미만의 여성)에게 일종의 색각 장애가 있습니다. 색맹 사용자는 특정 색을 구분하는 데 어려움이 있으므로 응용 프로그램에서의 상태 또는 의미를 전달할 때 색상만을 사용하지 않는 것이 중요합니다.
 장식 이미지(예: 아이콘 또는 배경)에 대해서는 색맹 사용자가 이미지를 최대한 인식할 수 있도록 색 조합을 선택해야 합니다.  

## <a name="accessibility-checklist"></a>접근성 검사 목록  
다음은 접근성 검사 목록의 간략화된 버전입니다.

1. 앱에 있는 콘텐츠 및 대화형 UI 요소의 접근성 있는 이름(필수) 및 설명(선택)을 설정합니다.
2. 키보드 접근성을 구현합니다.
3. UI를 시각적으로 검증하여 텍스트 대비가 적절한지, 요소가 고대비 테마에서 올바르게 렌더링되는지, 색이 제대로 사용되는지 확인합니다.
4. 접근성 도구를 실행하고, 보고된 문제를 해결하고, 화면 읽기 환경을 확인합니다. (접근성 테스트 항목을 참조하세요.)
5. 앱 매니페스트 설정이 접근성 지침을 따르는지 확인합니다.
6. Microsoft Store에서 접근할 수 있는 앱으로 선언하세요. ([스토어의 접근성](accessibility-in-the-store.md) 항목을 참조하세요.)

자세한 내용은 [접근성 검사 목록](accessibility-checklist.md) 항목 전체를 참조하세요.

## <a name="related-topics"></a>관련 항목  
* [포괄 소프트웨어 디자인](designing-inclusive-software.md)  
* [포괄 디자인](http://design.microsoft.com/inclusive)
* [피해야 할 접근성 사례](practices-to-avoid.md)
* [접근성을 위해 소프트웨어 엔지니어링](https://www.microsoft.com/download/details.aspx?id=19262)
* [Microsoft 접근성 개발자 허브](https://msdn.microsoft.com/enable)
* [접근성](accessibility.md)
