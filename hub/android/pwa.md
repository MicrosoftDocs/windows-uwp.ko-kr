---
title: Windows에서의 Android 개발에 대 한 PWA 방법
description: Windows에서 PWA 접근 방법을 사용 하 여 Android 앱 개발을 시작 하세요.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: windows, pwa, android, cordova,가 나 ic, phonegap, 하이브리드 웹 앱의 android
ms.date: 04/28/2020
ms.openlocfilehash: c0ff9acf1d8e93e82f1db424d7a356c974988683
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255217"
---
# <a name="get-started-developing-a-pwa-or-hybrid-web-app-for-android"></a>Android 용 PWA 또는 하이브리드 웹 앱 개발 시작

이 가이드는 웹 및 장치 플랫폼 (Android, iOS, Windows)에서 사용할 수 있는 단일 HTML/CSS/JavaScript 코드 베이스를 사용 하 여 Windows에서 하이브리드 웹 앱 또는 PWA (점진적 웹 앱) 만들기를 시작 하는 데 도움이 됩니다.

웹 기반 응용 프로그램은 올바른 프레임 워크 및 구성 요소를 사용 하 여 네이티브 앱과 매우 유사한 사용자를 찾는 방식으로 Android 장치에서 작동할 수 있습니다.

## <a name="features-of-a-pwa-or-hybrid-web-app"></a>PWA 또는 하이브리드 웹 앱의 기능

Android 장치에 설치할 수 있는 두 가지 주요 유형의 웹 앱이 있습니다. 주요 차이점은 응용 프로그램 코드가 응용 프로그램 패키지에 포함 되는지 (하이브리드) 웹 서버 (pwa)에서 호스트 되는지 여부입니다.

- **하이브리드 웹 앱**: 코드 (HTML, JS, CSS)는 apk에 패키지 되 고 Google Play 스토어를 통해 배포할 수 있습니다. 보기 엔진은 사용자의 인터넷 브라우저에서 격리 되며 세션 또는 캐시 공유는 없습니다.

- **PWAs (프로그레시브 Web Apps)**: 코드 (HTML, JS, CSS)는 웹에 있으며 apk로 패키지할 필요가 없습니다. 서비스 작업자를 사용 하 여 필요에 따라 리소스를 다운로드 하 고 업데이트 합니다. Chrome 브라우저는 앱을 렌더링 하 고 표시 하지만, 기본 브라우저 주소 표시줄 등을 포함 하지 않고 기본으로 표시 됩니다. 브라우저를 사용 하 여 저장소, 캐시 및 세션을 공유할 수 있습니다. 이는 기본적으로 Chrome 브라우저에 대 한 바로 가기를 특수 모드로 설치 하는 것과 같습니다. PWAs는 신뢰할 수 있는 웹 작업을 사용 하 Google Play 스토어에도 나열 될 수 있습니다.

PWAs와 하이브리드 웹 앱은 네이티브 Android 앱과 매우 유사 합니다.

- 앱 스토어 (Google Play 스토어 및/또는 Microsoft Store)를 통해 설치할 수 있습니다.
- 카메라, GPS, Bluetooth, 알림 및 연락처 목록과 같은 기본 장치 기능에 액세스할 수 있습니다.
- 오프 라인으로 작업 (인터넷 연결 안 함)

또한 PWAs에는 몇 가지 고유한 기능이 있습니다.

