---
title: Windows 10의 음성, 음성 및 대화
description: 이 페이지에서는 음성 사용 Windows 앱 개발을 시작할 수 있는 정보를 제공 합니다.
ms.topic: article
ms.date: 09/12/2019
keywords: Windows 10의 음성, 음성, 음성, 대화, win32 음성 앱, UWP speech 앱, WPF speech apps, WinForms speech apps
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 0051ecb0556b72f768c142f04c38f3f34b2dd334
ms.sourcegitcommit: 99a3f996a57ecc5df7382fb46bc9b8ba75614a53
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99579014"
---
# <a name="speech-voice-and-conversation-in-windows-10"></a>Windows 10의 음성, 음성 및 대화

![음성 주인공 이미지](images/hero-speech-composite-small.png)

음성은 사용자가 마우스, 키보드, 터치, 컨트롤러 또는 제스처를 기반으로 하 여 Windows 응용 프로그램, 보완 또는 대체 (기존 상호 작용 환경)를 조작 하는 데 효과적이 고 자연스럽 고 즐겁게 가능 합니다.

음성 인식, 받아쓰기, 음성 합성 (텍스트-음성 또는 TTS 라고도 함) 및 대화형 음성 도우미 (예: Cortana 또는 Alexa)와 같은 음성 기반 기능을 사용 하면 다른 입력 장치가 충분 하지 않을 때 응용 프로그램을 사용할 수 있는 액세스 가능 하 고 포함 된 사용자 환경을 제공할 수 있습니다.

이 페이지에서는 다양 한 Windows 개발 프레임 워크에서 Windows 응용 프로그램을 빌드하는 개발자에 게 음성 인식, 음성 합성 및 대화 지원을 제공 하는 방법에 대 한 정보를 제공 합니다.

## <a name="platform-specific-documentation"></a>플랫폼별 설명서

