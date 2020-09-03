---
Description: Windows 앱 패키지에 패키징된 데스크톱 애플리케이션에서 Windows 10 사용자를 위한 최신 환경을 추가하는 방법을 알아봅니다.
title: 패키지된 데스크톱 앱 현대화
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d1ce2e7dc434558ac1efd52f6def99d63b38c57e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161517"
---
# <a name="features-that-require-package-identity"></a>패키지 ID가 필요한 기능

[최신 Windows 10 환경](index.md)으로 데스크톱 앱을 업데이트하려는 경우 [패키지 ID](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)가 있는 데스크톱 앱에서만 많은 기능을 사용할 수 있습니다. 데스크톱 앱에 패키지 ID를 부여하는 방법에는 여러 가지가 있습니다.

* [MSIX 패키지](/windows/msix/desktop/desktop-to-uwp-root)에 패키지합니다. MSIX는 모든 Windows 앱, WPF, Windows Forms 및 Win32 앱에 유니버설 패키징 환경을 제공하는 최신 앱 패키지 형식입니다. 강력한 설치 및 업데이트 환경, 유연한 기능 시스템을 갖춘 관리형 보안 모듈, Microsoft Store, 엔터프라이즈 관리 및 여러 사용자 지정 모델에 대한 지원 기능을 제공합니다. 자세한 내용은 MSIX 설명서의 [데스크톱 애플리케이션 패키지](/windows/msix/desktop/desktop-to-uwp-root)를 참조하세요.
* 데스크톱 앱을 배포하기 위해 MSIX 패키지를 채택할 수 없는 경우 Windows 10 버전 2004부터 패키지 매니페스트만 포함된 *스파스 MSIX 패키지*를 만들어 패키지 ID를 부여할 수 있습니다. 자세한 내용은 [패키지되지 않은 데스크톱 앱에 ID 부여](grant-identity-to-nonpackaged-apps.md)를 참조하세요.

데스크톱 앱에 패키지 ID가 있는 경우 앱에서 다음 기능을 사용할 수 있습니다.

## <a name="use-windows-runtime-apis-that-require-package-identity"></a>패키지 ID가 필요한 Windows 런타임 API 사용

다음 Windows 런타임 API 목록에는 데스크톱 앱에서 사용할 패키지 ID가 필요합니다. [API 목록](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>확장 패키지와 통합

애플리케이션을 시스템에 통합해야 하는 경우(예: 방화벽 규칙 설정) 애플리케이션의 패키지 매니페스트에 설명해 두면 시스템이 나머지 과정을 알아서 처리합니다. 이런 작업 대부분에서 코드를 작성할 필요가 없습니다. 매니페스트의 몇몇 XML을 사용하여 사용자가 로그인할 때 프로세스를 시작하고, 파일 탐색기에 애플리케이션을 통합하고, 다른 앱에 표시되는 인쇄 대상 목록에 애플리케이션을 추가하는 등 다양한 작업을 수행할 수 있습니다.

자세한 내용은 [데스크톱 앱을 패키지 확장과 통합](desktop-to-uwp-extensions.md)을 참조하세요.

## <a name="extend-with-uwp-components"></a>UWP 구성 요소를 사용하여 확장

일부 Windows 10 환경(예: 터치 지원 UI 페이지)은 최신 앱 컨테이너 내부에서 실행해야 합니다. 일반적으로 Windows 런타임 API로 기존 데스크톱 애플리케이션을 [향상](desktop-to-uwp-enhance.md)시켜서 환경을 추가할 것인지 먼저 결정해야 합니다. 환경을 구현하기 위해 UWP 구성 요소를 사용해야 하는 경우 UWP 프로젝트를 솔루션에 추가하고 앱 서비스를 사용하여 데스크톱 애플리케이션과 UWP 구성 요소 간에 통신할 수 있습니다.

자세한 내용은 [UWP 구성 요소를 사용하여 데스크톱 앱 확장](desktop-to-uwp-extend.md)을 참조하세요.

## <a name="distribute"></a>배포

MSIX 패키지에 앱을 패키지하는 경우 Microsoft Store에 게시하거나 다른 시스템에 테스트용으로 로드하여 배포할 수 있습니다.

[패키지 데스크톱 앱 배포](desktop-to-uwp-distribute.md)를 참조하세요.