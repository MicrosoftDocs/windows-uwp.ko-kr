---
author: jnHs
Description: You can publish line-of-business (LOB) apps directly to enterprises for volume acquisition via the Microsoft Store for Business or Microsoft Store for Education, without making the apps broadly available in the Store.
title: 엔터프라이즈에 LOB 앱 배포
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, lob, 엔터프라이즈 앱, 비즈니스용 Store, 교육용 Store, 엔터프라이즈
ms.localizationpriority: medium
ms.openlocfilehash: 9149533a12263e105356a1683257c4d9172eefb5
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4498041"
---
# <a name="distribute-lob-apps-to-enterprises"></a>엔터프라이즈에 LOB 앱 배포


LOB 앱을 스토어에서 광범위하게 사용할 수 있도록 하지 않고 비즈니스용 Microsoft 스토어나 교육용 Microsoft 스토어를 통해 대량 구매하도록 엔터프라이즈에 직접 해당 앱을 게시할 수 있습니다.

> [!NOTE]
> 지금은 비즈니스용 Microsoft 스토어나 교육용 Microsoft 스토어를 통해 무료 앱만 독점적으로 엔터프라이즈에 배포할 수 있습니다. LOB로 유료 앱을 제출하는 경우 엔터프라이즈에서 사용할 수 없습니다. 

> [!IMPORTANT]
> [Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)를 사용하여 LOB 앱을 엔터프라이즈에 직접 게시할 수 없습니다. LOB 앱에 대한 모든 제출은 Windows 개발자 센터 대시보드를 사용해야 합니다.


## <a name="set-up-the-enterprise-association"></a>엔터프라이즈 연결 설정

LOB 앱을 엔터프라이즈에 독점적으로 게시하는 첫 번째 단계는 계정과 엔터프라이즈의 개인 저장소 간에 연결을 설정하는 것입니다.

