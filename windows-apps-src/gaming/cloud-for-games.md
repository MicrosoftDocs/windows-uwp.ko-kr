---
author: joannaleecy
title: "UWP 게임에 클라우드 서비스 사용"
description: "UWP 게임에 대한 백 엔드로 클라우드를 구현하는 방법을 알아봅니다."
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
translationtype: Human Translation
ms.sourcegitcommit: 0b2d81daa8bd0fd5694b81fa14fcd064e1600d35
ms.openlocfilehash: b23c33fac9ac8fe5e2d5563a0af6824c82a3969b

---
#  UWP 게임에 클라우드 서비스 사용

Windows 10의 UWP(유니버설 Windows 플랫폼)는 Microsoft 디바이스 간의 게임 개발에 사용할 수 있는 API 집합을 제공합니다. 플랫폼 및 디바이스 간의 게임을 개발하는 경우 수요에 따라 게임을 확장하기 위해 클라우드 백 엔드를 활용할 수 있습니다.

##  클라우드 컴퓨팅이란?

클라우드 컴퓨팅은 인터넷을 통해 요청 시 IT 리소스와 응용 프로그램을 사용하여 디바이스에 대한 데이터를 저장 및 처리합니다. _클라우드_란 용어는 비특정 위치에서 액세스할 수 있는 방대한 외부 리소스(로컬 리소스 아님)의 가용성을 상징적으로 비유한 것입니다.
클라우드 컴퓨팅의 원칙은 리소스와 소프트웨어를 사용할 수 있는 새로운 방법을 제공하는 것입니다. 사용자는 더 이상 전체 제품이나 리소스의 요금을 미리 지불할 필요가 없으며 플랫폼, 소프트웨어 및 리소스를 서비스로 사용할 수 있습니다. 클라우드 공급자는 사용량이나 서비스 계획 제공에 따라 고객에게 요금을 청구하는 경우가 많습니다.

##  클라우드 서비스를 사용하는 이유

게임에 클라우드 서비스를 사용할 경우의 한 가지 장점은 물리적 하드웨어에 미리 투자할 필요 없이 이후 단계에서 사용량이나 서비스 계획에 따라 요금을 지불하면 된다는 것입니다. 이는 새 게임 타이틀 개발과 관련된 위험을 관리하는 데 도움이 되는 한 가지 방법입니다. 

또 다른 장점은 게임이 방대한 클라우드 리소스를 활용하여 확장성을 실현할 수 있다는 것입니다(동시 플레이어 수, 리소스를 많이 사용하는 실시간 게임 계산 또는 데이터 요구 사항의 갑작스러운 급증을 효과적으로 관리). 이 경우 게임 성능이 항상 안정적인 상태로 유지됩니다. 또한 세계 어디서나 어떤 플랫폼에서 실행되는 어떤 디바이스에서도 클라우드 리소스에 액세스할 수 있으므로 전 세계 모든 사용자에게 게임을 제공할 수 있습니다.

플레이어에게 뛰어난 게임 플레이 환경을 제공하는 것이 중요합니다. 클라우드에서 실행되는 게임 서버는 클라이언트 쪽 업데이트와 별개이므로 전반적으로 보다 제어되고 안전한 게임 환경을 제공할 수 있습니다.   또한 클라이언트를 신뢰하지 않고 서버 쪽 게임 논리를 포함하여 클라우드를 통해 게임 플레이 일관성을 얻을 수도 있습니다. 서비스 간 연결을 구성하여 보다 통합된 게임 환경을 허용할 수도 있습니다. 예를 들어 게임에서 바로 구매를 다양한 결제 방법에 연결, 다양한 게임 네트워크를 통해 브리징, Facebook, Twitter 등의 인기 있는 소셜 미디어 포털에 게임 내 업데이트 공유 등이 포함됩니다. 

전용 클라우드 서버를 사용하여 큰 영구적 게임 월드를 만들고, 게이머 커뮤니티를 구축하고, 시간에 따른 게이머 데이터를 수집 및 분석하여 게임 플레이를 개선하고, 게임의 수익 창출 디자인 모델을 최적화할 수도 있습니다.

또한 비동기 멀티 플레이 기술을 사용하는 소셜 게임처럼 리소스를 많이 사용하는 게임 데이터 관리 기능이 필요한 게임을 클라우드 서비스로 구현할 수 있습니다.

##  게임 회사에서 클라우드 기술을 사용하는 방법