:::row:::
   :::column:::
      ![UWP(유니버설 Windows 플랫폼)](images/platform-uwp.png)

      **UWP(유니버설 Windows 플랫폼)**

      Windows 10 응용 프로그램 및 게임을 위한 최신 플랫폼에서 음성 지원 앱을 빌드 하 고 Windows 장치 (Pc, 휴대폰, Xbox One, HoloLens 등 포함)에서 Microsoft Store에 게시 합니다.

      [음성 조작](/windows/uwp/design/input/speech-interactions)

      [음성 인식](/windows/uwp/design/input/speech-recognition)

      [연속 받아쓰기](/windows/uwp/design/input/enable-continuous-dictation)

      [음성 합성](/uwp/api/windows.media.speechsynthesis)

      [대화형 에이전트](/uwp/api/windows.applicationmodel.conversationalagent)

      [Cortana 음성 명령](/cortana/voice-commands/vcd)<br>
      (Windows 10에서는 지원 되지 않음 업데이트 이상 2020)
   :::column-end:::
   :::column:::
      ![Win32 플랫폼 앱](images/platform-win32.png)

      **Win32 플랫폼**

      여기에서 제공 하는 도구, 정보 및 샘플 엔진과 응용 프로그램을 사용 하 여 Windows desktop 및 Windows Server 용 음성 지원 응용 프로그램을 개발 합니다.

      [Microsoft Speech Platform - SDK(소프트웨어 개발 키트) (버전 11)](https://www.microsoft.com/download/details.aspx?id=27226)
      
      [Microsoft Speech SDK, 버전 5.1](https://www.microsoft.com/download/details.aspx?id=10121)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![.NET](images/platform-dotnet.png)

      **.NET Framework**

      XAML UI 모델 및 .NET Framework를 사용하여 관리형 Windows 애플리케이션용으로 설정된 플랫폼에서 접근성 지원 앱과 도구를 개발합니다.

      [.NET Framework용 System.Speech 프로그래밍 가이드](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))
   :::column-end:::
   :::column:::
      ![Azure 음성 서비스](images/platform-azure-speech.png)

      **Azure 음성 서비스**

      음성 처리를 앱 및 서비스에 통합 합니다.

      [음성 텍스트 변환](https://azure.microsoft.com/services/cognitive-services/speech-to-text/)

      [텍스트 음성 변환](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
      
      [음성 번역](https://azure.microsoft.com/services/cognitive-services/speech-translation/)

      [화자 인식](https://azure.microsoft.com/en-us/services/cognitive-services/speaker-recognition/)

      [음성 우선 가상 도우미](/azure/cognitive-services/speech-service/voice-first-virtual-assistants)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **레거시 기능**

      Microsoft 음성 및 대화 기술의 레거시, 사용 되지 않는 버전 및/또는 지원 되지 않는 버전입니다.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Cortana 스킬 키트](/cortana/skills/)

      Cortana를 [Microsoft 365](/microsoft-365/admin/misc/cortana-integration)에 긴밀 하 게 포함 하 여 최신 생산성 환경을 전환 하는 목표의 일환으로 Cortana Skills Kit 개발자 플랫폼과이 플랫폼에서 빌드된 모든 기술을 사용 중지 합니다.
   :::column-end:::
   :::column:::

      [Microsoft 에이전트](/windows/win32/lwef/microsoft-agent)

      [Microsoft Speech SASDK (Software Development Kit) 버전 1.0](https://www.microsoft.com/download/details.aspx?id=2200)

      [SAPI (Microsoft Speech API) 5.3](/previous-versions/windows/desktop/ms723627(v=vs.85))

      [SAPI (Microsoft Speech API) 5.4](/previous-versions/windows/desktop/ee125663(v=vs.85))

      [Bing Speech 인식 컨트롤](/previous-versions/bing/speech/dn434583(v=msdn.10))
   :::column-end:::
:::row-end:::

## <a name="samples"></a>샘플

다양한 접근성의 특징과 기능을 보여 주는 Windows 샘플 전체를 다운로드하고 실행합니다.

:::row:::
   :::column:::
      [코드 샘플 브라우저](/samples/browse/?term=speech)

      새 샘플 브라우저 (MSDN 코드 갤러리 대체)
   :::column-end:::
   :::column:::
      [GitHub의 Windows 클래식 샘플](https://github.com/microsoft/Windows-classic-samples/search?q=speech&unscoped_q=speech)

      이러한 샘플에서는 Windows 및 Windows Server의 기능 및 프로그래밍 모델을 보여 줍니다. 
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [GitHub의 UWP(유니버설 Windows 플랫폼) 코드 샘플](https://github.com/microsoft/Windows-universal-samples/search?q=speech&unscoped_q=speech)

      이러한 샘플에서는 Windows 10용 Windows SDK(소프트웨어 개발 키트)의 UWP(유니버설 Windows 플랫폼)에 대한 API 사용 패턴을 보여 줍니다.
   :::column-end:::
   :::column:::
      [XAML 컨트롤 갤러리](https://github.com/microsoft/Xaml-Controls-Gallery)

      이 앱에서는 흐름 디자인 시스템에서 지원되는 다양한 Xaml 컨트롤을 보여 줍니다.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>동영상

음성 상호 작용을 통합 하는 Windows 응용 프로그램을 빌드하는 방법에 대 한 다양 한 비디오입니다.

:::row:::
   :::column:::
      **Cortana 및 음성 플랫폼 심층 분석**
   :::column-end:::
   :::column:::
      **유니버설 Windows 앱에서 Cortana 확장성**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/3-716/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/2-691/player]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>기타 리소스

:::row:::
   :::column:::
      **블로그 및 뉴스**

      Microsoft 음성 세계의 최신 버전입니다.
   :::column-end:::
   :::column:::
      **커뮤니티 및 지원**

      Windows 개발자와 사용자가 함께를 충족 하 고 배우는 경우
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [뉴스](https://news.microsoft.com/?s=speech)

      [음성 블로그](https://blogs.windows.com/windowsdeveloper/?s=speech)
   :::column-end:::
   :::column:::
      [Windows 커뮤니티-음성](https://community.windows.com/search?q=speech)

      [Windows 음성 개발자 포럼](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=firstpostdesc&searchTerm=speech)
   :::column-end:::
:::row-end:::