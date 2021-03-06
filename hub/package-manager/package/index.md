---
title: Windows 패키지 관리자에 패키지 제출
description: Windows 패키지 관리자를 애플리케이션이 포함된 소프트웨어 패키지의 배포 채널로 사용할 수 있습니다.
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: b2bde5a55d22d0541724cd777c315b2751cf1d36
ms.sourcegitcommit: 3153ef4838c35084a64173c7ed88719c8864f8cc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96755280"
---
# <a name="submit-packages-to-windows-package-manager"></a>Windows 패키지 관리자에 패키지 제출

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

ISV(독립 소프트웨어 공급업체)인 경우 Windows 패키지 관리자를 애플리케이션이 포함된 소프트웨어 패키지의 배포 채널로 사용할 수 있습니다. Windows 패키지 관리자는 현재 MSIX, MSI 및 EXE 형식의 설치 관리자를 지원합니다.

소프트웨어 패키지를 Windows 패키지 관리자에 제출하려면 다음 단계를 수행합니다.

1. [애플리케이션에 대한 정보를 제공하는 패키지 매니페스트 만들기](manifest.md). 매니페스트는 Windows 패키지 관리자 스키마를 따르는 YAML 파일입니다.
2. [Windows 패키지 관리자 리포지토리에 매니페스트 제출](repository.md). 이는 **winget** 도구에서 액세스할 수 있는 매니페스트 컬렉션이 포함된 GitHub의 오픈 소스 리포지토리입니다.

## <a name="related-topics"></a>관련 항목

* [Winget 도구 사용](../winget/index.md)
* [패키지 매니페스트 만들기](manifest.md)
* [리포지토리에 매니페스트 제출](repository.md)
