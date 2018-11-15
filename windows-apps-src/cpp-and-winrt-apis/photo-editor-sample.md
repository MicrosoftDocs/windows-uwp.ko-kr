---
author: JoshuaPartlow
description: 사진 편집기는 C++/WinRT 언어 프로젝션을 사용한 개발을 보여 주는 UWP 샘플 응용 프로그램입니다. 샘플 응용 프로그램을 사용하여 사진 라이브러리에서 사진을 검색한 다음 다양한 사진 효과를 사용하여 선택한 이미지를 편집합니다.
title: 사진 편집기 C++/WinRT 샘플 응용 프로그램
ms.author: wdg-dev-content
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 샘플, 응용 프로그램, 사진, 편집기
ms.localizationpriority: medium
ms.openlocfilehash: 60bfcd79ed2d659aff8d435bd397df05eb45af72
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6861316"
---
# <a name="photo-editor-cwinrt-sample-application"></a>사진 편집기 C++/WinRT 샘플 응용 프로그램
[사진 편집기 C++/WinRT 샘플 응용 프로그램](https://github.com/Microsoft/Windows-appsample-photo-editor) GitHub 리포지토리에서 샘플 응용 프로그램을 복제 또는 다운로드할 수 있습니다.

사진 편집기 응용 프로그램은 [C++/WinRT](intro-to-using-cpp-with-winrt.md) 언어 프로젝션을 사용한 개발을 보여 주는 UWP(유니버설 Windows 플랫폼) 샘플 응용 프로그램입니다. 샘플 응용 프로그램을 사용하여 **사진** 라이브러리에서 사진을 검색한 다음 다양한 사진 효과를 사용하여 선택한 이미지를 편집합니다. 샘플의 소스 코드에 C++/WinRT 프로젝션을 사용하여 수행되는 [데이터 바인딩](binding-property.md) 및 [비동기 작업 및 작업](concurrency.md)과 같은 일반적인 관행의 숫자가 표시됩니다. 다음은 샘플이 보여 주는 특정 기능 중 일부입니다.
    
- Windows 런타임(WinRT) API로 표준 C++17 구문 및 라이브러리 사용.
- co_await, co_return, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 및 [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_)의 사용을 포함하는 코루틴의 사용.
- 사용자 지정 Windows 런타임 클래스(런타임 클래스) 프로젝션된 형식과 구현 형식 만들기 및 사용. 이 사용 약관에 대한 자세한 내용은 [C++/WinRT를 통한 API 사용](consume-apis.md) 및 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.
- 이벤트 토큰 자동 해지 사용을 포함한 [이벤트 처리](handle-events.md).
- 이미지 효과를 위한 외부 Win2D NuGet 패키지 및 [Windows::UI::Composition](/uwp/api/windows.ui.composition) 사용.
- [{x:Bind} 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)을 포함하는 XAML 데이터 바인딩.
- [애니메이션 연결](../design/motion/connected-animation.md)을 포함한 XAML 스타일 및 UI 사용자 지정.
