---
title: Windows 패키지 관리자
description: Windows 패키지 관리자는 애플리케이션을 Windows 10에 설치하기 위한 명령줄 도구와 서비스 세트로 구성된 포괄적인 패키지 관리자 솔루션입니다.
ms.date: 05/03/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5b25f2c651e11a5ff97a630bb802b236771f5441
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334627"
---
# <a name="windows-package-manager-preview"></a>Windows 패키지 관리자(미리 보기)

[!INCLUDE [preview-note](../includes/package-manager-preview.md)]

Windows 패키지 관리자는 애플리케이션을 Windows 10에 설치하기 위한 명령줄 도구와 서비스 세트로 구성된 포괄적인 [패키지 관리자 솔루션](#understanding-package-managers)입니다.

## <a name="windows-package-manager-for-developers"></a>개발자용 Windows 패키지 관리자

개발자는 **winget** 명령줄 도구를 사용하여 큐레이팅된 애플리케이션 세트를 검색, 설치, 업그레이드, 제거 및 구성합니다. 설치되면 개발자가 Windows 터미널, PowerShell 또는 명령 프롬프트를 통해 **winget**에 액세스할 수 있습니다.

자세한 내용은 [winget 도구를 사용하여 애플리케이션 설치 및 관리](winget/index.md)를 참조하세요.

## <a name="windows-package-manager-for-isvs"></a>ISV용 Windows 패키지 관리자

ISV(독립 소프트웨어 공급업체)는 도구와 애플리케이션이 포함된 소프트웨어 패키지의 배포 채널로 Windows 패키지 관리자를 사용할 수 있습니다. 소프트웨어 패키지(.msix, .msi 또는 .exe 설치 관리자 포함)를 Windows 패키지 관리자에 제출하기 위해 ISV에서 [패키지 매니페스트](package/manifest.md)를 업로드하여 소프트웨어 패키지가 Windows 패키지 관리자에 포함되도록 고려할 수 있는 GitHub에 오픈 소스 **Microsoft 커뮤니티 패키지 매니페스트 리포지토리**를 제공합니다. 매니페스트는 자동으로 유효성이 검사되며 수동으로 검토할 수도 있습니다.

자세한 내용은 [Windows 패키지 관리자에 패키지 제출](package/repository.md)을 참조하세요.

## <a name="understanding-package-managers"></a>패키지 관리자 이해

패키지 관리자는 소프트웨어 설치, 업그레이드, 구성 및 사용을 자동화하는 데 사용되는 시스템 또는 도구 세트입니다. 대부분의 패키지 관리자는 개발자 도구를 검색하여 설치하도록 설계되었습니다.

개발자가 패키지 관리자를 사용하여 지정된 프로젝트에 대한 솔루션을 개발하는 데 필요한 도구에 대한 필수 구성 요소를 지정하는 것이 이상적입니다. 그런 다음, 패키지 관리자에서 선언적 지침에 따라 도구를 설치하고 구성합니다. 패키지 관리자는 환경을 준비하는 데 걸리는 시간을 줄이고 동일한 버전의 패키지가 머신에 설치되도록 합니다.

타사 패키지 관리자는 [Microsoft 커뮤니티 패키지 매니페스트 리포지토리](package/repository.md)를 활용하여 소프트웨어 카탈로그의 크기를 늘릴 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [winget 도구를 사용하여 소프트웨어 패키지 설치 및 관리](winget/index.md)
* [Windows 패키지 관리자에 패키지 제출](package/index.md)