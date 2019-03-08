---
title: Xbox Live Fiddler를 사용 하 여 문제 해결
description: Fiddler를 사용 하 여 Xbox Live 서비스 호출 문제를 해결 하는 방법을 알아봅니다.
ms.assetid: 7d76e444-027b-4659-80d5-5b2bf56d199e
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, fiddler, 서비스 호출, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: 84c6717a4f9f5aff9fd3ff1f68c870fdd9174865
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606718"
---
# <a name="troubleshooting-xbox-live-using-fiddler"></a>Xbox Live Fiddler를 사용 하 여 문제 해결

Fiddler는 웹 디버깅 프록시 장치와 인터넷 간의 모든 HTTP 및 HTTPS 트래픽을 기록 하는 경우 로그인을 신뢰 당사자 웹 서비스를 이해 하 고 웹 서비스 호출을 디버깅 하 고 Xbox Live 서비스에서 트래픽을 검사 하 고 사용 합니다.

## <a name="for-windows-uwp-pc-apps"></a>Windows PC UWP 앱 용

1. PC의 관리자 그룹에 현재 사용자 인지 확인
1. Fiddler에서 다운로드 [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)
1. "기본 제공 되는.NET 4에 대 한" 버전을 선택 했는지 확인
1. 설치 되 면 이동 도구-> Fiddler 옵션 및 HTTPS 연결 캡처 및 암호를 해독 하는 HTTPS 트래픽을 사용 하도록 설정 합니다.  런타임 및 Xbox LIVE 서비스 간의 모든 통신은 SSL을 사용 하 여 암호화 됩니다.  이 옵션이 없으면 유용한 내용은 전혀 표시 되지 않습니다.  Fiddler 팝업 해야 수 5 대화 상자 등 UAC 모든 대화를 수락 합니다.
1. "WinConfig", "제외 모두" 및 "변경 내용 저장"로 이동 합니다.  그렇지 않은 경우 스토어 앱을 사용 하 여 Fiddler 작동 하지 않습니다.
1. 이제 앱을 실행 하는 경우에 앱, 런타임 및 Xbox LIVE 간에 요청/응답을 표시 됩니다.

일반적으로 필요 하지 않습니다 하지만 수 UWP OS 수준 호출을 디버깅 하려면 로그인 하 고 게임 내 이벤트를 디버깅할 때 유용 해야 WinHTTP 트래픽을 캡처하도록 Fiddler를 구성 합니다.
이 사용 하 여 수행할 수 있습니다.
```cpp
    netsh winhttp set proxy 127.0.0.1:8888 "<-loopback>"
```
포트 8888 Fiddlers 도구에 구성한 포트와 일치 하는 경우 | 옵션 | 기본적으로 8888 연결 탭입니다.
이 취소 하려면 다음을 수행 합니다.
```cpp
    netsh winhttp reset proxy
```

## <a name="for-xbox-one-uwp-based-projects"></a>Xbox One UWP 프로젝트를 기반합니다.

여기에 나오는 단계를 수행 합니다. [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler)

## <a name="for-xbox-one-xdk-based-projects"></a>Xbox One XDK 기반 프로젝트에 대 한

정상 작동 시, 프록시를 통해 통신하는 콘솔은 해당 통신이 프록시에 의해 수정되어 플레이어가 치트키를 사용할 수 있는 위험에 처합니다. 따라서 콘솔은 프록시를 통해 통신을 사용할 수 없도록 설계 되었습니다. Xbox One 개발 키트에서 Fiddler를 사용하려면 개발 키트에서 Fiddler 프록시 사용을 허용하기 위해 몇 가지 특별한 구성 단계를 수행해야 합니다.

