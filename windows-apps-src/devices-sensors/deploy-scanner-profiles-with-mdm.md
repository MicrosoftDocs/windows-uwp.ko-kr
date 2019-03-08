---
title: MDM을 사용한 바코드 스캐너 프로필 배포
description: MDM 서버를 사용하여 바코드 스캐너 프로필을 배포할 수 있습니다.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dbcaa683e2c7a2bb18d88fcba03e10fa951d4459
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626158"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>MDM을 사용한 바코드 스캐너 프로필 배포

**참고**  이 기능을 사용 하려면 Windows 10 Mobile 이상.

MDM 서버를 사용하여 바코드 스캐너 프로필을 배포할 수 있습니다. 프로필을 배포 하려면 사용 하 여 *OemProfile* 에 [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) 에 배치할 합니다 \\데이터\\SharedData\\OEM\\ 공용\\프로필 폴더입니다. 이렇게 스캐너 프로필을 배포하면 드라이버 제조업체가 API 표면을 통해서는 노출되지 않는 설정을 구성하는 데 사용할 수 있습니다.

Microsoft는 스캐너 프로필의 구체적인 부분이나 구현 방법을 정의하지 않습니다.

## <a name="related-topics"></a>관련 항목
- [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [바코드 스캐너 장치 지원](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)