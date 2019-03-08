---
title: EDS 권한 부여
assetID: bd0bdc8e-084a-7140-98da-6d3721bda112
permalink: en-us/docs/xboxlive/rest/edsauthorization.html
description: " EDS 권한 부여"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3e5c5ef3bf3c864215544391bc291a26f6c05d0f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607608"
---
# <a name="eds-authorization"></a>EDS 권한 부여
 
  * [소개](#ID4EN)
  * [권한 부여 프로세스](#ID4EFB)
  * [3.0 토큰: 다중 사용자 vs입니다. 단일 사용자](#ID4EEC)
  * [EDS 다중 사용자 지원 하나요?](#ID4EYC)
 
<a id="ID4EN"></a>

 
## <a name="introduction"></a>소개
 
엔터테인먼트 검색 서비스 (EDS) 3.1 익명 트래픽을 지원 하지 않습니다. 인증이 EDS에 대 한 모든 요청에 대 한 필요 합니다. EDS는 XToken 제대로 클라이언트를 인증 하는 호출자가 필요 합니다. 이러한 토큰 XSTS에서 생성 되 고 다양 한 Xbox 인증 서비스 (XAS)을 통해 얻을 수 있습니다. 장치, 사용자와 토큰의 id 정의 모든 하는 제목에 대 한 별도 인증 서비스가 있습니다.
 
XSTS는 Xbox LIVE의 게이트 키퍼는입니다. 사용자 또는 장치를 Xbox LIVE 서비스에 연결할 권한이 있는지 확인 하려면 첫 번째 방어선 것입니다. 사용자를 인증 한 후의 XSTS 서비스에서 모든 구성 요소를 안전 하 게 자신을 식별 하는 데 사용할 수 있는 XToken를 생성 합니다. 이 XToken passport to LIVE입니다.
 
사용자 및 서비스를 사용 하려면 작업을 합니다. 그리고 대부분의 이러한 작업 및 사용자 서비스를 사용할 수 있습니다. 하지만 어떻게 해야 합니까 작업 사용자를 가장 하지는 및 사용자 본인이 실제로? 다른 사용자에 게 자신을 식별 하는 데 사용할 수 있는 토큰을 사용 하 여 제공 했습니다.
 
이러한 토큰은 XSTS에서 생성 되 고 XTokens 라고 일반적으로 합니다. XToken 여러 형식으로 가져올 수 있습니다 및 다양 한 다른 항목을 포함 하는 토큰을 포함 하는 데 사용 되는 광범위 한 용어 이지만 모든, 서명 됨, 만들어지고 XSTS 서버에서 필요에 따라 암호화 합니다. 내부적으로 XSTS 사용 하 여 MXAN 만들고 토큰 형식을 지정 합니다. MXAN 어느는 XToken에서 정보를 추출 하는 유일한 구성 요소 이며 토큰을 사용 하는 서비스 깨진 MXAN을 요청 헤더를 전달 합니다. 토큰 처리 되 고 유효성을 검사 하 고 토큰에 표현 된 클레임은 서비스에 반환 합니다. 서비스 수를 사용 하 여 이러한 클레임 값을 호출 하는 사용자 또는 장치를 식별 하 고 해당 정보를 기반으로 하는 작업을 수행 합니다.
 
사용자, 장치 및 제목-에 대 한 기본적인 id 토큰-는 Xbox 인증 서비스 (XAS)에서 제공 됩니다. 각 XAS는 담당 하는 다양 한 클레임에 대 한 값을 지정 하는 id 토큰을 생성 하는 일을 담당 합니다.
 
   * (장치에 대 한 XAS) XASD: 장치 id를 제공 하는 DToken 만듭니다
   * (사용자에 대 한 XAS) XASU: 사용자 id를 제공 하는 UToken 만듭니다
   * (제목에 대 한 XAS) XAST: 제목 id를 제공 하는 TToken 만듭니다
   
<a id="ID4EFB"></a>

 
## <a name="authorization-process"></a>권한 부여 프로세스
 
   * 하나 이상의 id 토큰을 가져옵니다. XToken 최대 중 각 D, U 및 T 토큰을 요청할 수 있습니다. D 또는 21. 중 하나 이상을 제공 해야 합니다. 
     * 장치 인증 세부 정보를 제공 하 여를 DToken XASD에서 요청
     * 특정 형태의 사용자 인증을 사용 하 여 XASU에서는 UToken를 요청 합니다. 사용자 인증은 아마도 MSA (RP) 토큰의 형태로 제공 됩니다.
     * XAST에서는 TToken를 요청 합니다. 사용할 수 있는 제목을 XAST에는 DToken도 제공 해야 하므로 현재 실행 중인 플랫폼에 따라 달라 집니다.
  
   * XSTS 요청을 만듭니다.
 
     * 에 대 한 토큰을 요청 하는 신뢰 당사자를 정의 합니다.
     * D, U 및/또는 T 토큰 요청 속성을 채웁니다.
    * XSTS 요청을 실행 하 고 결과 XToken를 캐시 합니다. 반환 된 XToken 포함 (최대) 모든 장치, 사용자 및 제목에서 id 정보를 id 토큰 및 모든 추가 클레임 (현재 구독 상태, 사용자 그룹 등).
   
<a id="ID4EEC"></a>

 
## <a name="30-tokens-multiuser-vs-single-user"></a>3.0 토큰: 다중 사용자 vs입니다. 단일 사용자
 
다음은 3.0 토큰의 형식입니다. `XBL3.0 x=&lt;hash>;&lt;token>`
 
에 따라는 &lt;해시 >, 토큰을 다르게 처리 됩니다.
 
   * 경우 &lt;해시 > 값과 같음 * (별표), 특정 사용자의 요청을 수행 하 고 토큰의 모든 사용자가 deserialize 된 보안 주체에 있습니다. 이것이 true 다중 사용자 형식입니다.
   * 경우 &lt;해시 >가 같으면 (대시로)에 없는 사용자가 요청을 수행 합니다. Deserialize 된 보안 주체의 모든 사용자가 제거 됩니다.
   * 경우 &lt;해시 > 같지 않은 * 또는-토큰에는 사용자 요청을 나타내는 식별자 됩니다. 지정 된 사용자만 deserialize 된 보안 주체에 있게 됩니다. 다른 모든 사용자가 제거 됩니다. 이것은 단일 사용자 3.0 토큰입니다.
   
<a id="ID4EYC"></a>

 
## <a name="does-eds-support-multi-users"></a>EDS 다중 사용자 지원 하나요?
 * 대답은 안 된다야. 설명 된 경우에서 콘솔을 단일 사용자 토큰을 항상 전송 됩니다. 여러 사용자의 로그인 인 경우에는 표시 된 "호출자" 다른 모든 id을 놓을 위치 여야 합니다.
  
<a id="ID4E6C"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EBD"></a>

 
##### <a name="parent"></a>Parent  

[추가 참조](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4END"></a>

 
##### <a name="further-information"></a>추가 정보 

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

   