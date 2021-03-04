---
title: Windows에서 네이티브 Android 개발
description: Windows에서 Android 네이티브 앱 개발을 시작 하는 방법에 대 한 단계별 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android, windows, android studio, visual studio, c + + android 게임, windows defender, 에뮬레이터, 가상 장치, 설치, java, kotlin
ms.date: 04/28/2020
ms.openlocfilehash: c08aac8968ecc16ec548fadd4bee7d83b74ea719
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823187"
---
# <a name="get-started-with-native-android-development-on-windows"></a>Windows에서 네이티브 Android 개발 시작

이 가이드에서는 Windows를 사용 하 여 네이티브 Android 응용 프로그램을 만드는 작업을 시작 합니다. 플랫폼 간 솔루션을 선호 하는 경우 몇 가지 옵션에 대 한 간략 한 요약은 [Windows에서 Android 개발 개요](./overview.md) 를 참조 하세요.

네이티브 Android 앱을 만드는 가장 간단한 방법은 [Java 또는 Kotlin](#java-or-kotlin)와 함께 Android Studio를 사용 하는 것 이지만, 특정 목적이 있는 경우 [Android 개발용으로 C 또는 c + +를 사용할](#use-c-or-c-for-android-game-development) 수도 있습니다. Android Studio SDK 도구는 코드, 데이터 및 리소스 파일을 보관 Android 패키지인 .apk 파일로 컴파일합니다. 하나의 APK 파일은 Android 앱의 모든 콘텐츠를 포함 하며, Android 지원 장치에서 앱을 설치 하는 데 사용 하는 파일입니다.

## <a name="install-android-studio"></a>Android Studio 설치

Android Studio는 Google의 Android 운영 체제에 대 한 공식 통합 개발 환경입니다. [최신 버전의 Windows 용 Android Studio를 다운로드](https://developer.android.com/studio)합니다.

- .Exe 파일을 다운로드 한 경우 (권장)를 두 번 클릭 하 여 시작 합니다.
- .Zip 파일을 다운로드 한 경우 ZIP 압축을 풀고, android-studio 폴더를 Program Files 폴더에 복사한 다음, android-studio > bin 폴더를 열고 studio64.exe (64 비트 컴퓨터의 경우) 또는 studio.exe (32 비트 컴퓨터의 경우)를 시작 합니다.

Android Studio 설치 마법사의 지시에 따라 권장 되는 SDK 패키지를 설치 합니다. 새 도구 및 기타 api를 사용할 수 있게 되 면 팝업을 **알려 주는 Android Studio** 하거나  >  **업데이트 확인** 을 선택 하 여 업데이트를 확인 합니다.

## <a name="create-a-new-project"></a>새 프로젝트 만들기

**파일**  >  **새로** 만들기  >  **새 프로젝트** 를 선택 합니다.

**프로젝트 선택** 창에서 다음 템플릿 중에서 선택할 수 있습니다.

- **기본 작업**: 응용 프로그램 표시줄, 부동 작업 단추, 두 개의 레이아웃 파일, 즉 작업에 대해 하나씩, 텍스트 콘텐츠를 구분 하는 데 사용할 수 있는 간단한 앱을 만듭니다.

- **빈 작업**: 샘플 텍스트 콘텐츠를 사용 하 여 빈 작업 및 단일 레이아웃 파일을 만듭니다.

- **아래쪽 탐색 활동**: 활동에 대 한 표준 아래쪽 탐색 모음을 만듭니다. Google의 재질 디자인 지침에서 [아래쪽 탐색 구성 요소](https://material.io/guidelines/components/bottom-navigation.html) 를 참조 하세요.

- Android Studio 문서에서 [활동 템플릿을 선택 하](https://developer.android.com/studio/projects/templates#SelectTemplate) 는 방법에 대해 자세히 알아보세요.

템플릿은 일반적으로 신규 및 기존 앱 모듈에 활동을 추가 하는 데 사용 됩니다. 예를 들어 앱 사용자에 대 한 로그인 화면을 만들려면 [로그인 활동 템플릿을](https://developer.android.com/studio/projects/templates#LoginActivity)사용 하 여 활동을 추가 합니다.

> [!NOTE]
> Android 운영 체제는 **구성 요소의** 개념을 기반으로 하며 **작업** 및 **의도** 를 사용 하 여 상호 작용을 정의 합니다. **활동** 은 사용자가 수행할 수 있는 단일의 집중 된 작업을 나타냅니다. **활동** 은 **뷰** 클래스를 기반으로 하는 클래스를 사용 하 여 사용자 인터페이스를 빌드하기 위한 창을 제공 합니다. Android 운영 체제에는  ,,,, `onCreate()` `onStart()` `onResume()` `onPause()` `onStop()` 및 `onDestroy()` 의 6 가지 콜백 집합으로 정의 된 활동에 대 한 수명 주기가 있습니다. 활동 구성 요소는 **의도** 개체를 사용 하 여 서로 상호 작용 합니다. 의도는 수행할 동작을 정의 하거나 수행할 동작의 유형을 설명 하는 작업을 정의 합니다 (다른 응용 프로그램에서 비롯 될 수도 있음). Android 문서에서 [활동](https://developer.android.com/reference/android/app/Activity), [활동 수명 주기](https://developer.android.com/guide/components/activities/activity-lifecycle)및 [의도](https://developer.android.com/reference/android/content/Intent.html) 에 대해 자세히 알아보세요.

### <a name="java-or-kotlin"></a>Java 또는 Kotlin

**Java** 는 1991의 언어로, 이제 Sun Microsystems는 무엇 이지만 Oracle에서 소유 하 고 있습니다. 전 세계에서 가장 큰 지원 커뮤니티 중 하나를 사용 하는 가장 인기 있는 강력한 프로그래밍 언어 중 하나입니다. Java는 클래스 기반 및 개체 지향적 이며 최대한 적은 구현 종속성을 갖도록 설계 되었습니다. 구문은 C 및 c + +와 유사 하지만 그 중 하나 보다 낮은 수준의 기능을 포함 합니다.

**Kotlin** 는 2011의 JetBrains에서 제공 하는 새로운 오픈 소스 언어로 처음 발표 되었으며 2017 이후 Android Studio의 Java에 대 한 대안으로 제공 되었습니다. 2019 년 5 월에 Google은 Android 앱 개발자에 게 선호 되는 언어를 제공 하기 위해 Kotlin 발표 되었습니다 .이 언어는 또한 강력한 지원 커뮤니티를 제공 하며 가장 빠르게 증가 하는 프로그래밍 언어 중 하나로 식별 되었습니다. Kotlin는 플랫폼 간, 정적 형식 및 Java와 완전히 상호 운용 되도록 디자인 되었습니다.

Java는 광범위 한 응용 프로그램에 더 광범위 하 게 사용 되며 선택 된 예외, 클래스가 아닌 기본 형식, 정적 멤버, 비 전용 필드, 와일드 카드 형식 및 3 진 연산자와 같이 Kotlin에서 제공 하지 않는 일부 기능을 제공 합니다. Kotlin은 Android에서 특별히 설계 되 고 권장 됩니다. 또한 형식 시스템에서 제어 하는 null 참조, 원시 형식, 고정 배열, 적절 한 함수 형식 (Java의 SAM 변환과 반대), 와일드 카드 없이 사용 되는 사이트 가변성, 스마트 캐스트 등 Java에서 제공 하지 않는 일부 기능도 제공 합니다. Kotlin 설명서는 [Java 비교에 대해](https://kotlinlang.org/docs/reference/comparison-to-java.html)자세히 설명 합니다.

### <a name="minimum-api-level"></a>최소 API 수준

응용 프로그램에 대 한 최소 API 수준을 결정 해야 합니다. 응용 프로그램에서 지원 되는 Android 버전을 결정 합니다. 더 낮은 API 수준은 이전 이므로 일반적으로 더 많은 장치를 지원 하지만 더 높은 API 수준이 더 최신이 고 줄임으로써 더 많은 기능을 제공 합니다.

![Android Studio 최소 API 선택 화면](../images/android-minimum-api-selection.png)

플랫폼 버전 릴리스와 연결 된 장치 지원 배포 및 주요 기능을 보여 주는 비교 차트를 열려면 **도움말 선택** 링크를 선택 합니다.

![Android Studio 최소 API 비교 화면](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>즉시 앱 지원 및 Androidx 아티팩트

**인스턴트 앱을 지원** 하 고 프로젝트 만들기 옵션에서 **androidx 아티팩트를 사용** 하는 확인란을 확인할 수 있습니다. *인스턴트 앱 지원이* 선택 되지 않고 *androidx* 가 권장 되는 기본값으로 확인 됩니다.

Google Play **인스턴트 앱** 은 사용자가 앱 또는 게임을 먼저 설치 하지 않고 사용해 볼 수 있는 방법을 제공 합니다. 이러한 인스턴트 앱은 Play 스토어, Google 검색, 소셜 네트워크 및 링크를 공유 하는 모든 곳에서 표시 될 수 있습니다. **인스턴트 앱 지원** 상자를 선택 하 여 프로젝트에 Google Play 인스턴트 개발 SDK를 포함 하도록 Android Studio 요청 합니다. [즉각적인 앱을 Google Play](https://developer.android.com/topic/google-play-instant) 하는 방법과 [인스턴트 사용 앱 번들을 만드는](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle)방법에 대해 자세히 알아보려면 Android 설명서를 참조 하세요.

**Androidx 아티팩트** 는 android 지원 라이브러리의 새 버전을 나타내며 android 릴리스에 대해 이전 버전과의 호환성을 제공 합니다. AndroidX는 사용 가능한 모든 패키지에 대 한 문자열 androidx로 시작 하는 일관 된 네임 스페이스를 제공 합니다.

> [!NOTE]
> 이제 AndroidX가 기본 라이브러리입니다. 이 확인란을 선택 취소 하 고 이전 지원 라이브러리를 사용 하려면 최신 Android Q SDK를 제거 해야 합니다. 자세한 내용은 StackOverflow에서 [Androidx 아티팩트 사용을 선택 취소](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts) 하는 방법에 대 한 자세한 내용은 이전 지원 라이브러리 패키지가 해당 androidx. * 패키지에 매핑 되었는지 확인 합니다. 모든 이전 클래스에 대 한 전체 매핑과 새 클래스에 대 한 빌드 아티팩트는 [AndroidX로 마이그레이션](https://developer.android.com/jetpack/androidx/migrate)을 참조 하세요.

## <a name="project-files"></a>프로젝트 파일

Android Studio **프로젝트** 창에는 다음 파일이 포함 되어 있습니다. 드롭다운 메뉴에서 Android 보기가 선택 되어 있어야 합니다.

**앱 > java >. 예: myfirstapp > MainActivity**

앱에 대 한 기본 활동 및 진입점입니다. 앱을 빌드 및 실행 하면 시스템에서이 작업의 인스턴스를 시작 하 고 해당 레이아웃을 로드 합니다.

**앱 > res > 레이아웃 > activity_main.xml**

활동의 UI (사용자 인터페이스)에 대 한 레이아웃을 정의 하는 XML 파일입니다. 여기에는 "헬로 월드" 텍스트를 포함 하는 TextView 요소가 포함 됩니다.

**앱 > 매니페스트 > AndroidManifest.xml**

앱 및 각 구성 요소의 기본적인 특징을 설명 하는 매니페스트 파일입니다.

**Gradle 스크립트 > Gradle**

이 이름을 사용 하는 두 개의 파일은 "Project: My First App", 전체 프로젝트에 대 한 "Module: app", 각 앱 모듈에 대해 "Module: app"입니다. 새 프로젝트에는 처음에 하나의 모듈만 있습니다. 모듈의 build. 파일을 사용 하 여 Gradle 플러그 인에서 앱을 빌드하는 방법을 제어 합니다. Android 문서, [빌드 구성](https://developer.android.com/studio/build/index) 문서에서 자세한 내용을 알아보세요.

## <a name="use-c-or-c-for-android-game-development"></a>Android 게임 개발용 C 또는 c + + 사용

Android 운영 체제는 시스템의 아키텍처에 포함 된 도구에서 기능과 Java 또는 Kotlin로 작성 된 응용 프로그램을 지원 하도록 설계 되었습니다. Android UI 및 의도 처리와 같은 많은 시스템 기능은 Java 인터페이스를 통해서만 노출 됩니다. 일부 관련 된 문제에도 불구 하 고 **ANDROID NDK (네이티브 개발 키트)를 통해 c 또는 c + + 코드를 사용** 하려는 경우가 있습니다. 게임 개발은 일반적으로 OpenGL 또는 Vulkan로 작성 된 사용자 지정 렌더링 논리를 사용 하 고 게임 개발에 초점을 맞춘 풍부한 C 라이브러리를 활용 하기 때문에 게임 개발을 위한 예입니다. C 또는 c + +를 사용 하면 짧은 대기 시간을 확보 하거나 계산 집약적인 응용 프로그램 (예: 물리학 시뮬레이션)을 실행 하는 장치에서 추가 성능을 유지할 수도 *있습니다* . 그러나 NDK는 **대부분의 초보자를 위한 Android 프로그래머에 게 적합 하지 않습니다** . NDK 사용에 대 한 특정 목적이 없으면 Java, Kotlin 또는 [플랫폼 간 프레임 워크](./overview.md)중 하나를 사용 하는 것이 좋습니다.

C/c + +를 사용 하 여 새 프로젝트를 만들려면 다음을 수행 합니다.

- Android Studio 마법사의 **프로젝트 선택** 섹션에서 *네이티브 c + +** 프로젝트 형식을 선택 합니다. **다음** 을 선택 하 고 나머지 필드를 완료 한 후 **다음** 을 다시 선택 합니다.

- 마법사의 **c + + 지원 사용자 지정** 섹션에서 **c + + 표준** 필드를 사용 하 여 프로젝트를 사용자 지정할 수 있습니다. 드롭다운 목록을 사용 하 여 사용 하려는 c + +의 표준화를 선택 합니다. **도구 체인 default** 를 선택 하면 기본 cmake 설정이 사용 됩니다. **마침** 을 선택합니다.

- 새 프로젝트를 Android Studio 만들면 **프로젝트** 창에서 네이티브 소스 파일, 헤더, cmake 또는 ndk 빌드에 대 한 빌드 스크립트, 프로젝트의 일부인 미리 빌드된 라이브러리를 포함 하는 **cpp** 폴더를 찾을 수 있습니다. `native-lib.cpp` `src/main/cpp/` `stringFromJNI()` "Hello From c + +" 라는 문자열을 반환 하는 간단한 함수를 제공 하는 폴더에서 샘플 c + + 소스 파일를 찾을 수도 있습니다. 또한 네이티브 라이브러리를 빌드하는 데 필요한 CMake 빌드 스크립트인 [`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html) 모듈의 루트 디렉터리에서 찾을 수 있습니다.

자세히 알아보려면 Android docs 항목: [프로젝트에 c 및 c + + 코드 추가](https://developer.android.com/studio/projects/add-native-code)를 참조 하세요. 샘플은 GitHub에서 [Android Studio c + + 통합 리포지토리를 사용 하는 ANDROID NDK 샘플](https://github.com/android/ndk-samples) 을 참조 하세요. Android에서 c + + 게임을 컴파일하고 실행 하려면 [Google Play 게임 서비스 API](https://developers.google.com/games/services/cpp/gettingStartedAndroid)를 사용 합니다.

## <a name="design-guidelines"></a>디자인 지침

장치 사용자는 응용 프로그램이 특정 방식으로 표시 되 고 동작 하는 것으로 간주 합니다. 사용자는 음성 제어를 살짝 밀거나 사용 하는지 여부에 상관 없이 응용 프로그램의 모양 및 사용 방법에 대 한 특정 기대치를 보유 하 게 됩니다. 이러한 기대치는 혼동 및 불만을 줄이기 위해 일관 되 게 유지 되어야 합니다. Android는 호환성, 성능 및 보안에 대 한 품질 지침과 함께 시각적 개체 및 탐색 패턴에 대 한 Google Material Design foundation을 결합 하는 이러한 플랫폼 및 장치 기대에 대 한 지침을 제공 합니다.

[Android 디자인 설명서](https://developer.android.com/design)에서 자세히 알아보세요.

### <a name="fluent-design-system-for-android"></a>Android 용 흐름 Design System

Microsoft는 또한 Microsoft의 모바일 앱 전체 포트폴리오에서 원활한 환경을 제공 하는 것을 목표로 하는 디자인 지침을 제공 합니다.

[Android 용 흐름 Design 시스템](https://www.microsoft.com/design/fluent/#/android) 설계 및 고유 하 게 흐름 하는 동안 기본적으로 android 인 사용자 지정 앱 빌드

- [Sketch 도구 키트](https://aka.ms/fluenttoolkits/android/sketch)
- [Figma 도구 키트](https://aka.ms/fluenttoolkits/android/figma)
- [Android 글꼴](https://fonts.google.com/specimen/Roboto)
- [Android 사용자 인터페이스 지침](https://developer.android.com/design/)
- [Android 앱 아이콘에 대 한 지침](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>추가 자료

- [Android 응용 프로그램 기본 사항](https://developer.android.com/guide/components/fundamentals)

- [Android 용 이중 화면 앱 개발 및 Surface 듀오 장치 SDK 다운로드](/dual-screen/android/)

- [Windows Defender 제외를 추가 하 여 성능 향상](defender-settings.md)

- [에뮬레이터 성능을 향상 시키기 위해 가상화 지원 사용](emulator.md#enable-virtualization-support)