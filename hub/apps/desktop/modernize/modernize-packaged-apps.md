---
Description: Windows 앱 패키지에 패키지 있습니다 데스크톱 응용 프로그램을 Windows 10 사용자를 위해 최신 환경 추가 하는 방법에 알아봅니다.
title: 패키지에 포함 된 데스크톱 앱을 현대화
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6d71233bc7b96af9d9b261406d6b149f36f65f29
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985293"
---
# <a name="features-that-require-package-identity"></a>패키지 id를 요구 하는 기능

사용 하 여 데스크톱 앱을 업데이트 하려는 경우 [최신 Windows 10 환경용](index.md), 많은 기능은 MSIX 패키지에 패키지 된 데스크톱 앱 에서만 사용할 수 있습니다.

MSIX 모든 Windows 앱, WPF, Windows Forms 및 Win32 앱에 대 한 유니버설 패키징 환경을 제공 하는 최신 Windows 앱 패키지 형식입니다. 데스크톱 Windows 앱 패키지를 사용 하면 앱 라이브 타일 및 알림 같은 최신 Windows 10 환경을 통합할 수 있습니다. 또한 강력한 설치 및 업데이트 환경, Microsoft Store, 엔터프라이즈 관리 및 기타 사용자 지정 배포 모델에 대 한 지원 시스템을 유연한 기능을 사용 하 여 관리 되는 보안 모델에 대 한 액세스를 가져옵니다. 자세한 내용은 [데스크톱 응용 프로그램 패키지](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) MSIX 설명서에서.

데스크톱 앱을 패키지 하는 경우에 패키지 id, 패키지 확장명 및 패키지 된 앱에서 UWP 구성 요소를 필요로 하는 UWP Api를 사용 합니다. 자세한 내용은 다음이 문서를 참조 하세요.

## <a name="use-uwp-apis-that-require-package-identity"></a>패키지 id를 필요로 하는 UWP Api를 사용 하 여

일부 UWP Api에 필요 [identity 패키지](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 데스크톱 앱에서 사용할 수 있습니다. 패키지 매니페스트 등 MSIX 패키지는이 id를 제공 합니다.

자세한 내용은 [Api에서이 목록은](desktop-to-uwp-supported-api.md#list-of-apis)합니다.

## <a name="integrate-with-package-extensions"></a>확장 패키지를 사용 하 여 통합

응용 프로그램이 시스템과 통합 해야 하는 경우 (예: 방화벽 규칙을 설정), 응용 프로그램의 패키지 매니페스트에 해당 항목을 설명 하 고 시스템은 나머지를 수행 합니다. 이런 작업 대부분에서 코드를 작성할 필요가 없습니다. 매니페스트에서 XML의 비트를 사용 하 여 수행할 수 있습니다, 사용자가 로그온 할 때 프로세스를 시작 하 고, 파일 탐색기에 응용 프로그램을 통합 하 고, 응용 프로그램을 추가 하는 등 다른 앱에서 표시 되는 인쇄 대상 목록입니다.

자세한 내용은 [데스크톱 앱의 패키지 확장을 사용 하 여 통합](desktop-to-uwp-extensions.md)합니다.

## <a name="extend-with-uwp-components"></a>UWP 구성 요소를 사용 하 여 확장

일부 Windows 10 환경(예, 터치 구현 UI 페이지)은 최신 앱 컨테이너 내부에서 실행해야 합니다. 일반적으로 먼저 결정 해야 하는지 여부를 하 여 체험을 추가할 수 있습니다 [향상](desktop-to-uwp-enhance.md) UWP Api를 사용 하 여 기존 데스크톱 응용 프로그램입니다. 경험을 얻는 UWP 구성 요소를 사용 해야 할 경우 UWP 프로젝트를 솔루션에 추가 및 앱 서비스를 사용 하 여 데스크톱 응용 프로그램 및 UWP 구성 요소 간 통신을 수 있습니다.

자세한 내용은 [UWP 구성 요소를 사용 하 여 데스크톱 앱을 확장](desktop-to-uwp-extend.md)합니다.

## <a name="distribute"></a>배포

Microsoft Store 게시 하거나 사이드 로드 하 여 응용 프로그램을 배포할 수 있습니다 다른 시스템에 놓습니다.

참조 [패키지에 포함 된 데스크톱 앱을 배포할](desktop-to-uwp-distribute.md)합니다.
