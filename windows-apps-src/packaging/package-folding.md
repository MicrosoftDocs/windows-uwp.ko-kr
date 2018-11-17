---
author: laurenhughes
title: 자산 패키지 및 패키지 접기를 사용하여 개발
description: 자산 패키지 및 패키지 접기를 사용하여 앱을 효율적으로 구성하는 방법을 알아보세요.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
keywords: Windows 10, 패키징, 패키지 레이아웃, 자산 패키지
ms.localizationpriority: medium
ms.openlocfilehash: efdf560158e2b57ae9e05ecc31d49c7cf981d8c0
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7159375"
---
# <a name="developing-with-asset-packages-and-package-folding"></a>자산 패키지 및 패키지 접기를 사용하여 개발 

> [!IMPORTANT]
> Microsoft Store에 앱을 제출하고자 하는 경우 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)에 문의하여 자산 패키지 및 패키지 접기에 대한 승인을 얻어야 합니다.

자산 패키지는 전체적인 패키지 크기 및 앱을 Microsoft Store에 게시하는 데 걸리는 시간에 나쁜 영향을 미칠 수 있습니다. [자산 패키지 소개](asset-packages.md)에서 자산 패키지에 대한 자세한 내용 및 이를 사용하여 개발 반복 속도를 높일 수 있는 방법을 알아볼 수 있습니다.

자신의 앱에 자산 패키지를 사용하는 것을 고려 중이거나 사용하려는 경우 자산 패키지를 사용하여 개발 프로세스를 변경할 수 있는 방법에 대해 궁금할 것입니다. 간단히 말하자면 사용자에게 앱 개발은 동일하게 유지됩니다. 이는 자산 패키지에 대한 패키지 접기 때문입니다.

## <a name="file-access-after-splitting-your-app"></a>앱을 분할한 후 파일 액세스

패키지 접기가 개발 프로세스에 어떤 영향도 미치지 않는 방법을 이해하기 위해 먼저 자산 패키지 또는 리소스 패키지를 사용하여 앱을 여러 개의 패키지로 분할 때 발생하는 일을 알아보겠습니다. 

대략적으로 앱의 파일을 아키텍처 패키지가 아닌 다른 패키지로 분할하면 코드가 실행되는 위치를 기준으로 해당 파일에 직접 액세스할 수 없습니다. 이러한 패키지가 모두 아키텍처 패키지가 설치된 다른 디렉터리에 설치되기 때문입니다. 예를 들어 게임을 만들고 있다면 및로 게임을 현지화 프랑스어 및 독일어 용으로 빌드된 x86 및 x64 컴퓨터를 다음 게임의 앱 번들 내에 이러한 앱 패키지 파일이 있어야 합니다.

-   MyGame_1.0_x86.appx
-   MyGame_1.0_x64.appx
-   MyGame_1.0_language-fr.appx
-   MyGame_1.0_language-de.appx

게임 사용자의 컴퓨터에 설치 되 면 각 앱 패키지 파일 **WindowsApps** 디렉터리에 자체 폴더를 갖게 됩니다. 따라서 64비트 Windows를 실행 중인 프랑스어 사용자의 경우 게임의 모습은 다음과 같습니다.

