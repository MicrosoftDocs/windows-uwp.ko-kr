---
author: laurenhughes
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: 수동 앱 패키징
description: 이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱의 수동 패키징에 대한 문서 또는 링크가 포함되어 있습니다.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 패키징
ms.localizationpriority: medium
ms.openlocfilehash: fcd6d937c7261b5cfa8af954eb5d2ec2869d8afd
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5126011"
---
# <a name="manual-app-packaging"></a>수동 앱 패키징

앱 패키지를 만들어 서명하려 하지만 앱 개발에 Visual Studio를 사용하지 않은 경우, 수동 앱 패키징 도구를 사용해야 합니다.

> [!IMPORTANT] 
> Visual Studio를 사용하여 앱을 개발하는 경우 Visual Studio 마법사를 사용하여 앱 패키지를 만들고 서명하는 것이 좋습니다. 자세한 내용은 [Visual Studio를 사용하여 UWP 앱 패키징](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)을 참조하세요.

## <a name="purpose"></a>용도

이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱의 수동 패키징에 대한 문서 또는 링크가 포함되어 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [MakeAppx.exe 도구를 사용하여 앱 패키지 만들기](create-app-package-with-makeappx-tool.md) | MakeAppx.exe는 앱 패키지와 번들을 만들고, 암호화 및 암호 해독하고, 파일을 추출합니다. |
| [패키지 서명 인증서 만들기](create-certificate-package-signing.md) | PowerShell 도구를 사용하여 앱 패키지 서명 인증서를 만들고 내보냅니다. |
| [SignTool을 사용하여 앱 패키지에 서명](sign-app-package-using-signtool.md) | SignTool을 사용하여 인증서로 앱 패키지에 수동으로 서명합니다. |

### <a name="advanced-topics"></a>고급 항목

이 섹션에는 더 효율적인 패키징 및 설치에 대한 대규모 및 복잡한 앱을 구성 요소화하는 고급 항목이 포함되어 있습니다. 

> [!IMPORTANT]
> Microsoft Store에 앱을 제출하고자 하는 경우 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)에 문의하여 이 섹션에 나온 기능의 모든 사용에 대한 승인을 얻어야 합니다.


| 항목 | 설명 |
|-------|-------------|
| [자산 패키지 소개](asset-packages.md) | 자산 패키지는 아키텍처 패키지 전체에서 중복된 파일에 대한 필요성을 효율적으로 제거하는 응용 프로그램의 일반적인 파일에 대한 중앙 집중식 위치 역할을 수행할 패키지의 유형입니다. |
| [자산 패키지 및 패키지 접기를 사용하여 개발](package-folding.md) | 자산 패키지 및 패키지 접기를 사용하여 앱을 효율적으로 구성하는 방법을 알아보세요. |
| [플랫 번들 앱 패키지](flat-bundles.md) | 앱의 패키지 파일에 대 한 플랫 번들을 만드는 방법을 설명 합니다. |
| [패키징 레이아웃으로 패키지 만들기](packaging-layout.md) | 패키징 레이아웃은 앱 패키징 구조를 설명하는 단일 문서입니다. 앱(기본 및 선택)의 번들, 번들의 패키지, 패키지의 파일을 지정합니다. |
