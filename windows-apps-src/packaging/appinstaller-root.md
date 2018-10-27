---
author: laurenhughes
title: 앱 설치 관리자를 사용하여 UWP 앱 설치
description: 이 섹션에는 앱 설치 관리자에 관한 문서 및 앱 설치 관리자의 기능을 사용하는 방법에 대한 링크가 포함되어 있습니다.
ms.author: lahugh
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, uwp 앱 설치 관리자, AppInstaller, 사이드로드, 관련 집합, 선택적 패키지
ms.localizationpriority: medium
ms.openlocfilehash: f3da1f4f393eea1362b6e59d2ad7e9a2a97bc33b
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5692802"
---
# <a name="install-uwp-apps-with-app-installer"></a>앱 설치 관리자를 사용하여 UWP 앱 설치

## <a name="purpose"></a>목적
이 섹션에는 앱 설치 관리자에 관한 문서 및 앱 설치 관리자의 기능을 사용하는 방법에 대한 링크가 포함되어 있습니다. 

앱 설치 관리자를 사용하면 앱 패키지를 두 번 클릭하여 UWP 앱을 설치할 수 있습니다. 즉, 사용자는 PowerShell 또는 다른 개발자 도구를 사용하여 UWP 앱을 배포할 필요가 없습니다. 앱 설치 관리자는 웹, 선택적 패키지 및 관련 집합에서 앱을 설치할 수도 있습니다. 앱 설치 관리자를 사용하여 앱을 설치하는 방법을 알아보려면 표의 항목을 참조하세요.

| 항목 | 설명 |
|-------|-------------|
| [Visual Studio를 사용하여 앱 설치 관리자 파일 만들기](create-appinstallerfile-vs.md)| Visual Studio를 사용하여 .appinstaller 파일로 자동 업데이트를 사용하는 방법을 알아보세요. |
| [웹 페이지에서 UWP 앱 설치](installing-UWP-apps-web.md) | 이 섹션에서는 사용자가 웹 페이지에서 직접 앱을 설치할 수 있도록 하기 위해 필요한 단계를 검토합니다. |
| [앱 설치 관리자 파일을 사용하여 관련 집합 설치](install-related-set.md) | 이 섹션에서는 앱 설치 관리자를 통해 관련 집합을 설치하는 방법을 알아봅니다. 또한 관련 집합을 정의하는 앱 설치 관리자 파일을 만드는 단계를 수행합니다. |
| [앱 설치 관리자 파일 관련 설치 문제 해결](troubleshoot-appinstaller-issues.md) | 앱 설치 관리자 파일로 응용 프로그램을 사이드로드할 때 일반적인 문제 및 해결 방법. |
| [앱 설치 관리자 파일(.appinstaller) 참조](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file) | 앱 설치 관리자 파일에 대한 전체 XML 스키마를 봅니다. |

## <a name="tutorials"></a>자습서 

이러한 자습서를 따라 다양한 배포 플랫폼에서 UWP 앱을 호스트하고 설치하는 방법을 알아보세요. 이 자습서는 Microsoft Store에 자신의 앱을 게시하고 싶지 않거나 게시할 필요가 없지만 여전히 Windows 10 패키지 및 배포 플랫폼을 활용하고자 하는 기업 및 개발자에게 유용합니다.

| 자습서 | 설명 |
|----------|-------------|
| [Azure 웹앱에서 UWP 앱 설치](web-install-azure.md) | Azure 웹앱을 만들고 이를 사용하여 UWP 앱 패키지를 호스트 및 배포합니다. |
| [IIS 서버에서 UWP 앱 설치](web-install-IIS.md) | IIS 서버를 설정하고, 웹앱이 앱 패키지를 호스트할 수 있는지 확인하고, 효과적으로 앱 설치 관리자를 사용합니다. |
| [웹 설치에 대해 AWS에서 UWP 앱 패키지 호스트](web-install-aws.md) | Amazon Simple Storage Service를 설정하여 웹 사이트에서 UWP 앱 패키지를 호스트하는 방법을 알아보세요. |