다른 개발자가 게임에서 클라우드 솔루션을 구현한 방법을 알아봅니다.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header">
        <th>개발자</th>
        <th>설명</th>
        <th>주요 게임 시나리오</th>
        <th>세부 정보</th>
    </tr>
    <tr>
        <td>[343 산업](https://www.halowaypoint.com/)</td>
        <td>_Halo 5: Guardians_는 자동 인덱싱 기능으로 인한 속도와 유연성 때문에 선택된 Microsoft Azure DocumentDB를 사용하여 [Halo: Spartan Companies](https://www.halowaypoint.com/spartan-companies)를 소셜 게임 플레이 플랫폼으로 구현했습니다.</td>
        <td>
            <ul>
                <li>멀티 플레이 게임에 대한 그룹 생성/관리를 처리할 확장 가능한 데이터 계층 <li>게임 및 소셜 미디어 통합 <li>여러 특성을 통해 데이터의 실시간 쿼리 <li>게임 플레이 도전 과제 및 통계 동기화 </ul>
        </td>
        <td>
            <ul>
                <li>[Azure DocumentDB를 사용하여 구현된 소셜 게임 플레이](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/)</td>
            </ul>
    </tr>
    <tr>
        <td>[Illyriad Games](http://web.ageofascent.com/)</td>
        <td>Illyriad Games는 최신 브라우저가 있는 디바이스에서 플레이할 수 있는 서사적 MMO(다중 접속 온라인) 3D 우주 게임인 _Age of Ascent_를 만들었습니다. 따라서 이 게임은 PC, 노트북, 휴대폰 및 기타 모바일 디바이스에서 플러그 인 없이 재생할 수 있습니다. 게임은 ASP.NET Core, HTML5, WebGL 및 Microsoft Azure를 사용합니다.</td>
        <td>
            <ul>
                <li>플랫폼 간 브라우저 기반 게임 <li>하나의 큰 영구적 개방형 월드 <li>리소스를 많이 사용하는 실시간 게임 플레이 계산 처리 <li>플레이어 수에 따라 확장 </ul>
        </td>
        <td>
            <ul>
                <li>[Azure 서비스 패브릭을 사용하여 게임 구성 요소를 마이크로 서비스로 관리(동영상)](https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s)  
                <li>[Age of Ascent 개발자 인터뷰(동영상)](https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET)
            </ul>
        </td>
    </tr>
    <tr>
        <td>[Next Games](http://www.nextgames.com/)</td>
        <td>Next Games는 AMC의 원작 시리즈를 기반으로 하는 _The Walking Dead: No Man's Land_ 비디오 게임입니다. The Walking Dead 게임은 Azure를 백 엔드로 사용했습니다. 개봉 주말과 첫 주에 1,000,000건의 다운로드가 발생했으며, 미국 앱 스토어에서 iPhone 및 iPad 무료 앱 분야 1위, 12개 국가에서 무료 앱 1위, 13개 국가에서 무료 게임 1위를 차지했습니다.
        </td>
        <td>
            <ul>
                <li>플랫폼 간 <li>순서 기반 멀티 플레이 <li>탄력적인 성능 확장 </ul>
        </td>
        <td>
            <ul>
                <li>[Next Games CTO인 Kalle Hiitola 인터뷰(동영상)](https://channel9.msdn.com/Blogs/AzureDocumentDB/azure-documentdb-walking-dead)
                <li>[Walking Dead는 보다 빠른 개발 주기와 매력적인 게임 플레이를 위해 DocumentDB를 사용합니다.](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/)
            </ul>
    </tr>
    </td>
        <td>[Pixel Squad](http://www.crimecoast.com/)</td>
        <td>Pixel Squad는 Unity 게임 엔진과 Azure를 사용하여 _Crime Coast_를 개발했습니다. _Crime Coast_는 Android, iOS 및 Windows 플랫폼에서 사용할 수 있는 소셜 전략 게임입니다. Azure Blob 저장소, Managed Azure Redis Cache, 부하 조정된 IIS VM 배열, Microsoft 알림 허브가 게임에 사용되었습니다. 확장을 관리하고 5000명의 동시 플레이어가 있는 플레이어 급증을 처리한 방법을 알아봅니다.
        </td>
        <td>
            <ul>
                <li>플랫폼 간 <li>온라인 멀티 플레이 게임 <li>플레이어 수에 따라 확장 </ul>
        </td>
        <td>
            <ul>
                <li>[Crime Coast MMO 게임에서 Azure 클라우드 서비스를 사용한 방법](https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te)
            </ul>
        </td>
    </tr> 
</table>

    
### 다른 링크

* [Hitcents, Game Troopers 및 InnoSpark의 비법인 Azure](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Azure를 사용하여 Bizspark 프로그램에서 게임 시작](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## 클라우드 백 엔드를 디자인하는 방법

제작자와 게임 디자이너가 게임에 필요한 게임 기능과 특성에 대해 논의하는 동안 게임 인프라의 디자인 방법을 고려하는 것이 좋습니다. 다양한 디바이스 및 여러 주요 플랫폼용 게임을 개발하려는 경우 Azure 클라우드를 게임 백 엔드로 사용할 수 있습니다.

### 단계별 학습 가이드

* [빌드 2016 Codelab: Microsoft Azure 앱 서비스 및 Microsoft SQL Azure 백 엔드를 사용하여 게임 점수 저장](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* [게임의 모바일 참여 전략 디자인](https://azure.microsoft.com/documentation/articles/mobile-engagement-gaming-scenario/)
* [Unity iOS 배포에 Azure Mobile Engagement 사용](https://azure.microsoft.com/documentation/articles/mobile-engagement-unity-ios-get-started/)

### IaaS, PaaS 또는 SaaS 이해

먼저 게임에 가장 적합한 서비스 수준을 고려해야 합니다. 다음 세 가지 서비스의 차이점을 알면 백 엔드 구축에 사용할 방법을 결정하는 데 도움이 됩니다.

* [IaaS(Infrastructure as a Service)](https://azure.microsoft.com/overview/what-is-iaas/)

    IaaS(Infrastructure as a Service)는 인터넷을 통해 프로비전 및 관리되는 인스턴트 컴퓨팅 인프라입니다. 여러 컴퓨터를 쉽게 사용하여 요청 시 빠르게 확장 및 축소할 수 있다고 상상해 보세요. IaaS는 물리적 서버와 기타 데이터 센터 인프라를 구입하고 관리하는 비용 및 복잡성을 방지하는 데 도움이 됩니다.

* [PaaS(Platform as a Service)](https://azure.microsoft.com/overview/what-is-paas/)

    PaaS(Platform as a Service)는 IaaS와 유사하지만 서버, 저장소, 네트워킹 등의 인프라 관리도 포함합니다. 따라서 물리적 서버와 데이터 센터 인프라를 구입하지 않는 것은 물론 소프트웨어 라이선스, 기본 응용 프로그램 인프라, 미들웨어, 개발 도구 또는 기타 리소스도 구입 및 관리할 필요가 없습니다.

* SaaS(Software as a Service)

    SaaS(Software as a Service)는 일반적으로 이미 빌드되어 기존 클라우드 플랫폼에서 호스트된 응용 프로그램입니다. 해당 서비스에서 보다 쉽게 게임 실행을 시작할 수 있도록 설계되었습니다.


### Azure를 사용하여 게임 인프라 디자인

다음은 Azure 클라우드 서비스를 게임에 사용할 수 있는 몇 가지 방법입니다. Azure는 Windows, Linux 및 Ruby, Python, Java, PHP 등의 익숙한 오픈 소스 기술에서 작동합니다. 자세한 내용은 [Azure 클라우드](https://azure.microsoft.com)를 참조하세요.

| 요구 사항                 | 활동 시나리오                            | 제품 제공                      | 제품 기능                               |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| 클라우드에서 도메인 호스트     | 효율적으로 DNS 쿼리에 응답            | [Azure DNS](https://azure.microsoft.com/services/dns/) | 고성능 및 가용성을 사용하여 도메인 호스트  |
| 로그인, ID 확인      | 게이머 로그인 및 게이머 ID가 인증됩니다.  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | 다단계 인증을 사용하여 클라우드 및 온-프레미스 웹앱에 대한 Single Sign-On            |
| IaaS(Infrastructure as a Service) 모델을 사용하는 게임      | 게임이 클라우드의 가상 컴퓨터에 호스트됩니다.       | [Azure VM](https://azure.microsoft.com/services/virtual-machines/) | 기본 제공 가상 네트워킹 및 부하 분산을 사용하여 1개에서 수천 개의 가상 컴퓨터 인스턴스까지 게임 서버로 확장, 온-프레미스 시스템을 사용한 하이브리드 일관성           |
| PaaS(Platform as a Service) 모델을 사용하는 웹 또는 모바일 게임            | 게임이 관리되는 플랫폼에서 호스트됩니다.                | [Azure 앱 서비스](https://azure.microsoft.com/services/app-service/) | 웹 사이트 또는 모바일 게임에 대한 PaaS(미들웨어/개발 도구/BI/DB 관리 및 Azure VM)   |
| 게임 데이터에 대한 클라우드 저장소       | 최신 게임 데이터가 클라우드에 저장되고 클라이언트 디바이스로 전송됩니다. | [Azure Blob 저장소](https://azure.microsoft.com/services/storage/blobs/)| 저장할 수 있는 파일 종류에 제한이 없습니다. 이미지, 오디오, 동영상 등 다량의 구조화되지 않은 데이터에 대한 개체 저장소입니다.  |
| 임시 데이터 저장소 테이블| 게임 트랜잭션(게임 상태 변화)이 일시적으로 테이블에 저장됩니다. | [Azure 테이블 저장소](https://azure.microsoft.com/services/storage/tables/)| 게임의 요구에 따라 유연한 스키마에 게임 데이터를 저장할 수 있습니다. |
| 게임 트랜잭션/요청 큐| 게임 트랜잭션이 큐의 형태로 처리됩니다. | [Azure 큐 저장소](https://azure.microsoft.com/services/storage/queues/)| 큐는 예기치 않은 트래픽 급증을 흡수하고 서버가 게임 중 갑작스러운 요청 증가로 과부하되지 않도록 방지할 수 있습니다.   |
| 확장 가능한 관계형 게임 데이터베이스| 데이터베이스에 대한 게임 내 트랜잭션과 같은 관계형 데이터의 구조화된 저장소 | [Azure SQL 데이터베이스](https://azure.microsoft.com/services/sql-database/)| 서비스로 제공되는 SQL 데이터베이스([VM의 SQL과 비교](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| 대기 시간이 짧은 확장 가능한 분산 게임 데이터베이스| 스키마 유연성을 사용하여 게임 및 플레이어 데이터의 빠른 읽기, 쓰기 및 쿼리 | [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)| 서비스로 제공되는 대기 시간이 짧은 NoSQL 문서 데이터베이스   |
| Azure 서비스와 함께 고유한 데이터 센터 사용 | 고유한 데이터 센터에서 게임이 검색되고 클라이언트 디바이스로 전송됩니다. | [Azure 스택](https://azure.microsoft.com/overview/azure-stack/) | 보다 효율적인 작업을 위해 조직이 고유한 데이터 센터에서 Azure 서비스를 제공할 수 있도록 합니다.  |
| 큰 데이터 청크 전송| Azure CDN을 사용하여 가장 가까운 CDN(Content Delivery Network) 팝 위치에서 게임 이미지, 오디오, 비디오 등의 큰 파일을 사용자에게 보낼 수 있습니다.    | [Azure Content Delivery Network](https://azure.microsoft.com/services/cdn/) | 큰 중앙 집중식 노드의 최신 네트워크 토폴로지를 기반으로 하는 Azure CDN은 갑작스러운 트래픽 급증과 부하 증가를 처리하여 속도와 가용성을 훨씬 증가시키고 사용자 환경을 개선합니다.  |
| 짧은 대기 시간               | 캐싱을 수행하여 제어가 강화되고 데이터 격리가 보장되는 빠르고 확장 가능한 게임을 빌드합니다. 게임의 일치 기능을 개선하는 데 사용할 수 있습니다. | [Azure Redis Cache](https://azure.microsoft.com/services/cache/) | 처리량이 높고 대기 시간이 짧은 일관성 있는 데이터 액세스를 통해 빠르고 확장 가능한 Azure 응용 프로그램 지원  |
| 높은 확장성, 짧은 대기 시간 | 대기 시간이 짧은 읽기 및 쓰기로 게임 사용자 수의 변동 처리 | [Azure 서비스 패브릭](https://azure.microsoft.com/services/service-fabric/) | 가장 복잡하고 대기 시간이 짧으며 데이터를 많이 사용하는 시나리오를 지원하고 한 번에 더 많은 사용자를 처리하기 위해 안정적으로 확장될 수 있습니다. 서비스 패브릭을 사용하면 상태 비저장 앱에 필요에 따라 별도의 저장소나 캐시를 만들 필요 없이 게임을 빌드할 수 있습니다. |
| 디바이스에서 초당 수백만 개의 이벤트 수집 가능                         | 디바이스에서 초당 수백만 개의 이벤트 기록 | [Azure 이벤트 허브](https://azure.microsoft.com/services/event-hubs/) | 게임, 웹 사이트, 앱 및 디바이스에서 클라우드 규모의 원격 분석 수집  |
| 실시간 게임 데이터 처리  | 실시간 게이머 데이터 분석을 통해 게임 플레이 향상| [Azure 스트림 분석](https://azure.microsoft.com/services/stream-analytics/) | 클라우드에서 실시간 스트림 처리  |
| 예측 게임 플레이 개발         | 게이머 데이터에 따라 사용자 지정 동적 게임 플레이 만들기  | [Azure 기계 학습](https://azure.microsoft.com/services/machine-learning/) | 예측 분석 솔루션을 쉽게 빌드, 배포 및 공유할 수 있도록 하는 완전히 관리되는 클라우드 서비스  |
| 게임 데이터 수집 및 분석| 관계형 및 비관계형 데이터베이스의 대량 병렬 처리 데이터 | [Azure 데이터 웨어하우스](https://azure.microsoft.com/services/sql-data-warehouse/)| 서비스로 제공되는 엔터프라이즈급 기능을 갖춘 탄력적인 데이터 웨어하우스   |
| 마케팅 캠페인을 만들어 사용 및 보존 증가  | 대상 플레이어에게 푸시 알림을 보내 관심을 생성하고 데이터 분석에 따라 특정 게임 작업 권장 | [모바일 참여](https://azure.microsoft.com/services/mobile-engagement/) |  iOS, Android, Windows, Windows Phone 등 모든 주요 플랫폼에서 게임 플레이 시간 및 사용자 보존 증가 |


##  스타트업 및 개발자 리소스

* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)

    Microsoft BizSpark는 Azure 클라우드 서비스, 소프트웨어 및 지원에 대한 무료 액세스를 제공하여 스타트업이 성공하도록 도와주는 글로벌 프로그램입니다. BizSpark 멤버는 각각 매달 $150 Azure 크레딧을 포함하는 Visual Studio Enterprise with MSDN 구독 5개를 받습니다. 따라서 5명 개발자가 Azure 서비스에서 매달 총 $750를 사용할 수 있습니다. BizSpark는 개인이 소유하고 5년 미만이며 연간 수익이 $1M 미만인 스타트업에서 사용할 수 있습니다. Microsoft는 스타트업이 성공할 수 있도록 지원함으로써 소중한 장기 파트너 관계를 구축할 수 있다고 믿습니다.
    
* [ID@Xbox](http://www.xbox.com/Developers/id)

    멀티 플레이 게임, 플랫폼 간 연결, 게임 점수, 도전 과제, 순위표 등의 Xbox Live 기능을 Windows 10 게임에 추가하려는 경우 ID@Xbox에 등록하여 창의력을 발휘하고 성공을 극대화하는 데 필요한 도구와 지원을 받으세요. ID@Xbox에 신청하기 전에 [Windows 개발자 센터](https://developer.microsoft.com/windows/programs/join)에서 개발자 계정을 등록하세요.

## 게임 백 엔드에 대한 SaaS(Software as a Service)

게임 개발에 집중할 수 있도록 주요 클라우드 서비스 공급자에 따라 게임에 대한 클라우드 백 엔드를 제공하는 회사입니다.

* [GameSparks](http://www.gamesparks.com/)

    GameSparks는 모든 게임의 서버 쪽 빌드에 사용할 수 있는 게임 개발자를 위한 클라우드 기반 개발 플랫폼입니다.

* [Photon Engine](https://www.photonengine.com/en/Photon)

    Photon은 게임용 독립 네트워킹 엔진 및 멀티 플레이 플랫폼입니다. SaaS(Software as a Service)를 제공하므로 완전히 관리되는 서비스인 Photon Cloud를 제공합니다. 호스트하는 동안 응용 프로그램 클라이언트에 완전히 집중할 수 있으며, 서버 작업 및 확장은 모두 Exit Games에서 처리합니다.

* [Playfab](https://playfab.com/)

    Playfab은 모바일, PC 또는 콘솔 게임에 세계 정상급 라이브 게임 관리 및 백 엔드 기술을 간단하고 빠르게 제공합니다.

## 관련 링크

* [Windows 10 게임 개발 가이드](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [Microsoft Azure](https://azure.microsoft.com/)
* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 



<!--HONumber=Aug16_HO3-->


