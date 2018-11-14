---
title: 연결 된 저장소 개요
author: aablackm
description: 연결 된 저장소를 사용 하 여 저장 하 고 장치 간 게임 데이터를 로드 하는 방법을 알아봅니다.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장소, xbox
ms.localizationpriority: medium
ms.openlocfilehash: e397f2f07bc62082cd542387fc1603e17be38694
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6267761"
---
# <a name="connected-storage"></a>연결된 저장소
연결 된 저장소는 타이틀 게임 플레이 데이터와 디바이스 간에 로밍 해야 하는 다른 관련 상태 데이터를 저장할 수 있도록 설계 되었습니다. 연결 된 저장소 API 타이틀을에 Xbox One 및 유니버설 Windows Platform(UWP) 로컬로 저장 되 고 인터넷에 연결 된 Xbox One 또는 UWP 제목 때마다 또한 클라우드로 동기화 제목 데이터를 저장, 로드 및 삭제할 수 있습니다. 저장 된 데이터 동기화 된 후 타이틀을 실행 되는 모든 장치에서 사용할 수 있습니다. 개발자는 홈에서 최상의 재생 환경을 제공할 수 있도록 최대한 정확 하 게 제목 상태를 저장 하는 것이 좋습니다. 연결 된 저장소가 수 있도록 집에서 게임에서 진행 되는 다음의 나머지 부분은 같은 게임을 지 원하는 다른 장치에서 게임 권한은 선택할 수 있습니다.

## <a name="connected-storage-features"></a>연결 된 저장소 기능

연결 된 저장소 API는 다음과 같은 기능을 제공합니다.

- 앱은 HDD에 시스템에서 로컬로 캐시 이며을 클라우드에 업로드 하는 메모리 버퍼로 16MB까지 데이터를 한 번에 저장할 신속 하 게 수 있습니다.
- 관리 파트너 및 ID@Xbox 개발자:
    - 클라우드 저장소의 사용자/앱 별 256mb의 합니다.
- Xbox Live 크리에이터 스 프로그램 개발자:
    - 클라우드 저장소의 사용자/앱 별 64 필요 합니다.
- 전원 오류에 대 한 강력한 응답-앱 부분 데이터를 저장 하 고 처리할 필요가 없습니다.
- 데이터는 앱이 실행 되지 않을 경우에 자동으로 클라우드로 업로드 됩니다.
- Xbox Live에 연결 된 Xbox One 또는 UWP 장치에서 데이터를 사용할 수 있습니다.
- Xbox Live 앱에서 참여 하지 않고도 장치 간 동기화 및 충돌 관리를 처리 합니다.

연결 된 저장소 시스템이 모든 저장 온전히 또는 전혀 사항이 있는지 확인 합니다. 즉, 개발자가 되지 있다고 정전이 나 앱을 갑자기 closing 사용자의 경우 게임 상태에 영향을 미치는 부분적으로 저장 된 데이터에 걱정할 수동으로 또는 다른 응용 프로그램/게임 콘솔에서 사용 하 여 합니다. 연결 된 저장소는 사용자가 게임 간을 전환할 수 있도록 하는 중요 한 부분으로 원활 하 게 게임 제목의 사용자 경험을 재생 하 고 앱 신속 하 고 자유롭게 하지만 여전히으로 돌아올 남아 있는 상태에서 원래 게임에도 보장 합니다. 고유한 제목에 이러한 기능을 구현 하기 위해 연결 된 저장소 Api에 대 한 이해 해야 합니다.

## <a name="connected-storage-structure"></a>연결 된 저장소 구조

연결 된 저장소 시스템을 컨테이너에서 하나 이상의 blob으로 데이터를 저장할 수 있습니다. 상위 수준에서 연결 된 저장소 시스템의 모든 데이터는 사용자와 연결 된 또는 사용자 또는 컴퓨터 Xbox 개발 키트의 경우 타이틀을 개발 합니다. 특정 사용자에 대 한 앱에서 모든 데이터를 저장 하거나 컴퓨터 연결 된 저장소 공간에 저장 됩니다. 앱의 각 사용자는 최대 256 또는 64 MB 총 저장소를 사용 하 여 연결 된 저장소 공간을 가져옵니다. 것이 중요 한 앱만이 저장소 전용인, 다른 앱과 공유 되지 않습니다.

### <a name="containers-and-blobs"></a>컨테이너와 blob

연결 된 저장소 컨테이너 또는 줄여서 컨테이너 저장소의 기본 단위입니다. 다음 다이어그램에 표시 된 대로 각 연결 된 저장소 공간에서 여러 컨테이너를 포함할 수 있습니다.

연결 된 저장소 공간 (제목/컴퓨터 또는 제목/사용자)