Fiddler는 프리웨어이며, [Fiddler 웹 사이트](https://www.telerik.com/fiddler/)에서 다운로드할 수 있습니다.

Fiddler는 콘솔에서 보고하는 네트워크 상태에 영향을 줄 수 있습니다. Fiddler를 실행하는 컴퓨터에서 업스트림 연결이 사용되지 않는 경우 콘솔의 인증이 만료될 때까지 콘솔이 연결 끊김을 검색하지 못할 수 있습니다. Fiddler를 사용하는 경우 Fiddler를 사용하여 연결 끊김을 시뮬레이션하는 대신 콘솔과 Fiddler를 실행하는 컴퓨터 간의 연결을 끊어야 합니다. 여전히 향상, 네트워크 스트레스를 사용 하 여 도구에 따라서 테스트용 연결 끊기를 기반으로 시뮬레이션 합니다.

설치 하 고 개발 PC에서 Fiddler를 사용 하도록 설정

설치 하 고 개발 키트에서 트래픽을 모니터링 하도록 Fiddler를 사용 하도록 설정 하려면 다음이 단계를 따릅니다.

1. [Fiddler 웹 사이트](https://www.telerik.com/fiddler/)의 지침에 따라 개발 PC에 Fiddler를 설치합니다.
1. Fiddler를 시작 하 고 도구 메뉴에서 Fiddler 옵션을 선택 합니다.
1. 연결 탭을 선택 하 고 확인란을 연결할 허용 원격 컴퓨터 선택 되어 있는지 확인 합니다.
1. 설정에 변경 내용을 적용 하려면 확인을 클릭 합니다. 변경 내용을 적용하려면 Fiddler를 다시 시작해야 하며 방화벽을 수동으로 구성해야 할 수 있다는 내용의 대화 상자가 표시됩니다. 이 대화 상자에서 확인을 클릭 합니다. 하지만 아직 Fiddler 시작 안 함.
1. 원격 컴퓨터에서 연결할 수 있도록 필요한 방화벽 규칙을 구성합니다. Windows 방화벽 제어판 애플릿을 시작합니다. 고급 설정을 클릭 한 다음 인바운드 규칙입니다. "FiddlerProxy" 라는 규칙을 찾고 다음 설정 중 각 규칙에 대 한 표시 되는지 확인 하 고 오른쪽으로 스크롤합니다.

| 설정          | 기본 설정 값                |
|------------------|--------------------------------|
| 이름             | FiddlerProxy                   |
| 그룹            | (값 설정 하지 마십시오는 그룹에 대 한) |
| 프로필          | 모두                            |
| Enabled          | 예                            |
| 작업           | 허용                          |
| 재정의         | 아니오                             |
| 프로그램          | fiddler.exe 경로            |
| LocalAddress     | 임의                            |
| RemoteAddress    | 임의                            |
| 프로토콜         | TCP                            |
| LocalPort        | 임의                            |
| RemotePort       | 임의                            |
| AllowedUsers     | 임의                            |
| AllowedComputers | 임의                            |


1. 캡처 및 HTTPS 트래픽을 해독 하도록 Fiddler를 구성 합니다.
    1. 최상의 성능을 사용 하려면 Fiddler 단추 표시줄에서 Stream 단추를 클릭 하 여 스트리밍 모드를 사용 하도록 설정 합니다.
    1. Fiddler에서 도구를 차례로 Fiddler 옵션 HTTPS를 선택 합니다.
    1. 암호를 해독 하는 HTTPS 트래픽을 확인란 합니다. Messagebox CA 인증서를 신뢰를 클릭 하는 Windows를 구성할 지 여부를 요청 하는 경우 아니요
    1. 내보내기 루트 인증서 바탕 화면 단추를 클릭 합니다.
1. Fiddler를 종료 하 고 다시 시작 하십시오.

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>Fiddler를 인터넷의 프록시로 사용하도록 개발 키트를 구성하려면
이전 릴리스에서 사용 되는 방법에서 개발자 키트에서 fiddler 구성을 간소화 되었습니다.

1. 복사 하는 데스크톱, 개발 키트에서 내보낸 루트 인증서 Fiddler``` xs:\Microsoft\Cert\FiddlerRoot.cer```합니다.  다음 명령을 사용할 수 있습니다.  ```xbcp [local Fiddler Root directory]\FiddlerRoot.cer xs:\Microsoft\Cert\FiddlerRoot.cer```
1. 라는 텍스트 파일을 만듭니다 ```ProxyAddress.txt```, IP 주소 또는 개발 PC의 호스트 이름을 사용 하 여 파일의 유일한 콘텐츠로 Fiddler 수신 하는 Fiddler 및 포트 번호를 실행 합니다. 이름/i P 주소와 포트 형식을 다음과 같이 지정 합니다. "호스트: 포트"입니다. (기본적으로 Fiddler 사용 포트 8888) 예를 들어, "10.124.220.250:8888" 또는 "my_dev_pc.contoso.com:8888". 개발 키트 xs:\Microsoft\Fiddler\ProxyAddress.txt으로이 파일을 복사 합니다.  다음 명령을 사용할 수 있습니다.  ```xbcp [local Proxy Address file directory]\ProxyAddress.txt xs:\Microsoft\Fiddler\ProxyAddress.txt```
1. 입력 하 여 dev 키트를 다시 부팅 ```xbreboot``` 명령 프롬프트에

### <a name="to-stop-using-fiddler"></a>Fiddler 사용을 중지하려면

Fiddler를 사용 하 여 인터넷에 대 한 프록시 및 따라서 모든 개발 키트의 네트워크 트래픽이 Fiddler에 추적을 중지 하려면 단순히 이름 바꾸기 또는 dev 키트에서 FiddlerRoot.cer 파일 삭제 하 고 dev 키트를 다시 부팅 합니다.

### <a name="how-it-works"></a>작동 방식

Xs:\Microsoft\Cert\FiddlerRoot.cer 파일과 부팅 시 xs:\Microsoft\Fiddler\ProxyAddress.txt 파일의 존재 하면 ProxyAddress.txt에 지정 된 프록시 서버를 사용 하도록 자체적으로 구성 하는 개발 키트. 두 파일이 없는 경우 다음 dev 키트 구성 하지 않을 예정 Fiddler 프록시를 통해 작동 하도록 자체입니다.

Fiddler가 설치된 각 PC마다 다른 Fiddler 루트 인증서를 사용합니다. 각 PC의 Fiddler 루트 인증서 FiddlerRoot.cer 이름은 둘 중으로 둘 이상의 PC 개발 키트에 대 한 Fiddler 프록시를 제공 하려면 사용할 수 있는 경우 xs:\Microsoft\Cert 디렉터리로 복사할 수 있습니다. 모든 인증서 디렉터리에 있는 인증서에 Fiddler 프록시를 인증 하는 경우 확인 됩니다. 인증서 디렉터리에서 해당 인증서가 있는지와 Fiddler 인스턴스에서 어떤 PC의 주소는 ProxyAddress.txt 프록시로 사용 됩니다.
