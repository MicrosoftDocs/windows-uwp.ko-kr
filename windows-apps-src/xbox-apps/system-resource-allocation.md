---
title: Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스
description: Xbox 앱에서 실행 되는 UWP 앱이 시스템 및 다른 앱과 리소스를 공유 하는 방법 및 UWP 앱 또는 게임의 리소스 요구 사항에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: dbc6af56867508e3178ddd8b731af49270faf3d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174687"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스

Xbox One에서 실행 되는 UWP 앱은 시스템 및 기타 앱과 리소스를 공유 합니다. Xbox One 앱에서 UWP에 사용할 수 있는 리소스는 앱으로 제출 하는지 또는 Xbox Live 크리에이터 프로그램 게임으로 제출 하는지에 따라 달라 집니다.

* 포그라운드로 실행 되는 동안 사용 가능한 최대 메모리:
    * 앱: 1gb
    * 게임: 5gb

백그라운드에서 실행 되는 앱에 사용할 수 있는 최대 메모리는 128 MB입니다. 백그라운드 모드는 배경 음악 플레이어와 같은 동시 응용 프로그램에만 적용 됩니다.  게임은 백그라운드에서 일시 중단 되 고 종료 됩니다.


이러한 제한을 초과 하면 메모리 할당 오류가 발생 합니다. 메모리 사용 모니터링에 대 한 자세한 내용은 [Memorymanager 클래스](/uwp/api/windows.system.memorymanager) 참조를 참조 하세요.

> [!NOTE]
> Visual Studio 디버거에서 앱 또는 게임을 실행 하는 경우 이러한 메모리 제약 조건은 적용 되지 않습니다. 이 제한은 디버깅 모드에서 실행 되지 않는 경우에만 적용할 수 있습니다.

* CPU
    * 앱: 시스템에서 실행 되는 앱 및 게임의 수에 따라 2-4 CPU 코어를 공유 합니다.
    * 게임: 4 전용 및 2 개의 공유 CPU 코어

* GPU
    * 앱: 시스템에서 실행 되는 앱 및 게임의 수에 따라 GPU의 45%를 공유 합니다.
    * 게임: 사용 가능한 GPU 주기에 대 한 모든 권한을 가집니다.

* DirectX 지원
    * 앱: DirectX 11 기능 수준 10.
    * 게임: DirectX 12 및 DirectX 11 기능 수준 10.

* 모든 앱과 게임은 Xbox 용 스토어에 개발 하거나 제출할 수 있도록 x64 아키텍처를 대상으로 해야 합니다.  

**응용 프로그램 개발**의 경우 표준 PC와 비교할 때 사용할 수 있는 리소스를 제한할 수 있으며 시스템에서 실행 되는 앱 및 게임의 수에 따라 달라질 수 있습니다.

**게임 개발**의 경우, Xbox one은 다른 게임 콘솔과 마찬가지로, 특정 하드웨어 기반 개발 키트가 모든 잠재력에 액세스할 수 있어야 하는 특수 한 하드웨어 부분입니다. Xbox one 하드웨어의 최대 잠재력에 액세스 해야 하는 게임에서 작업 하는 경우 [ID@Xbox](https://www.xbox.com/Developers/id) Xbox one development kit에 액세스 하려면 프로그램에 등록 하는 것이 좋습니다.

## <a name="see-also"></a>참고 항목
- [Xbox One의 UWP](index.md)
- [Xbox Live 크리에이터 프로그램 시작](/gaming/xbox-live/get-started-with-creators/creators-program)
- [Xbox One의 DirectX 및 UWP](https://walbourn.github.io/)