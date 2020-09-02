---
title: Xbox One에서 UWP 앱 개발 시작
description: Xbox One에서 UWP (유니버설 Windows 플랫폼) 앱 개발을 시작 하도록 개발 PC와 Xbox one 콘솔을 설정 하는 방법에 대해 알아봅니다.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 243f8972f1ea5879ee77f036d97c05e29d446b12
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304705"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Xbox One에서 UWP 앱 개발 시작

이러한 단계를 **신중** 하 게 수행 하 여 유니버설 WINDOWS 플랫폼 (UWP) 개발을 위한 PC 및 Xbox one을 성공적으로 설정 합니다. 설정을 확인 한 후에는 xbox one의 개발자 모드에 대해 자세히 알아보고 [Xbox one 용 uwp](index.md) 페이지에서 uwp 앱을 빌드할 수 있습니다. 

## <a name="before-you-start"></a>시작하기 전에

시작 하기 전에 다음을 수행 해야 합니다.
-   최신 버전의 Windows 10을 사용 하 여 PC를 설정 합니다.
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2019.

    > [!NOTE]
    > Visual Studio 2019 is required if you are using the Windows 10, build 15063 SDK. -->

- Xbox One 콘솔에 5gb 이상의 사용 가능한 공간이 있어야 합니다.

## <a name="setting-up-your-development-pc"></a>개발 PC 설정

1.  Visual Studio 2015 업데이트 3, Visual Studio 2017 또는 Visual Studio 2019를 설치 합니다.

    Visual Studio 2015 업데이트 3을 설치 하는 경우 **사용자 지정** 설치를 선택 하 고 **유니버설 Windows 앱 개발 도구** 확인란을 선택 했는지 확인 합니다. 기본 설치의 일부가 아닙니다. C + + 개발자 인 경우 **사용자 지정 설치** 를 선택 하 고 **c + +** 를 선택 합니다.

    Visual Studio 2017 또는 Visual Studio 2019을 설치 하는 경우 **유니버설 Windows 플랫폼 개발** 워크 로드를 선택 해야 합니다. C + + 개발자 인 경우 오른쪽의 **요약** 창에 있는 **유니버설 Windows 플랫폼 개발**에서 **c + + 유니버설 Windows 플랫폼 도구** 확인란을 선택 했는지 확인 합니다. 기본 설치의 일부가 아닙니다.

    자세한 내용은 [Xbox 개발 환경에서 UWP 설정](development-environment-setup.md)을 참조 하세요.

2.  최신 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)를 설치 합니다.

3.  개발 PC에 개발자 모드를 사용 하도록 설정 합니다 (**설정/업데이트 & 보안/개발자 기능/개발자 모드 사용**).

이제 개발 PC가 준비 되었으므로이 비디오를 시청 하거나 계속 해 서 개발을 위해 Xbox One을 설정 하는 방법을 확인 하 고 UWP 앱을 만들어 배포할 수 있습니다.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Xbox one 콘솔 설정

1.  Xbox One에서 개발자 모드를 활성화 합니다. 앱을 다운로드 하 고 활성화 코드를 가져온 다음 파트너 센터 앱 개발자 계정의 **Xbox one 콘솔 관리** 페이지에 입력 합니다. 자세한 내용은 [Xbox One Developer 모드 정품 인증](devkit-activation.md)을 참조 하세요. 

2.  **개발 모드 정품 인증** 앱을 열고 **전환 및 다시 시작**을 선택 합니다. 축 하 합니다. 이제 개발자 모드에서 Xbox One을 만들었습니다.
  
  > [!NOTE]
  > 소매 게임과 앱은 개발자 모드에서 실행 되지 않지만 사용자가 만든 앱 또는 게임은 실행 됩니다. 자주 사용 하는 게임 및 앱을 실행 하려면 일반 정품 모드로 전환 합니다.
    
  > [!NOTE]
  > 개발자 모드에서 Xbox One에 앱을 배포 하려면 먼저 사용자가 콘솔에 로그인 해야 합니다. 기존 Xbox Live 계정을 사용 하거나 개발자 모드에서 콘솔에 대 한 새 계정을 만들 수 있습니다. 

## <a name="creating-your-first-project-in-visual-studio"></a>Visual Studio에서 첫 번째 프로젝트 만들기

자세한 내용은 [Xbox 개발 환경에서 UWP 설정](development-environment-setup.md)을 참조 하세요.

1.  **C #의 경우**: 새 유니버설 Windows 프로젝트를 만들고 **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다. **디버그** 탭을 선택 하 고 **대상 장치** 를 **원격 컴퓨터로**변경 하 고, **원격 컴퓨터** 필드에 Xbox one 콘솔의 IP 주소 또는 호스트 이름을 입력 하 고, **인증 모드** 드롭다운 목록에서 **유니버설 (암호화 되지 않은 프로토콜)** 을 선택 합니다.   

    콘솔 (홈의 오른쪽에 있는 큰 타일)에서 Dev Home을 시작 하 고 왼쪽 위 모서리를 살펴보면 Xbox One IP 주소를 찾을 수 있습니다. 개발자 홈에 대 한 자세한 내용은 [Xbox one 도구 소개](introduction-to-xbox-tools.md)를 참조 하세요.  

2.  **C + + 및 HTML/Javascript 프로젝트의 경우**: c # 프로젝트와 비슷한 경로를 따르고, 프로젝트 속성에서 **디버깅** 탭으로 이동 하 고, 디버거의 **원격 컴퓨터** 를 선택 하 여 드롭다운 목록을 열고, **컴퓨터 이름** 필드에 콘솔의 IP 주소 또는 호스트 이름을 입력 하 고, **인증 유형** 필드에서 **유니버설 (암호화 되지 않은 프로토콜)** 을 선택 합니다.

3. 상단 메뉴 모음에서 녹색 재생 단추 왼쪽의 드롭다운에서 **x64** 를 선택 합니다.
   
4.  F5 키를 누르면 앱이 작성 되 고 Xbox One에 배포 되기 시작 합니다.
  
5.  처음으로이 작업을 수행 하면 Visual Studio에서 Xbox One에 대 한 PIN을 입력 하 라는 메시지를 표시 합니다. Xbox One에서 Dev Home을 시작 하 고 **Visual Studio Pin 표시** 단추를 선택 하 여 pin을 가져올 수 있습니다.
  
6.  페어링된 후에 앱이 배포 되기 시작 합니다. 처음으로이 작업을 수행 하는 데 약간의 시간이 걸릴 수 있습니다 (모든 도구를 Xbox로 복사 해야 하지만 몇 분 이상 소요 되는 경우 문제가 발생할 수 있음). 위의 모든 단계를 수행 했는지 확인 합니다. 특히 **인증 모드** 를 **유니버설**로 설정 했 고 Xbox one에 대 한 유선 네트워크 연결을 사용 하 고 있는지 확인 합니다.  

7. 가만히 둡니다. 콘솔에서 실행 되는 첫 번째 앱을 사용해 보세요.  

## <a name="thats-it"></a>간단하죠.

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>참고 항목  
- [자주 묻는 질문](frequently-asked-questions.md)  
- [Xbox 개발자 프로그램에서 UWP에 대해 알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md) 
