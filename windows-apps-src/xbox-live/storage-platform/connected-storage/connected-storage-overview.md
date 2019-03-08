---
title: 연결 된 저장소 개요
description: 연결 된 저장소를 사용 하 여 저장 하 고 장치 간에 게임 데이터를 로드 하는 방법을 알아봅니다.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장소, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 40ad13e46e074154d72d7aad236747c3374110ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639058"
---
# <a name="connected-storage"></a>연결된 저장소
연결 된 저장소는 게임 플레이 데이터 및 장치 간에 로밍 해야 하는 기타 관련 상태 데이터를 저장 하 여 제목 수 있도록 설계 되었습니다. 연결 된 저장소 API를 통해 Xbox One 및 유니버설 Windows Platform(UWP) 저장, 로드 및 로컬로 저장 및 Xbox One 또는 UWP 제목 인터넷에 연결 될 때마다 또한 클라우드로 동기화 되는 제목 데이터를 삭제 하려면 타이틀 수 있습니다. 저장 된 데이터 동기화가 발생 한 후을 제목을 실행 하는 다른 모든 장치에 제공 됩니다. 개발자는 집 밖 최고의 재생 환경을 제공 하려면 가능한 한 정확 하 게 제목 상태를 저장 하는 것이 좋습니다. 연결 된 저장소를 사용 하면 집, 게임 진행 되 고 중단 했던 위치 같은 게임을 지 원하는 다른 모든 장치에 게임 권한은 선택 합니다.

## <a name="connected-storage-features"></a>연결 된 저장소 기능

연결 된 저장소 API는 다음과 같은 기능을 제공합니다.

- 앱 다음 hdd 시스템에서 로컬로 캐시 되 고 클라우드로 업로드 하는 메모리 버퍼에 데이터의 최대 16MB를 한 번에 저장할 신속 하 게 수 있습니다.
- 관리 되는 파트너 및 ID@Xbox 개발자:
    - 클라우드 저장소의 사용자/앱 당 256mb 이상.
- Xbox Live 크리에이터 스 프로그램 개발자:
    - 64MB 사용자/앱 당 클라우드 저장소입니다.
- 전원 오류에 대 한 강력한 응답-앱 저장 되는 일부 데이터와 함께 처리할 필요가 없습니다.
- 앱 실행 중이 아닌 경우에 데이터를 클라우드로 자동으로 업로드 됩니다.
- Xbox Live에 연결 된 Xbox One 또는 UWP 장치의 데이터를 사용할 수 있습니다.
- Xbox Live 앱 참여를 요구 하지 않고 장치 간 동기화 및 충돌 관리를 처리 합니다.

연결 된 저장소 시스템에서는 모든 저장 한 전체적으로 나타나거나 전혀 나타나지 않았는지입니다. 즉, 개발자는 되지 있다고 정전 또는 앱을 갑자기 종료 되는 사용자의 경우 게임 상태에 영향을 주는 부분적으로 저장 된 데이터에 대 한 걱정 수동으로 또는 다른 앱/게임 콘솔을 열어서 합니다. 또한 이렇게 하면 게임을 원활 하 게 연결 된 저장소는 중요 한 부분은 사용자가 게임 간에 전환할 수 있습니다 프로그램 타이틀의 사용자를 위한 환경을 재생 앱 신속 하 게 하 고 자유롭게 있지만 여전히로 돌아가 남아 있는 상태에서 원래 게임입니다. 사용자 고유의 제목에 이러한 기능을 구현 하기 위해 연결 된 저장소 Api를 이해 해야 합니다.

## <a name="connected-storage-structure"></a>연결 된 저장소 구조

연결 된 저장소 시스템 컨테이너에 하나 이상의 blob으로 데이터를 저장 하는 앱을 허용 합니다. 높은 수준에서 연결 된 저장소 시스템의 모든 데이터는 사용자와 연결 하거나 사용자 또는 컴퓨터 Xbox 개발 키트의 경우 제목 개발 합니다. 특정 사용자에 대 한 앱에서 모든 데이터를 저장 하거나 컴퓨터를 연결 된 저장소 공간에 저장 됩니다. 앱의 각 사용자는 최대 256 또는 64의 총 저장소를 사용 하 여 연결 된 저장소 공간을 가져옵니다. 것에 앱이이 저장소 전용 이므로 다른 앱과 공유 되지 않습니다.

### <a name="containers-and-blobs"></a>컨테이너 및 blob

연결 된 저장소 컨테이너 또는 줄여서 컨테이너는 저장소의 기본 단위. 다음 다이어그램에 나와 있는 것 처럼 각 연결 된 저장소 공간에서 다양 한 컨테이너를 포함할 수 있습니다.

