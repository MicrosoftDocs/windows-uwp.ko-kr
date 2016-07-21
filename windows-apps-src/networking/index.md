---
author: DelfCo
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: "네트워크 및 웹 서비스에 액세스하는 기술입니다."
title: "네트워킹 및 웹 서비스"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: b4e291c0948a7734b8ba188bc060dfba2e9a83d1

---

# 네트워킹 및 웹 서비스

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

다음 네트워킹 및 웹 서비스 기술은 UWP(유니버설 Windows 플랫폼) 개발자가 사용할 수 있습니다.

| 항목                                                                                   | 설명                                                                      |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| [네트워킹 기본 사항](networking-basics.md)                                               | 네트워크 지원 앱에 대해 수행해야 하는 사항입니다.                     |
| [네트워킹 기술 선택](which-networking-technology.md)                          | UWP 개발자가 사용할 수 있는 네트워킹 기술에 대한 빠른 개요 및 앱에 적합한 기술을 선택하는 방법에 대한 제안 사항입니다.               |
| [백그라운드에서의 네트워크 통신](network-communications-in-the-background.md) | 앱은 포그라운드에 없을 때 통신을 유지하기 위해 백그라운드 작업과 두 가지 기본 메커니즘, 즉 소켓 브로커와 컨트롤 채널 트리거를 사용합니다.                  |
| [소켓](sockets.md)                                                                   | UWP 앱 개발자는 [Windows.Networking.Sockets](https://msdn.microsoft.com/library/windows/apps/xaml/windows.networking.sockets.aspx) 및 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523)을 둘 다 사용하여 다른 디바이스와 통신할 수 있습니다. 이 항목에서는 네트워킹 작업을 수행하기 위한 Windows.Networking.Sockets 네임스페이스 사용에 대해 자세한 지침을 제공합니다. |
| [WebSocket](websockets.md)                                                             | WebSocket은 HTTP(S)를 사용하여 웹을 통해 클라이언트와 서버 간의 신속하고 보안이 유지된 양방향 통신을 위한 메커니즘을 제공합니다.                 |
| [HttpClient](httpclient.md)                                                             | [Windows.Web.Http](https://msdn.microsoft.com/library/windows/apps/dn279692) 네임스페이스 API를 사용하여 HTTP 2.0 및 HTTP 1.1 프로토콜을 통해 정보를 보내고 받습니다.             |
| [RSS/Atom 피드](web-feeds.md)                                                          | [Windows.Web.Syndication](https://msdn.microsoft.com/library/windows/apps/br243632) 네임스페이스의 기능을 사용하여 RSS 및 Atom 표준에 따라 생성된 신디케이티드 피드로 인기 있는 최신 웹 콘텐츠를 검색하거나 만듭니다.                   |
| [백그라운드 전송](background-transfers.md)                                         | 백그라운드 전송 API를 사용하여 네트워크를 통해 파일을 안정적으로 복사합니다.           |
| [EDP ID를 사용하여 네트워크 연결 태그 지정](tagging_network_connections_with_edp_identity.md) | 이 항목에서는 EDP(엔터프라이즈 데이터 보호) 시나리오에서 네트워크 연결을 만들기 전에 보호된 스레드 컨텍스트를 만드는 방법을 보여 줍니다. |



<!--HONumber=Jul16_HO2-->


