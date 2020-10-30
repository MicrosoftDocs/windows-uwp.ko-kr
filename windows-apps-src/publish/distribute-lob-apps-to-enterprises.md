---
description: 비즈니스에 대 한 LOB (기간 업무) 앱은 스토어에서 앱을 광범위 하 게 사용할 수 있도록 설정 하지 않고도 비즈니스에 대 한 Microsoft Store 또는 교육용 Microsoft Store를 통해 기업에 직접 게시할 수 있습니다.
title: 엔터프라이즈에 LOB 앱 배포
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 01/16/2020
ms.topic: article
keywords: windows 10, uwp, lob, 기간 업무 (lob), 엔터프라이즈 앱, 스토어 비즈니스용 스토어, 교육용 스토어, enterprise
ms.localizationpriority: medium
ms.openlocfilehash: b9bb384ac8514431899fd6ac37cf56303a80d214
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033926"
---
# <a name="distribute-lob-apps-to-enterprises"></a>엔터프라이즈에 LOB 앱 배포

앱을 공개적으로 사용할 수 있도록 설정 하지 않고도 [Msix 패키지](/windows/msix/) 를 사용 하 여 조직의 사용자에 게 lob (기간 업무) 앱을 배포 하는 몇 가지 옵션이 있습니다. 장치 관리 도구를 사용 하거나, 앱 설치 관리자 기반 배포를 구성 하거나, 앱을 직접 테스트용으로 로드 하거나, 비즈니스 또는 교육용 Microsoft Store Microsoft Store에 앱을 게시할 수 있습니다.

## <a name="microsoft-endpoint-configuration-manager-and-microsoft-intune"></a>Microsoft 끝점 Configuration Manager 및 Microsoft Intune

조직에서 Microsoft 끝점 Configuration Manager 또는 Microsoft Intune를 사용 하 여 장치를 관리 하는 경우 이러한 도구를 사용 하 여 LOB 앱을 배포할 수 있습니다. 자세한 내용은 다음 문서를 참조하세요.

* [Configuration Manager의 애플리케이션 관리 소개](/configmgr/apps/understand/introduction-to-application-management)
* [Microsoft Intune에 대한 앱 수명 주기 개요](/intune/apps/app-lifecycle)

## <a name="app-installer"></a>앱 설치 관리자

앱 설치 관리자를 사용 하면 MSIX 앱 패키지를 직접 두 번 클릭 하거나 웹 서버에서 앱 패키지를 설치 하는 appinstaller 파일을 두 번 클릭 하 여 Windows 10 앱을 설치할 수 있습니다. 즉, 사용자가 PowerShell 또는 다른 개발자 도구를 사용 하 여 LOB 앱을 설치할 필요가 없습니다. 앱 설치 관리자는 선택적 패키지 및 관련 집합을 포함 하는 앱 패키지를 설치할 수도 있습니다.

앱 설치 관리자는 기업에서 오프라인으로 사용하기 위해 비즈니스용 Microsoft Store [웹 포털](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1)에서 다운로드할 수 있습니다. 앱 설치 관리자에 대 한 자세한 내용은 [앱 설치 관리자를 사용 하 여 Windows 10 앱 설치](/windows/msix/app-installer/app-installer-root)를 참조 하세요.

## <a name="sideloading"></a>테스트용 로드

조직의 사용자에 게 직접 LOB 앱을 배포 하는 또 다른 옵션은 테스트용 로드입니다. 이 옵션은 사용자가 MSIX 앱 패키지를 직접 설치할 수 있도록 하는 앱 설치 기반 배포와 유사 합니다. Windows 10 버전 2004부터 테스트용 로드는 기본적으로 사용 하도록 설정 되며, 사용자는 서명 된 MSIX 앱 패키지를 두 번 클릭 하 여 앱을 설치할 수 있습니다. Windows 10 버전 1909 및 이전 버전에서 테스트용 로드에는 PowerShell 스크립트를 추가로 구성 하 고 사용 해야 합니다. 자세한 내용은 [테스트용으로 로드 LOB apps In Windows 10](/windows/application-management/sideload-apps-in-windows-10)을 참조 하세요.

