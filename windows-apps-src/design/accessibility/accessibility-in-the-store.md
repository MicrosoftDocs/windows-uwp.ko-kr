---
Description: Microsoft Store에서 액세스할 수 있는 Windows 앱을 선언 하기 위한 요구 사항을 설명 합니다.
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
title: 스토어의 접근성
label: Accessibility in the Store
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad128aca1a633c5ce33830b5ee9231f7a794a60c
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82969678"
---
# <a name="accessibility-in-the-store"></a>스토어의 접근성  



Microsoft Store에서 액세스할 수 있는 Windows 앱을 선언 하기 위한 요구 사항을 설명 합니다.

인증을 위해 Microsoft Store에 앱을 제출 하는 동안 앱을 액세스할 수 있는 것으로 선언할 수 있습니다. 앱을 내게 필요한 옵션으로 선언 하면 시각 장애가 있는 사용자와 같이 액세스 가능한 앱에 관심이 있는 사용자를 쉽게 검색할 수 있습니다. 사용자는 Microsoft Store을 검색 하는 동안 **액세스 가능한** 필터를 사용 하 여 액세스할 수 있는 앱을 검색 합니다. 앱을 액세스할 수 있는 것으로 선언 하면 **액세스 가능한** 태그가 앱의 설명에 추가 됩니다.

접근성 있는 앱으로 등록할 경우 앱은 다음 중 하나 이상을 사용하는 주요 시나리오에 필요한 [기본 접근성 정보](basic-accessibility-information.md)를 포함하는 것입니다.

* 키보드입니다.
* 고대비 테마입니다.
* 가변 dpi (인치당 도트 수) 설정입니다.
* 내레이터, 돋보기 및 화상 키보드를 비롯 한 Windows 내게 필요한 옵션 기능 등의 일반적인 보조 기술입니다.

내게 필요한 옵션을 작성 하 고 테스트 한 경우 앱을 액세스할 수 있는 것으로 선언 해야 합니다. 이는 다음을 수행 했음을 의미 합니다.

* 이름, 역할, 값 등을 포함 하 여 UI 요소에 대 한 모든 관련 액세스 가능성 정보를 설정 합니다.
* 전체 키보드 접근성을 구현 하 여 사용자가 다음을 수행할 수 있도록 합니다.
    * 키보드만 사용 하 여 기본 앱 시나리오를 수행 합니다.
    * 논리적 순서로 UI 요소 중에서 탭 합니다.
    * 화살표 키를 사용 하 여 컨트롤 내의 UI 요소 사이를 이동 합니다.
    * 바로 가기 키를 사용 하 여 기본 앱 기능에 연결 합니다.
    * 키보드를 사용 하지 않는 장치에 대해 탭 및 화살표에 대해 내레이터 터치 제스처를 사용 합니다.
* 앱 UI를 시각적으로 액세스할 수 있는지 확인: 4.5의 최소 텍스트 명암비는 1이 고,는 정보를 전달 하기 위해 색만 사용 하지 않습니다.
* [**검사**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) 및 [**UIAVerify**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-automation-verify) 와 같은 내게 필요한 옵션 테스트 도구를 사용 하 여 접근성 구현을 확인 하 고 이러한 도구에서 보고 하는 모든 우선 순위 1 오류를 해결 했습니다.
* 내레이터, 돋보기, 화상 키보드, 고대비 테마 및 조정 된 dpi 설정을 사용 하 여 앱의 기본 시나리오를 최종에서 끝으로 확인 했습니다.

이러한 절차를 검토 하 고 이러한 절차를 수행 하는 데 도움이 되는 리소스에 대 한 링크를 보려면 [내게 필요한 옵션 검사 목록을](accessibility-checklist.md) 참조 하십시오.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목    
* [액세스 가능성](accessibility.md) 
