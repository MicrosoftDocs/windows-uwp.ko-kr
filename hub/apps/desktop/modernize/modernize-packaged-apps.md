---
Description: Windows 앱 패키지에 패키징된 데스크톱 응용 프로그램에서 Windows 10 사용자를 위한 최신 환경을 추가 하는 방법에 대해 알아봅니다.
title: 현대화 패키지 된 데스크톱 앱
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ec1c56f205b270262f618ffb46db16b38276975d
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142516"
---
# <a name="features-that-require-package-identity"></a>패키지 ID가 필요한 기능

[최신 Windows 10 환경을](index.md)사용 하 여 데스크톱 앱을 업데이트 하려는 경우 [패키지 id](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)가 있는 데스크톱 앱 에서만 많은 기능을 사용할 수 있습니다. 데스크톱 앱에 패키지 id를 부여 하는 방법에는 여러 가지가 있습니다.

* [Msix 패키지](/windows/msix/desktop/desktop-to-uwp-root)에 패키지 합니다. MSIX은 모든 Windows 앱, WPF, Windows Forms 및 Win32 앱에 대 한 범용 패키징 환경을 제공 하는 최신 앱 패키지 형식입니다. 유연 하 게 설치 하 고 업데이트 하는 환경, 유연한 기능 시스템을 포함 하는 관리 되는 보안 모델, Microsoft Store, 엔터프라이즈 관리 및 여러 사용자 지정 배포 모델을 지원 합니다. 자세한 내용은 MSIX 설명서의 [데스크톱 애플리케이션 패키지](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)를 참조하세요.
* 데스크톱 앱을 배포 하기 위해 MSIX 패키지를 채택할 수 없는 경우 Windows 10 Insider Preview 빌드 10.0.19000.0부터 패키지 매니페스트만 포함 된 *sparse MSIX 패키지* 를 만들어 패키지 id를 부여할 수 있습니다. 자세한 내용은 [패키지 되지 않은 데스크톱 앱에 Id 부여](grant-identity-to-nonpackaged-apps.md)를 참조 하세요.

데스크톱 앱에 패키지 id가 있는 경우 앱에서 다음 기능을 사용할 수 있습니다.

## <a name="use-uwp-apis-that-require-package-identity"></a>패키지 id가 필요한 UWP Api 사용

다음 UWP Api 목록은 데스크톱 앱에서 패키지 id를 사용 해야 합니다. [api 목록](desktop-to-uwp-supported-api.md#list-of-apis)

## <a name="integrate-with-package-extensions"></a>확장 패키지와 통합

응용 프로그램을 시스템과 통합 해야 하는 경우 (예: 방화벽 규칙 설정) 응용 프로그램의 패키지 매니페스트에 이러한 항목을 설명 하 고 시스템은 나머지 작업을 수행 합니다. 이런 작업 대부분에서 코드를 작성할 필요가 없습니다. 매니페스트의 XML을 사용 하면 사용자가 로그온 할 때 프로세스를 시작 하 고, 응용 프로그램을 파일 탐색기에 통합 하 고, 다른 앱에 표시 되는 인쇄 대상 목록을 응용 프로그램에 추가 하는 등의 작업을 수행할 수 있습니다.

자세한 내용은 [패키지 확장과 함께 데스크톱 앱 통합](desktop-to-uwp-extensions.md)을 참조 하세요.

## <a name="extend-with-uwp-components"></a>UWP 구성 요소를 사용하여 확장

일부 Windows 10 환경(예, 터치 구현 UI 페이지)은 최신 앱 컨테이너 내부에서 실행해야 합니다. 일반적으로 UWP Api를 사용 하 여 기존 데스크톱 응용 프로그램을 [향상](desktop-to-uwp-enhance.md) 시켜 환경을 추가할 수 있는지 여부를 먼저 확인 해야 합니다. UWP 구성 요소를 사용 해야 하는 경우 환경을 얻으려면 UWP 프로젝트를 솔루션에 추가 하 고 app services를 사용 하 여 데스크톱 응용 프로그램과 UWP 구성 요소 간에 통신할 수 있습니다.

자세한 내용은 [UWP 구성 요소를 사용 하 여 데스크톱 앱 확장](desktop-to-uwp-extend.md)을 참조 하세요.

## <a name="distribute"></a>배포

MSIX 패키지에 앱을 패키지 하는 경우 Microsoft Store 게시 하거나 다른 시스템에 테스트용으로 로드 하 여 배포할 수 있습니다.

[패키지 된 데스크톱 앱 배포를](desktop-to-uwp-distribute.md)참조 하세요.