![connected_storage_space_containers.png](../../images/connected_storage/connected_storage_space_containers.png)

 데이터는 blob을 호출 하는 하나 이상의 버퍼로 컨테이너에 저장 됩니다. 다음 다이어그램은 디스크에는 컨테이너 내부 시스템 표현을 보여 줍니다. 각 컨테이너에 대 한 컨테이너에서 각 blob에 대 한 데이터 파일에 대 한 참조를 포함 하는 컨테이너 파일이 있습니다.

컨테이너의 다이어그램

![container_storage_blobs.png](../../images/connected_storage/container_storage_blobs.png)

컨테이너에서 데이터를 저장 하기 위해 적절 한 Api 컨테이너 메서드 이름 및 blob (버퍼 개체)의 지도 제공 SubmitUpdatesAsync를 호출 합니다. SubmitUpdatesAsync 호출에 설명 된 모든 변경 내용을 자동으로 적용 됩니다, 즉, 중 하나, 요청에 따라 모든 blob 업데이트 또는 전체 작업이 종료 되 고 컨테이너 호출 하기 전에 상태로 유지 됩니다.

개별 저장 SubmitUpdatesAsync를 사용 하는 작업은 한 번에 16mb 데이터 제한 됩니다.

## <a name="connected-storage-api"></a>연결 된 저장소 API

연결 된 저장소 XDK 및 UWP 앱에 대 한 별도 Api에 있습니다. 다행히 이러한 Api와 유사 서로 매우 밀접 하 게 합니다. 두 Api 네임 스페이스 및 클래스 이름에 주로 다릅니다. API 사용 하 여 데이터를 [저장](connected-storage-saving.md), [로드](connected-storage-loading.md)및 [삭제](connected-storage-deleting.md) 하는 데 필요한 함수는 동일 하 게 이름이 지정 됩니다.

추가 차이점 두 연결 된 저장소 Api [XDK에서 UWP로 포팅 Xbox Live 코드](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)의 연결 된 저장소 섹션에 자세히 설명 되어 있습니다.

XDK 연결 된 저장소 Api XDK.chm 파일 경로 아래에 설명 된 찾을 수 있습니다: **Xbox ONE XDK >> API 참조 >> 플랫폼 API 참조 >> 시스템 API 참조 >> Windows.Xbox.Storage**합니다.
XDK Api도 [developer.microsoft.com 사이트](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)에 문서화 되어 있습니다.
XDK Api에 대 한 링크는 Microsoft Account(MSA) Xbox 개발자 Kit(XDK) 액세스 가능 하도록 설정 되어 있어야 합니다.
Windows.Xbox.Storage에는 Xbox One 본체에 대 한 연결 된 저장소 네임 스페이스의 이름입니다.

UWP 연결 된 저장소 Api Xbox Live SDK.chm 파일 경로 아래에 설명 된 찾을 수 있습니다: **Xbox Live Api >> Xbox Live 플랫폼 확장 SDK API 참조 >> Windows.Gaming.XboxLive.Storage**합니다.
UWP 연결 된 저장소 Api도 [Windows.Gaming.XboxLive.Storage 네임 스페이스 참조](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)에서 온라인으로 설명 되어 있습니다.
Windows.Gaming.XboxLive.Storage에는 UWP 앱에 대 한 연결 된 저장소 네임 스페이스의 이름입니다.

연결 된 저장소를 사용 하려면 연결 된 저장소 *공간*을 확보 해야 합니다. 연결 된 저장소 공간 사용자 또는 컴퓨터와 연결 하 고 해당 사용자 또는 *컨테이너* 및 *blob*의 형태로 컴퓨터와 관련 된 연결 된 저장소 데이터를 모두 보유 합니다. 컴퓨터 또는 사용자에 대 한 연결 된 저장소 공간을 확보를 읽고 쓰는 엔터티 저장 된 데이터에 액세스할 수 있게 됩니다. XDK와 UWP 타이틀이 호출을 연결 된 저장소 공간을 구입는 `GetForUserAsync` 메서드를 XDK 제목 호출할 수도 있습니다는 `GetForMachineAsync` 메서드를 UWP 타이틀이 됩니다 호출할 수 `GetForMachineAsync`합니다. `GetForUserAsync` 및 `GetForMachineAsync` 에 포함 된 합니다 `ConnectedStorageSpace` 클래스는 XDK에서. `GetForUserAsync` 에 포함 된는 `GameSaveProvider` UWP API에 대 한 클래스입니다. 이러한 방법은 사용자가 한 디바이스에서 데이터를 저장 하 고 다른 장치에 처음으로 게임 플레이 다시 시작 하는 경우에 특히 잠재적으로 장기 실행 작업을 있습니다. `GetForUserAsync` 그런 다음 만들기, 액세스 및 컨테이너 삭제를 사용할 수 있는 사용자에 대 한 연결 된 저장소 공간을 검색 합니다.

