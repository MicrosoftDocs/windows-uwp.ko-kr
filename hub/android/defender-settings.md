---
title: Defender 설정을 업데이트 하 여 성능 속도 향상
description: 지정 된 파일 형식 확인을 제외 하도록 Windows Defender 설정을 업데이트 하 여 성능 속도와 빌드 시간을 개선 하는 방법에 대해 알아봅니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android, windows, windows defender, 설정, 구성, 제외,% USERPROFILE%, devenv.exe, 성능, 속도, 빌드, gradle
ms.date: 04/28/2020
ms.openlocfilehash: d818c4f568698a121fb7051ec5e6e2d246bff924
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255137"
---
# <a name="update-windows-defender-settings-to-improve-performance"></a>성능 향상을 위한 Windows Defender 설정 업데이트

이 가이드에서는 windows 컴퓨터의 빌드 시간 및 전반적인 성능 속도를 개선 하기 위해 Windows Defender 보안 설정에서 제외를 설정 하는 방법을 설명 합니다.

## <a name="windows-defender-overview"></a>Windows Defender 개요

Windows 10 버전 1703 이상에서 windows [Defender 바이러스 백신](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-security-center-antivirus) 앱은 windows 보안의 일부입니다. Windows Defender는 바이러스, 랜 섬 웨어, 스파이웨어 및 기타 보안 위협에 대해 기본 제공 되는 실시간 보호를 통해 PC를 안전 하 게 유지 하는 것을 목표로 합니다.

**그러나**Windows Defender의 실시간 보호를 사용 하면 Android 앱을 개발할 때 파일 시스템 액세스 및 빌드 속도가 크게 느려질 수 있습니다.

Android 빌드 프로세스 중에 많은 파일이 컴퓨터에 만들어집니다. 바이러스 백신 실시간 검사를 사용 하도록 설정 하면 바이러스 백신에서 해당 파일을 검색 하는 동안 새 파일이 만들어질 때마다 빌드 프로세스가 중단 됩니다.

다행히 Windows Defender에는 바이러스 백신 검사 프로세스에서 안전 하 게 알고 있는 파일, 프로젝트 디렉터리 또는 파일 형식을 제외 하는 기능이 있습니다.

> [!WARNING]
> 컴퓨터의 악성 소프트웨어를 안전 하 게 보호 하려면 실시간 검색 또는 Windows Defender 바이러스 백신 소프트웨어를 완전히 사용 하지 않도록 설정 하면 안 됩니다.

## <a name="add-exclusions-to-windows-defender"></a>Windows Defender에 제외 추가

Android 빌드 속도를 향상 시키려면 다음을 수행 하 여 [Windows Defender Security Center](windowsdefender://) 에서 제외를 추가 합니다.

1. Windows 메뉴 **시작** 단추를 선택 합니다.
2. **Windows 보안** 입력
3. **바이러스 및 위협 방지** 선택
4. **바이러스 & 위협 방지 설정** 에서 **설정 관리** 를 선택 합니다.
5. **제외** 제목으로 스크롤하고 **제외 추가 또는 제거** 를 선택 합니다.
6. **+ 제외 추가**를 선택 합니다. 그런 다음 추가 하려는 제외가 **파일**, **폴더**, **파일 형식**또는 **프로세스**인지 여부를 선택 해야 합니다.

![Windows Defender 제외 추가 스크린샷](../images/windows-defender-exclusions.png)

## <a name="recommended-exclusions"></a>권장 제외

다음 목록에서는 Windows Defender 실시간 검색의 제외로 추가 하도록 권장 되는 각 Android Studio 디렉터리의 기본 위치를 보여 줍니다.

- Gradle 캐시:`%USERPROFILE%\.gradle`
- Android Studio 프로젝트:`%USERPROFILE%\AndroidStudioProjects`
- Android SDK:`%USERPROFILE%\AppData\Local\Android\SDK`
- Android Studio 시스템 파일:`%USERPROFILE%\.AndroidStudio<version>\system`

Android Studio에 의해 설정 된 기본 위치를 사용 하지 않았거나 GitHub에서 프로젝트를 다운로드 한 경우에는 이러한 디렉터리 위치가 프로젝트에 적용 되지 않을 수 있습니다 (예:). 현재 Android 개발 프로젝트의 디렉터리에 제외를 추가 하는 것이 좋습니다. 언제 든 지 찾을 수 있습니다.

고려해 야 할 추가 제외 사항은 다음과 같습니다.

- Visual Studio 개발 환경 프로세스:`devenv.exe`
- Visual Studio 빌드 프로세스:`msbuild.exe`
- JetBrains 디렉터리:`%LOCALAPPDATA%\JetBrains\<Transient directory (folder)>`

그룹 정책 제어 환경의 디렉터리 위치를 사용자 지정 하는 방법을 비롯 하 여 바이러스 백신 검색 제외를 추가 하는 방법에 대 한 자세한 내용은 Android Studio 설명서의 [바이러스 백신 영향](https://developer.android.com/studio/intro/studio-config#antivirus-impact) 섹션을 참조 하십시오.

> [!Note]
> Daniel Knoodle는 [Visual Studio 2017에 대 한 Windows Defender 제외](https://gist.github.com/dknoodle/5a66b8b8a3f2243f4ca5c855b323cb7b#file-windows-defender-exclusions-vs-2017-ps1-L10)를 추가 하는 권장 스크립트가 포함 된 GitHub 리포지토리를 설정 했습니다.

## <a name="additional-resources"></a>추가 리소스

- [Android 용 이중 화면 앱 개발 및 Surface 듀오 장치 SDK 다운로드](https://docs.microsoft.com/dual-screen/android/)

- [Windows Defender 제외를 추가 하 여 성능 향상](./defender-settings.md)

- [에뮬레이터 성능을 향상 시키기 위해 가상화 지원 사용](./emulator.md#enable-virtualization-support)
