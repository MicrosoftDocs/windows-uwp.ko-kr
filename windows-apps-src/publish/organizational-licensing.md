---
Description: 앱 제출 시 조직 라이선스 섹션에서 비즈니스용 Microsoft 스토어 및 교육용 Microsoft 스토어를 통해 앱을 대량 구매용으로 제공할지 여부 및 그 방법을 지정할 수 있습니다.
title: 조직 라이선스 옵션
ms.assetid: 1EB139B0-67E7-4F66-AAEF-491B1E52E96F
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, 비즈니스용 Store, 교육용 Store, 조직, 볼륨 라이선싱, 엔터프라이즈, 교육 Store, 비즈니스 Store, 대량 구매, 대량
localizationpriority: high
ms.openlocfilehash: 4babaa9bca13fe1861d5772ba70ec5998cd06285
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600978"
---
# <a name="organizational-licensing-options"></a>조직 라이선스 옵션


앱 제출 시 [가격 책정 및 가용성](set-app-pricing-and-availability.md#organizational-licensing) 페이지의 **조직 라이선스** 섹션에서 비즈니스용 Microsoft 스토어 및 교육용 Microsoft 스토어를 통해 앱을 대량 구매용으로 제공할지 여부 및 그 방법을 지정할 수 있습니다.

이러한 설정을 통해 앱을 사용할 수 있도록 선택할 수 있습니다를 얻어 Windows 10에서 조직에 범위는 늘리는 기회를 제공 하는 해당 사용자에 대 한 여러 라이선스를 배포 하는 조직 (비즈니스 및 교육용) 장치 형식, Pc, 태블릿 및 휴대폰 등.

또한 엔터프라이즈에 직접 게시하는 [LOB(기간 업무) 앱](distribute-lob-apps-to-enterprises.md)에 대해 조직 라이선스를 허용해야 합니다.

> [!NOTE]
> 각 앱에 대한 선택은 서로 독립적으로 구성됩니다. 언제든지 새 제출을 만들어 앱의 기본 설정을 변경할 수 있으며, 변경 내용은 제출에서 [인증 프로세스](the-app-certification-process.md)를 완료한 후 적용됩니다.

> [!IMPORTANT]
> [Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)를 사용하는 제출은 비즈니스용 Microsoft Store 및 교육용 Microsoft Store에서 사용할 수 없습니다. 앱에서 사용할 수 있도록 볼륨 구입에 대 한 조직에서 하려면 만들기 하 고 파트너 센터에서 제출 내용을 제출 해야 합니다.


## <a name="allowing-your-app-to-be-offered-to-organizations"></a>앱을 조직에 제공하도록 허용

**스토어에서 관리(온라인)하는 라이선싱 및 배포를 보유한 조직이 내 앱을 사용할 수 있도록 설정** 상자는 기본적으로 선택되어 있습니다. 이는 스토어의 온라인 라이선싱 시스템을 통해 앱 라이선스를 관리하는 조직에서 볼륨을 취득할 수 있는 앱 카탈로그에 앱을 포함하기를 원함을 의미합니다.

> [!NOTE]
> 모든 조직에서 앱을 사용할 수 있음을 보장하지는 않습니다.

볼륨을 취득하는 조직에 앱을 제공하도록 허용하지 않으려면 이 상자의 선택을 취소합니다. 이 변경 내용은 앱에서 인증 프로세스를 완료한 후에만 적용됩니다. 조직에서 이전에 앱의 라이선스를 취득한 경우 이러한 라이선스는 여전히 유효하며, 이미 앱을 소유한 사람은 앱을 계속 사용할 수 있습니다.

> [!TIP]
> LOB(기간 업무) 앱을 특정 조직에만 게시하려면 엔터프라이즈 연결을 설정하고 조직이 직접 개인 저장소에 앱을 추가할 수 있도록 허용하면 됩니다. 자세한 내용은 [엔터프라이즈에 LOB 앱 배포](distribute-lob-apps-to-enterprises.md)를 참조하세요.


## <a name="allowing-disconnected-offline-licensing"></a>연결이 끊어진(오프라인) 라이선스 허용

많은 조직에서 오프라인 라이선스로 앱을 사용할 수 있기를 원합니다. 예를 들어 일부 조직에서는 인터넷에 연결되지 않는 디바이스에 앱을 배포해야 합니다. 이러한 고객에게 앱을 제공하려면 **조직을 위해 조직에서 관리하는(오프라인) 라이선싱 및 배포 허용** 상자를 선택합니다.

이 상자는 기본적으로 **선택 취소** 상태입니다. 조직에서 관리하는(오프라인) 라이선싱을 사용하여 앱을 설치할 것으로 확인된 조직에서 앱을 사용할 수 있도록 허용하려면 상자를 선택해야 합니다. 조직에서 이 방식으로 최종 사용자에게 유료 앱을 설치하려면 추가 유효성 검사를 거쳐야 합니다.

오프라인 라이선싱을 통해 조직에서는 앱을 볼륨 단위로 구입한 후 각 디바이스를 스토어의 라이선싱 시스템에 연결할 필요 없이 앱을 설치할 수 있습니다. 조직에서는 특정 디바이스를 사용할 수 있을 때 스토어에 알리지 않고 자체 관리 도구를 통해 또는 OS 이미지에 앱을 사전 로드하여 디바이스에 설치할 수 있는 라이선스와 함께 앱 패키지를 다운로드할 수 있습니다. 이 시나리오를 사용하면 배포 유연성이 크게 증가하며 이러한 고객이 앱에 대해 느끼는 매력을 크게 높일 수 있습니다.

> [!IMPORTANT]
> .xap 패키지에 대한 오프라인 라이선싱은 지원되지 않습니다.

 
## <a name="paid-app-support"></a>유료 앱 지원

현재 특정 시장에 위치한 개발자 계정은 비즈니스용 Microsoft 스토어를 통해 유료 앱의 대량 구매 서비스를 제공할 수 있습니다. 

> [!NOTE]
> 일부 시장에서는 비즈니스용 Microsoft Store 또는 교육용 Microsoft Store에서 표시되는 앱 가격이 동일한 기준 가격에 대해 Microsoft Store 소매 고객에게 표시되는 가격과 다를 수 있습니다. 조직 구매를 통해 얻은 수익 지급액은 앱을 소비자가 구매한 경우와 동일하게 적용됩니다. 자세한 내용은 [지급 받기](getting-paid-apps.md) 및 [앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)을 참조하세요. 비즈니스용 Microsoft 스토어 및 교육용 Microsoft 스토어가 제공되는 시장의 목록은 [비즈니스용 Microsoft 스토어 및 교육용 Microsoft 스토어 개요](https://technet.microsoft.com/itpro/windows/manage/windows-store-for-business-overview#supported-markets)를 참조하세요.

국가 또는 지역이 아래에 나열되지 않은 경우에는 유료 앱이 비즈니스용 Microsoft 스토어 및 교육용 Microsoft 스토어에서 현재 제공되지 않고 있다는 뜻입니다. 이 경우, 나중에 추가 개발자 계정 시장에서 제출 지원을 추가할 수 있기 때문에 유료 앱에 대해 선택한 조직 라이선스가 나중에 적용될 수 있습니다.

현재 개발자가 비즈니스용 Microsoft 스토어 및 교육용 Microsoft 스토어를 통해 조직 고객에게 유료 앱을 배포할 수 있는 국가 및 지역은 다음과 같습니다.

- 오스트리아
- 벨기에
- 불가리아
- 캐나다
- 크로아티아
- 키프로스
- 체코
- 덴마크
- 에스토니아
- 핀란드
- 프랑스
- 독일
- 그리스
- 헝가리
- 아일랜드
- 맨 섬
- 이탈리아
- 라트비아
- 리히텐슈타인
- 리투아니아
- 룩셈부르크
- 몰타
- 모나코
- 네덜란드
- 노르웨이
- 폴란드
- 포르투갈
- 루마니아
- 슬로바키아
- 슬로베니아
- 스페인
- 스웨덴
- 스위스
- 영국
- 미국
