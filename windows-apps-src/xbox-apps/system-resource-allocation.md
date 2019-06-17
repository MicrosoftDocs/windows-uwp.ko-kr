---
title: Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스
description: Xbox의 UWP 시스템 리소스
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: 8cc6ca24453be83f5c10cc6c86c508a5a3f99c4c
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131927"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스

Xbox One에서 실행되는 UWP 앱은 시스템 및 다른 앱과 리소스를 공유합니다. Xbox One 앱의 UWP에 사용할 수 있는 리소스는 사용자가 앱으로 제출했는지 Xbox Live 크리에이터스 프로그램 게임으로 제출했는지 여부에 따라 다릅니다.

* 전경에서 실행될 때 사용할 수 있는 최대 메모리는 다음과 같습니다.
    * 앱: 1GB
    * 게임. 5GB

백그라운드에서 실행되는 앱에서 사용할 수 있는 최대 메모리는 128MB입니다. 백그라운드 모드는 백그라운드 음악 플레이어와 같은 동시 응용 프로그램에만 적용됩니다.  게임이 중단되고 백그라운드에서 종료됩니다.

이러한 제한을 초과하면 메모리 할당 오류가 발생합니다. 메모리 사용 모니터링에 대한 자세한 내용은 [MemoryManager 클래스](https://docs.microsoft.com/uwp/api/windows.system.memorymanager) 참조를 참조하세요.

> [!NOTE]
> Visual Studio 디버거에서 앱 이나 게임을 실행 하는 경우 이러한 메모리 제약 조건 적용 되지 않습니다. 이 제한은 디버깅 모드에서 실행하지 않는 경우에만 적용됩니다.

* CPU
    * 앱: 시스템에서 실행되는 앱과 게임 수에 따라 CPU 코어 2~4개 공유.
    * 게임. 전용 및 2 4 CPU 코어를 공유 합니다.

* GPU
    * 앱: 시스템에서 실행되는 앱과 게임 수에 따라 GPU의 45% 공유.
    * 게임: 사용 가능한 GPU 주기에 대한 모든 권한.

* DirectX 지원
    * 앱: DirectX 11 기능 수준 10입니다.
    * 게임. DirectX 12 및 DirectX 11 기능 수준 10입니다.

* 모든 앱 및 게임은 Microsoft Store용으로 개발하거나 제출하려면 x64 아키텍처를 대상으로 해야 합니다.  

**응용 프로그램을 개발**을 위해 사용할 수 있는 리소스가 표준 PC에 비에 제한될 수 있고 시스템에서 실행 중인 앱과 게임의 수에 따라 다를 수 있습니다.

**게임 개발**의 경우 다른 게임 콘솔과 마찬가지로 Xbox One은 전체 잠재 기능에 액세스하기 위해 특정 하드웨어 기반 개발 키트가 필요한 특수 하드웨어입니다. Xbox One 하드웨어의 최대 잠재 기능에 액세스해야 하는 게임을 개발하는 경우 Xbox One 개발 키트에 액세스하기 위해 [ID@Xbox](https://www.xbox.com/Developers/id) 프로그램에 등록할 수 있습니다.

## <a name="see-also"></a>참조
- [Xbox One에서 UWP](index.md)
- [Xbox Live 크리에이터 스 프로그램 시작](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-program)
- [DirectX 및 Xbox One에서 UWP](https://walbourn.github.io/)