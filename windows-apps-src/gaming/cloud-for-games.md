---
title: UWP 게임에 클라우드 서비스 사용
description: 플랫폼 및 장치에서 UWP 게임을 개발 하는 경우 클라우드 백 엔드를 사용 하 여 수요에 따라 게임 크기를 조정 하는 데 도움을 줍니다.
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.date: 03/27/2018
ms.topic: article
keywords: windows 10, uwp, 게임, 클라우드 서비스
ms.localizationpriority: medium
ms.openlocfilehash: ca575975b27a71798b7cad1cac0cc83ec870d756
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173177"
---
#  <a name="using-cloud-services-for-uwp-games"></a>UWP 게임에 클라우드 서비스 사용

Windows 10의 UWP (유니버설 Windows 플랫폼)는 Microsoft 장치에서 게임을 개발 하는 데 사용할 수 있는 Api 집합을 제공 합니다. 플랫폼 및 장치에서 게임을 개발할 때 클라우드 백 엔드를 활용 하 여 수요에 따라 게임을 확장할 수 있습니다.

게임을 위한 전체 클라우드 백 엔드 솔루션을 찾고 있는 경우 [게임 백 엔드를 위한 Software as a Service](#software-as-a-service-for-game-backend)를 참조 하세요.

##  <a name="what-is-cloud-computing"></a>클라우드 컴퓨팅이란?

클라우드 컴퓨팅은 인터넷을 통해 주문형 IT 리소스 및 응용 프로그램을 사용 하 여 장치에 대 한 데이터를 저장 하 고 처리 합니다. _클라우드_ 라는 용어는 특정 위치에서 액세스할 수 있는 방대한 리소스 (로컬 리소스 아님)의 가용성에 대 한 비유입니다.
클라우드 컴퓨팅의 원칙은 리소스와 소프트웨어를 사용 하는 새로운 방법을 제공 합니다. 사용자는 더 이상 전체 전체 제품 또는 리소스에 대 한 비용을 지불할 필요가 없으며, 대신 플랫폼, 소프트웨어 및 리소스를 서비스로 사용할 수 있습니다. 클라우드 공급자는 사용량 또는 서비스 계획 제공에 따라 고객에 게 요금을 청구 하는 경우가 많습니다.

##  <a name="why-use-cloud-services"></a>클라우드 서비스를 사용 하는 이유

게임에 클라우드 서비스를 사용 하는 경우의 이점 중 하나는 물리적 하드웨어 서버에 사전 투자 하지 않아도 되지만 이후 단계에서 사용량 또는 서비스 계획에 따라서만 요금을 지불 해야 한다는 것입니다. 새 게임 타이틀을 개발 하는 데 관련 된 위험을 관리 하는 한 가지 방법입니다. 

또 다른 이점은 게임에서 방대한 클라우드 리소스를 탭 하 여 확장성을 달성할 수 있습니다 (동시 플레이어 수, 실시간 게임 계산 또는 데이터 요구 사항에 따라 급격 한 급증을 효과적으로 관리). 이를 통해 게임의 성능은 시계를 기준으로 안정적으로 유지 됩니다. 또한 전 세계 어디에서 든 모든 플랫폼에서 실행 되는 모든 장치에서 클라우드 리소스에 액세스할 수 있습니다. 즉, 모든 사람에 게 게임을 전체적으로 가져올 수 있습니다.

플레이어에 게 놀라운 게임 효과를 제공 하는 것이 중요 합니다. 클라우드에서 실행 되는 게임 서버는 클라이언트 쪽 업데이트와 독립적 이기 때문에 전체 게임에 대해 더 제어 되 고 안전한 환경을 제공할 수 있습니다.   또한 클라이언트를 신뢰 하지 않고 서버 쪽 게임 논리를 포함 하 여 클라우드를 통해 게임의 일관성을 유지할 수 있습니다. 서비스 간 연결을 구성 하 여 보다 통합 된 게임 환경을 허용할 수도 있습니다. 예를 들어 게임 내 구매를 다양 한 지불 방법에 연결 하 고, 다양 한 게임 네트워크를 브리징 하며, Facebook 및 Twitter와 같은 인기 있는 소셜 미디어 포털에 게임 내 업데이트를 공유 하는 것이 있습니다. 

또한 전용 클라우드 서버를 사용 하 여 대기업을 만들고, 게이머 커뮤니티를 구축 하 고, 시간에 따라 게이머 데이터를 수집 및 분석 하 여 게임 플레이를 개선 하 고, 게임의 수익 화 디자인 모델을 최적화할 수 있습니다.

또한 클라우드 서비스를 사용 하 여 비동기 멀티 플레이 메커니즘을 사용 하는 소셜 게임과 같은 집약적 게임 데이터 관리 기능을 필요로 하는 게임을 구현할 수 있습니다.

##  <a name="how-game-companies-use-the-cloud-technology"></a>게임 회사에서 클라우드 기술을 사용 하는 방법

다른 개발자가 게임에서 클라우드 솔루션을 구현 하는 방법을 알아보세요.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header" align="left">
        <th>개발자</th>
        <th>Description</th>
        <th>주요 게임 시나리오</th>
        <th>자세한 정보</th>
    </tr>
    <tr>
        <td><a href="https://www.tencent.com">Tencent 게임</a></td>
        <td><b>Tencent 게임</b> 은 기존 PC 게임을 서비스로 제공할 수 있도록 Azure Service Fabric를 사용 하 여 혁신적인 솔루션을 개발 했습니다. 클라우드 게임 솔루션은 백 엔드에서 마이크로 서비스로 워크 로드를 실행 하는 ' 씬 클라이언트 + 리치 클라우드 ' 모델을 사용 합니다.</td>
        <td>
            <ul>
                <li>기존 PC 게임은 전 세계 사용자에 게 클라우드 게임으로 제공 됩니다. <li>최적화 된 게임 배달 프로세스 <li>복잡성을 줄이고, 종속성으로 인 한 워크 로드 반복을 줄이고, 새 기능을 독립적으로 업그레이드 하는 기능을 제공 하는 마이크로 서비스로 격리 된 게임 기능 <li>최신 게임 콘텐츠를 재생 하는 작은 설치 패키지 다운로드 (GB에서 MB로 축소 되는 패키지 크기) <li>유지 관리 비용 절감 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">클라우드 게임 솔루션을 구축 하는 tencent 게임 및 Microsoft</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">Service Fabric를 사용 하 여 게임 빌드: Tencent의 구현에 대 한 세부 정보 (비디오)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 산업</a></td>
        <td><b>Halo 5:</b> 자동 인덱싱 기능으로 인해 속도와 유연성을 위해 선택 된 Azure Cosmos DB (DocumentDB API를 통해)를 사용 하 여 회사를 소셜 게임 플랫폼으로 <a href="https://www.halowaypoint.com/spartan-companies">부흥 시킴 회사</a> 를 구현 합니다.</td>
        <td>
            <ul>
                <li>멀티 플레이 게임을 위한 그룹 생성/관리를 처리 하는 확장 가능한 데이터 계층 <li>게임 및 소셜 미디어 통합 <li>여러 특성을 통한 실시간 데이터 쿼리 <li>플레이 게임 성과 통계의 동기화 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">Azure Cosmos DB를 사용 하 여 구현 된 소셜 게임 (DocumentDB API를 통해)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://web.ageofascent.com/">Illyriad 게임</a></td>
        <td>Illyriad 게임은 최신 브라우저를 사용 하는 장치에서 재생할 수 있는 대규모 (멀티 플레이 온라인) 3D 공간 게임과 <b>어센더</b>를 만들었습니다. 따라서이 게임은 플러그 인 없이 Pc, 노트북, 휴대폰 및 기타 모바일 장치에서 재생할 수 있습니다. 게임은 ASP.NET Core, HTML5, WebGL 및 Azure를 사용 합니다.</td>
        <td>
            <ul>
                <li>플랫폼 간 브라우저 기반 게임 <li>단일 초대형 영구 오픈 세계 <li>집약적 실시간 게임 계산을 처리 합니다. <li>플레이어 수로 크기 조정 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">Service Fabric를 사용 하 여 게임 빌드: 어센더 MMO 게임 (비디오)</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">Azure Service Fabric (비디오)를 사용 하 여 마이크로 서비스로 게임 구성 요소 관리</a> 
                <li><a href="/Blogs/Azure/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">어센더 개발자로 인터뷰 (비디오)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.nextgames.com/">다음 게임</a></td>
        <td>다음 게임은 AMC의 원본 시리즈를 기반으로 하는 <b>움직이는 비활성: No Man의 육지</b> 비디오 게임의 작성자입니다. 작동 중단 된 게임은 Azure를 백 엔드로 사용 했습니다. 주말에는 여는 주말에 100만 번 다운로드 했 고, 게임이 미국 앱 스토어에서 iPhone & iPad 무료 앱을 #1 12 개국 무료 앱을 #1 하 고, 13 개 국가에서 무료 게임을 #1 했습니다.
        </td>
        <td>
            <ul>
                <li>플랫폼 간 <li>기반 여럿이 설정 <li>탄력적으로 크기 조정 성능 <li>게이머 사기 방지 <li>동적 콘텐츠 배달 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">구축 방법: 다음 게임 Azure의 글로벌 온라인 게임 플랫폼 (비디오를 사용 하는 블로그)</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">더 빠른 개발 주기 및 보다 뛰어난 게임을 위해 DocumentDB API를 통해 Azure Cosmos DB을 사용 합니다.</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.crimecoast.com/">픽셀 분</a></td>
        <td>Unity 게임 엔진과 Azure를 사용 하는 픽셀 분 분의 개발 된 <b>범죄</b> <b>범죄 해안</b> 은 Android, IOS 및 Windows 플랫폼에서 사용할 수 있는 소셜 전략 게임입니다. Azure Blob storage, 관리 되는 Azure Redis Cache, 부하 분산 된 IIS Vm의 배열 및 Microsoft Notification hubs가 게임에 사용 되었습니다. 5000 동시 플레이어와의 크기 조정 및 처리 플레이어를 관리 하는 방법을 알아봅니다.
        </td>
        <td>
            <ul>
                <li>플랫폼 간 <li>멀티 플레이 온라인 게임 <li>플레이어 수로 크기 조정 </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">범죄 해안 MMO 게임에서 Azure Cloud Services를 사용 하는 방법</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>기타 링크

* [Io-interactive 및 Azure: 클라우드를 사용 하는 경우에만 가능한 할 수 있는 대상 등의 게임 기능 만들기](https://channel9.msdn.com/Series/Hitman)
* [Hitcents, Game Troopers 및 InnoSpark에 대 한 비밀 기술인 Azure](https://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Azure를 사용 하는 Bizspark 프로그램의 게임 시작과](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>클라우드 백 엔드를 설계 하는 방법

생산자 및 게임 디자이너는 게임에 필요한 게임 기능과 기능에 대해 논의 하는 동안 게임 인프라를 디자인 하는 방법을 고려 하는 것이 좋습니다. Azure는 다양 한 주요 플랫폼에서 다양 한 장치에 대 한 게임을 개발 하려는 경우 게임 백 엔드로 사용할 수 있습니다.

### <a name="understanding-iaas-paas-or-saas"></a>IaaS, PaaS 또는 SaaS 이해

먼저 게임에 가장 적합 한 서비스 수준을 고려해 야 합니다. 다음 세 가지 서비스의 차이점을 알면 백 엔드를 구축 하는 데 사용할 방법을 결정 하는 데 도움이 될 수 있습니다.

* [IaaS (Infrastructure as a Service)](https://azure.microsoft.com/overview/what-is-iaas/)

    IaaS (Infrastructure as a Service)는 인터넷을 통해 프로 비전 되 고 관리 되는 인스턴트 컴퓨팅 인프라입니다. 수요에 따라 빠르게 규모를 확장 및 축소할 수 있는 많은 컴퓨터의 가능성을 상상해 보세요. IaaS를 사용 하면 고유한 물리적 서버 및 기타 데이터 센터 인프라를 구매 하 고 관리 하는 비용과 복잡성을 피할 수 있습니다.

* [PaaS (Platform as a Service)](https://azure.microsoft.com/overview/what-is-paas/)

    PaaS (Platform as a Service)는 IaaS와 유사 하지만 서버, 저장소, 네트워킹 등의 인프라 관리도 포함 합니다. 따라서 물리적 서버 및 데이터 센터 인프라를 구매 하지 않고 소프트웨어 라이선스, 기본 응용 프로그램 인프라, 미들웨어, 개발 도구 또는 기타 리소스를 구입 하 고 관리할 필요가 없습니다.

* [SaaS(Software as a Service)](https://azure.microsoft.com/overview/what-is-saas/)

    SaaS(Software as a Service)를 도입하면 사용자가 인터넷을 통해 클라우드 기반 앱에 연결하여 사용할 수 있습니다. 클라우드 서비스 공급자 로부터 종 량 제 방식으로 구매 하는 완전 한 소프트웨어 솔루션을 제공 합니다.  일반적인 예로는 전자 메일, 일정 및 office 도구 (예: Office 앱 Microsoft 365)가 있습니다. 조직의 앱 사용을 임대 하 고 사용자가 일반적으로 웹 브라우저를 사용 하 여 인터넷을 통해 연결 합니다. 모든 기본 인프라, 미들웨어, 앱 소프트웨어 및 앱 데이터는 서비스 공급자의 데이터 센터에 있습니다. 서비스 공급자는 하드웨어 및 소프트웨어를 관리 하 고 적절 한 서비스 계약을 사용 하 여 게임과 데이터의 가용성과 보안도 보장 합니다. SaaS를 통해 조직은 최소의 사전 투자 비용으로 신속하게 앱을 시작하고 실행할 수 있습니다.


### <a name="design-your-game-infrastructure-using-azure"></a>Azure를 사용 하 여 게임 인프라 디자인

다음은 게임에 Azure 클라우드 제품을 사용할 수 있는 몇 가지 방법입니다. Azure는 Windows, Linux 및 Ruby, Python, Java, PHP 등의 친숙 한 오픈 소스 기술을 사용 하 여 작동 합니다. 자세한 내용은 [게이밍 용 Azure](https://azure.microsoft.com/solutions/gaming/)를 참조 하세요.

| 요구 사항                 | 활동 시나리오                            | 제품 제공                      | 제품 기능                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| 클라우드에서 도메인 호스트     | 효율적으로 DNS 쿼리에 응답            | [Azure DNS](https://azure.microsoft.com/services/dns/) | 고성능 및 고가용성으로 도메인 호스트  |
| 로그인, id 확인      | 게이머 로그인 및 게이머 id가 인증 됨  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Multi-factor authentication을 사용 하 여 모든 클라우드 및 온-프레미스 웹 앱에 대 한 Single sign-on            | 
| IaaS (infrastructure as a service model)를 사용 하는 게임      | 게임이 클라우드의 가상 컴퓨터에서 호스팅됩니다.       | [Azure VM](https://azure.microsoft.com/services/virtual-machines/) | 기본 제공 가상 네트워킹 및 부하 분산을 사용 하 여 1 개에서 수천 개의 가상 머신 인스턴스를 게임 서버로 확장 온-프레미스 시스템을 사용한 하이브리드 일관성           |
| PaaS (platform as a service model)를 사용 하는 웹 또는 모바일 게임            | 게임이 관리 되는 플랫폼에서 호스팅됩니다.                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | 웹 사이트 또는 모바일 게임을 위한 PaaS (미들웨어/개발 도구/a s/DB 관리를 포함 하는 Azure Vm 의미)   |
| 운영 체제를 보다 효율적으로 제어 하는 확장성 있는 고가용성 n 계층 클라우드 게임        | 게임이 관리 되는 플랫폼에서 호스팅됩니다.                | [Azure Cloud Service](https://azure.microsoft.com/services/app-service/) | 확장 가능 하 고 안정적 이며 운영 비용이 저렴 한 응용 프로그램을 지원 하도록 설계 된 PaaS   |
| 더 나은 성능 및 가용성을 위해 지역 간 부하 분산 | 들어오는 게임 요청을 라우팅합니다. 는 부하 분산의 첫 번째 수준으로 작동할 수 있습니다.       | [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | 는 여러 자동 장애 조치 옵션을 제공 하 고 트래픽을 가중치가 적용 된 값과 동일 하 게 분산 하는 기능을 제공 합니다. 온-프레미스 및 클라우드 시스템을 원활 하 게 결합할 수 있습니다. |
| 게임 데이터를 위한 클라우드 저장소       | 최신 게임 데이터가 클라우드에 저장 되어 클라이언트 장치에 전송 됩니다. | [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)| 저장할 수 있는 파일의 종류에 대 한 제한은 없습니다. 이미지, 오디오, 비디오 등의 구조화 되지 않은 대량의 데이터를 위한 개체 저장소입니다.  |
| 임시 데이터 저장소 테이블| 게임 트랜잭션 (게임 상태 변경)은 일시적으로 테이블에 저장 됩니다. | [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/)| 게임 데이터는 게임의 요구에 따라 유연한 스키마에 저장할 수 있습니다. |
| 게임 트랜잭션/요청을 큐에 대기| 게임 트랜잭션은 큐 형식으로 처리 됩니다. | [Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/)| 큐는 예기치 않은 트래픽 급증을 흡수 하며, 게임 중에 갑자기 많은 요청이 급증 하 여 서버가 부담 하지 않도록 방지할 수 있습니다.   |
| 확장성 있는 관계형 게임 데이터베이스| 데이터베이스에 대 한 게임 내 트랜잭션과 같은 관계형 데이터의 구조적 저장소 | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)| SQL Database as a Service ([VM에서 sql과 비교](/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview))  |
| 확장 가능한 분산 대기 시간이 짧은 게임 데이터베이스| 스키마 유연성을 사용 하 여 게임 및 플레이어 데이터를 빠르게 읽고 쓰고 쿼리 합니다. | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| 짧은 대기 시간 NoSQL 문서 Database as a Service   |
| Azure 서비스에서 자체 데이터 센터 사용 | 게임은 사용자의 데이터 센터에서 검색 되 고 클라이언트 장치로 전송 됩니다. | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | 조직에서 사용자의 데이터 센터에서 Azure 서비스를 제공 하 여 더 많은 정보를 얻을 수 있습니다.  |
| 대량 데이터 청크 전송| 게임 이미지, 오디오, 비디오 등의 많은 파일은를 사용 하 여 가장 가까운 CDN (Content Delivery Network) pop 위치에서 사용자에 게 보낼 수 있습니다 Azure CDN    | [Azure Content Delivery Network](https://azure.microsoft.com/services/cdn/) | 대규모 중앙 노드의 최신 네트워크 토폴로지를 기반으로 구축 된 Azure CDN 갑작스러운 트래픽 급증 및 과도 한 부하를 처리 하 여 속도와 가용성이 크게 향상 되므로 사용자 환경 기능이 크게 향상 되었습니다.  |
| 짧은 대기 시간               | 캐싱을 수행 하 여 더 많은 제어 및 보장 된 데이터 격리로 빠르고 확장 가능한 게임을 빌드 하세요. 게임의 일치 기능을 개선 하는 데에도 사용할 수 있습니다. | [Azure Redis 캐시(영문)](https://azure.microsoft.com/services/cache/) | 빠르고 확장 가능한 Azure 응용 프로그램에 대 한 높은 처리량, 일관 된 짧은 대기 시간 데이터 액세스  |
| 높은 확장성, 짧은 대기 시간 | 낮은 대기 시간 읽기 및 쓰기를 사용 하 여 게임 사용자 수의 변동 처리 | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | 가장 복잡 하 고 대기 시간이 짧고 데이터를 많이 사용 하는 시나리오를 활용 하 고, 한 번에 더 많은 사용자를 처리 하도록 안정적으로 확장할 수 있습니다. Service Fabric를 사용 하면 상태 비저장 앱에 필요한 별도의 저장소나 캐시를 만들지 않고도 게임을 빌드할 수 있습니다. |
| 디바이스에서 초당 수백만 개의 이벤트 수집 가능                         | 장치에서 초당 수백만 개의 이벤트 기록 | [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) | 게임, 웹 사이트, 앱 및 장치에서 클라우드 규모 원격 분석 수집  |
| 게임 데이터에 대 한 실시간 처리  | 게임 플레이를 개선 하기 위해 게이머 데이터에 대 한 실시간 분석 수행| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | 클라우드에서 실시간 스트림 처리  |
| 예측 게임 개발         | 게이머 데이터를 기반으로 사용자 지정 된 동적 게임 만들기  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | 예측 분석 솔루션을 쉽게 빌드, 배포 및 공유할 수 있도록 하는 완전히 관리 되는 클라우드 서비스입니다.  |
| 게임 데이터 수집 및 분석| 관계형 데이터베이스와 비관계형 데이터베이스 모두에서 데이터의 대규모 병렬 처리 | [Azure Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)| 엔터프라이즈 클래스 기능이 포함 된 탄력적 데이터 웨어하우스 as a service   |
| 사용자를 참여 하 여 사용량 및 보존 향상| 모든 백 엔드에서 대상 푸시 알림을 전달 하 여 관심을 생성 하 고 특정 게임 작업을 권장 합니다. | [Azure Notification Hubs](https://azure.microsoft.com/services/notification-hubs/)| &mdash;IOS, Android, Windows, Kindle, Baidu의 모든 주요 플랫폼에서 수백만 개의 모바일 장치에 연결 하는 빠른 브로드캐스트 푸시 모든 백 엔드 &mdash; 클라우드 또는 온-프레미스에서 게임을 호스트할 수 있습니다.|
| 콘텐츠를 보호 하는 동안 로컬 및 전 세계의 대상에 미디어 콘텐츠 스트리밍| 모든 장치에서 브로드캐스트 품질 게임 트레일러 및 시네마 클립을 조사할 수 있습니다.| [Azure Media Services](https://azure.microsoft.com/services/media-services/)| Content Delivery Network 기능이 통합 된 주문형 및 라이브 비디오 스트리밍. 모든 재생 요구에 한 명의 플레이어를 사용 하 여 콘텐츠 보호 및 암호화를 포함 합니다.| 
| 모바일 앱 개발, 배포 및 베타 테스트 | 모바일 앱을 테스트 하 고 배포 합니다. 앱 성능 및 사용자 환경 관리 | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| 응용 프로그램 배포 및 사용자 피드백 플랫폼과 충돌 보고 및 사용자 메트릭을 통합 합니다. Android, Cordova, iOS, OS X, Unity, Windows 및 Xamarin 앱을 지원 합니다. 또한 [Visual Studio Mobile Center](https://visualstudio.microsoft.com/app-center/) &mdash; 풍부한 분석, 충돌 보고, 푸시 알림, 앱 배포 등을 결합 하는 앱에 대 한 Visual Studio 모바일 센터 업무 제어를 고려해 야 합니다. |
| 마케팅 캠페인을 만들어 사용량 및 보존 향상  | 대상 플레이어에 게 푸시 알림을 보내 관심을 생성 하 고 데이터 분석에 따라 특정 게임 작업을 권장 합니다. | [Mobile engagement](https://azure.microsoft.com/services/mobile-engagement/) -2018 년 3 월에 사용 중지 되며 현재 기존 고객 에게만 제공 됩니다. |  IOS, Android, Windows, Windows Phone 등 모든 주요 플랫폼에서 게임 플레이 시간 및 사용자 보존 향상 |


##  <a name="startup-and-developer-resources"></a>시작 및 개발자 리소스

* [Microsoft for Startups](https://startups.microsoft.com)

    Microsoft for go는 시작의 증가를 가속화 하는 데 도움이 되는 제품, 기술 및 출시 간 혜택을 제공 합니다. 한 가지 혜택에는 Azure 무료 계정을 가져오는 것이 포함 됩니다. 30 일간 서비스를 탐색 하는 데 $200 크레딧, 인기 있는 무료 서비스는 12 개월, 항상 무료 25 + 서비스를 사용할 수 있습니다. 자세한 내용은 [Azure 무료 계정으로 시작의 아이디어 가져오기](https://azure.microsoft.com/free/startups/)를 참조 하세요.
    
* [개발자 프로그램](e2e.md#developer-programs)

    Microsoft는 [ID@Xbox](https://www.xbox.com/Developers/id) 및 [Xbox Live 크리에이터 프로그램과](https://developer.microsoft.com/games/xbox/xboxlive/creator) 같은 여러 개발자 프로그램을 제공 하 여 게임을 개발 하 고 게시 하는 데 도움을 줍니다.

## <a name="learning-resources"></a>학습 리소스

* 빌드 2016: [CodeLabs &mdash; Microsoft Azure App Service 및 Microsoft SQL Azure 백 엔드를 사용 하 여 Unity에서 게임 점수를 저장 합니다](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure) .
* 빌드 2017: [Microsoft Azure을 사용 하 여 최고 수준의 게임 환경 제공: Halo, io-interactive 및 이동 데드 (비디오)와 같은 제목에서 얻은 교훈](https://channel9.msdn.com/Events/Build/2017/P4062)
* GitHub에서 Azure를 사용 하 여 일반적인 게임 워크 로드를 지원 하도록 설계 된 재사용 가능한 문서 블록, 프로젝트, 서비스 및 모범 사례 집합: [azure에서 게임에 대 한 빌딩 블록](https://github.com/MicrosoftDX/nether)
* [Azure의 게임 서비스 (비디오)](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>도구 및 기타 유용한 링크

* [MSDN 포럼 &mdash; Azure platform](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [클라우드 기반 부하 테스트 도구](https://visualstudio.microsoft.com/team-services/cloud-load-testing/)
* [Sdk 및 명령줄 도구](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>게임 백 엔드를 위한 Software as a Service

[Playfab](https://playfab.com/) 은 현재 8000만 달 활성 플레이어로 1200 이상의 라이브 게임을 진행 합니다. 실시간 제어를 사용 하는 전체 stack LiveOps를 포함 하는 전체 백 엔드 플랫폼입니다. 

Sdk를 사용 하 여 모바일, PC 또는 콘솔 게임에이 솔루션을 통합할 수 있습니다. Android, iOS, Unreal, Unity 및 Windows를 포함 하 여 인기 있는 모든 게임 엔진 및 플랫폼에 사용할 수 있는 Sdk가 있습니다. 시작 하려면 [설명서](https://api.playfab.com/)를 참조 하세요.

이 제품은 인증, 플레이어 데이터 관리, 멀티 플레이 및 실시간 분석과 같은 게임 서비스를 제공 하 여 사용자가 게임을 확장 하는 데 도움이 됩니다. 실시간 데이터 파이프라인 및 LiveOps의 강력한 기능을 활용 하 여 사용자 지정 된 게임 항목, 이벤트 및 판촉을 사용자에 게 참여 하세요. 또한 A/B 테스트를 수행 하 고, 보고서를 생성 하 고, 푸시 알림을 보낼 수 있습니다. 

지속적으로 혁신 새 기능을 추가 하 고 있습니다. 자세한 내용은 [기능](https://playfab.com/features/) 및 가격 책정에 대 한 [간단한 가격 책정](https://playfab.com/pricing/)을 참조 하세요.

## <a name="related-links"></a>관련 링크

* [Windows 10 게임 개발 가이드](./e2e.md)
* [게임용 Azure](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [Microsoft for Startups](https://startups.microsoft.com)
* [ID@Xbox](https://www.xbox.com/Developers/id)


 

 