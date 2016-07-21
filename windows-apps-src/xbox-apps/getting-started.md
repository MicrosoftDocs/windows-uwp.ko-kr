---
author: Mtoepke
title: "Xbox One에서 UWP 앱 개발 시작"
description: "UWP 개발에 대해 PC 및 Xbox One을 설정하는 방법"
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 5f050eee9430dc7aaa2738a4610a2c4f5083839d
ms.openlocfilehash: 92a9cc54c6257c35b1e7ae19838b01c8452c4b36

---

#Xbox One에서 UWP 앱 개발 시작

UWP 개발에 대해 PC 및 Xbox One을 성공적으로 설정하려면 **신중하게** 이러한 단계를 따르세요. 설정을 마친 후 [Xbox One용 UWP](index.md) 페이지에서 Xbox One의 개발자 모드 및 UWP 앱 빌드에 대해 자세히 알아볼 수 있습니다. 

## 시작하기 전에
시작하기 전에 다음을 수행해야 합니다.
-   [Windows 개발자 센터](https://dev.windows.com) 계정을 만듭니다.
-   [Windows 참가자 프로그램](https://insider.windows.com/)에 참여합니다. Windows SDK Preview를 다운로드하려면 참여해야 합니다.
-   Windows 10 PC를 설정합니다(최신 Windows 10 참가자 플라이트를 포함하는 모든 버전을 사용할 수 있음). 이 Preview에서는 개발 도구를 사용하기 위해 Windows 10을 실행해야 합니다. 
-   Xbox One 본체를 네트워크에 연결합니다. 최상의 성능을 위해 유선 연결을 사용합니다.
- Xbox One 본체에 사용 가능한 공간이 5GB 이상 있어야 합니다.

## 개발 PC 설정
1.  Visual Studio 2015 업데이트 2를 설치합니다. **사용자 지정** 설치를 선택한 다음 **유니버설 Windows 앱 개발 도구** 확인란을 선택해야 합니다. 이 도구는 기본 설치에 포함되지 않습니다. 자세한 내용은 [개발 환경 설정](development-environment-setup.md)을 참조하세요(또한 C++ 개발자인 경우 사용자 지정 설치를 선택한 다음 C++도 선택해야 함).

2.  최신 Windows 10 SDK Preview 빌드를 설치합니다. 이 빌드는 [Windows 참가자 프로그램](http://go.microsoft.com/fwlink/p/?LinkId=780552)에서 다운로드할 수 있습니다.
  
  > **중요**&nbsp;&nbsp;이 Preview SDK를 PC에 설치하면 이 PC에서 빌드된 앱을 스토어에 제출할 수 없으므로 프로덕션 개발 PC에는 설치하지 마세요. 

## Xbox One 본체 설정
1.  Xbox One에서 개발자 모드를 활성화합니다. 앱을 다운로드하고, 활성화 코드를 가져온 다음 개발자 센터 계정의 xboxactivate 페이지에 입력합니다. 자세한 내용은 [Xbox One에서 개발자 모드 사용](devkit-activation.md)을 참조하세요. 

2.  Developer Preview를 실행하도록 Xbox One이 시스템 업데이트를 다운로드할 때까지 기다립니다. 이 작업은 최대 4시간이 걸릴 수 있습니다. 기다리지 않으려면 전원 단추를 10초 동안 길게 눌렀다가 다시 켜서 콘솔을 "하드" 재부팅합니다. 업데이트가 트리거됩니다.  

3.  개발자 모드 활성화 앱으로 이동한 다음 **전환 후 다시 시작**을 선택합니다. 축하합니다. 이제 개발자 모드의 Xbox One을 사용할 수 있습니다.
  
  > **참고**&nbsp;&nbsp;정품 게임과 앱은 개발자 모드에서 실행되지 않지만 직접 만든 앱이나 게임은 실행됩니다. 좋아하는 게임과 앱을 실행하려면 다시 정품 모드로 전환합니다.
  
  > **참고**&nbsp;&nbsp;개발자 모드에서 Xbox One에 앱을 배포하려면 먼저 사용자가 본체에 로그인되어 있어야 합니다. 기존 Xbox Live 계정을 사용하거나, 개발자 모드에서 콘솔용 계정을 새로 만들 수 있습니다. 

## Visual Studio 2015에서 첫 번째 프로젝트 만들기

자세한 내용은 [개발 환경 설정](development-environment-setup.md)을 참조하세요.

1.  C#: 새 유니버설 Windows 만들기 프로젝트의 경우 프로젝트 속성으로 이동한 다음 **디버그** 탭을 선택합니다. **대상 디바이스**를 **원격 컴퓨터**로 변경하고 **원격 컴퓨터** 필드에 Xbox One의 IP 주소 또는 호스트 이름을 입력한 다음 **인증 모드** 드롭다운 목록에서 **유니버설(암호화되지 않은 프로토콜)**을 선택합니다.   

    본체에서 개발자 홈(홈 오른쪽에 있는 큰 타일)을 시작하고 왼쪽 위 모서리를 보면 Xbox One IP 주소를 찾을 수 있습니다. 개발자 홈에 대한 자세한 내용은 [Xbox One 도구 소개](introduction-to-xbox-tools.md)를 참조하세요.  

2.  C++ 및 HTML/Javascript 프로젝트의 경우 유사한 경로를 따르되 프로젝트 속성에서 **디버깅** 탭으로 이동합니다. 디버거에서 **원격 컴퓨터**를 선택하여 드롭다운 목록을 열고 **컴퓨터 이름** 필드에 콘솔의 IP 주소 또는 호스트 이름을 입력한 다음 **인증 유형** 필드에서 **유니버설(암호화되지 않은 프로토콜)**을 선택합니다.
   
3.  F5 키를 누르면 앱이 빌드되고 Xbox One에서 배포를 시작합니다.
  
4.  이 작업을 처음 수행할 때는 Visual Studio에서 Xbox One의 PIN을 입력하라는 메시지를 표시합니다. Xbox One에서 개발자 홈을 시작하고 **Visual Studio와 연결** 단추를 누르면 PIN을 가져올 수 있습니다.
  
5.  연결한 후 앱 배포가 시작됩니다. 이 작업을 처음 수행할 때는 속도가 약간 느릴 수 있지만(모든 도구를 Xbox에 복사해야 함), 몇 분 이상 걸리는 경우 문제가 있는 것입니다. 위의 단계를 모두 따랐는지 확인하고(특히 **인증 모드**를 **유니버설**로 설정했나요?), Xbox One에 대해 유선 네트워크 연결을 사용 중인지 확인합니다.  

6. 잠시 기다려 주세요. 첫 번째 앱이 콘솔에서 실행되는 것을 즐기세요.  

## 정말 간단하죠!

![Hello World](images/getting-started-hello-world.png)

## 참고 항목  
- [FAQ](frequently-asked-questions.md)  
- [알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)



<!--HONumber=Jul16_HO2-->