```example
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   `-- …
|-- MyGame_1.0_language-fr
|   `-- …
`-- …(other apps)
```

Note 사용자에 게 적용 되지 않는 파일 됩니다 앱 패키지는 (x86 및 독일어 패키지)를 설치 합니다. 

이 사용자의 경우 게임의 주요 실행 파일은 **MyGame_1.0_x64** 폴더에 위치하고 여기에서 실행될 것이며, 일반적으로 이 폴더의 파일에만 액세스할 수 있습니다. **MyGame_1.0_language-fr** 폴더의 파일에 액세스하려면 MRT API 또는 PackageManager API를 사용해야 합니다. MRT Api 설치 된 언어에서 가장 적절 한 파일을 자동으로 선택 수, [Windows.ApplicationModel.Resources.Core](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core)에서 MRT Api에 대 한 자세한 내용을 찾을 수 있습니다. 또는 [PackageManager 클래스](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager)를 사용하여 프랑스어 언어 패키지가 설치된 위치를 찾을 수 있습니다. 앱 패키지가 설치된 위치는 변경될 수 있으며 사용자에 따라 다를 수 있으므로 이 위치를 넘겨 짐작해서는 안 됩니다. 

## <a name="asset-package-folding"></a>자산 패키지 접기

그러면 어떻게 자산 패키지의 파일에 액세스할 수 있을까요? 물론 사용하고 있는 파일 액세스 API를 계속 사용하여 아키텍처 패키지의 다른 파일에 액세스할 수 있습니다. 자산 패키지 파일이 패키지 접기 프로세스를 통해 설치될 때 아키텍처 패키지에 접히기 때문입니다. 또한 자산 패키지 파일은 원래 아키텍처 패키지 내에 있는 파일이어야 하므로 개발 과정에서 느슨한 파일 배포에서 패키지된 배포로 전환할 때 API 사용을 변경할 필요가 없습니다. 

패키지 접기 작동 방식에 대해 자세히 알아보기 위해 예를 들어 시작해 봅니다. 다음 파일 구조를 가진 게임 프로젝트 경우:

```example
MyGame
|-- Audios
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Videos
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
```

게임을 3개의 패키지로 분할하려면 x64 아키텍처 패키지, 오디오에 대한 자산 패키지, 동영상에 대한 자산 패키지, 게임이 이러한 패키지로 나뉩니다.

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_1.0_Audios.appx
`-- Audios
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
MyGame_1.0_Videos.appx
`-- Videos
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
```

게임을 설치하면 x64 패키지가 먼저 배포됩니다. 이전 예제의 **MyGame_1.0_language-fr**과 같이 두 자산 패키지는 여전히 자체 폴더에 배포됩니다. 그러나 패키지 접기 때문에 자산 패키지 파일도 하드 링크되어 **MyGame_1.0_x64** 폴더에 나타나야 합니다(따라서 파일이 두 위치에 표시되더라도 디스크 공간을 두 배만큼 차지하지 않음). 자산 패키지 파일이 나타나는 위치는 정확히 패키지의 루트에 상대적인 위치입니다. 배포된 게임의 최종 레이아웃 모습은 다음과 같습니다.

```example 
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   |-- Audios
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Videos
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Engine
|   |   `-- ...
|   |-- XboxLive
|   |   `-- ...
|   `-- Game.exe
|-- MyGame_1.0_Audios
|   `-- Audios
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
|-- MyGame_1.0_Videos
|   `-- Videos
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
`-- …(other apps)
```

자산 패키지에 패키지 접기를 사용할 때 여전히 동일한 방식으로 자산 패키지에 분할한 파일에 액세스할 수 있으며(아키텍처 폴더는 원래 프로젝트 폴더와 정확히 동일한 구조를 가짐) 코드에 영향을 주지 않고 자산 패키지를 추가하거나 자산 패키지 간에 파일을 이동할 수 있습니다. 

이제 보다 복잡한 패키지 예를 살펴보겠습니다. 수준에 따라 파일을 분할하려고 할 때 원래 프로젝트 폴더와 동일한 구조를 유지하려는 경우 패키지의 모습은 다음과 같습니다.

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_Level1.appx
|-- Audios
|   `-- Level1
|       `-- ...
`-- Videos
    `-- Level1
        `-- ...

MyGame_Level2.appx
|-- Audios
|   `-- Level2
|       `-- ...
`-- Videos
    `-- Level2
        `-- ...
```
이를 통해 **Level1** 폴더와 **MyGame_Level1** 패키지의 파일 및 **Level2** 폴더와 **MyGame_Level2** 패키지의 파일이 패키지 접기에서 **Audios** 및 **Videos** 폴더에 병합될 수 있습니다. 일반적으로 매핑 파일에 패키징된 파일에 대해 지정된 상대 경로 및 MakeAppx.exe에 대한 [패키징 레이아웃](packaging-layout.md)은 패키지 접기 후 액세스에 사용해야 하는 경로입니다. 

마지막으로, 동일한 상대 경로를 가지는 다른 자산 패키지에 두 개의 파일이 있는 경우 패키지 접기에서 충돌을 일으킵니다. 충돌이 발생하면 앱의 배포에서 오류가 발생하고 실패합니다. 또한 패키지 접기는 하드 링크를 활용하기 때문에 자산 패키지를 사용하는 경우 앱은 NTFS 드라이브 이외에 배포할 수 없습니다. 사용자가 앱을 이동식 드라이브로 이동할 가능성이 있는 경우 자산 패키지를 사용하지 않아야 합니다. 