> [!IMPORTANT]
> 이 연결 프로세스는 엔터프라이즈에 의해 시작되어야 하고 개발자 계정을 만들 때 사용한 Microsoft 계정과 연결된 메일 주소를 사용해야 합니다. 자세한 내용은 [기간 업무 앱 작업](http://go.microsoft.com/fwlink/p/?LinkId=698846)을 참조하세요.

엔터프라이즈에서 독점적으로 사용하기 위해 앱을 게시하도록 초대하는 경우 해당 연결을 확인하는 링크가 포함된 메일을 받게 됩니다. **계정 설정**의 **엔터프라이즈 연결** 섹션으로 이동하여 이러한 연결을 확인할 수도 있습니다(개발자 계정을 여는 데 사용된 Microsoft 계정으로 로그인한 경우).

연결을 확인하려면 **동의**를 클릭합니다. 그러면 계정에서 해당 엔터프라이즈의 독점적 사용을 위해 앱을 게시할 수 있습니다.


## <a name="submit-lob-apps"></a>LOB 앱 제출

엔터프라이즈의 독점적 사용을 위해 앱을 게시할 준비가 된 후 프로세스는 앱 제출 프로세스와 유사합니다. 앱은 동일한 [인증 프로세스](the-app-certification-process.md)를 거치며 모든 [Microsoft Store 정책](https://docs.microsoft.com/legal/windows/agreements/store-policies)을 준수해야 합니다. 프로세스 중 몇 부분만 서로 다릅니다.


### <a name="visibility"></a>표시 여부

엔터프라이즈 연결을 설정한 후 앱을 제출할 때마다 제출의 **가격 책정 및 가용성** 페이지에 있는 **표시 여부** 섹션에서 드롭다운 상자가 표시됩니다. 이는 기본적으로 **소매 배포**로 설정되어 있습니다. 앱을 엔터프라이즈에서 독점적으로 사용하게 만들려면 **LOB(기간 업무) 배포**를 선택해야 합니다.

**LOB 배포**를 선택하면 일반적인 **표시 여부** 옵션이 독점적 앱을 게시할 수 있는 엔터프라이즈 목록으로 대체됩니다. 선택한 엔터프라이즈 외부의 사용자는 앱을 보거나 다운로드할 수 없습니다.

앱을 LOB로 게시하기 위해서는 하나 이상의 엔터프라이즈를 선택해야 합니다.

<span id="organizational" />

### <a name="organizational-licensing"></a>조직 라이선스

기본적으로 앱을 제출할 때 **스토어 관리(온라인) 볼륨 라이선싱** 상자가 클릭되어 있습니다. LOB 앱을 게시할 때 엔터프라이즈에서 앱을 대량으로 구매할 수 있도록 이 상자가 계속 클릭한 상태를 유지해야 합니다. 이렇게 하면 **배포 및 표시 여부** 섹션에서 선택한 엔터프라이즈의 외부에 있는 사용자는 해당 앱을 사용할 수 없게 됩니다.

연결이 끊어진 (오프라인) 라이선싱을 통해 엔터프라이즈가 앱을 사용할 수 있도록 하려면 **연결이 끊어진 (오프라인) 라이선싱** 상자를 선택하면 됩니다.

자세한 내용은 [조직 라이선스 옵션](organizational-licensing.md)을 참조하세요.


### <a name="age-ratings"></a>연령별 등급

LOB 앱의 경우 제출 프로세스의 [연령별 등급](age-ratings.md) 단계는 소매 앱의 경우와 동일하게 작동하지만 설문을 완료하거나 기존 IARC 등급 ID를 가져오는 대신 앱의 스토어 연령별 등급을 수동으로 나타낼 수 있는 추가 옵션도 있습니다. 이 수동 등급은 LOB 배포에만 사용할 수 있으므로 앱의 **표시 여부** 설정을 **소매 배포**로 변경할 경우, 연령별 등급 설문지를 완료해야 제출을 게시할 수 있습니다.


## <a name="enterprise-deployment-of-lob-apps"></a>LOB 앱의 엔터프라이즈 배포

**스토어에 제출**을 클릭한 후 앱이 인증 프로세스를 거칩니다. 준비가 되면 엔터프라이즈 관리자가 해당 앱을 비즈니스용 Microsoft 스토어나 교육용 Microsoft 스토어 포털의 개인 저장소에 추가해야 합니다. 이제 엔터프라이즈에서 사용자에게 앱을 배포할 수 있습니다.

> [!NOTE]
> LOB 앱을 가져오려면 조직이 [지원 시장](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets)에 위치해 있어야 하며, 앱을 제출할 때 [해당 시장을 제외](define-pricing-and-market-selection.md)하지 않아야 합니다. 

자세한 내용은 [기간 업무 앱 사용](http://go.microsoft.com/fwlink/p/?LinkId=698846) 및 [개인 저장소를 사용하여 앱 배포](http://go.microsoft.com/fwlink/p/?LinkId=698847)를 참조하세요.


## <a name="update-lob-apps"></a>LOB 앱 업데이트

이미 LOB로 게시한 앱에 대한 업데이트를 게시하려면 새 제출을 만들기만 하면 됩니다. 새 패키지를 업로드하거나 다른 사항을 변경한 다음 **스토어에 제출**을 클릭하면 업데이트된 버전을 사용할 수 있습니다. 앱을 구입하는 엔터프라이즈를 추가로 선택하거나 이전에 해당 앱을 배포한 엔터프라이즈 중 하나를 제거하는 등 의도적으로 변경하려고 하지 않는 한, **표시 여부**의 엔터프라이즈 선택은 동일하게 유지되어야 합니다.

이전에 기간 업무로 게시한 앱 제공을 중지하고 새로운 구입을 방지하려는 경우 새 제출을 만들어야 합니다. 먼저, **LOB(기간 업무) 배포**에서 **소매 배포**로 **표시 여부** 선택을 변경해야 합니다. 그런 다음 [검색 기능](choose-visibility-options.md#discoverability) 섹션에서 **Store에서 사용할 수 있지만 검색 되지 않는 제품으로 설정**과 **취득 중지** 옵션을 선택합니다.

제출이 인증 프로세스를 거치면 더 이상 이 앱을 새로 구입할 수 없습니다(이미 해당 앱을 보유한 경우 계속 사용할 수는 있음).

> [!NOTE]
> 앱을 **소매 배포**로 변경할 경우, 아직 [연령별 등급 설문지](age-ratings.md)를 완료하지 않았으면 이를 완료해야 합니다(앱을 새로 구입할 수 없는 경우에도 마찬가지임).


## <a name="distribute-lob-apps-through-sideloading"></a>테스트용 로드를 통해 LOB 앱 배포

비즈니스용 Microsoft 스토어와 교육용 Microsoft 스토어를 통해 엔터프라이즈가 앱을 사용할 수 있도록 하면 앱이 스토어에서 서명되었고 표준 스토어 정책을 준수함을 보장할 수 있습니다.

경우에 따라 회사에서 LOB 앱이 Windows 개발자 센터를 통해 제출되는 것을 원하지 않을 수 있습니다(예: 규제 준수 때문이거나 앱에서 추가 기능이 필요한 경우). 이 경우 엔터프라이즈에서 비즈니스용 Microsoft 스토어나 교육용 Microsoft 스토어를 사용하지 않고 테스트용 로드를 통해 컴퓨터에 직접 앱을 배포할 수 있습니다.

자세한 내용은 [Windows 10에서 LOB 앱을 테스트용으로 로드](http://go.microsoft.com/fwlink/p/?LinkId=623433)를 참조하세요.

 

 




