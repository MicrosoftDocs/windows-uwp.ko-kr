---
author: PatrickFarley
title: 교육용 앱 개발.
description: 이 섹션에서는 Windows10 플랫폼에 대한 교육용 앱을 작성하는 데 사용할 수 있는 유니버설 Windows 앱 리소스를 설명합니다.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 교육
ms.assetid: 2431f253-efe3-4895-b131-34653b61f13c
ms.localizationpriority: medium
ms.openlocfilehash: da03a3c478ca45cc2d2b518988738e510a6c5ea9
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2018
ms.locfileid: "4173980"
---
# <a name="develop-universal-windows-apps-for-education"></a>교육용 유니버설 Windows 앱 개발
![시험 응시 앱 스크린샷](images/take-a-test-screen-small.png)

다음 리소스는 유니버설 Windows 교육용 앱을 작성하는 데 도움이 됩니다.

### <a name="accessibility"></a>접근성
교육용 앱은 접근성이 높아야 합니다. 자세한 내용은 [접근성용 앱 개발](https://developer.microsoft.com/windows/accessible-apps)을 참조하세요.


### <a name="secure-assessments"></a>보안 평가
평가/테스트 앱의 경우 종종 학생들이 테스트 중 다른 컴퓨터나 인터넷 리소스를 사용할 수 없도록 *잠금* 환경을 만들어야 합니다. 이 기능은 [시험 응시 API](take-a-test-api.md)를 통해 사용할 수 있습니다. 고위험 테스트를 위해 온라인 액세스가 잠긴 상태인 테스트 환경에 대한 예는 Windows IT 센터에서 [시험 응시](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) 웹앱을 참조하세요.

### <a name="user-input"></a>사용자 입력
사용자 입력은 교육용 앱에서 중요한 부분입니다. UI 컨트롤은 사용자의 포커스에 문제가 생기지 않도록 반응성이 좋고 직관적이어야 합니다. 유니버설 Windows 앱에서 사용할 수 있는 입력 옵션에 대한 일반적인 개요는 [입력 지침서](https://docs.microsoft.com/windows/uwp/design/input/input-primer)와 디자인 및 UI 섹션에서 아래의 항목을 참조하세요. 또한 다음 샘플 앱에서는 유니버설 Windows 플랫폼에서 처리하는 기본 UI를 소개합니다.
- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)에서는 유니버설 Windows 앱에서 입력을 처리하는 방법을 보여 줍니다.
- [사용자 조작 모드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)에서는 사용자 조작 모드를 검색하고 응답하는 방법을 보여 줍니다.
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)에서는 새 시스템 작성 포커스 화면 효과를 활용하거나 시스템 작성 포커스 화면 효과가 요구에 맞지 않는 경우 사용자 지정 포커스 화면 효과를 만드는 방법을 보여 줍니다.

Windows Ink 플랫폼은 학생에게 익숙한 입력 모드에 맞게 조정하여 교육용 앱을 돋보이게 만들 수 있습니다. 앱에 Windows Ink를 구현하는 전체 가이드는 [펜 조작 및 Windows Ink](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions) 및 그 아래 항목을 참조하세요. 다음 샘플 앱은 이 API의 작업 예제를 제공합니다.
- [잉크 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Ink)에서는 JavaScript로 작성된 유니버설 Windows 앱에서 잉크 기능을 사용하는 방법(예: 잉크 스트로크 캡처, 조작 및 해석)을 보여 줍니다.
- [간단한 잉크 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)에서는 C#으로 작성된 유니버설 Windows 앱에서 잉크 기능을 사용하는 방법(예: 사용자 입력에서 잉크 캡처, 잉크 스트로크에 대해 필기 인식 수행)을 보여 줍니다.
- [복잡한 잉크 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)에서는 고급 InkPresenter 기능을 사용하여 잉크를 다른 개체와 인터리빙하고 잉크 선택, 복사/붙여넣기, 이벤트 처리를 수행하는 방법을 보여 줍니다. C++로 작성된 유니버설 Windows 플랫폼을 토대로 작성되었으며 데스크톱 및 모바일 Windows10 SKU 둘 다에서 실행할 수 있습니다.


### <a name="microsoft-store"></a>Microsoft Store
교육용 앱은 주로 특정 조직에게 특별한 상황에서 출시됩니다. 이에 대한 자세한 내용은 [엔터프라이즈에 LOB 앱 배포](https://msdn.microsoft.com/windows/uwp/publish/distribute-lob-apps-to-enterprises)를 참조하세요.

## <a name="related-topics"></a>관련 항목
- Windows IT 센터의 [Windows10 for Education](https://technet.microsoft.com/edu/windows/index)
