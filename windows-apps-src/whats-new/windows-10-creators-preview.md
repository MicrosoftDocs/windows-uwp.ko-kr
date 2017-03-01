---
author: QuinnRadich
title: "Windows 10 크리에이터 업데이트 미리 보기의 새로운 기능 - UWP 앱 개발"
description: "유니버설 Windows 플랫폼에서 지원하는 도구, 기능 및 환경을 계속 제공하는 Windows 10 크리에이터 업데이트를 미리 살펴봅니다."
ms.assetid: 27a9ce65-c811-4f79-bf65-3493337199c8
keywords: "새 소식, 새 기능, 미리 보기, 업데이트, 새로운 내용, Windows 10, 크리에이터"
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: 29a888286062c59ea257cf0e43d3598d847a6588
ms.openlocfilehash: d36b84299b8e469624cef54f89c94744f3f4ae83
ms.lasthandoff: 02/08/2017

---

# <a name="whats-new-in-the-windows-10-creators-update-preview"></a>Windows 10 크리에이터 업데이트 미리 보기의 새로운 기능

Windows 10 크리에이터 업데이트 미리 보기는 유니버설 Windows 플랫폼에서 지원하는 도구, 기능 및 환경을 계속 제공합니다. Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](https://msdn.microsoft.com/library/windows/apps/bg124288)하거나 [Windows의 기존 앱 코드](https://msdn.microsoft.com/library/windows/apps/mt238321)를 사용하는 방법을 알아볼 수 있습니다.

이러한 기능은 Windows 10 크리에이터 업데이트가 출시되기 전에는 공개적으로 제공되지 않습니다. 현재는 [Windows 참가자](https://insider.windows.com/)에 액세스할 수 있는 미리 보기 빌드에서 사용할 수 있습니다. 향후 더 많은 설명서가 제공되면 이 페이지에 추가 기능에 대한 정보가 업데이트될 것입니다.

이 업데이트 및 기타 Windows 업데이트에서 강조 표시된 기능에 대한 자세한 내용은 [Windows 개발자의 날 사이트](https://developer.microsoft.com/en-us/windows/projects/campaigns/windows-developer-day) 및 [Windows 10의 멋진 기능](http://go.microsoft.com/fwlink/?LinkId=823181)을 참조하세요. 그리고 이전에 있었던 그리고 앞으로 있을 기능 추가에 대한 개략적인 내용은 [Windows 개발자 플랫폼 기능](https://developer.microsoft.com/en-us/windows/platform/features)을 참조하세요.

특징 | 설명
 :---- | :----
**UWP 문서 현대화** | 유니버설 Windows 플랫폼 설명서는 현재 docs.microsoft.com에서 사용할 수 있습니다. 이제 간편하게 코드 샘플을 제출하고, 기술 전문 지식을 추가하고, 피드백을 제공할 수 있습니다. 자세한 내용은 이 [개발자 이야기 동영상](https://channel9.msdn.com/Blogs/One-Dev-Minute/Modernizing-the-Windows-UWP-Docs)을 참조하세요.
**고객 주문 데이터베이스 샘플 업데이트** | UWP 제품군의 UI에 포함되는 Telerik의 데이터 그리드 컨트롤 및 데이터 항목 유효성 검사를 사용하도록 GitHub의 [고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 샘플이 업데이트되었습니다. UWP 제품군의 UI는 .NET foundation을 통해 오픈 소스 프로젝트로 제공되는 20개 이상의 컨트롤 모음입니다.
**잉크 분석** | 이제 Windows Ink는 잉크 스트로크를 쓰기 또는 그리기 스트로크로 분류하고 텍스트, 모양 및 기본 레이아웃 구조를 인식할 수 있습니다.
**Android용 Project Rome SDK** | UWP용 Project Rome 기능이 Android 플랫폼에 추가되었습니다. 이제 Windows *또는* Android 장치를 사용하여 원격으로 앱을 실행하고 모든 Windows 장치에서 작업을 계속할 수 있습니다. 시작 방법은 공식 [플랫폼간 시나리오에 대한 Project Rome 리포지토리](https://github.com/Microsoft/project-rome)를 참조하세요.
**XAML 컨트롤** | 이제 ContentDialog에는 기본, 보조 및 닫기라는 세 개의 단추가 있습니다. 세 단추 중 하나를 기본 작업으로 설정할 수도 있습니다. <br> ShowAsMonochrome 속성을 사용하여 비트맵 아이콘을 단색 또는 컬러로 표시할 수 있습니다. <br> 새 SelectionChangedTrigger를 사용하여 ComboBox가 키보드 선택을 처리하는 방법을 변경할 수 있습니다. <br> ListViewBase의 새로운 PrepareConnectedAnimation 및 TryStartConnectedAnimationAsync API가 추가되어 목록 및 그리드 보기와 연결된 애니메이션을 간편하게 사용할 수 있습니다. <br> 새 Icon 속성을 사용하여 MenuFlyoutItem 또는 MenuFlyoutSubItem에 아이콘을 추가할 수 있습니다. <br> SvgImageSource 클래스를 사용하여 XAML에서 SVG 이미지를 추가할 수 있습니다. <br> LoadedImageSource 클래스를 사용하여 XAML에서 구성 표면을 추가할 수 있습니다. <br> XAMLLight 클래스와 UIElement.Lights 속성을 사용하여 XAML에서 CompositionLight 효과를 추가할 수 있습니다. <br> XamlCompositionBrushBase를 사용하여 XAML에서 구성 브러시를 사용할 수 있습니다.

