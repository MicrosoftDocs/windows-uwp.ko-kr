---
title: Windows에서 Android 장치 또는 에뮬레이터 실행
description: Windows에서 Android 장치 또는 에뮬레이터에서 앱을 테스트 하 고 hyper-v 및 Windows 하이퍼바이저 플랫폼 (WHPX)으로 가상화를 사용 하도록 설정 합니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android, windows, 에뮬레이터, 가상 장치, 장치 설정, 장치 사용, 개발자, 구성, 가상화, visual studio, hyper-v, intel, haxm, amd, Windows 하이퍼바이저 플랫폼, WHPX
ms.date: 04/28/2020
ms.openlocfilehash: 57e1d8d62ea7b3918c5e52724c11febcb9f03d72
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161557"
---
# <a name="test-on-an-android-device-or-emulator"></a>Android 장치 또는 에뮬레이터에서 테스트

Windows 컴퓨터의 실제 장치 또는 에뮬레이터를 사용 하 여 Android 응용 프로그램을 테스트 하 고 디버그 하는 여러 가지 방법이 있습니다. 이 가이드에서는 몇 가지 권장 사항을 설명 했습니다.

## <a name="run-on-a-real-android-device"></a>실제 Android 장치에서 실행

실제 Android 장치에서 앱을 실행 하려면 먼저 Android 장치를 개발에 사용 하도록 설정 해야 합니다. Android의 개발자 옵션은 버전 4.2부터, Android 버전에 따라 달라질 수 있으므로 기본적으로 숨겨져 있습니다.

### <a name="enable-your-device-for-development"></a>디바이스를 개발에 사용하도록 설정

최신 버전의 Android 9.0 +를 실행 하는 장치의 경우:

1. USB 케이블로 Windows 개발 컴퓨터에 장치를 연결 합니다. USB 드라이버를 설치 하는 알림을 받을 수 있습니다.
2. Android 장치에서 **설정** 화면을 엽니다.
3. **휴대폰 정보**를 선택 합니다.
4. **이제 개발자** 가 될 때까지 아래쪽으로 스크롤하고 **빌드 번호** 를 7 번 누릅니다. 탭합니다.
5. 이전 화면으로 돌아가서 **System**을 선택 합니다.
6. **고급**을 선택 하 고 아래쪽으로 스크롤한 다음 **개발자 옵션**을 탭 합니다.
7. **개발자 옵션** 창에서 아래로 스크롤하여 **USB 디버깅**을 찾아서 사용 하도록 설정 합니다.

이전 버전의 Android를 실행 하는 장치의 경우 [개발용 장치 설정](/xamarin/android/get-started/installation/set-up-device-for-development)을 참조 하세요.

### <a name="run-your-app-on-the-device"></a>장치에서 앱 실행

1. Android Studio 도구 모음에서 **구성 실행** 드롭다운 메뉴에서 앱을 선택 합니다.

    ![Android Studio 실행 구성 메뉴](../images/android-run-config-menu.png)

2. **대상 장치** 드롭다운 메뉴에서 앱을 실행 하려는 장치를 선택 합니다.

    ![Android Studio 대상 장치 메뉴](../images/android-target-device-menu.png)

3. ▷ 실행을 선택 합니다. 그러면 연결 된 장치에서 앱이 시작 됩니다.

## <a name="run-your-app-on-a-virtual-android-device-using-an-emulator"></a>에뮬레이터를 사용 하 여 가상 Android 장치에서 앱 실행

Windows 컴퓨터에서 Android 에뮬레이터를 실행 하는 방법에 대 한 첫 번째 사항은 가상화 지원을 사용 하도록 설정 하 여 에뮬레이터 성능이 크게 향상 Android Studio 되었다는 것입니다.

### <a name="enable-virtualization-support"></a>가상화 지원 사용

Android emulator를 사용 하 여 가상 장치를 만들기 전에 Hyper-v 및 WHPX (Windows 하이퍼바이저 플랫폼) 기능을 켜서 가상화를 사용 하는 것이 좋습니다. 이렇게 하면 컴퓨터의 프로세서가 에뮬레이터의 실행 속도를 크게 향상 시킬 수 있습니다.

