---
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: UWP(유니버설 Windows 플랫폼) 개발자가 사용할 수 있는 네트워킹 및 웹 서비스 기술에 대한 문서 링크 목록을 봅니다.
title: 네트워킹 및 웹 서비스
ms.date: 11/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4a80c130de1966973fe928480009eaa3bd0a8887
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171251"
---
# <a name="networking-and-web-services"></a>네트워킹 및 웹 서비스

다음 네트워킹 및 웹 서비스 기술은 UWP(유니버설 Windows 플랫폼) 개발자가 사용할 수 있습니다.

| 항목 | 설명 |
| - | - |
| [네트워킹 기본 사항](networking-basics.md) | 네트워크 지원 앱에 대해 수행해야 하는 사항입니다. |
| [네트워킹 기술 선택](which-networking-technology.md) | UWP 개발자가 사용할 수 있는 네트워킹 기술에 대한 빠른 개요 및 앱에 적합한 기술을 선택하는 방법에 대한 제안 사항입니다. |
| [백그라운드 네트워크 통신](network-communications-in-the-background.md) | 백그라운드가 아닌 상태에서 네트워크 통신을 계속하기 위해 앱에서 백그라운드 작업과 소켓 브로커 또는 컨트롤 채널 트리거를 사용할 수 있습니다. |
| [소켓](sockets.md) | 소켓은 많은 네트워킹 프로토콜이 구현되는 하위 수준 데이터 전송 기술입니다. UWP는 연결이 오래되었거나 설정된 연결이 필요하지 않은지 여부에 관계없이 클라이언트-서버 또는 피어 투 피어 애플리케이션에 대한 TCP 및 UDP 소켓 클래스를 제공합니다. |
| [WebSockets](websockets.md) | WebSocket은 HTTP(S)를 사용하고 UTF-8 및 이진 메시지를 모두 지원하는 웹을 통해 클라이언트와 서버 간의 빠르고 안전한 양방향 통신을 위한 메커니즘을 제공합니다. |
| [HttpClient](httpclient.md) | [Windows.Web.Http](/uwp/api/Windows.Web.Http) 네임스페이스 API를 사용하여 HTTP 2.0 및 HTTP 1.1 프로토콜을 통해 정보를 보내고 받습니다. |
| [RSS/Atom 피드](web-feeds.md) | [Windows.Web.Syndication](/uwp/api/Windows.Web.Syndication) 네임스페이스의 기능을 사용하여 RSS 및 Atom 표준에 따라 생성된 신디케이티드 피드로 인기 있는 최신 웹 콘텐츠를 검색하거나 만듭니다. |
| [백그라운드 전송](background-transfers.md) | 백그라운드 전송 API를 사용하여 네트워크를 통해 파일을 안정적으로 복사합니다. |