- 앱 스토어를 사용 하지 않고 웹에서 직접 Android 홈 화면에 설치할 수 있습니다.
- 는 신뢰할 수 있는 [웹 작업을 사용 하 여](https://css-tricks.com/how-to-get-a-progressive-web-app-into-the-google-play-store/) Google Play 스토어를 통해 추가로 설치할 수 있습니다.
- 웹 검색을 통해 검색 하거나 URL 링크를 통해 공유할 수 있습니다.
- [서비스 작업자](https://developers.google.com/web/fundamentals/primers/service-workers) 를 사용 하 여 네이티브 코드를 패키지할 필요가 없도록 합니다.

하이브리드 앱 또는 PWA를 만드는 데 프레임 워크가 필요 하지는 않지만이 가이드에서 다루는 몇 가지 인기 있는 프레임 워크가 있습니다 (예를 들어, Cordova를 사용 하는 PhoneGap (cordova 사용) 및 지 수를 사용 하는 Cordova 또는 콘덴서 포함).

## <a name="apache-cordova"></a>Apache Cordova

[Apache Cordova](https://cordova.apache.org/) 는 [플러그 인](https://cordova.apache.org/plugins/?platforms=cordova-android)을 사용 하 여 네이티브 [웹 보기](https://developer.android.com/reference/android/webkit/WebView) 및 네이티브 Android 플랫폼에서 사용 되는 JavaScript 코드 간의 통신을 단순화할 수 있는 오픈 소스 프레임 워크입니다. 이러한 플러그 인은 코드에서 호출 하 고 네이티브 Android 장치 Api를 호출 하는 데 사용할 수 있는 JavaScript 끝점을 노출 합니다. 일부 예제 Cordova 플러그 인은 배터리 상태, 파일 액세스, 진동/벨 소리 등의 장치 서비스에 대 한 액세스를 포함 합니다. 이러한 기능은 일반적으로 웹 앱 또는 브라우저에서 사용할 수 없습니다.

Cordova의 인기 배포판에는 다음 두 가지가 있습니다.

- [PhoneGap](https://phonegap.com/)

- [Ionic](https://ionicframework.com/)

## <a name="adobe-phonegap"></a>Adobe PhoneGap

[PhoneGap](https://phonegap.com/): [명령줄](http://docs.phonegap.com/getting-started/1-install-phonegap/cli/), [데스크톱 앱](https://phonegap.com/products#desktop-app-section), [PhoneGap 빌드](https://build.phonegap.com/)등의 추가 도구를 사용 하 여 Cordova를 지 원하는 adobe에서 지원 되는 프레임 워크 이며, 로컬 컴퓨터에 네이티브 sdk를 설치할 필요 없이 네이티브 앱을 빌드하는 Adobe 서버에 코드를 업로드할 수 있게 해 주는 서비스입니다. 이를 통해 Windows 컴퓨터를 사용 하 여 iOS 앱을 빌드하는 등의 작업을 수행할 수 있습니다.

### <a name="install-phonegap"></a>PhoneGap 설치

PhoneGap를 사용 하 여 PWA 또는 하이브리드 웹 앱 빌드를 시작 하려면 먼저 다음 도구를 설치 해야 합니다.

- 에 대 한 node.js를 사용 합니다. [Windows 용 NodeJS를 다운로드](https://nodejs.org/en/) 하거나 Wsl (Linux 용 Windows 하위 시스템)을 사용 하는 [nodejs 설치 가이드](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2) 를 따르세요. 여러 프로젝트와 버전의 NodeJS로 작업할 경우 [nvm (Node Version Manager)](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2#install-nvm-nodejs-and-npm) 을 사용 하는 것을 고려할 수 있습니다.

명령줄에서 다음을 입력 하 여 PhoneGap를 설치 합니다.

```bash
npm install -g phonegap
```

새 PhoneGap 프로젝트를 만들려면 해당 단계에 따라 [시작](https://phonegap.com/getstarted/)합니다. PhoneGap 문서의 [Pwa 기능](http://stage.docs.phonegap.com/tutorials/stockpile/911-pwa-features/) 섹션을 방문 하 여 앱을 pwa에 하이브리드로 이동 하는 방법을 알아보세요.  

## <a name="ionic"></a>Ionic

가 나 [ic](https://ionicframework.com/) 는 각 플랫폼 (Android, IOS, Windows)의 디자인 언어와 일치 하도록 앱의 UI (사용자 인터페이스)를 조정 하는 프레임 워크입니다. 가 나 ic를 사용 하면 두 [각도](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro) 를 모두 사용 하거나 [반응할](https://ionicframework.com/react)수 있습니다.

> [!NOTE]
> ' [콘덴서](https://capacitor.ionicframework.com/)' 라고 하는 Cordova의 대안을 사용 하는 새 버전의 새 버전이 있습니다. 이 대안은 컨테이너를 사용 하 여 앱을 [더 웹](https://ionicframework.com/blog/announcing-capacitor-1-0/)에 맞게 만듭니다.

### <a name="get-started-with-ionic-by-installing-required-tools"></a>필요한 도구를 설치 하 여 지 이상 회사 시작

이상 및 Ic를 사용 하 여 PWA 또는 하이브리드 웹 앱 빌드를 시작 하려면 먼저 다음 도구를 설치 해야 합니다.

- 에 대 한 node.js를 사용 합니다. [Windows 용 NodeJS를 다운로드](https://nodejs.org/en/) 하거나 Wsl (Linux 용 Windows 하위 시스템)을 사용 하는 [nodejs 설치 가이드](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2) 를 따르세요. 여러 프로젝트와 버전의 NodeJS로 작업할 경우 [nvm (Node Version Manager)](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2#install-nvm-nodejs-and-npm) 을 사용 하는 것을 고려할 수 있습니다.

- 코드를 작성 하는 VS Code. [Windows 용 VS Code를 다운로드](https://code.visualstudio.com/)합니다. Linux 명령줄을 사용 하 여 앱을 빌드하는 것을 선호 하는 경우 [Wsl 원격 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) 을 설치할 수도 있습니다.

- 기본 CLI (명령줄 인터페이스)를 사용 하기 위한 Windows 터미널입니다. [Microsoft Store에서 Windows 터미널을 설치](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)합니다.

- 버전 제어를 위한 Git입니다. [Git를 다운로드](https://git-scm.com/downloads)합니다.

## <a name="create-a-new-project-with-ionic-cordova-and-angular"></a>가 나 Ic Cordova 및 각도를 사용 하 여 새 프로젝트 만들기

명령줄에서 다음을 입력 하 여 하드 및 Cordova를 설치 합니다.

```bash
npm install -g @ionic/cli cordova
```

다음 명령을 입력 하 여 "탭" 앱 템플릿을 사용 하 여에 지 및 Ic 각도 앱을 만듭니다.

```bash
ionic start photo-gallery tabs
```

앱 폴더로 변경 합니다.

```bash
cd photo-gallery
```

웹 브라우저에서 앱을 실행 합니다.

```bash
ionic serve
```

자세한 내용은 없음 [Ic Cordova 각도 문서](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro)를 참조 하세요. 응용 프로그램을 PWA로 이동 하는 방법에 대 한 자세한 내용은 응용 [프로그램을 pwa](https://ionicframework.com/docs/angular/pwa) 로 이동 하는 방법에 대해 알아봅니다.

## <a name="create-a-new-project-with-ionic-capacitor-and-angular"></a>새 프로젝트를 만듭니다.

명령줄에서 다음을 입력 하 여 기능을 설치 합니다.

```bash
npm install -g @ionic/cli native-run cordova-res
```

"탭" 앱 템플릿을 사용 하 여 고 지를 추가 하 고 명령을 입력 하 여 콘덴서를 추가 합니다.

```bash
ionic start photo-gallery tabs --type=angular --capacitor
```

앱 폴더로 변경 합니다.

```bash
cd photo-gallery
```

구성 요소를 추가 하 여 앱을 PWA로 만듭니다.

```bash
npm install @ionic/pwa-elements
```

파일에 다음을 추가 하 여 가져옵니다 @ionic/pwa-elements `src/main.ts`

```typescript
import { defineCustomElements } from '@ionic/pwa-elements/loader';

// Call the element loader after the platform has been bootstrapped
defineCustomElements(window);
```

웹 브라우저에서 앱을 실행 합니다.

```bash
ionic serve
```

자세한 내용은 없음 [Ic 콘덴서 각도 문서](https://ionicframework.com/docs/angular/your-first-app)를 참조 하세요. 응용 프로그램을 PWA로 이동 하는 방법에 대 한 자세한 내용은 응용 [프로그램을 pwa](https://ionicframework.com/docs/angular/pwa) 로 이동 하는 방법에 대해 알아봅니다.  

## <a name="create-a-new-project-with-ionic-and-react"></a>가 나 Ic를 사용 하 여 새 프로젝트를 만들고 반응

명령줄에서 다음을 입력 하 여 하드 없는 Ic CLI를 설치 합니다.

```bash
npm install -g @ionic/cli
```

다음 명령을 입력 하 여 반응으로 새 프로젝트를 만듭니다.

```bash
ionic start myApp blank --type=react
```

앱 폴더로 변경 합니다.

```bash
cd myApp
```

웹 브라우저에서 앱을 실행 합니다.

```bash
ionic serve
```

자세한 내용은이 [문서](https://ionicframework.com/docs/react/quickstart)를 참조 하세요. 응용 프로그램을 PWA로 이동 하는 방법에 대 한 자세한 내용은 응용 프로그램을 pwa에 반응 하 게 [만드는](https://ionicframework.com/docs/react/pwa) 방법 섹션을 참조 하세요.

## <a name="test-your-ionic-app-on-a-device-or-emulator"></a>장치 또는 에뮬레이터에서 인지 Ic 앱 테스트

Android 장치에서 변경 된 앱을 테스트 하려면 장치 플러그 인을 사용 하 여 ([개발을 위해 먼저 사용 하도록 설정 되었는지 확인](emulator.md#enable-your-device-for-development)) 명령줄에서 다음을 입력 합니다.

```bash
ionic cordova run android
```

Android 장치 에뮬레이터에서 응용 프로그램을 테스트 하려면 다음을 수행 해야 합니다.

1. [JDK (Java Development Kit), Gradle 및 Android SDK 필수 구성 요소를 설치](https://cordova.apache.org/docs/en/latest/guide/platforms/android/#installing-the-requirements)합니다.

2. [AVD (Android 가상 장치)를 만듭니다](https://developer.android.com/studio/run/managing-avds.html).

3. 응용 프로그램을 빌드하고 응용 프로그램을 에뮬레이터에 배포 하는 데 사용할 수 있는 `ionic cordova emulate [<platform>] [options]`하드 ic 명령을 입력 합니다. 이 경우 명령은 다음과 같아야 합니다.

```bash
ionic cordova emulate android --list
```

자세한 내용은 향상 된 Ic 문서에서 [Cordova 에뮬레이터](https://ionicframework.com/docs/cli/commands/cordova-emulate) 를 참조 하세요.

## <a name="additional-resources"></a>추가 리소스

- [Android 용 이중 화면 앱 개발 및 Surface 듀오 장치 SDK 다운로드](https://docs.microsoft.com/dual-screen/android/)

- [Windows Defender 제외를 추가 하 여 성능 향상](defender-settings.md)

- [에뮬레이터 성능을 향상 시키기 위해 가상화 지원 사용](emulator.md#enable-virtualization-support)
