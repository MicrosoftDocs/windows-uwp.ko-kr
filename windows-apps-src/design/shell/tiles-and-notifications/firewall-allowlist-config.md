---
author: mijacobs
Description: 대부분의 기업에서는 불필요 한 트래픽을 차단 하도록 방화벽을 사용 합니다. 이 문서에는 WNS 트래픽이 방화벽을 통해 전달 되도록 허용 하는 방법을 설명 합니다.
title: WNS 트래픽을 방화벽 Allowlist 추가
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: WNS, windows 10, uwp, windows 알림 서비스, 알림, windows, 방화벽 문제 해결, IP, 트래픽, enterprise, 네트워크, 공용 IP 주소, FQDN, VIP, IPv4
ms.localizationpriority: medium
ms.openlocfilehash: 9ed4ad6ed828abda9d487ef96beca9b655c92421
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366676"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>엔터프라이즈 방화벽을 통해 Windows 알림 트래픽을 허용

## <a name="background"></a>배경
원치 않는 네트워크 트래픽을 차단 하도록 방화벽을 사용 하는 대부분의 기업에서는 그러나 중요 한 등 Windows 알림 서비스 통신 차단할 수도 있습니다. 즉, WNS를 통해 전송 되는 모든 알림이 삭제 됩니다. 이 방지 하려면 네트워크 관리자 WNS 트래픽이 방화벽을 통해 전달 되도록 허용 하는 예외 목록에 승인 된 WNS 채널 목록을 추가할 수 있습니다. 추가 하는 방법에 대 한 자세한 내용은 같습니다. 


## <a name="what-information-should-be-added-to-the-allowlist"></a>allowlist에 추가할 정보
Fqdn, Vip 및 IP를 포함 하는 목록을 Windows 알림 서비스에서 사용 되는 범위를 해결 하는 다음 됩니다. 

> [!IMPORTANT]
> 좋습니다 FQDN 기준으로 목록을 허용 하는 이러한 변경 하지 않기 때문입니다. FQDN으로 목록에 허용 하는 경우에 IP 주소 범위를 허용 하도록 필요가 없습니다.

> [!IMPORTANT]
> IP 주소 범위를; 주기적으로 변경 됩니다. 이 인해이 페이지에 포함 되지 않습니다. IP 범위 목록을 확인 하려는 경우에 다운로드 센터에서 파일을 다운로드할 수 있습니다. [Windows 알림 서비스 (WNS) VIP 및 IP 범위](https://www.microsoft.com/download/details.aspx?id=44238)합니다. 하십시오 돌아갑니다 정기적으로 최신 정보가 있는지 확인 합니다. 


### <a name="fqdns-vips-and-ips"></a>Fqdn, Vip 및 Ip
그 다음 표에 설명 되어 각 다음 XML 문서의 요소 (에서 [약관 표기법](#terms-and-notations)합니다. IP 범위 Fqdn 일정 하 게 유지 됩니다 처럼 Fqdn만을 사용 하는 것이 좋습니다에이 문서에서 의도적으로 남아 있습니다. 그러나 다운로드 센터에서 전체 목록이 포함 된 XML 파일을 다운로드할 수 있습니다. [Windows 알림 서비스 (WNS) VIP 및 IP 범위](https://www.microsoft.com/download/details.aspx?id=44238)합니다. 새 Vip 또는 IP 범위는 수 **업로드한 후 1 주 동안 유효**합니다.

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

### <a name="terms-and-notations"></a>사용 약관 표기법
표기법 및 위의 XML 조각에서 사용 되는 요소에 대 한 설명은 다음과 같습니다.

| 용어 | 설명 |
|---|---|
| **십진수 표기법 (예: 64.4.28.0/26)** | 십진수 표기법으로의 IP 주소 범위를 설명 하는 방법입니다. 예를 들어 64.4.28.0/26 64.4.28.0의 처음 26 비트는 마지막 6 비트 변수는 상수를 의미 합니다.  이 경우 IPv4 범위가 64.4.28.0-64.4.28.63 합니다. |
| **ClientDNS** | 클라이언트 장치 (Windows Pc, 데스크톱)에 대 한 Fully-Qualified 도메인 이름 (FQDN) 필터는 WNS에서 알림을 수신 합니다. 이러한 WNS 기능을 사용 하려면 WNS 클라이언트가 방화벽을 통해 허용 되어야 합니다.  것이 좋습니다 허용 목록 IP/VIP 범위 대신 Fqdn으로 하므로 이러한 변경 되지 것입니다. |
| **ClientIPsIPv4** | 클라이언트 장치 (Windows Pc, 데스크톱)에서 액세스 서버의 IPv4 주소는 WNS에서 알림을 수신 합니다. |
| **CloudServiceDNS** | 이들은 알림으로 WNS에 보내도록 클라우드 서비스에는 강연과 WNS 서버용 Fully-Qualified 도메인 이름 (FQDN) 필터입니다. 이러한 순서로 WNS 알림을 보내도록 서비스에 대 한 방화벽을 통해 허용 되어야 합니다.  것이 좋습니다 허용 목록 IP/VIP 범위 대신 Fqdn으로 하므로 이러한 변경 되지 것입니다.|
| **CloudServiceIPs** | CloudServiceIPs는 WNS 알림을 보내는 데 클라우드 서비스에 대 한 서버의 IPv4 주소  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Microsoft 푸시 알림 서비스 (MPNS) 공용 IP 범위
MPNS 레거시 알림 서비스를 사용 하는 경우 IP 주소 범위를 허용 목록에 추가 해야 합니다. 다운로드 센터에서 사용할 수 있습니다: [Microsoft 푸시 알림 서비스 (MPNS) 공용 IP 범위](https://www.microsoft.com/download/details.aspx?id=44535)합니다.


## <a name="related-topics"></a>관련 항목

* [빠른 시작: 푸시 알림 보내기](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [요청, 생성 및 알림 채널을 저장 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [응용 프로그램을 실행 하는 것에 대 한 알림을 차단 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [WNS와 함께 Windows 푸시 알림 서비스 ()를 인증 하는 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [푸시 알림 서비스 요청 및 응답 헤더](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [지침 및 푸시 알림에 대 한 검사 목록](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