연결 된 저장소 공간 (제목 m/컴퓨터 또는 제목/사용자)

![connected_storage_space_containers.png](../../images/connected_storage/connected_storage_space_containers.png)

 데이터는 blob를 호출 하는 하나 이상의 버퍼로 컨테이너에 저장 됩니다. 다음 다이어그램에서는 디스크에서 컨테이너의 내부 시스템 표현을 보여 줍니다. 각 컨테이너에 대 한 컨테이너의 각 blob에 대 한 데이터 파일에 대 한 참조를 포함 하는 컨테이너 파일이 있습니다.

컨테이너의 다이어그램

![container_storage_blobs.png](../../images/connected_storage/container_storage_blobs.png)

컨테이너에서 데이터를 저장 하려면 적절 한 Api 컨테이너 SubmitUpdatesAsync, 이름 및 blob (버퍼 개체)의 맵을 제공 메서드를 호출 합니다. SubmitUpdatesAsync 호출에 설명 된 모든 변경 내용을 원자 단위로 적용 됩니다, 즉, 중 하나 모든 blob는 요청에 따라 업데이트 됩니다 또는 전체 작업이 종료 되 고 컨테이너를 호출 하기 전에 상태로 유지 됩니다.

한 번에 SubmitUpdatesAsync를 사용 하는 작업을 저장 하는 개별 데이터의 16MB로 제한 됩니다.

## <a name="connected-storage-api"></a>연결 된 저장소 API

연결 된 저장소에는 XDK 및 UWP 앱에 대 한 별도 Api가 있습니다. 다행 스럽게도 이러한 Api와 유사 서로 매우 밀접 하 게 합니다. 두 가지 Api 네임 스페이스 및 클래스 이름에 주로 다릅니다. 하는 데 필요한 함수 [저장](connected-storage-saving.md)를 [로드](connected-storage-loading.md), 및 [삭제](connected-storage-deleting.md) API 사용 하 여 데이터의 이름이 동일 합니다.

두 개의 연결 된 저장소 Api 간의 추가 차이점의 연결 된 저장소 섹션에 자세히 나와 [XDK에서 UWP로 포팅 Xbox Live 코드](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)합니다.

XDK.chm 파일 경로 아래에 설명 된 XDK 연결 된 저장소 Api를 찾을 수 있습니다. **Xbox 하나 XDK >> API 참조 >> 플랫폼 API 참조 >> 시스템 API 참조 >> Windows.Xbox.Storage**합니다.
XDK Api에도 설명 되어는 [developer.microsoft.com 사이트](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)합니다.
XDK Api에 대 한 링크는 Microsoft Account(MSA) Xbox 개발자 Kit(XDK) 액세스 가능 하도록 설정 되어 있어야 합니다.
Windows.Xbox.Storage에는 Xbox One 콘솔에 대 한 저장소 연결 된 네임 스페이스의 이름입니다.

UWP 연결 된 저장소 Api 경로 아래에 있는 Xbox Live SDK.chm 파일에 문서화를 찾을 수 있습니다. **Xbox Live Api >> Xbox Live 플랫폼 확장 SDK API 참조 >> Windows.Gaming.XboxLive.Storage**합니다.
UWP 연결 된 저장소 Api도 문서화 되어 온라인 아래 합니다 [Windows.Gaming.XboxLive.Storage 네임 스페이스 참조](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)합니다.
Windows.Gaming.XboxLive.Storage에는 UWP 앱에 대 한 저장소 연결 된 네임 스페이스의 이름입니다.

연결 된 저장소를 획득 해야 하는 연결 된 저장소를 사용 하려면 *공간*입니다. 연결 된 저장소 공간을 사용자 또는 컴퓨터에 연결 되며 모든 연결 된 해당 사용자 또는 컴퓨터의 형태로 저장소 연결 된 데이터를 보유 *컨테이너* 하 고 *blob*합니다. 사용자나 컴퓨터에 대 한 연결 된 저장소 공간을 가져오는 읽기 및 쓰기 저장 하는 엔터티 데이터에 액세스할 수 있게 됩니다. XDK와 UWP 모두 타이틀을 획득 하기 위해 연결 된 저장소 공간을 호출 합니다 `GetForUserAsync` 메서드를 XDK 제목 호출할 수도 있습니다는 `GetForMachineAsync` 메서드를 UWP 제목 되지 호출할 수 `GetForMachineAsync`. `GetForUserAsync` 및 `GetForMachineAsync` 에 포함 된는 `ConnectedStorageSpace` 는 XDK에서 클래스입니다. `GetForUserAsync` 에 포함 된 `GameSaveProvider` UWP API의 클래스. 이 메서드는 실행 시간이 긴 작업을 사용자 장치 하나에 저장 된 데이터에 다른 장치에서 처음으로 게임 플레이 다시 시작 하는 경우에 특히. `GetForUserAsync` 만들기, 액세스 및 컨테이너를 삭제 하려면 사용할 수 있는 사용자에 대 한 연결 된 저장소 공간을 검색 합니다.

