---
title: 플랫 번들 앱 패키지
description: 플랫 번들을 만들어 앱 패키지에 대한 참조가 있는 앱의 .appx 패키지 파일을 번들로 만드는 방법을 설명합니다.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, 패키징, 패키지 구성, 플랫 번들
ms.localizationpriority: medium
ms.openlocfilehash: b7066b7f2e5bd72ebee3169e03c7940b6fef4dba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638488"
---
# <a name="flat-bundle-app-packages"></a>플랫 번들 앱 패키지 

> [!IMPORTANT]
> Microsoft Store에 앱을 제출하고자 하는 경우 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)에 문의하여 플랫 번들에 대한 승인을 얻어야 합니다.

플랫 번들은 앱의 패키지 파일을 번들로 묶는 향상 된 방법입니다. 일반적인 Windows 앱 패키지 파일을 번들 내에 포함 해야 하는 여러 수준의 패키징 구조를 사용 하는 앱 번들 파일을, 플랫 번들만 앱 번들 외부에서 사용할 수 있도록 하는 앱 패키지 파일을 참조 하 여이 제거 합니다. 번들에 맞춰 앱 패키지 파일을 더 이상 들어 있으므로 수 있는 병렬 처리 결과입니다 (때문에 각 앱 패키지 파일을 동시에 처리할 수)의 시간, 더 빠른 게시, 업로드 감소 되어 궁극적으로 더 빠르게 개발 반복 합니다.

![플랫 번들 다이어그램](images/bundle-combined.png)

플랫 번들의 다른 장점은 패키지를 덜 만들어도 된다는 점입니다. 앱 패키지 파일이 참조만 되 되므로 두 버전의 앱 패키지 두 버전 간에 변경 되지 않은 경우 동일한 패키지 파일을 참조할 수 있습니다. 이를 통해 다음 버전의 앱에 대한 패키지를 만들 때 변경한 앱 패키지만 만들면 됩니다.
기본적으로 플랫 번들 앱 패키지 파일 자체와 동일한 폴더 내에서 참조 합니다. 그러나 이 참조는 다른 경로(상대 경로, 네트워크 공유 및 http 위치)로 변경될 수 있습니다. 이를 수행하려면 플랫 번들을 만들 때 **BundleManifest**를 직접 제공해야 합니다. 

## <a name="how-to-create-a-flat-bundle"></a>플랫 번들을 만드는 방법

MakeAppx.exe 도구를 사용하거나 번들의 구조를 정의할 패키징 레이아웃을 사용하여 플랫 번들을 만들 수 있습니다.

### <a name="using-makeappxexe"></a>MakeAppx.exe 사용
MakeAppx.exe를 사용 하 여 플랫 번들을 만들려면 명령을 사용 하 여 "MakeAppx.exe 번들" 평소 처럼 있지만 /fb 스위치를 사용 하 여의 플랫 앱 번들 파일 (만 앱 패키지 파일을 참조 하 고 모든 실제 페이로드는 이므로 매우 작은 수를 생성 합니다. ). 

명령 구문의 예는 다음과 같습니다.

```syntax
MakeAppx bundle [options] /d <content directory> /fb /p <output flat bundle name>
```

MakeAppx.exe를 만드는 방법에 대한 자세한 내용은 [MakeAppx.exe 도구로 앱 패키지 만들기](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)를 참조하세요.

### <a name="using-packaging-layout"></a>패키지 레이아웃 사용
또는 압축 레이아웃을 사용하여 플랫 번들을 만들 수 있습니다. 이 작업을 수행하려면 앱 번들 매니페스트의 **PackageFamily** 요소에 있는 **FlatBundle** 특성을 **true**로 설정합니다. 패키징 레이아웃에 대한 자세한 내용은 [패키징 레이아웃으로 패키지 만들기](packaging-layout.md)를 참조하세요.

## <a name="how-to-deploy-a-flat-bundle"></a>플랫 번들을 배포하는 방법 
플랫 번들을 배포하기 전에 앱 번들뿐만 아니라 동일한 인증서를 사용하여 각 앱 패키지에 서명해야 합니다. 이 개별 파일은 이제 모든 앱 패키지 파일 (.appx/.msix) 더 이상 앱 번들 (.appxbundle/.msixbundle) 파일에 포함 되지 않은 때문입니다. 패키지가 서명 된 후 사용 합니다 [Add-appxpackage cmdlet](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) 앱 번들 파일을 가리키도록 (앱 패키지는 앱 번들 예상 수 있도록 가정) 앱을 배포 하는 PowerShell에서. 