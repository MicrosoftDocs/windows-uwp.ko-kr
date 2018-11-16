---
author: QuinnRadich
title: 2017년 9월 Windows 문서의 새로운 내용 - UWP 앱 개발
description: 2017년 9월 Windows 10 개발자 설명서에 추가된 새로운 기능, 동영상 및 개발자 지침
keywords: 새로운 기능, 업데이트, 기능, 개발자 지침, Windows 10, 1709
ms.author: quradic
ms.date: 09/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 436b610be942b33849b6da31cf5f9353a505a700
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6973912"
---
# <a name="whats-new-in-the-windows-developer-docs-in-september-2017"></a>2017년 9월 Windows 개발자 문서의 새로운 내용

Windows 개발자 설명서는 Windows 플랫폼 전체에서 개발자가 사용할 수 있는 새로운 기능에 대한 정보로 계속 업데이트되고 있습니다. 다음과 같은 기능 개요, 개발자 지침 및 샘플이 Windows 개발자를 위한 새 정보와 업데이트된 정보를 포함하여 최근에 추가되었습니다.

물론 가을 크리에이터스 업데이트가 곧 출시됩니다. 다음 달에 더 많은 문서가 공개될 예정이니 기다려주세요!

Windows 10에 [도구 및 SDK를 설치](http://go.microsoft.com/fwlink/?LinkId=821431)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/your-first-app.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 알아볼 수 있습니다.

## <a name="features"></a>기능

### <a name="xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램

이제 Xbox Live 크리에이터스 프로그램이 공개되었으니, Windows 10 PC와 Xbox One 콘솔에서 모두 실행할 수 있는 UWP 게임을 쉽게 빌드하고 게시할 수 있습니다. 자세한 내용은 [Xbox Live 크리에이터스 프로그램 시작](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)을 참조하세요.

## <a name="developer-guidance"></a>개발자 지침

### <a name="xaml-basics-tutorials"></a>XAML 기본 사항 자습서

새로운 [PhotoLab 샘플](https://github.com/Microsoft/Windows-appsample-photo-lab)과 함께 제공할 네 권의 [XAML 기본 사항 자습서](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-intro)를 작성했습니다. 각각 사용자 인터페이스, 데이터 바인딩, 사용자 지정 스타일, 적응형 레이아웃 등 XAML 프로그래밍의 네 가지 주요 구성 요소를 다룹니다. 각 자습서는 부분적으로 완성된 PhotoLab 샘플 버전으로 시작하고, 최종 앱의 누락된 구성 요소를 단계별로 빌드합니다. 

![사진 갤러리 페이지를 보여주는 PhotoLab 샘플 스크린샷](images/PhotoLab-gallery-page.png)  

새 문서의 개요를 살펴보면 다음과 같습니다.

+ [**사용자 인터페이스 만들기**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-ui)는 기본 사진 갤러리 인터페이스를 만드는 방법을 보여줍니다.
+ [**데이터 바인딩 만들기**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-data-binding)는 사진 갤러리에 데이터 바인딩을 추가하여 실제 이미지 데이터로 채우는 방법을 보여줍니다.
+ [**사용자 지정 스타일 만들기**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-style)는 사진 편집 메뉴에 세련된 사용자 지정 스타일을 추가하는 방법을 보여줍니다.
+ [**적응형 레이아웃 만들기**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-adaptive-layout)는 갤러리 레이아웃을 적응형으로 만들어 모든 장치와 화면 크기에 최적으로 표시되도록 하는 방법을 보여줍니다.

### <a name="get-started-tutorials"></a>시작 자습서

UWP 문서의 시작 섹션이 [자습서 섹션의 새 시작 페이지](https://docs.microsoft.com/windows/uwp/get-started/create-uwp-apps)로 업데이트되었습니다. 이 섹션에서는 시작 경험에 새롭게 개선된 구조를 제공하여, 위에서 언급한 XAML 기본 사항 자습서를 비롯해 사용자가 자신에게 적합한 자습서를 쉽게 찾고 사용할 수 있도록 합니다.

### <a name="voice-and-tone"></a>음성 및 톤

앱에서 텍스트를 작성하는 방법에 대한 조언을 제공하기 위해 [UWP 앱의 음성과 톤에 대한 지침](https://docs.microsoft.com/windows/uwp/in-app-help/voice-and-tone)을 새로 추가했습니다. 어떤 앱을 제작하든, 이해하기 쉽고 친근하며 필요한 정보를 주는 언어를 사용해야 합니다.

## <a name="samples"></a>샘플

### <a name="photolab-sample"></a>PhotoLab 샘플

[PhotoLab 샘플](https://github.com/Microsoft/windows-appsample-photo-lab)은 기본 사진 갤러리와 사진 편집 경험을 제공합니다.

![사진 편집 페이지를 보여주는 PhotoLab 샘플 스크린샷](images/PhotoLab-editing-page.png)  

### <a name="customer-orders"></a>고객 주문

새로운 .NET Core 2.0 및 Entity Framework를 사용할 수 있도록 [고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 샘플이 업데이트되었습니다.