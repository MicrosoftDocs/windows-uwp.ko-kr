---
author: Xansky
Description: "UWP(유니버설 Windows 플랫폼) 앱을 Windows 스토어에 접근성 있는 앱으로 등록하기 위한 요구 사항에 대해 설명합니다."
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
title: "스토어의 접근성"
label: Accessibility in the Store
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 46dfe4fba383861c704b2ba9070bdd8102b10562

---

# 스토어의 접근성  



UWP(유니버설 Windows 플랫폼) 앱을 Windows 스토어에 접근성 있는 앱으로 등록하기 위한 요구 사항에 대해 설명합니다.

인증을 위해 Windows 스토어에 앱을 제출할 때 접근성 있는 앱으로 등록할 수 있습니다. 접근성 있는 앱으로 등록할 경우 시각 장애인 같이 접근성 있는 앱에 관심이 있는 사용자가 앱을 더 쉽게 찾을 수 있습니다. 사용자는 Windows 스토어에서 검색할 때 **접근성** 필터를 사용하여 접근성 있는 앱을 찾습니다. 접근성 있는 앱으로 등록할 경우 앱 설명에 **접근성 있는** 태그도 추가됩니다.

접근성 있는 앱으로 등록할 경우 앱은 다음 중 하나 이상을 사용하는 주요 시나리오에 필요한 [기본 접근성 정보](basic-accessibility-information.md)를 포함하는 것입니다.

* 키보드
* 고대비 테마
* 인치당 도트 수(dpi)가 높은 설정
* 내레이터, 돋보기, 화상 키보드 등을 포함하는 Windows 접근성 기능과 같은 일반 보조 기술

접근성을 구현하기 위해 앱을 빌드 및 테스트한 경우 접근성 있는 앱으로 등록해야 합니다. 이는 다음을 수행했음을 의미합니다.

* UI 요소에 대한 이름, 역할, 값 등과 같은 관련 접근성 정보를 모두 설정했습니다.
* 사용자가 다음을 할 수 있는 전체 키보드 접근성을 구현했습니다.
    * 키보드만 사용하여 기본 앱 시나리오 수행
    * 논리적인 순서로 UI 요소 간 탭 이동
    * 화살표 키를 사용하여 컨트롤 내 UI 요소 탐색
    * 바로 가기 키로 주요 앱 기능에 연결
    * 키보드가 없는 장치에서 탭 및 화살표와 동일하게 내레이터 터치 제스처 사용
* 앱 UI가 시각적으로 접근성이 있는지 확인했습니다. 즉, 텍스트 명암비가 최소 4.5:1 이상이고 정보를 전달하기 위해 색만 사용하지는 않는지 등을 확인했습니다.
* [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 및 [**UIAVerify**](https://msdn.microsoft.com/library/windows/desktop/Hh920986)과 같은 접근성 테스트 도구를 사용하여 접근성 구현을 검증하고 이러한 도구로 우선 순위 1인 오류를 모두 해결했습니다.
* 내레이터, 돋보기, 화상 키보드, 고대비 테마 및 조정된 dpi 설정을 사용하여 종단 간에 앱의 주요 시나리오를 검증했습니다.

이러한 절차와 해당 절차를 완료하는 데 도움이 되는 리소스에 대한 링크를 검토하려면 [접근성 검사 목록](accessibility-checklist.md)을 참조하세요.

<span id="related_topics"/>
## 관련 항목    
* [접근성](accessibility.md)



<!--HONumber=Jun16_HO4-->


