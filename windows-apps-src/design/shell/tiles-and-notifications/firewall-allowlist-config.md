---
author: mijacobs
Description: 많은 기업에서 방화벽을 사용 하 여 원치 않는 트래픽을 차단 합니다. 이 문서에서는 WNS 트래픽이 방화벽을 통과 하도록 허용 하는 방법을 설명 합니다.
title: 방화벽에 WNS 트래픽 추가 Allowlist
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
keywords: windows 10, uwp, WNS, windows 알림 서비스, 알림, windows, 방화벽, 문제 해결, IP, 트래픽, 엔터프라이즈, 네트워크, IPv4, VIP, FQDN, 공용 IP 주소
ms.localizationpriority: medium
ms.openlocfilehash: 0ba6d2e678eee0d851b4f2e3897f9fc067b74580
ms.sourcegitcommit: 3360db6bc975516e01913d3d73599c964a411052
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70296978"
---
# <a name="enterprise-firewall-and-proxy-configurations-to-support-wns-traffic"></a>WNS 트래픽을 지원 하기 위한 엔터프라이즈 방화벽 및 프록시 구성

## <a name="background"></a>배경
많은 기업에서 방화벽을 사용 하 여 원치 않는 네트워크 트래픽을 차단 합니다. 불행 하 게도 Windows Notification Service 통신과 같은 중요 한 작업을 차단할 수 있습니다. 즉, WNS를 통해 전송 되는 모든 알림은 특정 네트워크 구성에서 삭제 됩니다. 이를 방지 하기 위해 네트워크 관리자는 승인 목록에 승인 된 WNS 채널 목록을 추가 하 여 WNS 트래픽이 방화벽을 통과 하도록 할 수 있습니다. 다음은 다양 한 프록시 형식에 대 한 지원 뿐만 아니라 추가 방법에 대 한 자세한 내용입니다.

## <a name="proxy-support"></a>프록시 지원

> [!Note]
> Windows 클라이언트는 모든 프록시를 지원 **하지 않습니다** . WNS에 대 한 연결은 직접 연결 이어야 합니다.

**개봉박두!** 다른 네트워크 구성, 프록시 및 방화벽을 적극적으로 조사 하 고 있습니다. 이 페이지는 일반적인 엔터프라이즈 시나리오 및 WNS 지원에 대 한 자세한 내용으로 업데이트할 예정입니다.


## <a name="what-information-should-be-added-to-the-allowlist"></a>Allowlist에 추가 해야 하는 정보
다음은 Windows 알림 서비스에서 사용 하는 Fqdn, Vip 및 IP 주소 범위를 포함 하는 목록입니다. 

> [!IMPORTANT]
> FQDN으로 목록을 허용 하는 것은 변경 되지 않으므로이를 허용 하는 것이 좋습니다. FQDN으로 목록을 허용 하는 경우 IP 주소 범위를 허용 하지 않아도 됩니다.

> [!IMPORTANT]
> IP 주소 범위는 주기적으로 변경 됩니다. 이로 인해이 페이지에 포함 되지 않습니다. IP 범위 목록을 보려는 경우 다운로드 센터에서 파일을 다운로드할 수 있습니다. [WNS (Windows 알림 서비스) VIP 및 IP 범위](https://www.microsoft.com/download/details.aspx?id=44238) 최신 정보를 확인 하려면 정기적으로 다시 확인 하세요. 


### <a name="fqdns-vips-and-ips"></a>Fqdn, Vip 및 Ip
다음 XML 문서의 각 요소에 대 [한 설명은 용어 및 표기법](#terms-and-notations)의 표에 설명 되어 있습니다. Fqdn이 일정 하 게 유지 되므로 Fqdn만 사용 하는 것이 좋습니다. 그러나 다운로드 센터에서 전체 목록이 포함 된 XML 파일을 다운로드할 수 있습니다. [WNS (Windows 알림 서비스) VIP 및 IP 범위](https://www.microsoft.com/download/details.aspx?id=44238) 새 Vip 또는 IP 범위는 **업로드 후 1 주일에 적용**됩니다.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<WNSPublicIpAddresses xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- This file contains the FQDNs, VIPs, and IP address ranges used by the Windows Notification Service. A new text file will be uploaded every time a new VIP or IP range is released in production.  Please copy the below information and perform the necessary changes on your site. Endpoints in CloudService nodes are used for cloud services to send notifications to WNS. Endpoints in Client nodes are used by devices to receive notifications from WNS. --> 
    <CloudServiceDNS>
    <DNS FQDN="*.notify.windows.com"/>
    </CloudServiceDNS>
    <ClientDNS>
        <DNS FQDN="*.wns.windows.com"/>
        <DNS FQDN="*.notify.live.net"/>
    </ClientDNS>
    <CloudServiceIPs>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </CloudServiceIPs>
    <ClientIPsIPv4>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </ClientIPsIPv4>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>용어 및 표기법
위의 XML 조각에 사용 된 표기법 및 요소에 대 한 설명은 다음과 같습니다.

| 용어 | 설명 |
|---|---|
| **점-10 진수 표기법 (예: 64.4.28.0/26)** | 점-10 진수 표기법은 IP 주소의 범위를 설명 하는 방법입니다. 예를 들어 64.4.28.0/26은 64.4.28.0의 처음 26 비트가 상수이 고, 마지막 6 비트는 가변적입니다.  이 경우 IPv4 범위는 64.4.28.0-64.4.28.63입니다. |
| **ClientDNS** | 이는 WNS에서 알림을 수신 하는 클라이언트 장치 (Windows Pc, 데스크톱)의 FQDN (정규화 된 도메인 이름) 필터입니다. 이는 WNS 클라이언트가 WNS 기능을 사용 하기 위해 방화벽을 통해 허용 되어야 합니다.  IP/VIP 범위 대신 Fqdn으로 목록을 허용 하는 것이 좋습니다 .이는 변경 되지 않기 때문입니다. |
| **ClientIPsIPv4** | 클라이언트 장치 (Windows Pc, 데스크톱)에서 WNS 로부터 알림을 수신 하 여 액세스 하는 서버의 IPv4 주소입니다. |
| **CloudServiceDNS** | 클라우드 서비스에서 notificatios를 WNS로 보내기 위해 통신 하는 WNS 서버에 대 한 FQDN (정규화 된 도메인 이름) 필터입니다. 이는 서비스에서 WNS 알림을 보내기 위해 방화벽을 통해 허용 되어야 합니다.  IP/VIP 범위 대신 Fqdn으로 목록을 허용 하는 것이 좋습니다 .이는 변경 되지 않기 때문입니다.|
| **CloudServiceIPs** | CloudServiceIPs는 클라우드 서비스에서 WNS에 알림을 보내는 데 사용 되는 서버의 IPv4 주소입니다.  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>MPNS (Microsoft 푸시 알림 서비스) 공용 IP 범위
레거시 notification service MPNS를 사용 하는 경우 허용 목록에 추가 해야 하는 IP 주소 범위를 다운로드 센터에서 사용할 수 있습니다. [MPNS (Microsoft 푸시 알림 서비스) 공용 IP 범위](https://www.microsoft.com/download/details.aspx?id=44535)


## <a name="related-topics"></a>관련 항목

* [빠른 시작: 푸시 알림 보내기](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [알림 채널을 요청, 생성 및 저장 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [실행 중인 응용 프로그램에 대 한 알림을 가로채는 방법](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [WNS (Windows 푸시 알림 서비스)를 사용 하 여 인증 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [푸시 알림 서비스 요청 및 응답 헤더](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [푸시 알림에 대 한 지침 및 검사 목록](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
