---
title: UWP용으로 개발하는 경우 Xbox One에서 Fiddler를 사용하는 방법
description: 프리웨어 Fiddler 도구를 사용하여 UWP Xbox One 개발 키트의 네트워크 트래픽을 확인하는 방법을 설명합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 9c133c77-fe9d-4b81-b4b3-462936333aa3
ms.localizationpriority: medium
ms.openlocfilehash: c27891b47bb9f7774799c912cc6f4cae3cea92bc
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8461309"
---
# <a name="how-to-use-fiddler-with-xbox-one-when-developing-for-uwp"></a>UWP용으로 개발하는 경우 Xbox One에서 Fiddler를 사용하는 방법

Fiddler는 Xbox One 개발 키트와 인터넷 간의 모든 HTTP 및 HTTPS 트래픽을 기록하는 웹 디버깅 프록시입니다. Fiddler를 사용하여 Xbox 서비스와 신뢰 당사자 웹 서비스를 들어오고 나가는 트래픽을 기록하고 검사하여 웹 서비스 호출을 파악하고 디버깅합니다. 

정상 작동 시, 프록시를 통해 통신하는 콘솔은 해당 통신이 프록시에 의해 수정되어 플레이어가 치트키를 사용할 수 있는 위험에 처합니다. 따라서 콘솔은 프록시를 통한 통신을 허용하지 않도록 설계되었습니다. Xbox One 개발 키트에서 Fiddler를 사용하려면 개발 키트에서 Fiddler 프록시 사용을 허용하기 위해 몇 가지 특별한 구성 단계를 수행해야 합니다. 

Fiddler는 프리웨어이며, [Fiddler 웹 사이트](http://www.fiddler2.com/fiddler2/)에서 다운로드할 수 있습니다. 

Fiddler는 콘솔에서 보고하는 네트워크 상태에 영향을 줄 수 있습니다. Fiddler를 실행하는 컴퓨터에서 업스트림 연결이 사용되지 않는 경우 콘솔의 인증이 만료될 때까지 콘솔이 연결 끊김을 검색하지 못할 수 있습니다. Fiddler를 사용하는 경우 Fiddler를 사용하여 연결 끊김을 시뮬레이션하는 대신 콘솔과 Fiddler를 실행하는 컴퓨터 간의 연결을 끊어야 합니다.

### <a name="to-install-and-enable-fiddler-on-your-development-pc"></a>개발 PC에서 Fiddler를 설치하여 사용하려면
다음 단계를 따라 Fiddler를 설치하고 사용하여 개발 키트에서 트래픽을 모니터링하세요.

1. [Fiddler 웹 사이트](http://www.fiddler2.com/fiddler2/)의 지침에 따라 개발 PC에 Fiddler를 설치합니다. 
2. Fiddler를 시작하고 **Tools**(도구) 메뉴에서 **Fiddler Options**(Fiddler 옵션)를 선택합니다. 
3. **Connections**(연결) 탭을 선택하고 **Allow remote computers to connect**(원격 컴퓨터 연결 허용)를 선택해야 합니다. 
4. **OK**(확인)를 클릭하여 변경 내용을 적용합니다. 변경 내용을 적용하려면 Fiddler를 다시 시작해야 하며 방화벽을 수동으로 구성해야 할 수 있다는 내용의 대화 상자가 표시됩니다. 이 대화 상자에서 **OK**(확인)를 클릭하지만 *아직 Fiddler를 다시 시작하지는 않습니다*.
5. 원격 컴퓨터에서 연결할 수 있도록 필요한 방화벽 규칙을 구성합니다. Windows 방화벽 제어판 애플릿을 시작합니다. **고급 설정**을 클릭한 다음 **인바운드 규칙**을 클릭합니다. "FiddlerProxy"라는 규칙을 찾고 오른쪽으로 스크롤하여 다음 표의 각 설정이 해당 규칙에 대해 표시되는지 확인합니다.
  
  | 설정           | 기본 설정 값                |
  | ----              | ----                           |
  | 이름              | FiddlerProxy                   |
  | 그룹             | *값 없음* |
  | 프로필           | 모두                            |
  | 설정됨           | 예                            |
  | 액션            | 허용                          |
  | 재정의          | 아니요                             |
  | 프로그램           | *Fiddler.exe 경로*          |
  | LocalAddress      | 임의                            |
  | RemoteAddress     | 임의                            |
  | 프로토콜          | TCP                            |
  | LocalPort         | 임의                            |
  | RemotePort        | 임의                            |
  | AllowedUsers      | 임의                            |
  | AllowedComputers  | 임의                            |


6. 다음을 수행하여 HTTPS 트래픽을 캡처하고 암호 해독하도록 Fiddler를 구성합니다.
  1. 최상의 성능을 위해서는 단추 모음에서 **Stream**(스트림) 단추를 클릭하여 스트리밍 모드를 사용하도록 Fiddler를 설정합니다.
  2. Fiddler **Tools**(도구) 메뉴에서 **Fiddler Options**(Fiddler 옵션)를 클릭한 다음 **HTTPS**를 클릭합니다.
  3. **Decrypt HTTPS traffic**(HTTPS 트래픽 암호 해독) 확인란을 선택합니다. 대화 상자에서 CA 인증서를 신뢰하도록 Windows를 구성할 것인지 여부를 묻는 경우 **No**(아니요)를 클릭합니다.
  4. **Export Root Certificate to Desktop**(데스크톱으로 루트 인증서 내보내기)을 클릭합니다.
7. Fiddler를 종료하고 다시 시작합니다.

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>Fiddler를 인터넷의 프록시로 사용하도록 개발 키트를 구성하려면

1. Xbox 디바이스 포털 UI의 **네트워크** 도구로 이동합니다.
2. 바탕 화면으로 내보낸 Fiddler 루트 인증서를 찾습니다. 
3. Fiddler를 실행하는 개발 PC의 호스트 이름 또는 IP 주소를 입력합니다.
4. Fiddler에서 수신 대기하는 포트 번호(기본적으로 Fiddler에서 포트 8888 사용)를 입력합니다. 
5. **사용**을 클릭합니다. 이렇게 하면 개발 키트가 다시 시작됩니다.

### <a name="to-stop-using-fiddler"></a>Fiddler 사용을 중지하려면
인터넷의 프록시로 Fiddler 사용을 중지(그리고 Fiddler에서 개발 키트의 네트워크 트래픽의 모든 추적을 중지)하려면 다음을 수행합니다.

1. Xbox 디바이스 포털 UI의 **네트워크** 도구로 이동합니다.
2. **사용 안 함**을 클릭합니다.

> [!NOTE]
> Fiddler가 설치된 각 PC마다 다른 Fiddler 루트 인증서를 사용합니다. 개발 키트용 Fiddler 프록시를 제공하는 데 사용되는 PC가 두 대 이상인 경우 두 PC 간에 전환할 때 새 루트 인증서를 선택해야 합니다. If you are using only one PC, you need to select the root certificate only the first time you enable Fiddler. IP 주소 및 포트는 여전히 지정해야 합니다.

## <a name="see-also"></a>참고 항목
- [Fiddler 설정 API 참조](wdp-fiddler-api.md)
- [질문과 대답](frequently-asked-questions.md)
- [Xbox One의 UWP](index.md)



