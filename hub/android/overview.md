---
title: Windows 기반 Android 개발 개요
description: 네이티브 Android 개발, 플랫폼 간 개발 및 Android 게임 개발을 포함 하 여 Windows에서 Android 개발을 시작 하세요.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: windows의 android, xamarin android, 네이티브, cordova, phonegap, c + + android 게임, windows defender, 에뮬레이터에 반응
ms.date: 04/28/2020
ms.openlocfilehash: b839972033c9edfad3524909345380e7fac9462e
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823167"
---
# <a name="overview-of-android-development-on-windows"></a>Windows 기반 Android 개발 개요

Windows 운영 체제를 사용 하 여 Android 장치 앱을 개발 하기 위한 여러 경로가 있습니다. 이러한 경로는 **[기본 Android 개발](#native-android)**, **[플랫폼 간 개발](#cross-platform)** 및 **[Android 게임 개발](#game-development)** 의 세 가지 주요 형식으로 분류 됩니다. 이 개요는 Android 앱을 개발 하는 데 따라야 하는 개발 경로를 결정 하는 데 도움이 되며,를 사용 하 여 개발 하기 위해 Windows를 사용 하는 데 도움이 되는 [다음 단계](#next-steps) 를 제공 합니다

- [네이티브 Android](native-android.md)
- [Xamarin.Android](xamarin-android.md)
- [Xamarin.Forms](xamarin-forms.md)
- [React Native](react-native.md)
- [Cordova,가 나 Ic 또는 PhoneGap](pwa.md)
- [게임 개발에 대 한 c/c + +](native-android.md#use-c-or-c-for-android-game-development)

또한이 가이드에서는 Windows를 사용 하는 방법에 대 한 팁을 제공 합니다.

- [Android 장치 또는 에뮬레이터에서 테스트](emulator.md)
- [성능 향상을 위한 Windows Defender 설정 업데이트](defender-settings.md)
- [Android 용 이중 화면 앱 개발 및 Surface 듀오 장치 SDK 다운로드](/dual-screen/android/)

## <a name="native-android"></a>네이티브 Android

[Windows에서 네이티브 android 개발](./native-android.md) 은 앱이 Android (IOS 또는 Windows 장치 아님)만 대상으로 하 고 있음을 의미 합니다. [Android Studio](https://developer.android.com/studio/install#windows) 또는 [Visual Studio](https://visualstudio.microsoft.com/vs/android/) 를 사용 하 여 Android 운영 체제용으로 특별히 설계 된 에코 시스템 내에서 개발할 수 있습니다. 성능은 Android 장치에 대해 최적화 되 고, 사용자 인터페이스 모양과 느낌은 장치의 다른 네이티브 앱과 일치 하 고, 사용자 장치의 기능이 나 기능을 사용 하 여 액세스 하 고 활용할 수 있습니다. 앱을 네이티브 형식으로 개발 하면 Android 장치에 대해 특별히 설정 된 모든 상호 작용 패턴 및 사용자 환경 표준을 따르도록 하는 데 도움이 됩니다.

## <a name="cross-platform"></a>플랫폼 간

플랫폼 간 프레임 워크는 Android, iOS 및 Windows 장치 간에 (주로) 공유할 수 있는 단일 코드 베이스를 제공 합니다. 플랫폼 간 프레임 워크를 사용 하면 앱에서 장치 플랫폼에 대해 동일한 모양, 느낌 및 환경을 유지 하는 데 도움이 될 뿐만 아니라 업데이트 및 수정 프로그램의 자동 롤아웃을 기능과 수 있습니다. 다양 한 장치별 코드 언어를 이해 하는 대신, 응용 프로그램은 일반적으로 하나의 언어로 된 공유 코드 베이스로 개발 됩니다.

플랫폼 간 프레임 워크는 가능한 한 네이티브 앱에 가까운 모양과 느낌을 목표로 하며, 기본적으로 개발 된 앱으로 원활 하 게 통합 되는 것이 아니므로 속도가 줄어들고 성능이 저하 될 수 있습니다. 또한 플랫폼 간 앱을 빌드하는 데 사용 되는 도구에는 각각의 서로 다른 장치 플랫폼에서 제공 하는 모든 기능이 포함 되지 않을 수 있으며, 잠재적으로 해결 방법이 필요 합니다.

코드 베이스는 일반적으로 웹 서비스를 호출 하 고, 데이터베이스에 액세스 하 고, 하드웨어 기능을 호출 하 고, 상태를 관리 하기 위해 페이지, 단추 컨트롤, 레이블, 목록 등의 사용자 **인터페이스를 만들기** 위해 **UI 코드로** 구성 됩니다. 평균적으로이의 90%는 다시 사용 될 수 있지만 일반적으로 각 장치 플랫폼에 대 한 코드를 사용자 지정 해야 합니다. 이러한 일반화는 대부분 빌드 중인 앱의 유형에 따라 다르지만 결정 하는 데 도움이 될 수 있는 약간의 컨텍스트를 제공 합니다.  

## <a name="choosing-a-cross-platform-framework"></a>플랫폼 간 프레임 워크 선택

[Xamarin 네이티브 (Xamarin Android)](xamarin-android.md)

- UI 코드: Android Designer 및 재질 테마를 사용 하는 XML
- 논리 코드: c # 또는 F #
- 일부 네이티브 Android 요소를 계속 사용할 수 있지만 다른 플랫폼 (iOS, Windows)에 대 한 코드 베이스의 재사용에 적합 합니다.
- 논리 코드만 UI 코드가 아닌 플랫폼에서 공유 됩니다.
- 장치 관련 사용자 인터페이스를 사용 하는 더 복잡 한 앱에 적합 합니다.

[Xamarin Forms (Xamarin.ios)](xamarin-forms.md)

- UI 코드: XAML 및 .NET (Visual Studio 포함)
- 논리 코드: C #
- Android, iOS 및 Windows 장치 앱에서 60 ~ 90%의 논리 및 UI 코드를 공유 합니다. 
- Button, Label, Entry, ListView, StackLayout, Calendar, TabbedPage 등의 일반적인 사용자 컨트롤을 사용 합니다. 단추 만들기 및 Xamarin Forms는 바인딩 라이브러리를 사용 하 여 c #에서 Java 또는 Swift 코드를 호출 하는 각 플랫폼에 대해 네이티브 단추를 호출 하는 방법을 확인 합니다.
- 내부 또는 LOB (기간 업무) 앱, 프로토타입 또는 Mvp와 같은 간단한 앱에 적합 합니다. 간단한 사용자 인터페이스를 활용 하 여 약간의 표준 또는 제네릭을 볼 수 있는 모든 앱입니다.

[React Native](react-native.md)

- UI 코드: JavaScript
- 논리 코드: JavaScript
- 네이티브에 반응 하는 목표는 코드를 한 번만 작성 하 고 한 번 (반응 방식) 학습 하 고 어디서 나 쓰기를 수행 하는 것이 아니라 모든 플랫폼에서 실행할 수 있다는 것입니다.
- 커뮤니티에서는 Xcode 또는 Android Studio을 사용 하지 않고 앱을 빌드하는 데 도움이 되는 도구를 추가 하 고 응답 하는 네이티브 앱을 만드는 등의 도구를 추가 했습니다.
- Xamarin (c #)과 유사 하 게 네이티브 (JavaScript)는 네이티브 UI 요소를 호출 합니다 (Java/Kotlin 또는 Swift를 작성 하지 않아도).

[PWA(프로그레시브 웹앱)](pwa.md)

- UI 코드: HTML, CSS, JavaScript
- 논리 코드: JavaScript
- PWAs는 웹 앱과 네이티브 앱 기능을 모두 활용할 수 있도록 표준 패턴을 사용 하 여 빌드된 웹 앱입니다. 프레임 워크를 사용 하지 않고 빌드할 수 있지만 몇 가지 인기 있는 프레임 워크는 PhoneGap [ic](https://ionicframework.com/docs/intro) 및 [](https://phonegap.com/about/)입니다.
- PWAs는 장치 (Android, iOS 또는 Windows)에 설치할 수 있으며, 서비스 근로자의 통합 덕분에 오프 라인으로 작업할 수 있습니다.
- PWAs는 웹 URL만 사용 하 여 앱 스토어 없이 배포 및 설치할 수 있습니다. Microsoft Store 및 Google Play 스토어 PWAs를 나열할 수 있습니다. Apple 스토어는 현재 12.2 이상을 실행 하는 iOS 장치에 설치할 수 있지만 현재는 그렇지 않습니다.
- 자세히 알아보려면 MDN의 [PWAs 소개를](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction) 확인 하세요.

## <a name="game-development"></a>게임 개발

일반적으로 게임은 일반적으로 OpenGL 또는 Vulkan로 작성 된 사용자 지정 렌더링 논리를 사용 하기 때문에 Android 용 게임 개발은 표준 Android 앱 개발에서 고유 합니다. 이러한 이유로, 게임 개발을 지 원하는 여러 C 라이브러리를 사용할 수 있기 때문에 개발자는 android 용 게임을 만들기 위해 Android [네이티브 개발 키트 (NDK)](/cpp/cross-platform/create-an-android-native-activity-app?view=vs-2019)와 함께 [Visual Studio에서 c/c + +](/cpp/cross-platform/?view=vs-2019)를 사용 하는 것이 일반적입니다. [게임 개발에 대 한 c/c + + 시작](native-android.md#use-c-or-c-for-android-game-development)

Android 용 게임 개발을 위한 또 다른 일반적인 경로는 게임 엔진을 사용 하는 것입니다. [Visual Studio를 사용](/visualstudio/cross-platform/visual-studio-tools-for-unity?view=vs-2019)하는 Unity, [unreal Engine](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/GettingStarted/index.html), xamarin with xamarin, [urhosharp](/xamarin/graphics-games/urhosharp/introduction)with xamarin, [SkiaSharp](/xamarin/xamarin-forms/user-interface/graphics/skiasharp/) 의 [MonoGame](/xamarin/graphics-games/monogame/introduction/)Cocoonjs, App Game Kit, Fusion, 코로나 SDK, 코코스 및 기타와 같은 다양 한 무료 오픈 소스 엔진을 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [Windows에서 네이티브 Android 개발 시작](native-android.md)
- [Xamarin Android를 사용 하 여 Android 용 개발 시작](xamarin-android.md)
- [Xamarin.ios를 사용 하 여 Android 용 개발 시작](xamarin-forms.md)
- [네이티브 응답을 사용 하 여 Android 개발 시작](react-native.md)
- [Android 용 PWA 개발 시작](pwa.md)
- [Android 용 이중 화면 앱 개발 및 Surface 듀오 장치 SDK 다운로드](/dual-screen/android/)
- [Windows Defender 제외를 추가 하 여 성능 향상](defender-settings.md)
- [에뮬레이터 성능을 향상 시키기 위해 가상화 지원 사용](emulator.md#enable-virtualization-support)