---
title: Xbox 개발 환경에서 UWP 설정
description: 로컬 네트워크를 통해 Xbox One 콘솔에 연결 된 개발 PC로 구성 된 Xbox 개발 환경에서 UWP를 설정 하 고 테스트 하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
ms.localizationpriority: medium
ms.openlocfilehash: 408568e38bd9147564e7ebece17466e22364f3cc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164187"
---
# <a name="set-up-your-uwp-on-xbox-development-environment"></a>Xbox 개발 환경에서 UWP 설정

Xbox 개발 환경의 UWP (유니버설 Windows 플랫폼)는 로컬 네트워크를 통해 Xbox One 콘솔에 연결 된 개발 PC로 구성 됩니다.
개발 PC에는 Visual Studio 2015 업데이트 3, Visual Studio 2017 또는 Visual Studio 2019이 필요 합니다.
또한 개발 PC에는 Windows 10, Windows 10 SDK 빌드 14393 이상 및 지원 도구 범위가 필요 합니다.

이 문서에서는 개발 환경을 설정 하 고 테스트 하는 단계를 설명 합니다.

## <a name="visual-studio-setup"></a>Visual Studio 설치

1. Visual Studio 2015 업데이트 3, Visual Studio 2017 또는 Visual Studio 2019를 설치 합니다. 자세한 내용 및 설치에 대 한 자세한 내용은 [Windows 10 용 도구 다운로드](https://dev.windows.com/downloads)를 참조 하세요. 최신 버전의 Visual Studio를 사용 하 여 개발자와 보안에 대 한 최신 업데이트를 받을 수 있도록 하는 것이 좋습니다.


2. Visual Studio 2017 또는 Visual Studio 2019을 설치 하는 경우 **유니버설 Windows 플랫폼 개발** 워크 로드를 선택 해야 합니다. C + + 개발자 인 경우 **유니버설 Windows 플랫폼 개발**의 오른쪽에 있는 **요약** 창에서 **c + + 유니버설 Windows 플랫폼 도구** 확인란을 선택 해야 합니다. 기본 설치의 일부가 아닙니다.

    ![Visual Studio 2019 설치](images/development-environment-setup-1.png)

    Visual Studio 2015 업데이트 3을 설치 하는 경우 **유니버설 Windows 앱 개발 도구** 확인란이 선택 되어 있는지 확인 합니다.

    ![Visual Studio 2015 업데이트 2 설치](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Windows 10 SDK 설치

최신 Windows 10 SDK를 설치 합니다. Visual Studio 설치와 함께 제공 되지만 별도로 다운로드 하려는 경우 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)를 참조 하세요.


## <a name="enabling-developer-mode"></a>개발자 모드 사용

개발 PC에서 앱을 배포 하려면 먼저 개발자 모드를 사용 하도록 설정 해야 합니다. **설정** 앱에서 개발자를 위한 **업데이트 & 보안**으로 이동 하  /  **For developers**고 **개발자 기능 사용**에서 **개발자 모드**를 선택 합니다.

## <a name="setting-up-your-xbox-one"></a>Xbox One 설정

Xbox One에 앱을 배포 하려면 먼저 사용자가 콘솔에 로그인 되어 있어야 합니다. 기존 Xbox Live 계정을 사용 하거나 개발자 모드에서 콘솔에 대 한 새 계정을 만들 수 있습니다. 

## <a name="create-your-first-app"></a>첫 번째 앱 만들기

1. 개발 PC가 대상 Xbox One 콘솔과 동일한 로컬 네트워크에 있는지 확인 합니다. 즉, 일반적으로 동일한 라우터를 사용 하 고 동일한 서브넷에 있어야 합니다. 유선 네트워크에 연결 하는 것이 좋습니다.

2. Xbox One 콘솔이 개발자 모드 인지 확인 합니다.  자세한 내용은 [Xbox One Developer 모드 정품 인증](devkit-activation.md)을 참조 하세요.

3. UWP 앱에 사용할 프로그래밍 언어를 결정 합니다.

4. 개발 PC의 Visual Studio에서 **새로 만들기/프로젝트**를 선택 합니다.

5. **새 프로젝트** 창에서 **Windows 유니버설/비어 있는 앱 (유니버설 windows)** 을 선택 합니다.

### <a name="starting-a-c-project"></a>C # 프로젝트 시작

  ![새 프로젝트 대화 상자](images/development-environment-setup-2.png)

1. **새 유니버설 Windows 프로젝트** 대화 상자의 **최소 버전** 드롭다운에서 빌드 14393 이상을 선택 합니다. **대상 버전** 드롭다운에서 최신 SDK를 선택 합니다. **개발자 모드** 대화 상자가 표시 되 면 **확인**을 클릭 합니다. 비어 있는 새 앱이 만들어집니다.

2. 원격 디버깅을 위해 개발 환경 구성:

    a. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 **속성**을 선택 합니다.

    b. **디버그** 탭에서 **플랫폼** 을 x **64**로 변경 합니다. (x86은 더 이상 Xbox에서 지원 되는 플랫폼이 아닙니다.)

    c. **시작 옵션**에서 **대상 장치** 를 **원격 컴퓨터**로 변경 합니다.

    d. **원격 컴퓨터**에서 Xbox one 콘솔의 시스템 IP 주소 또는 호스트 이름을 입력 합니다. IP 주소 또는 호스트 이름을 가져오는 방법에 대 한 자세한 내용은 [Xbox one 도구 소개](introduction-to-xbox-tools.md)를 참조 하세요.

    e. **인증 모드** 드롭다운 목록에서 **유니버설 (암호화 되지 않은 프로토콜)** 을 선택 합니다.

    ![C # BlankApp 속성 페이지](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>C + + 프로젝트 시작

  ![C + + 프로젝트](images/development-environment-setup-3.png)

1. **새 유니버설 Windows 프로젝트** 대화 상자의 **최소 버전** 드롭다운에서 빌드 14393 이상을 선택 합니다. **대상 버전** 드롭다운에서 최신 SDK를 선택 합니다. **개발자 모드** 대화 상자가 표시 되 면 **확인**을 클릭 합니다. 비어 있는 새 앱이 만들어집니다.

2. 원격 디버깅을 위해 개발 환경 구성:

   a. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 **속성**을 선택 합니다.

   b. **디버깅** 탭에서 디버거를 **원격 컴퓨터** **시작으로** 변경 합니다.

   c. **컴퓨터 이름**에 Xbox one 콘솔의 시스템 IP 주소 또는 호스트 이름을 입력 합니다. IP 주소 또는 호스트 이름을 가져오는 방법에 대 한 자세한 내용은 [Xbox one 도구 소개](introduction-to-xbox-tools.md)를 참조 하세요.

   d. **인증 유형** 드롭다운 목록에서 **유니버설 (암호화 되지 않은 프로토콜)** 을 선택 합니다.

   e. **플랫폼** 드롭다운에서 **x64**를 선택 합니다.

    ![C + + BlankApp 속성 페이지](images/development-environment-setup-4.png)

### <a name="pin-pair-your-device-with-visual-studio"></a>Visual Studio를 사용 하 여 장치를 고정 합니다.

1. 설정을 저장 하 고 Xbox One 콘솔이 개발자 모드 인지 확인 합니다.

2. Visual Studio에서 프로젝트를 열고 F5 키를 누릅니다.

3. 첫 번째 배포 인 경우 장치를 고정 쌍으로 연결 하 라는 대화 상자가 표시 됩니다.

    a. PIN을 얻으려면 Xbox One 콘솔의 홈 화면에서 **개발자 홈** 을 엽니다.

    b. **홈** 탭의 **빠른 작업**에서 **Visual Studio pin 표시**를 선택 합니다.
  
    ![Visual Studio 대화 상자와 페어링](images/development-environment-setup-5.png)

    c. **Visual Studio를 사용 하 여 페어링** 대화 상자에 사용자의 PIN을 입력 합니다. 다음 PIN은 예제 일 뿐입니다. 그는 다릅니다.

    ![Visual Studio PIN 대화 상자와 페어링](images/devhome_pin.png)

    d. 배포 오류 (있는 경우)는 **출력** 창에 표시 됩니다.

축 하 합니다. Xbox에 첫 번째 UWP 앱을 만들고 배포 했습니다.

## <a name="see-also"></a>참고 항목
- [Xbox One 개발자 모드 활성화](devkit-activation.md)  
- [Windows 10 용 다운로드 및 도구](https://developer.microsoft.com/windows/downloads)  
- [Windows 참가자 프로그램](https://insider.windows.com/)  
- [Xbox One 도구 소개](introduction-to-xbox-tools.md) 
- [Xbox One의 UWP](index.md)