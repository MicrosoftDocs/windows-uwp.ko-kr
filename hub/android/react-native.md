---
title: Windows에서 Android 개발용 네이티브에 반응
description: Windows에서 Xamarin Native를 사용 하 여 Android 앱 개발을 시작 하세요.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android, windows, 기본, 에뮬레이터, azure, 번들러, metro, 터미널
ms.date: 04/28/2020
ms.openlocfilehash: db49e3ed12fee8e7ced7680e305a84afb89f32a2
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255237"
---
# <a name="get-started-developing-for-android-using-react-native"></a>네이티브 응답을 사용 하 여 Android 개발 시작

이 가이드는 Windows의 기본 동작을 사용 하 여 Android 장치에서 작동 하는 플랫폼 간 앱을 만들기 시작 하는 데 도움이 됩니다.

## <a name="overview"></a>개요

네이티브에 반응 하는 것은 Facebook에서 만든 [오픈 소스](https://github.com/facebook/react-native) 모바일 응용 프로그램 프레임 워크입니다. 이는 네이티브 UI 컨트롤을 제공 하 고 네이티브 플랫폼에 대 한 모든 액세스를 제공 하는 Android, iOS, 웹 및 UWP (Windows) 용 응용 프로그램을 개발 하는 데 사용 됩니다. 네이티브에 반응 하는 작업을 수행 하려면 JavaScript 기본 사항을 이해 해야 합니다.

## <a name="get-started-with-react-native-by-installing-required-tools"></a>필수 도구를 설치 하 여 기본 응답 시작

1. [Visual Studio Code](https://code.visualstudio.com) (또는 원하는 코드 편집기)를 설치 합니다.

2. [Windows 용 Android Studio를 설치](https://developer.android.com/studio)합니다. Android Studio은 기본적으로 최신 Android SDK를 설치 합니다. 네이티브에 반응 하려면 Android 6.0 (Marshmallow) SDK 이상이 필요 합니다. 최신 SDK를 사용 하는 것이 좋습니다.

3. Java SDK 및 Android SDK에 대 한 환경 변수 경로를 만듭니다.
    - Windows 검색 메뉴에서 "시스템 환경 변수 편집"을 입력 하면 **시스템 속성** 창이 열립니다.
    - **환경 변수 ...** 를 선택한 다음 **사용자 변수**에서 **새로 만들기 ...** 를 선택 합니다.
    - 변수 이름 및 값 (경로)을 입력 합니다. Java 및 Android Sdk의 기본 경로는 다음과 같습니다. Java 및 Android Sdk를 설치할 특정 위치를 선택한 경우 변수 경로를 적절 하 게 업데이트 해야 합니다.
        - JAVA_HOME: C:\Program Files\Android\Android Studio\jre\jre
        - ANDROID_HOME: C:\Users\username\AppData\Local\Android\Sdk

    ![환경 변수 경로를 추가 하는 스크린샷](../images/add-environmental-variable-path.png)

4. [Windows 용 NodeJS 설치](https://nodejs.org/en/) 여러 프로젝트와 버전의 NodeJS로 작업할 경우 [Windows 용 nvm (Node Version Manager)](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) 을 사용 하는 것을 고려할 수 있습니다. 새 프로젝트에 대 한 최신 LTS 버전을 설치 하는 것이 좋습니다.

> [!NOTE]
> 기본 CLI (명령줄 인터페이스) 및 [버전 제어를 위해 Git](https://git-scm.com/downloads)를 사용 하기 위해 [Windows 터미널](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) 을 설치 하 고 사용 하는 것을 고려할 수도 있습니다. [JAVA JDK](https://www.oracle.com/java/technologies/javase-downloads.html) 는 Android Studio v 2.2 +와 함께 제공 되지만, Android Studio와 별도로 JDK를 업데이트 해야 하는 경우 [Windows x64 설치 관리자](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html)를 사용 합니다.

## <a name="create-a-new-project-with-react-native"></a>네이티브에 반응 하 여 새 프로젝트 만들기

1. Npm를 사용 하 여 Windows 명령 프롬프트, PowerShell, [Windows 터미널](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)또는 VS Code의 통합 터미널 (> 통합 터미널 보기)에서를 사용 하 [여 명령줄 유틸리티를 설치](https://docs.expo.io/versions/latest/) 합니다.

    ```powershell
    npm install -g expo-cli
    ```

2. 주문형 o를 사용 하 여 iOS, Android 및 웹에서 실행 되는 반응 네이티브 앱을 만듭니다. 그런 다음 **공백**, **공백 (typescript)**, **탭** (반응 탐색을 사용 하는 예제 화면), **최소**또는 **최소 (typescript)** 를 포함 하는 프로젝트 템플릿을 선택 해야 합니다.

    ```powershell
    expo init my-new-app
    ```

    > [!NOTE]
    > 를 사용 `npx create-react-native-app`하는 데를 사용 하는 경우에도 계속 작동 하지만, 계속 해 서 o CLI init에 [몇 가지 추가 이점이](https://github.com/react-native-community/discussions-and-proposals/issues/23)있습니다.

3. 새 "내 새-앱" 디렉터리를 엽니다.

    ```powershell
    cd my-new-app
    ```

4. 프로젝트를 실행 하려면 다음 명령을 입력 합니다. 그러면 기본 인터넷 브라우저에서 노드 Metro 번들러을 표시 하는 localhost 창이 열립니다. 또한 명령줄 및 Metro 번들러 브라우저 창에서 QR 코드를 표시 합니다. * 명령을 `npm start` `npm run android` 사용할 수도 있습니다.

     ```powershell
    expo start
    ```

    ![브라우저의 Metro 번들러 스크린샷](../images/metro-bundler.png)

5. Android 장치에서 실행 되는 프로젝트를 보려면 먼저 Android 장치에 Google Play 스토어를 사용 하 여 주문형 [o 클라이언트 앱을 설치](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_US) 해야 합니다. 주문형 o 클라이언트 앱이 설치 되 면 장치에서 열고 **QR 코드 스캔**을 선택 합니다. QR 코드를 등록 한 후에는 장치 및 브라우저의 localhost에서 실행 되는 Metro 번들러 창 모두에서 패키지 빌드를 볼 수 있습니다.

6. Android 에뮬레이터에서 실행 되는 프로젝트를 보려면 먼저 Android Studio를 열고 가상 장치를 만들고 시작 해야 합니다. **도구** > **avd 관리자** > **[+ 가상 장치 만들기](https://developer.android.com/studio/run/managing-avds#createavd)**... 가상 장치를 만든 후에는 Android 가상 Device Manager의 **작업** 열 아래에서 시작 단추를 선택 하 여 장치 에뮬레이트를 시작 ▷. 가상 장치가 열리면 인터넷 브라우저 창에서 실행 되는 Metro 번들러 창으로 돌아가서 왼쪽 열에서 "Android 장치/에뮬레이터에서 실행"을 선택 합니다. Metro 번들러 "시뮬레이터를 여는 중 ..." 라는 팝업이 표시 됩니다. 그런 다음 에뮬레이트된 Android 장치에서 클라이언트 앱이 열린 것을 확인 하 고 JavaScript 번들 다운로드가 완료 되 면 반응 하는 네이티브 앱이 표시 되는 것을 볼 수 있습니다. 문제가 발생 하는 경우에 [는 Android 에뮬레이터 문서를](https://docs.expo.io/workflow/android-studio-emulator/)참조 하세요.

7. 반응 하는 네이티브 프로젝트를 열어 앱에서 작업을 시작 합니다. 사용자의 장치 또는 Android Emulator에서 자동으로 실행 되는 앱에서 변경 내용이 자동으로 업데이트 되는 것을 볼 수 있습니다.

8. 방문 페이지 보기 텍스트를 "Hello World!"로 변경해 보세요. 선택한 IDE에서이 작업을 수행할 수 있습니다. VS Code 또는 Android Studio 하는 것이 좋습니다. 방문 페이지 파일은 선택한 템플릿에 따라 달라 집니다. , `App.tsx`또는 `HomeScreen.js`일 `App.js`수 있습니다.

    ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
        </View>
      );
    }
    ```

9. 이미지를 추가 해 보세요. 먼저 앱에서 "android" 및 "ios" 폴더와 동일한 수준에서 폴더를 만들어야 합니다. "images" 라고 하겠습니다. 해당 폴더에 이미지를 저장 합니다 `your-image.png` (예:). 아래 형식을 사용 하 여 이미지를 참조 하 고 높이 및 너비를 사용 하 여 스타일을 지정 합니다.

     ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
          <Image source={require('./images/your-image.png')} style = {{height: 200, width: 250, }} />
        </View>
      );
    }
    ```

> [!TIP]
> Windows 10 앱으로 실행 되도록 반응 하는 네이티브 앱에 대 한 지원을 추가 하려면 [windows 용 기본 응답 시작](https://microsoft.github.io/react-native-windows/docs/getting-started) 문서를 참조 하세요.

## <a name="additional-resources"></a>추가 리소스

- [Android 용 이중 화면 앱 개발 및 Surface 듀오 장치 SDK 다운로드](https://docs.microsoft.com/dual-screen/android/)

- [Windows Defender 제외를 추가 하 여 성능 향상](defender-settings.md)

- [에뮬레이터 성능을 향상 시키기 위해 가상화 지원 사용](emulator.md#enable-virtualization-support)
