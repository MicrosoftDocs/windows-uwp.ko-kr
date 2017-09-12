---
title: "MDM을 사용한 바코드 스캐너 프로필 배포"
author: PatrickFarley
description: "MDM 서버를 사용하여 바코드 스캐너 프로필을 배포할 수 있습니다."
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.openlocfilehash: a63a09e64b6e2b935963a3f49ed7cbc6b82bdcef
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2017
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>MDM을 사용한 바코드 스캐너 프로필 배포

**참고** 이 기능을 사용하려면 Windows 10 Mobile 이상이 필요합니다.

MDM 서버를 사용하여 바코드 스캐너 프로필을 배포할 수 있습니다. 프로필을 배포하려면 [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)의 *OemProfile*을 사용하여 프로필을 \\Data\\SharedData\\OEM\\Public\\Profile 폴더에 저장합니다. 이렇게 스캐너 프로필을 배포하면 드라이버 제조업체가 API 표면을 통해서는 노출되지 않는 설정을 구성하는 데 사용할 수 있습니다.

Microsoft는 스캐너 프로필의 구체적인 부분이나 구현 방법을 정의하지 않습니다.

## <a name="related-topics"></a>관련 항목
[바코드 스캐너](barcode-scanner.md)

[EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)