## <a name="microsoft-store-for-business-or-microsoft-store-for-education"></a>Microsoft Store for Business 또는 교육용 Microsoft Store

앱을 저장소에서 광범위 하 게 사용할 수 있도록 설정 하지 않고도 비즈니스용 Microsoft Store 또는 교육용 Microsoft Store를 통해 볼륨 획득을 위해 엔터프라이즈에 직접 LOB (기간 업무) 앱을 게시할 수 있습니다. 이 옵션을 사용 하는 경우 앱은 스토어에서 서명 되며 표준 저장소 정책을 준수 해야 합니다.

> [!NOTE]
> 이번에는 무료 앱만 비즈니스 또는 교육용 Microsoft Store를 Microsoft Store 통해 기업에 독점적으로 배포할 수 있습니다. 유료 앱을 LOB로 제출 하는 경우에는 엔터프라이즈에서 사용할 수 없습니다. 

> [!IMPORTANT]
> [Microsoft Store 제출 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 를 사용 하 여 LOB 앱을 엔터프라이즈에 직접 게시할 수는 없습니다. LOB 앱에 대 한 모든 제출은 파트너 센터를 통해 게시 되어야 합니다.

### <a name="set-up-the-enterprise-association"></a>엔터프라이즈 연결 설정

LOB 앱을 엔터프라이즈에 독점적으로 게시 하는 첫 번째 단계는 계정과 기업의 개인 저장소 간 연결을 설정 하는 것입니다.

> [!IMPORTANT]
> 이 연결 프로세스는 기업에서 시작 해야 하며 개발자 계정을 만드는 데 사용 된 Microsoft 계정와 연결 된 전자 메일 주소를 사용 해야 합니다. 자세한 내용은 [lob (기간 업무) 앱 사용](/microsoft-store/working-with-line-of-business-apps)을 참조 하세요.

엔터프라이즈에서 독점 사용을 위해 앱 게시를 초대 하도록 선택 하면 연결을 확인 하는 링크가 포함 된 전자 메일을 받게 됩니다. **계정 설정** 의 **엔터프라이즈 연결** 섹션으로 이동 하 여 이러한 연결을 확인할 수도 있습니다 (개발자 계정을 여는 데 사용 된 Microsoft 계정로 로그인 한 경우).

연결을 확인 하려면 **동의** 를 클릭 합니다. 그러면 계정에서 해당 엔터프라이즈의 독점적 사용을 위해 앱을 게시할 수 있습니다.

### <a name="submit-lob-apps"></a>LOB 앱 제출

엔터프라이즈의 배타 사용을 위해 앱을 게시할 준비가 되 면 프로세스는 앱 제출 프로세스와 유사 합니다. 앱은 동일한 [인증 프로세스](the-app-certification-process.md)를 거치 며 모든 [Microsoft Store 정책을](store-policies.md)준수 해야 합니다. 프로세스에는 몇 가지 다른 부분이 있습니다.

#### <a name="visibility"></a>가시 거리

엔터프라이즈 연결을 설정한 후 앱을 제출할 때마다 제출의 **가격 책정 및 가용성** 페이지의 **표시 유형** 섹션에 드롭다운 상자가 표시 됩니다. 기본적으로이는 **소매점 배포** 로 설정 됩니다. 엔터프라이즈 전용 앱을 만들려면 **lob (기간 업무) 배포** 를 선택 해야 합니다.

**LOB (기간 업무) 배포** 를 선택 하면 일반 **표시 유형** 옵션이 전용 앱을 게시할 수 있는 엔터프라이즈 목록으로 대체 됩니다. 선택 하는 기업 외에는 앱을 보거나 다운로드할 수 없습니다.

앱을 기간 업무 (lob)로 게시 하려면 하나 이상의 엔터프라이즈를 선택 해야 합니다.

<span id="organizational" />

#### <a name="organizational-licensing"></a>조직 라이선스

기본적으로 앱을 제출할 때 **저장소 관리 (온라인) 볼륨 라이선스** 에 대 한 상자를 확인 합니다. LOB 앱을 게시할 때 enterprise에서 앱을 볼륨으로 가져올 수 있도록이 확인란을 선택 된 상태로 두어야 합니다. 그러면 **배포 및 표시 유형** 섹션에서 선택한 기업 외부의 사용자가 앱을 사용할 수 없게 됩니다.

연결 되지 않은 (오프 라인) 라이선스를 통해 엔터프라이즈에서 앱을 사용할 수 있게 하려는 경우 **연결 끊김 (오프 라인) 라이선스** 확인란도 선택할 수 있습니다.

자세한 내용은 [조직 라이선스 옵션](organizational-licensing.md)을 참조 하세요.

#### <a name="age-ratings"></a>연령별 등급

LOB 앱의 경우 제출 프로세스의 [연령 등급](age-ratings.md) 단계는 소매점 앱과 동일 하 게 작동 하지만, 질문을 완료 하거나 기존 IARC 등급 ID를 가져오는 대신 앱의 스토어 사용 기간 등급을 수동으로 지정할 수 있는 추가 옵션도 있습니다. 이 수동 등급은 LOB 배포에만 사용할 수 있으므로 앱의 **표시 유형** 설정을 **일반 정품** 으로 변경 하는 경우 제출을 게시 하기 전에 연령별 등급 질문을 해야 합니다.

### <a name="enterprise-deployment-of-lob-apps"></a>LOB 앱의 엔터프라이즈 배포

**스토어에 제출** 을 클릭 하면 앱이 인증 프로세스를 거칩니다. 준비가 되 면 엔터프라이즈 관리자는 회사의 Microsoft Store 또는 교육용 Microsoft Store의 개인 저장소에 추가 해야 합니다. 그러면 기업은 사용자에 게 앱을 배포할 수 있습니다.

> [!NOTE]
> LOB 앱을 받으려면 조직이 [지원 되는 시장](/windows/whats-new/windows-store-for-business-overview#supported-markets)에 있어야 하며 앱을 제출할 때 [해당 시장을 제외](./define-market-selection.md) 해서는 안 됩니다. 

자세한 내용은 lob ( [기간 업무) 앱 작업](/microsoft-store/working-with-line-of-business-apps) 및 [개인 저장소를 사용 하 여 앱 배포](/microsoft-store/distribute-apps-from-your-private-store)를 참조 하세요.

### <a name="update-lob-apps"></a>LOB 앱 업데이트

이미 LOB로 게시 한 앱에 업데이트를 게시 하려면 새 제출을 만들면 됩니다. 새 패키지를 업로드 하거나 다른 내용을 변경한 다음 **스토어에 제출** 을 클릭 하 여 업데이트 된 버전을 사용할 수 있도록 합니다. 앱을 얻기 위해 추가 엔터프라이즈를 선택 하거나 이전에 배포한 기업 중 하나를 제거 하는 등의 변경 작업을 수행 하려는 경우가 아니면 엔터프라이즈 선택을 동일 하 게 **표시** 해야 합니다.

이전에 lob (기간 업무)로 게시 한 앱의 제공을 중지 하 고 새로운 합병을 방지 하려는 경우 새 제출을 만들어야 합니다. 먼저 **LOB (기간 업무) 배포** 에서 **일반 정품 배포** 에 대 한 **표시 유형** 선택을 변경 해야 합니다. 그런 다음 검색 기능 [섹션에서](choose-visibility-options.md#discoverability) **이 제품을 사용할 수 있지만 스토어에서 검색 안 함** **옵션을** 선택 합니다.

인증 프로세스를 통과 한 후에는 앱을 더 이상 새 합병에 사용할 수 없습니다 (이미 있는 모든 사용자가 계속 사용할 수 있음).

> [!NOTE]
> 앱을 **일반 정품 배포** 로 변경 하는 경우 새 합병에 앱을 사용할 수 없는 경우에도 아직 수행 하지 않은 경우에는 [연령별 등급 질문](age-ratings.md) 을 완료 해야 합니다.
