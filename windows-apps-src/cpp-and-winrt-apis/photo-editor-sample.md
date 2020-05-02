---
description: Photo Editor는 C++/WinRT 언어 프로젝션을 통한 개발을 보여 주는 UWP 샘플 애플리케이션입니다. 샘플 애플리케이션을 사용하면 사진 라이브러리에서 사진을 검색한 다음, 다양한 사진 효과를 사용하여 선택한 이미지를 편집할 수 있습니다.
title: Photo Editor C++/WinRT 샘플 애플리케이션
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 샘플, 애플리케이션, 사진, 편집기
ms.localizationpriority: medium
ms.openlocfilehash: dcefe2ad8321ae85fcb814bbaead0bb0e5373300
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81266911"
---
# <a name="photo-editor-cwinrt-sample-application"></a>Photo Editor C++/WinRT 샘플 애플리케이션

> [!NOTE]
> 샘플은 Windows 10 버전 1903(10.0, 빌드 18362) 및 Visual Studio 2019를 대상으로 테스트되었습니다. 원하는 경우 프로젝트 속성을 사용하여 프로젝트의 대상을 Windows 10 버전 1809(10.0, 빌드 17763)로 다시 지정하거나 Visual Studio 2017에서 샘플을 열 수 있습니다.

샘플 애플리케이션을 복제 또는 다운로드하려면 코드 샘플 갤러리의 [사진 편집기 C++/WinRT 샘플 애플리케이션](/samples/microsoft/windows-appsample-photo-editor/photo-editor-cwinrt-sample-application/)을 참조하세요.

사진 편집기 애플리케이션은 [C++/WinRT](intro-to-using-cpp-with-winrt.md) 언어 프로젝션을 사용한 개발을 보여 주는 UWP(유니버설 Windows 플랫폼) 샘플 애플리케이션입니다. 샘플 애플리케이션을 사용하면 **사진** 라이브러리에서 사진을 검색한 다음, 다양한 사진 효과를 사용하여 선택한 이미지를 편집할 수 있습니다. 샘플의 원본 코드에 C++/WinRT 프로젝션을 사용하여 수행되는 [데이터 바인딩](binding-property.md) 및 [비동기 작업](concurrency.md)과 같은 일반적인 사례의 숫자가 표시됩니다. 다음은 샘플이 보여 주는 특정 기능 중 일부입니다.

- WinRT(Windows 런타임) API로 표준 C++17 구문 및 라이브러리 사용
- co_await, co_return, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 및 [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation-1)의 사용을 포함하는 코루틴의 사용
- 사용자 지정 Windows 런타임 클래스(런타임 클래스) 프로젝션된 형식과 구현 형식 만들기 및 사용. 이 사용 약관에 대한 자세한 내용은 [C++/WinRT를 통한 API 사용](consume-apis.md) 및 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조
- 이벤트 토큰 자동 취소 사용을 포함한 [이벤트 처리](handle-events.md)
- 이미지 효과를 위한 외부 Win2D NuGet 패키지 및 [Windows::UI::Composition](/uwp/api/windows.ui.composition) 사용
- [{x:Bind} 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)을 포함하는 XAML 데이터 바인딩
- [연결된 애니메이션](../design/motion/connected-animation.md)을 포함한 XAML 스타일 지정 및 UI 사용자 지정
