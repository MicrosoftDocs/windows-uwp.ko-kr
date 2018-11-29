---
title: 유니버설 Windows 플랫폼을 통해 앱 제작
description: Windows10 용 유니버설 Windows 플랫폼 (UWP) 앱을 만들 생각 보다 쉽습니다.
ms.date: 5/7/2018
ms.topic: article
keywords: windows 10, uwp, 시작
ms.localizationpriority: medium
ms.openlocfilehash: 2f4e38d590fc2e905221c71c1fbc6b137f5fdea0
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8194071"
---
# <a name="start-coding"></a>코딩 시작

![앱 빌드](images/build-your-app.png)

[UWP 플랫폼](universal-application-platform-guide.md)을 시작합니다! 이 페이지에서 안내 올바른 정보를 만들려는 Windows 10 앱 코딩을 시작 해야 합니다.

개발을 시작하기 전에 [설정해야 합니다](get-set-up.md).

## <a name="learning-tracks"></a>학습 트랙

다음 학습 트랙은 몇 가지 기본 작업을 완료하기 전에 알아야 할 사항과 해당 정보를 찾을 수 있는 위치를 소개합니다. 자습서는 아니지만 올바르게 작업을 수행하고 있는지 확인할 수 있는 참조 코드를 제공합니다.

| 작업 | 설명 |
| --- | --- |
| [양식 작성](construct-form-learning-track.md) | 사용하기 쉽고 화면 크기와 관계없이 멋지게 보이는 양식을 만드는 방법을 알아보세요. | 
| [목록에서 고객 표시](display-customers-in-list-learning-track.md) | UI에서 데이터를 표시하고 편집하는 방법을 알아보세요. | 
| [설정 저장 및 로드](settings-learning-track.md) | 설정을 저장하고 검색하는 위치와 방법에 대해 알아봅니다. |
| [파일 작업](fileio-learning-track.md) | 파일을 읽고 쓰는 방법과 액세스할 수 있는 또는 없는 폴더를 알아봅니다. | 

Windows 10에 익숙한 경험 있는 개발자를 위해 모든 학습 트랙이 기록됩니다. 막 시작하는 경우 [새 개발자를 위한](#For-new-developers) 콘텐츠로 시작합니다.

## <a name="for-new-developers"></a>새 개발자용

새 개발자를 위해 Microsoft는 Windows 10 개발에 필요한 코드 및 도구를 사용하는 방법의 기본 사항을 학습할 수 있는 많은 리소스를 제공합니다. 

* ["Hello World" 앱 만들기](your-first-app.md)

코딩, the C# 언어, Visual Studio, 또는 유니버설 Windows 플랫폼의 기능에 대해 더 자세한 안내를 원하는 경우 다음 리소스를 참고하세요.

**문서:**

* [C# 시작](https://docs.microsoft.com/dotnet/csharp/getting-started/)
* [C# 빠른 시작](https://docs.microsoft.com/dotnet/csharp/quick-starts/index)
* [Visual Studio 시작](https://docs.microsoft.com/visualstudio/ide/)

**비디오**

* [Microsoft Virtual Academy](https://mva.microsoft.com/training-topics/c-app-development#!level=Beginner&lang=1033)
* [LinkedIn Learning](https://www.linkedin.com/learning/learning-universal-windows-app-development/welcome)

## <a name="using-the-docs"></a>문서 사용

우리의 학습 트랙을 이미 탐색해 보았거나 학습 트랙에서 다루지 않은 사항에 관심이 있는 경우, 설명서를 통해 스스로 둘러보는 것이 좋습니다. 각 영역에서 찾을 수 사항에 대한 간단한 개요는 다음과 같습니다.

| 영역 | 설명 |
| --- | --- |
| **새로운 기능?** | Windows 10의 각 주요 업데이트와 함께 새로운 지침으로 확장된 문서를 볼 수 있습니다. 이 문서에는 각 릴리스에 대해 추가한 기능에 대한 정보 및 개발자 지침뿐만 아니라 새 API 목록이 들어 있습니다. </br>   [최신 Windows 10 릴리스의 개발자용 새로운 기능](../whats-new/windows-10-version-latest.md) </br> 주요 릴리스에서만 문서를 업데이트하는 것은 아닙니다. 새로운 정보를 탐색할 수 있는 새로운 정보가 항상 추가되며 최신 정보에 대해 알려 드립니다. </br>   [문서의 새 소식](../whats-new/windows-docs-latest.md) |
| **디자인 및 UI** | 문서의 이 영역에 시각적 표시 및 앱의 UI에 있는 모든 정보가 포함되어 있습니다. XAML 표시 언어의 사양에 관심 있거나 사용자 문서에 대한 고유한 모습의 사용자 문서를 만들려는 경우 여기에서 시작합니다. </br>   [UWP 앱의 디자인 기본 사항](../design/basics/index.md) |
| **앱 개발** | 특정 Windows 10 기능에 대한 자세한 정보를 싶거나 UWP 개발로 수행할 수 있는 작업에 관심 있는 경우 문서의 이 영역을 살펴보세요. </br>   [UWP 앱 기능](../develop/index.md) </br> Windows 10 앱에 대한 API 참조는 일련의 관련 문서에서 호스트되며, 여기서 확인할 수 있습니다. </br>   [Windows UWP 네임스페이스](https://docs.microsoft.com/en-us/uwp/api/) </br>   [파일 및 XML 스키마](https://docs.microsoft.com/uwp/schemas/) |
| **게임 개발** | 이 문서는 Windows 또는 Xbox에서 게임을 개발하는 방법에 대한 정보를 포함합니다. 설치 지침, 개발자 프로그램 및 DirectX 또는 Xbox 기능을 사용하여 프로그래밍하는 지침을 포함합니다. </br>   [게임 개발 시작](../gaming/getting-started.md) |
| **게시** | 이러한 문서에는 앱 제출에서 프로모션 가격 책정 및 고객 참여까지 Microsoft Store에 앱을 게시하는 방법에 대한 정보가 포함되어 있습니다. </br>   [Microsoft Store에 앱을 게시합니다.](../publish/index.md) |

## <a name="other-docs"></a>기타 문서

웹 개발 또는 Mixed Reality 같은 일부 특별한 Windows 10 플랫폼에는 자체 문서가 있습니다. 이러한 기능으로 앱을 개발하는 것에 관심이 있는 경우 해당 설명서를 살펴보세요.

| 문서 | 설명 |
| --- | --- |
| **Microsoft Azure** | 클라우드 개발 및 Microsoft Azure에 대한 정보는[Microsoft Azure 개발자 설명서](https://docs.microsoft.com/azure/)에서 찾을 수 있습니다. |
| **웹 개발** | Microsoft Edge, WebVR 및 기타 Windows 웹 개발 기능에 대한 정보는 [Microsoft Edge 개발자 설명서](https://docs.microsoft.com/microsoft-edge/)에서 찾을 수 있습니다. |
| **Windows Mixed Reality** | Mixed Reality는 물리적 및 디지털 개체가 공존하는 환경에 실제 및 가상 콘텐츠를 혼합합니다. Microsoft HoloLens 및 기타 몰입형 헤드셋용 앱 빌드에 대한 정보는 [Windows Mixed Reality 설명서](https://docs.microsoft.com/en-us/windows/mixed-reality/)에서 찾을 수 있습니다.|