컨테이너를 만들거나 이전에 만든된 컨테이너에 액세스 하려면 호출을 `CreateContainer` 함수에 `ConnectedStorageSpace` 또는 `GameSaveProvider` 클래스를이 액세스할 수 있습니다 명명 된 컨테이너에 사용자 또는 컴퓨터와 연결 된는 `ConnectedStorageSpace` 또는 `GameSaveProvider`컨테이너를 만드는 데 사용 합니다. `ConnectedStorageSpace` 하 고 `GameSaveProvider` 클래스도 포함 합니다.는 `DeleteContainerAsync` 컨테이너를 삭제할 수 있도록 하는 함수는 삭제 될 컨테이너의 이름을 제공 합니다.

컨테이너 호출에서 blob를 업데이트 하려면 `SubmitUpdatesAsync` 에 `ConnectedStorageContainer` 클래스는 XDK 또는 `GameSaveContainer` UWP API의 클래스입니다. `SubmitUpdatesAsync` 호출에 대 한 표시 이름과 삭제할 blob 이름의 목록 컨테이너를 저장할 blob에 쓸 데이터와 이름 및 버퍼의 목록을 제공할 수 있습니다. 이 궁극적으로 연결 된 저장소 공간에 대 한 데이터를 업데이트 하기 위해 호출 해야 하는 함수.

연결 된 저장소 Api 사용에서의 예제를 보려면 다음 저장소 연결 된 문서를 참조 합니다. [데이터 저장](connected-storage-saving.md)
[데이터 로드](connected-storage-loading.md)
[데이터 삭제](connected-storage-deleting.md)

> [!NOTE]
> 보안 참고 사항:
>
> UWP (유니버설 Windows 플랫폼) 앱 Pc에서 실행 되는 로컬 사용자에 액세스할 수 있으며 기본적으로 사용자 조작 으로부터 보호 되지 않는 위치에 로컬 데이터를 저장 합니다.
>사용자 고유의 암호화를 적용 하는 것이 좋습니다 및 사용자가 외부 게임에서 게임 저장을 수정 하는 것을 방지 하기 위해 게임 저장 데이터에 유효성 검사 합니다.
>이와 동일한 이유로 PC 및 Xbox 버전을 저장 공유 하거나 분리 되도록 게임의 허용 하려는 경우를 결정 해야 합니다.

## <a name="managing-local-storage"></a>로컬 저장소를 관리합니다.

Xbox 개발 키트 또는 UWP 앱에서 저장소 연결을 테스트할 때는 개발 장치에 로컬로 저장 된 데이터를 조작 하는 것이 해야 합니다.

도구 xbstorage는 XDK와 함께 제공 하 고 개발 콘솔에서 로컬 저장소를 조작할 수 있도록 명령줄 도구입니다.
UWP 개발자를 위한는 gamesaveutil.exe Fall Creators Update(10.0.16299.91) 이상용 Windows 10 SDK와 함께 제공 되는 호출 하는 PC에 대 한 동일한 도구.

이러한 도구를 사용 하면 이러한 명령 사용 하 여 장치의 로컬 저장소를 조작할 수 있습니다:

|명령  |설명  |
|---------|---------|
|다시 설정    | 연결 된 저장소에서 공장 재설정을 수행 합니다. |
|가져오기   | 연결 된 저장소 공간에 지정된 된 XML 파일에서 데이터를 가져옵니다. |
|export   | 지정된 된 XML 파일에 연결 된 저장소 공간에서 데이터를 내보냅니다. |
|삭제   | 연결 된 저장소 공간에서 데이터를 삭제합니다. |
|생성 | 더미 데이터를 생성 하 고 지정된 된 XML 파일에 저장 합니다. |
|시뮬레이션 | 저장소 공간 부족 조건 시뮬레이션합니다. |

읽을 gamesaveutils.exe xbstorage 도구를 사용할 수 있는 함수에 자세히 알아보려면 [연결 된 로컬 저장소 관리](connected-storage-xb-storage.md)합니다.

## <a name="technical-overview"></a>기술 개요

깊이 연결 된 저장소 읽기 XDK에서 작동 하는 방법을 살펴보고 더 자세히 살펴봅니다 하는 [연결 된 저장소 기술 개요](connected-storage-technical-overview.md)합니다. 기술 개요는 특별히 XDK 개발자를 위해 작성 되었습니다 하지만 일반적 내에 포함 된 개념은 연결 된 저장소에 적용 합니다.