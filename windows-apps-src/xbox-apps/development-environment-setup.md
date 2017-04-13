---
author: Mtoepke
title: "Xbox 개발 환경에서의 UWP 설정"
description: "Xbox 개발 환경에서의 UWP 설정 및 테스트 단계"
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
ms.openlocfilehash: dcceab233f19696f11075fdc4e46fe4a3aa7e5b9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-up-your-uwp-on-xbox-development-environment"></a>Xbox 개발 환경에서의 UWP 설정

Xbox 개발 환경에서 UWP(유니버설 Windows 플랫폼)는 로컬 네트워크를 통해 Xbox One 본체에 연결된 개발 PC로 구성됩니다. 
개발 PC에는 Windows 10, Visual Studio 2015 업데이트 2, Windows 10 SDK Preview 빌드 14295, 그리고 다양한 지원 도구가 필요합니다.


이 문서에서는 개발 환경의 설정 및 테스트 단계를 설명합니다.

## <a name="visual-studio-setup"></a>Visual Studio 설치

1. Visual Studio 2015 업데이트 2 이상을 설치합니다. 설치에 대한 자세한 내용은 [Windows 10용 다운로드 및 도구](https://dev.windows.com/downloads)를 참조하세요.

1. Visual Studio 2015 업데이트 2를 설치할 때 **유니버설 Windows 앱 개발 도구** 확인란이 선택되었는지 확인합니다.

  ![Visual Studio 2015 업데이트 2 설치](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Windows 10 SDK 설치

최신 Windows 10 SDK Preview 빌드를 설치합니다. 설치 정보는 [개발자용 Insider Preview 업데이트 다운로드](http://go.microsoft.com/fwlink/p/?LinkId=780552)를 참조하세요.

> [!IMPORTANT]
> 최신 SDK를 설치해야 하지만 운영 체제는 최신 Windows Insider Preview 버전을 설치하지 _않아도_ 됩니다.

## <a name="enabling-developer-mode"></a>개발자 모드 사용

개발 PC에서 응용 프로그램을 배포할 수 있으려면 먼저 Windows 메뉴(설정 / 업데이트 및 보안 / 개발자용 / 개발자 모드)를 통해 개발자 모드를 사용하도록 설정해야 합니다.

## <a name="setting-up-your-xbox-one"></a>Xbox One 설정

Xbox One에 앱을 배포하려면 먼저 사용자가 본체에 로그인되어 있어야 합니다. 기존 Xbox Live 계정을 사용하거나, 개발자 모드에서 콘솔용 계정을 새로 만들 수 있습니다. 

## <a name="create-your-first-application"></a>첫 번째 응용 프로그램 만들기

1. 개발 PC가 대상 Xbox One 본체와 동일한 로컬 네트워크에 있는지 확인합니다. 일반적으로 이는 동일한 라우터를 사용하고 동일한 서브넷에 있어야 함을 의미합니다. 유선 네트워크 연결을 사용하는 것이 좋습니다.

1. Xbox One 본체가 개발자 모드에 있는지 확인합니다.  자세한 내용은 [Xbox One에서 개발자 모드 활성화](devkit-activation.md)를 참조하세요.

1. UWP 앱에 사용할 프로그래밍 언어를 결정합니다.

1. 개발 PC에서 **새 프로젝트**를 선택한 다음 **Windows/유니버설/비어 있는 앱**을 선택합니다.

### <a name="starting-a-c-project"></a>C# 프로젝트 시작

  ![새 프로젝트 대화 상자](images/vs_universal_blank.jpg)

1. **새 유니버설 Windows 프로젝트** 대화 상자에서 기본 옵션을 선택합니다. **개발자 모드** 대화 상자가 나타날 경우 **확인**을 클릭합니다. 새 비어 있는 앱을 만듭니다.

1. 원격 디버깅을 위한 개발 환경을 구성합니다.

  1. 프로젝트를 마우스 오른쪽 단추로 클릭한 다음 **속성**을 선택합니다.
  1. **디버그** 탭에서 **플랫폼**을 **활성(x64)**으로 변경합니다. (x86은 이제 Xbox에서 지원되는 플랫폼이 아닙니다.)   
  1. **대상 디바이스**를 **원격 컴퓨터**로 변경합니다.
  1. **원격 컴퓨터**에 시스템 IP 주소 또는 Xbox One 본체의 호스트 이름을 입력합니다. IP 주소 또는 호스트 이름을 얻는 방법에 대한 자세한 내용은 [Xbox One 도구 소개](introduction-to-xbox-tools.md)를 참조하세요.
  1. **인증 모드** 드롭다운 목록에서 **유니버설(암호화되지 않은 프로토콜)**을 선택합니다.

    ![C# BlankApp 속성 페이지](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>C++ 프로젝트 시작

  ![C++ 프로젝트](images/vs_universal_cpp_blank.jpg)

1. **새 유니버설 Windows 프로젝트** 대화 상자에서 기본 옵션을 선택합니다. **개발자 모드** 대화 상자가 나타날 경우 **확인**을 클릭합니다. 새 비어 있는 앱을 만듭니다.

1. 원격 디버깅을 위한 개발 환경을 구성합니다.

   1. 프로젝트를 마우스 오른쪽 단추로 클릭한 다음 **속성**을 선택합니다.
   1. **디버깅** 탭에서 **실행할 디버거**를 **원격 컴퓨터**로 변경합니다.
   1. **컴퓨터의 이름**에 시스템 IP 주소 또는 Xbox One 본체의 호스트 이름을 입력합니다. IP 주소 또는 호스트 이름을 얻는 방법에 대한 자세한 내용은 [Xbox One 도구 소개](introduction-to-xbox-tools.md)를 참조하세요.
   1. **인증 유형** 드롭다운 목록에서 **유니버설(암호화되지 않은 프로토콜)**을 선택합니다.

    ![C++ BlankApp 속성 페이지](images/vs_remote_cpp.jpg)

### <a name="pin-pair-your-device-with-visual-studio"></a>디바이스와 Visual Studio PIN 페어링

1. 설정을 저장하고 Xbox One 본체가 개발자 모드인지 확인합니다.

1. F5 키를 누릅니다.

1. 이번이 첫 번째 배포인 경우 Visual Studio에 디바이스를 PIN 페어링하라는 대화 상자가 나타날 수 있습니다.

  1. PIN을 얻으려면 Xbox One 본체의 홈 화면에서 **개발자 홈**을 엽니다.
  1. **Visual Studio와 페어링**을 선택합니다.

    ![Visual Studio와 페어링 대화 상자](images/devhome_visualstudio.png)

  1. **Visual Studio와 페어링** 대화 상자에 PIN을 입력합니다. 다음 PIN은 예시일 뿐이며, 사용자에 따라 다릅니다.

    ![Visual Studio PIN과 페어링 대화 상자](images/devhome_pin.png)

  1. 배포 오류가 있는 경우 **출력** 창에 나타납니다.

축하합니다. Xbox에서 첫 UWP 앱을 성공적으로 만들고 배포했습니다.



## <a name="see-also"></a>참고 항목
- [Xbox One에서 개발자 모드 사용](devkit-activation.md)  
- [Windows 10용 다운로드 및 도구](https://dev.windows.com/downloads)  
- [개발자용 Insider Preview 업데이트 다운로드](http://go.microsoft.com/fwlink/?LinkId=780552)  
- [Xbox One 도구 소개](introduction-to-xbox-tools.md) 
- [Xbox One의 UWP](index.md)

----
