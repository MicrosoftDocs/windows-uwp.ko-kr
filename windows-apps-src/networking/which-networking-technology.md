---
ms.assetid: 2CC2E526-DACB-4008-9539-DA3D0C190290
description: UWP 개발자가 사용할 수 있는 네트워킹 기술에 대한 빠른 개요 및 앱에 적합한 기술을 선택하는 방법에 대한 제안 사항입니다.
title: 네트워킹 기술 선택'
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c4b1a0dab6bf1eb3301ba9fb97abd95fd896c53e
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259170"
---
# <a name="which-networking-technology"></a>네트워킹 기술 선택


UWP 개발자가 사용할 수 있는 네트워킹 기술에 대한 빠른 개요 및 앱에 적합한 기술을 선택하는 방법에 대한 제안 사항입니다.

## <a name="sockets"></a>소켓

다른 장치와 통신할 때 사용자 고유의 프로토콜을 사용하려는 경우 [소켓](sockets.md)을 사용합니다.

UWP(유니버설 Windows 플랫폼) 개발자는 두 가지 소켓 구현 [**Windows.Networking.Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets) 및 [Winsock](https://docs.microsoft.com/windows/desktop/WinSock/windows-sockets-start-page-2)을 사용할 수 있습니다. 새 코드를 작성하는 경우 Windows.Networking.Socket은 UWP 개발자용으로 디자인된 최신 API를 활용합니다. 플랫폼 간 네트워킹 라이브러리나 기존의 다른 Winsock 코드를 사용하려는 경우 또는 기본적으로 Winsock API를 사용하려는 경우에 사용합니다.

### <a name="when-to-use-sockets"></a>소켓을 사용하는 경우

-   두 소켓 구현 모두 선택한 프로토콜(TCP 또는 UDP)을 사용하여 다른 디바이스와 통신하도록 지원합니다.

-   사용할 수 있는 기존 코드 및 환경에 따라 요구 사항에 가장 적합한 소켓을 선택합니다.

### <a name="when-not-to-use-sockets"></a>소켓을 사용하지 않는 경우

-   소켓을 사용하여 사용자 고유의 HTTP(S) 스택을 구현하지 마세요. 대신 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)를 사용합니다.
-   WebSocket([**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 및 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 클래스)이 통신 요구 사항(웹 서버와의 TCP)을 충족하는 경우 소켓으로 유사한 기능을 구현하는 데 시간과 리소스를 소비하는 대신 이를 사용하는 것이 좋습니다.

## <a name="websockets"></a>WebSocket

[WebSocket](websockets.md) 프로토콜은 웹을 통해 클라이언트와 서버 간의 신속하고 보안이 유지된 양방향 통신을 위한 메커니즘을 정의합니다. 데이터가 전이중 단일 소켓 연결을 통해 즉시 전송되므로 두 엔드포인트에서 실시간으로 메시지를 보내고 받을 수 있습니다. WebSocket은 즉각적인 소셜 네트워크 알림WebSocket은 즉각적인 소셜 네트워크 알림 및 최신 정보(예: 게임 통계)의 표시에 보안 및 고속 데이터 전송이 필요한 실시간 게임에 적합합니다. UWP 개발자는 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 및 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 클래스를 사용하여 Websocket 프로토콜을 지원하는 서버와 연결할 수 있습니다.

### <a name="when-to-use-websockets"></a>WebSocket을 사용하는 경우

-   디바이스와 서버 간에 지속적으로 데이터를 보내고 받으려는 경우

### <a name="when-not-to-use-websockets"></a>WebSocket을 사용하지 않는 경우

-   데이터를 간헐적으로 보내거나 받는 경우 WebSocket 연결을 설정하고 유지 관리하는 것보다 디바이스에서 서버로 개별 HTTP 요청을 보내는 것이 더 간단합니다.
-   WebSocket은 용량이 매우 많은 상황에 적합하지 않을 수 있습니다. 디자인에서 사용하기 전에 데이터 흐름을 모델링하고 WebSocket을 통한 트래픽을 시뮬레이트하는 것이 좋습니다.

## <a name="httpclient"></a>HttpClient

HTTP(S)를 사용하여 웹 서비스 또는 웹 서버와 통신하는 경우 [HttpClient](httpclient.md)(및 나머지 [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 네임스페이스 API)를 사용하는 것이 좋습니다.

### <a name="when-to-use-httpclient"></a>HttpClient를 사용하는 경우

-   HTTP(S)를 사용하여 웹 서비스와 통신하는 경우
-   용량이 작은 소수의 파일을 업로드하거나 다운로드하는 경우
-   WebSocket([**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 및 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 클래스)이 통신 요구 사항(웹 서버와의 TCP)을 충족하고 해당 웹 서버가 WebSocket을 지원하는 경우 HttpClient로 유사한 기능을 구현하는 데 시간과 리소스를 소비하는 대신 이를 사용하는 것이 좋습니다.
-   네트워크를 통해 콘텐츠를 스트리밍하는 경우

### <a name="when-not-to-use-httpclient"></a>HttpClient를 사용하지 않는 경우

-   대용량 파일 또는 많은 수의 파일을 전송하는 경우 대신 백그라운드 전송을 사용하는 것이 좋습니다.
-   연결 형식에 따라 업로드/다운로드를 제한하거나 진행률을 저장하고 중단 후 업로드/다운로드를 다시 시작하려면 백그라운드 전송을 사용해야 합니다.
-   두 디바이스 간에 통신할 때 하나의 디바이스가 HTTP(S) 서버 역할을 하도록 디자인되지 않은 경우 소켓을 사용해야 합니다. 사용자 고유의 HTTP 서버를 구현하고 [HttpClient](httpclient.md)를 사용하여 통신하지 마세요.

## <a name="background-transfers"></a>백그라운드 전송

네트워크를 통해 안정적으로 파일을 전송하려는 경우 [백그라운드 전송 API](background-transfers.md)를 사용합니다. 백그라운드 전송 API는 앱이 일시 중단된 동안 백그라운드 실행되고 앱 종료 이후에도 유지되는 고급 업로드 및 다운로드 기능을 제공합니다. 이 API는 네트워크 상태를 모니터링하여 연결이 끊어진 경우 자동으로 전송을 일시 중단 및 다시 시작하며, 전송이 데이터 및 배터리를 인식합니다. 즉, 현재 연결 및 디바이스 배터리 상태에 따라 다운로드 작업이 조정됩니다. 앱이 모바일 또는 배터리 전원을 디바이스에서 실행되는 경우 이러한 접근 권한 값이 필요합니다. 이 API는 HTTP(S)를 사용한 대용량 파일 업로드 및 다운로드에 적합합니다. FTP도 지원되지만 다운로드에만 지원됩니다.

Windows 10의 새로운 백그라운드 전송 기능은 로컬 카탈로그를 업데이트하거나, 다른 앱을 활성화하거나, 다운로드가 완료된 경우 사용자에게 알릴 수 있도록 파일 전송이 완료되면 사후 처리를 트리거하는 기능입니다.

### <a name="when-to-use-background-transfers"></a>백그라운드 전송을 사용하는 경우

-   대용량 파일 또는 많은 수의 파일을 안정적으로 전송하려는 경우 백그라운드 전송을 사용합니다.
-   백그라운드 작업과 함께 사후 처리 파일을 전송하려는 경우 백그라운드 전송 완료 그룹과 함께 백그라운드 전송을 사용합니다.
-   네트워크 중단 후 진행 중인 전송을 다시 시작하려는 경우 백그라운드 전송을 사용합니다.
-   데이터 요금제와 같이 네트워크 상태에 따라 전송 동작을 변경하려는 경우 백그라운드 전송을 사용합니다.

### <a name="when-not-to-use-background-transfers"></a>백그라운드 전송을 사용하지 않는 경우

-   용량이 작은 소수의 파일을 전송할 때 전송이 완료된 후 사후 처리가 필요 없는 경우에는 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) PUT 또는 POST 메서드를 사용하는 것이 좋습니다.
-   데이터를 스트림하고 데이터가 도착하면 로컬에서 사용하려는 경우 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)를 사용합니다.

## <a name="additional-network-related-technologies"></a>추가 네트워크 관련 기술

### <a name="connection-quality"></a>연결 품질

[  **Windows.Networking.Connectivity**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity) API는 네트워크 연결, 비용 및 사용 정보에 액세스할 수 있도록 지원합니다. 이 API를 사용하는 방법에 대한 자세한 내용은 [네트워크 연결 상태 액세스 및 네트워크 비용 관리](https://docs.microsoft.com/previous-versions/windows/apps/hh452983(v=win.10))를 참조하세요.

### <a name="dns-service-discovery"></a>DNS 서비스 검색

[  **Windows.Networking.ServiceDiscovery.Dnssd**](https://docs.microsoft.com/uwp/api/Windows.Networking.ServiceDiscovery.Dnssd) API는 IETF [RFC 2782](https://www.rfc-archive.org/getrfc.php?rfc=2782)에 설명된 DNS-SD 프로토콜을 사용하여 네트워크의 다른 장치에 네트워크 서비스를 보급하도록 지원합니다.

### <a name="communicating-over-bluetooth"></a>Bluetooth를 통한 통신

[  **Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth) API는 특히 Bluetooth를 사용하여 다른 디바이스에 연결하고 데이터를 전송하도록 지원합니다. 자세한 내용은 [RFCOMM을 사용하여 파일 보내기 및 받기](https://docs.microsoft.com/windows/uwp/devices-sensors/send-or-receive-files-with-rfcomm)를 참조하세요.

### <a name="push-notifications-wns"></a>푸시 알림(WNS)

[  **Windows.Networking.PushNotifications**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications) API는 WNS(Windows 알림 서비스)를 사용하여 네트워크를 통해 푸시 알림을 받을 수 있도록 지원합니다. 이 API를 사용하는 방법에 대한 자세한 내용은 [WNS(Windows 푸시 알림 서비스) 개요](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)를 참조하세요.

### <a name="near-field-communications"></a>근거리 통신

[  **Windows.Networking.Proximity**](https://docs.microsoft.com/uwp/api/Windows.Networking.Proximity) API는 디바이스와의 근접 연결 또는 탭 연결을 사용하여 손쉬운 데이터 전송을 지원하는 앱에 근거리 통신을 사용하도록 지원합니다. 이 API를 사용하는 방법에 대한 자세한 내용은 [근접 연결 및 탭 지원](https://docs.microsoft.com/previous-versions/windows/apps/hh465229(v=win.10))을 참조하세요.

### <a name="rssatom-feeds"></a>RSS/Atom 피드

[  **Windows.Web.Syndication**](https://docs.microsoft.com/uwp/api/Windows.Web.Syndication) API는 RSS 및 Atom 형식을 사용하여 배포 피드를 관리하도록 지원합니다. 이 API를 사용하는 방법에 대한 자세한 내용은 [RSS/Atom 피드](web-feeds.md)를 참조하세요.

### <a name="wi-fi-enumeration-and-connection-control"></a>Wi-Fi 열거형 및 연결 제어

[  **Windows.Devices.WiFi**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFi) API는 Wi-Fi 어댑터를 열거하고, 사용 가능한 Wi-Fi 네트워크를 검색하고, 어댑터를 네트워크에 연결하도록 지원합니다.

### <a name="radio-control"></a>무선 제어

[  **Windows.Devices.Radios**](https://docs.microsoft.com/uwp/api/Windows.Devices.Radios) API는 로컬 장치에서 Wi-Fi, Bluetooth 등의 무선을 찾고 제어하도록 지원합니다.

### <a name="wi-fi-direct"></a>Wi-Fi Direct

[  **Windows.Devices.WiFiDirect**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFiDirect) API는 Wi-Fi Direct를 사용하여 애드혹 로컬 무선 네트워크를 만드는 방식으로 다른 로컬 장치와 연결하고 통신하도록 지원합니다.

### <a name="wi-fi-direct-services"></a>Wi-Fi Direct 서비스

[  **Windows.Devices.WiFiDirect.Services**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFiDirect.Services) API는 Wi-Fi Direct 서비스를 제공하고 연결하도록 지원합니다. Wi-Fi Direct 서비스는 Wi-Fi Direct 애드혹 네트워크의 장치(Service Advertiser)에서 Wi-Fi Direct 연결을 통해 다른 장치(Service Seeker)에 접근 권한 값을 제공하는 방식입니다.

### <a name="mobile-operators"></a>통신사

Windows 10에서는 이전에 디바이스 제조업체 및 통신사에만 노출된 일부 API를 광범위한 개발자 그룹에 노출합니다. 이러한 API는 현재 노출되어 있지만 앱을 게시하기 전에 Microsoft의 승인을 받아야 하는 특정 앱 접근 권한 값으로 제어됩니다. 이러한 API의 실제 사용은 주로 디바이스 제조업체 및 통신사로 제한됩니다.

### <a name="network-operations"></a>네트워크 운영

[  **Windows.Networking.NetworkOperators**](https://docs.microsoft.com/uwp/api/Windows.Networking.NetworkOperators) API는 휴대폰의 구성 및 프로비저닝을 주로 처리합니다. 따라서 이를 제어하는 접근 권한 값의 사용 권한은 디바이스 제조업체 및 통신사로 제한됩니다.

### <a name="sms"></a>SMS

[  **Windows.Devices.Sms**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sms) 네임스페이스는 SMS 및 관련 메시지를 하위 수준 엔터티로 처리합니다. 이는 통신사에서 앱 지정 SMS에 사용하도록 제공되며, 대부분의 앱 개발자용으로 승인되지 않는 접근 권한 값으로 제어됩니다. 메시지를 처리할 앱을 작성하는 경우 대신 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) API를 사용해야 합니다. 이 API는 SMS 메시지뿐만 아니라 실시간 채팅 앱과 같은 다른 소스의 메시지도 처리하도록 디자인되었으므로 훨씬 풍부한 채팅/메시징 환경을 지원합니다.