컨테이너를 만들거나 이전에 만든된 컨테이너에 액세스 하려면 다음을 호출 합니다 `CreateContainer` 의 기능에 `ConnectedStorageSpace` 또는 `GameSaveProvider` 클래스를 사용자에 대 한 명명 된 컨테이너에 대 한 액세스를 제공는이 또는 컴퓨터와 관련 된는 `ConnectedStorageSpace` 또는 `GameSaveProvider` 를 만드는 데 사용 합니다 컨테이너입니다. `ConnectedStorageSpace` 및 `GameSaveProvider` 클래스도 포함 합니다 `DeleteContainerAsync` 컨테이너를 삭제할 수 있는 함수는 삭제 되 고 컨테이너의 이름을 제공 합니다.

컨테이너 호출에서 blob을 업데이트 하려면 `SubmitUpdatesAsync` 에 `ConnectedStorageContainer` 는 XDK 또는 클래스는 `GameSaveContainer` UWP API의 클래스. `SubmitUpdatesAsync` 삭제 될 blob의 이름 목록 및 호출에 대 한 표시 이름 컨테이너를 저장 데이터 blob으로 쓸 이름과 버퍼의 목록을 제공할 수 있습니다. 연결 된 저장소 공간에서 데이터를 업데이트 하려면 궁극적으로 해야 하는 함수입니다.

연결 된 저장소 Api 사용에서의 예제를 보려면 다음 연결 된 저장소 문서를 참고: [데이터 저장](connected-storage-saving.md)
[데이터 로드](connected-storage-loading.md)
[데이터 삭제](connected-storage-deleting.md)

> [!NOTE]
> 보안에 대 한 참고 사항:
>
> Pc에서 실행 되는 유니버설 Windows 플랫폼 (UWP) 앱 기본적으로 사용자를 통해 변조 로부터 보호 되지 않는 및 로컬 사용자가 액세스할 수 있는 위치에 로컬 데이터를 저장 합니다.
>고유한 암호화를 적용 하는 것이 좋습니다 하 고 사용자가 게임 외부 게임 저장을 수정 하는 것을 방지 하기 위해 게임 저장 데이터 유효성 검사 합니다.
>이 같은 이유로 PC 및 Xbox 버전을 저장을 공유 하거나 분리 되도록 게임의 허용 하려는 경우 결정 해야 합니다.

## <a name="managing-local-storage"></a>로컬 저장소 관리

Xbox 개발 키트 또는 UWP 앱에 연결 된 저장소를 테스트할 때는 개발자 장치에 로컬로 저장 된 데이터를 조작 해야 할 수 있습니다.

도구 xbstorage는 XDK와 함께 제공 하 고 개발 콘솔에서 로컬 저장소를 조작할 수 있도록 명령줄 도구입니다.
UWP 개발자를 위한은 동일한 도구 라는 gamesaveutil.exe Fall Creators Update(10.0.16299.91) 이상과 Windows 10 SDK와 함께 제공 되는 PC입니다.

이러한 도구를 사용 하면 이러한 명령 사용 하 여 장치에 로컬 저장소를 조작할 수 있습니다:

|명령  |설명  |
|---------|---------|
|다시 설정    | 연결 된 저장소 초기화 팩터리를 수행 합니다. |
|가져오기   | 연결 된 저장소 공간에 지정된 된 XML 파일에서 데이터를 가져옵니다. |
|내보내기   | 지정된 된 XML 파일에 연결 된 저장소 공간에서 데이터를 내보냅니다. |
|삭제   | 연결 된 저장소 공간에서 데이터를 삭제합니다. |
|생성 | 더미 데이터를 생성 하 고 지정된 된 XML 파일에 저장 합니다. |
|시뮬레이션 | 저장 공간 부족 시뮬레이션합니다. |

xbstorage에서 사용할 수 있는 기능에 대 한 자세한 내용을 보려면 도구 및 gamesaveutils.exe 읽기 [연결 된 로컬 저장소를 관리](connected-storage-xb-storage.md)합니다.

## <a name="technical-overview"></a>기술 개요

연결 된 저장소의 XDK에서 작동 하는 방법을 살펴보고 깊이 자세히 되려면 [연결 된 저장소 기술 개요](connected-storage-technical-overview.md)를 읽습니다. 기술 개요 XDK 개발자 용으로 특별히 작성 된 하지만 일반적 내에 포함 된 개념은 연결 된 저장소에 적용 합니다.