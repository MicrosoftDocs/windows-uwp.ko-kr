---
title: Windows UI 라이브러리
description: WinUI 2.x 및 Windows 앱 개발에 대한 정보를 제공합니다.
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, 도구 키트 sdk, winui, Windows UI 라이브러리
ms.custom: RS5
ms.openlocfilehash: 69e70431303d809ea7303d29a002c72a1c63480a
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/12/2021
ms.locfileid: "100334954"
---
# <a name="windows-ui-library-2x"></a>Windows UI 라이브러리 2.x

![WinUI 컨트롤](images/winUI-library-767.png)

Windows UI 라이브러리는 Windows 앱에 사용할 수 있는 공식 네이티브 Windows UI 컨트롤 및 기타 사용자 인터페이스 요소를 제공합니다.

이 라이브러리는 이전 버전의 Windows 10과 하위 수준 호환성을 유지하므로, 사용자에게 최신 OS가 없더라도 앱이 작동합니다.

> [!NOTE]
> Windows 10 UI 플랫폼에 대한 주요 업데이트인 [Windows UI 라이브러리 3 Preview 4(2021년 2월)](../winui3/index.md)를 확인하세요.

## <a name="features"></a>기능

* **새 컨트롤**: Windows UI 라이브러리에는 기본 Windows 플랫폼의 일부로 제공되지 않는 새 컨트롤이 포함되어 있습니다.

* **기존 컨트롤의 업데이트된 버전**: 이 라이브러리에는 이전 버전의 Windows 10에서 사용할 수 있는 기존 Windows 플랫폼 컨트롤의 업데이트된 버전이 포함되어 있습니다.

* **이전 버전의 Windows 10 지원**: Windows UI 라이브러리 API는 이전 버전의 Windows 10에서 작동하므로, 최신 OS를 사용하지 않는 사용자를 지원하기 위해 버전 확인 또는 조건부 XAML을 포함할 필요가 없습니다.

* **XamlDirect 지원**: 미들웨어 개발자용으로 설계된 Xaml Direct API는 보다 나은 CPU 및 작업 세트 성능을 제공하는 하위 수준 Xaml 기능에 대한 액세스를 제공합니다. XamlDirect를 사용하면 여러 대상 Windows 10 버전을 처리하는 특수 코드를 작성할 필요 없이 이전 버전의 Windows 10에서 XamlDirect API를 사용할 수 있습니다.

## <a name="examples"></a>예

Xaml 컨트롤 갤러리 샘플 앱에는 WinUI 컨트롤을 사용하기 위한 대화형 데모와 샘플 코드가 포함되어 있습니다.

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)에서 XAML 컨트롤 갤러리 앱 설치

* [GitHub에서 오픈 소스](
https://github.com/Microsoft/Xaml-Controls-Gallery)로도 제공되는 Xaml 컨트롤 갤러리

## <a name="documentation"></a>문서

Windows UI 라이브러리 컨트롤에 대한 방법 문서가 [유니버설 Windows 플랫폼 컨트롤 설명서](/windows/uwp/design/controls-and-patterns/)에 포함되어 있습니다.

API 참조 문서는 [Windows UI 라이브러리 API](/windows/winui/api/)에 있습니다.

## <a name="install-and-use-the-windows-ui-library"></a>Windows UI 라이브러리 설치 및 사용

지침은 [Windows UI 라이브러리 시작](getting-started.md)을 참조하세요.

## <a name="open-source-and-developer-roadmap"></a>오픈 소스 및 개발자 로드맵

WinUI는 GitHub에 호스팅되는 오픈 소스 프로젝트입니다. [Windows UI 라이브러리 리포지토리](https://aka.ms/winui)에서 버그 보고서를 제출하고, 기능을 요청하고, 커뮤니티 코드를 제공할 수 있습니다.

저희는 더 많은 개발자 시나리오를 지원하기 위해 계속해서 WinUI를 개발 및 개선하고 있습니다. WinUI 계획에 대한 최신 정보는 Windows UI 라이브러리 리포지토리의 [로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)을 참조하세요.

## <a name="nuget-package-list"></a>NuGet 패키지 목록

Windows UI 라이브러리에는 여러 NuGet 패키지가 포함되어 있습니다. [Windows UI 라이브러리 NuGet 패키지 목록](nuget-packages.md).

## <a name="see-also"></a>참고 항목

[Windows UI 라이브러리 2.x 릴리스 정보](release-notes/index.md)
