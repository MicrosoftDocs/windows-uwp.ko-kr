---
title: EDS 권한 부여
assetID: bd0bdc8e-084a-7140-98da-6d3721bda112
permalink: en-us/docs/xboxlive/rest/edsauthorization.html
author: KevinAsgari
description: " EDS 권한 부여"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c4233555093823f09c6c43d92f6863574191068f
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5547623"
---
# <a name="eds-authorization"></a>EDS 권한 부여
 
  * [소개](#ID4EN)
  * [인증 프로세스](#ID4EFB)
  * [3.0 토큰: 단일 및 다중 사용자](#ID4EEC)
  * [EDS 여러 사용자를 지원 합니까?](#ID4EYC)
 
<a id="ID4EN"></a>

 
## <a name="introduction"></a>소개
 
엔터테인먼트 검색 서비스 (EDS) 3.1 익명 트래픽을 지원 하지 않습니다. 인증이 모든 요청 EDS에 대 한 필요 합니다. EDS는 XToken 제대로 클라이언트를 인증 하는 호출자가 필요 합니다. 이러한 토큰 XSTS에 의해 생성 되 고 다양 한 Xbox 인증 서비스 (XAS)을 통해 얻을 수 있습니다. 디바이스, 사용자 및 토큰의 id를 모두 정의는 타이틀을 위한 별도 인증 서비스 있습니다.
 
XSTS는 Xbox LIVE에 대 한 키퍼입니다. 사용자 또는 디바이스의 Xbox LIVE 서비스에 연결할 권한이 있는 경우 확인 하려면 첫 번째 방어선 것입니다. 사용자를 인증 한 후의 XSTS 안전 하 게 서비스에서 구성 요소에 자신을 식별 하는 데 사용할 수 있는 XToken을 생성 합니다. 이 XToken LIVE에 passport입니다.
 
사람 및 서비스를 사용할 것입니다. 및 우리는 대부분의 이러한 작업 및 사용자가 서비스를 사용할 수 있습니다. 하지만 어떻게 해야 합니까 작업 사람, 가장 하지는 및 사용자는 실제로 본인이? 다른 사람에 게 자신을 식별 하는 데 사용할 수 있는 토큰을 사용 하 여을 제공 합니다.
 
이러한 토큰은 XSTS 생성 및 Xtoken 이라고 일반적으로 합니다. XToken는 여러 가지 다양 한을 포함 하며 다른 여러 가지 형태로 발생할 수 있는 토큰을 설명 하는 데 사용 되는 광범위 한 용어 하지만 모든 생성, 서명 및 필요에 따라 XSTS 서버에 의해 암호화 합니다. 내부적으로 XSTS을 만들고 서식을 토큰 MXAN를 사용 합니다. MXAN 적이 정보는 XToken 추출 하는 유일한 구성 요소입니다. 요청 헤더를 크랙 될 MXAN을 전달 하는 토큰을 사용 하는 서비스입니다. 토큰 처리 되 고 유효성을 검사 하 고 서비스에는 토큰에 표현 클레임 반환 됩니다. 서비스 수 있는 다음 사용 하 여 이러한 클레임 값을 호출 하는 사용자 또는 장치를 식별 하 고 해당 정보를 기반으로 작업을 수행 합니다.
 
사용자, 장치 및-제목에 대 한 기본 id 토큰-는 Xbox 인증 서비스 (XAS)으로 제공 됩니다. 각 XAS는 담당 하는 다양 한 클레임에 대 한 값을 지정 하는 identity 토큰을 생성 해야 합니다.
 
   * XASD (XAS 장치용): 장치 id를 제공 하는 DToken를 만듭니다.
   * XASU (사용자에 대 한 XAS): 사용자 id를 제공 하는 UToken를 만듭니다.
   * XAST (제목에 대 한 XAS): 제목 id를 제공 하는 TToken를 만듭니다.
   
<a id="ID4EFB"></a>

 
## <a name="authorization-process"></a>인증 프로세스
 
   * 하나 이상의 id 토큰을 가져옵니다. 각 중 하나만 D, U 및 T 토큰을 사용 하 여는 XToken을 요청할 수 있습니다. D, 또는 끝내려면 중 하나 이상 제공 해야 합니다. 
     * 디바이스 인증 정보를 제공 하 여 XASD에서는 DToken 요청
     * 일부 형태의 사용자 인증을 사용 하 여 XASU에서는 UToken를 요청 합니다. 아마도 사용자 인증 토큰이 MSA (RPS)의 형태로 제공 됩니다.
     * XAST에서는 TToken를 요청 합니다. 사용할 수 있는 제목을 XAST 하는 DToken도 제공 해야 하므로 현재 실행 중인 플랫폼에 따라 달라 집니다.
  
   * XSTS 요청을 만듭니다.
 
     * 토큰을 요청 하는 신뢰 당사자를 정의 합니다.
     * D, U, 및/또는 T 토큰을 사용 하 여 요청 속성을 채웁니다.
    * XSTS 요청을 실행 하 고 결과 XToken을 캐시 합니다. 반환 된 XToken (최대) 포함 된 모든 id 토큰 및 추가 클레임 (예: 현재 구독 상태, 사용자 그룹)에서 디바이스, 사용자 및 제목 id 정보.
   
<a id="ID4EEC"></a>

 
## <a name="30-tokens-multiuser-vs-single-user"></a>3.0 토큰: 단일 및 다중 사용자
 
다음은 3.0 토큰의 형식입니다. `XBL3.0 x=&lt;hash>;&lt;token>`
 
에 따라 합니다 &lt;해시 >, 토큰 다르게 처리 됩니다.
 
   * 하는 경우 &lt;해시 > 같으면 * (별표) 특정 사용자의 요청을 수행 하 고 역직렬화 된 계정에 존재 하는 토큰에 있는 모든 사용자. 다중 사용자는 true입니다.
   * 하는 경우 &lt;해시 > 요청을 수행 하는 사용자가 같은-(dash)가 있습니다. 역직렬화 된 계정에 사용자가 제거 됩니다.
   * 하는 경우 &lt;해시 > 같지 않은 * 또는-다음 나타내는 토큰에는 사용자 요청 하는 식별자입니다. 지정 된 사용자만 역직렬화 된 계정에 표시 됩니다. 다른 모든 사용자가 제거 됩니다. 단일 사용자 3.0 토큰입니다.
   
<a id="ID4EYC"></a>

 
## <a name="does-eds-support-multi-users"></a>EDS 여러 사용자를 지원 합니까?
 * 대답은 안 된다야. 콘솔 설명 된 경우, 단일 사용자 토큰 전송 합니다. 로그인 한 경우에 여러 사용자 인 경우에 표시 된 "발신자를"를 다른 모든 id 삭제 됩니다 이어야 합니다.
  
<a id="ID4E6C"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBD"></a>

 
##### <a name="parent"></a>부모  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4END"></a>

 
##### <a name="further-information"></a>자세한 정보 

[마켓플레이스 URI](../uri/marketplace/atoc-reference-marketplace.md)

   