> Hyper-v 및 Windows 하이퍼바이저 플랫폼을 실행 하려면 컴퓨터에서 다음을 수행 해야 합니다.
>
> * 4GB의 메모리를 사용할 수 있습니다.
> * SLAT (두 번째 수준 주소 변환)를 사용 하는 64 비트 Intel processor 또는 AMD Ryzen CPU가 있어야 합니다.
> * Windows 10 build 1803 +를 실행 하 고 있습니다 ([빌드 # 확인](ms-settings:about)).
> * 업데이트 된 그래픽 드라이버 (Device Manager > 디스플레이 어댑터 > 업데이트 드라이버)
>
> 컴퓨터가이 조건에 맞지 않으면 [INTEL HAXM](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows) 또는 [AMD 하이퍼바이저](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors)를 실행할 수 있습니다. 자세한 내용은 [에뮬레이터 성능에 대 한 하드웨어 가속](/xamarin/android/get-started/installation/android-emulator/hardware-acceleration) 또는 [Android Studio 에뮬레이터 설명서](https://developer.android.com/studio/run/emulator)문서를 참조 하세요.

1. 명령 프롬프트를 열고 명령을 입력 하 여 컴퓨터 하드웨어 및 소프트웨어가 Hyper-v와 호환 되는지 확인 합니다. `systeminfo`

    ![명령 프롬프트에서 systeminfo의 hyper-v 요구 사항](../images/systeminfo.png)

2. Windows 검색 상자 (왼쪽 아래)에 "windows 기능"을 입력 합니다. 검색 결과에서 **Windows 기능 사용/사용 안 함** 을 선택 합니다.

3. **Windows 기능** 목록이 표시 되 면를 스크롤하여 **Hyper-v** 찾기 (관리 도구 및 플랫폼 모두 포함)와 **Windows 하이퍼바이저 플랫폼**을 선택 하 고 확인란을 선택 하 고 **확인**을 선택 합니다.

4. 메시지가 표시되면 컴퓨터를 다시 시작합니다.

### <a name="emulator-for-native-development-with-android-studio"></a>Android Studio를 사용한 네이티브 개발용 에뮬레이터

네이티브 Android 앱을 빌드 및 테스트 하는 경우 [Android Studio를 사용 하](./native-android.md)는 것이 좋습니다. 앱을 테스트할 준비가 되 면 다음을 수행 하 여 앱을 빌드하고 실행할 수 있습니다.

1. Android Studio 도구 모음에서 **구성 실행** 드롭다운 메뉴에서 앱을 선택 합니다.

    ![Android Studio 실행 구성 메뉴](../images/android-run-config-menu.png)

2. **대상 장치** 드롭다운 메뉴에서 앱을 실행 하려는 장치를 선택 합니다.

    ![Android Studio 대상 장치 메뉴](../images/android-target-device-menu.png)

3. ▷ 실행을 선택 합니다. 그러면 [Android Emulator](https://developer.android.com/studio/run/emulator)시작 됩니다.

> [!TIP]
> 앱이 에뮬레이터 장치에 설치 되 면 [변경 내용 적용](https://developer.android.com/studio/run#apply-changes) 을 사용 하 여 새 apk를 빌드하지 않고도 특정 코드 및 리소스 변경 사항을 배포할 수 있습니다.

### <a name="emulator-for-cross-platform-development-with-visual-studio"></a>Visual Studio를 사용 하 여 플랫폼 간 개발을 위한 에뮬레이터

Windows Pc에는 많은 [Android 에뮬레이터 옵션](https://www.androidauthority.com/best-android-emulators-for-pc-655308/) 을 사용할 수 있습니다. 최신 Android OS 이미지 및 Google Play 서비스에 대 한 액세스를 제공 하므로 Google의 [android 에뮬레이터](https://developer.android.com/studio/run/emulator)를 사용 하는 것이 좋습니다.

### <a name="install-android-emulator-with-visual-studio"></a>Visual Studio를 사용 하 여 Android emulator 설치

1. 아직 설치 하지 않은 경우 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)을 다운로드 합니다. Visual Studio 설치 관리자를 사용 하 여 [작업을 수정](/visualstudio/install/modify-visual-studio?view=vs-2019#modify-workloads) 하 고 **.net 워크 로드를 사용한 모바일 개발**이 있는지 확인 합니다.

2. 새 프로젝트를 만듭니다. [Android Emulator를 설정한](/xamarin/android/get-started/installation/android-emulator/)후에는 [Android Device Manager](/xamarin/android/get-started/installation/android-emulator/device-manager?pivots=windows&tabs=windows#requirements) 를 사용 하 여 다양 한 Android 가상 장치를 만들고, 복제 하 고, 사용자 지정 하 고, 시작할 수 있습니다. 도구 메뉴에서 **도구**  >  **Android**  >  **Android Device Manager**를 사용 하 여 Android Device Manager를 시작 합니다.

3. Android Device Manager 열리면 **+ 새로** 만들기를 선택 하 여 새 장치를 만듭니다.

4. 장치에 이름을 지정 하 고, 드롭다운 메뉴에서 기본 장치 유형을 선택 하 고, 프로세서 및 OS 버전을 선택 하 고, 가상 장치에 대 한 몇 가지 다른 변수를 선택 해야 합니다. 자세한 내용은 [주 화면을 Android Device Manager](/xamarin/android/get-started/installation/android-emulator/device-manager?pivots=windows&tabs=windows#main-screen).

5. Visual Studio 도구 모음에서 **디버그** (앱이 시작 된 후 에뮬레이터 내에서 실행 되는 응용 프로그램 프로세스에 연결) 또는 **릴리스** 모드 (디버거를 사용 하지 않도록 설정) 중에서 선택 합니다. 그런 다음 장치 드롭다운 메뉴에서 가상 장치를 선택 하 고 **재생** 단추 ▷를 선택 하 여 에뮬레이터에서 응용 프로그램을 실행 합니다.

    ![Visual Studio 시작 Android Emulator](../images/vs-target-device-menu.png)

## <a name="additional-resources"></a>추가 리소스

- [Android 용 이중 화면 앱 개발 및 Surface 듀오 장치 SDK 다운로드](/dual-screen/android/)

- [Windows Defender 제외를 추가 하 여 성능 향상](defender-settings.md)