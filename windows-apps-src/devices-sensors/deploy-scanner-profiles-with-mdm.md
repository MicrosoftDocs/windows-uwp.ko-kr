---
title: MDM을 사용한 바코드 스캐너 프로필 배포
author: PatrickFarley
description: MDM 서버를 사용하여 바코드 스캐너 프로필을 배포할 수 있습니다.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.author: pafarley
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cfd9692620273952483ec7da65a69b643cb5bf4f
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5690577"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>MDM을 사용한 바코드 스캐너 프로필 배포

**참고**이 기능을 사용 하려면 Windows10 모바일 이상.

MDM 서버를 사용하여 바코드 스캐너 프로필을 배포할 수 있습니다. 프로필을 배포하려면 [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)의 *OemProfile*을 사용하여 프로필을 \\Data\\SharedData\\OEM\\Public\\Profile 폴더에 저장합니다. 이렇게 스캐너 프로필을 배포하면 드라이버 제조업체가 API 표면을 통해서는 노출되지 않는 설정을 구성하는 데 사용할 수 있습니다.

Microsoft는 스캐너 프로필의 구체적인 부분이나 구현 방법을 정의하지 않습니다.

## <a name="related-topics"></a>관련 항목
- [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [바코드 스캐너 장치 지원](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)