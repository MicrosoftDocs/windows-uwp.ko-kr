---
title: MDM을 사용 하 여 바코드 스캐너 프로필 배포
description: 바코드 스캐너 프로필은 MDM 서버를 사용 하 여 배포할 수 있습니다.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1bac497ec52dd0897af8c6c606bcdc041007c579
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173277"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>MDM을 사용 하 여 바코드 스캐너 프로필 배포

**참고**    이 기능에는 Windows 10 Mobile 이상이 필요 합니다.

바코드 스캐너 프로필은 MDM 서버를 사용 하 여 배포할 수 있습니다. 프로필을 배포 하려면 [Enterpriseextfilesystem CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp) 에서 *oemprofile* 을 사용 하 여 \\ 데이터 \\ shareddata \\ OEM \\ 공용 \\ 프로필 폴더에 배치 합니다. 이러한 스캐너 프로필은 드라이버 제조업체에서 API 화면을 통해 노출 되지 않는 설정을 구성 하는 데 사용할 수 있습니다.

Microsoft는 스캐너 프로필의 세부 정보 또는 구현 방법을 정의 하지 않습니다.

## <a name="related-topics"></a>관련 항목
- [EnterpriseExtFileSystem CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [바코드 스캐너 장치 지원](./pos-device-support.md#barcode-scanner)