---
title: 로컬 연결 된 저장소 관리
description: 개발 환경에서 로컬 저장소 연결 된 데이터를 관리 하는 방법에 알아봅니다.
ms.assetid: 630cb5fc-5d48-4026-8d6c-3aa617d75b2e
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장소, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 09fce637c50b0a03230d0808e51a9e5cfadc7179
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642058"
---
# <a name="managing-local-connected-storage"></a>로컬 연결 된 저장소 관리
연결 된 저장소는 클라우드에서 게임 데이터를 저장 하는 데 사용 됩니다, 이기도 저장소 연결 서비스에는 로컬 저장소 구성 요소입니다. PC 또는 콘솔에 인지 클라우드로 동기화 된 데이터가 포함 된 저장소 연결 된 데이터의 로컬 캐시 합니다. XDK 또는 UWP 제목 만들면 인지 로컬 저장소 연결 된 데이터를 관리할 수 있도록 하는 도구입니다.

로컬에 캐시 된 저장소 연결 된 데이터를 관리 하려면 적절 한 도구를 찾으려면 다음 표를 참조 하세요.

|제목 분류  |장치  |로컬 저장소 도구  |
|---------|---------|---------|
|XDK     |Xbox One 콘솔     |*xbstorage*         |
|UWP     |PC         |*gamesaveutil*         |
|UWP     |Xbox One 콘솔     |Xbox 장치 포털 (XDP |)

- *Xbstorage* 는 Xbox 1 본체에 연결 된 저장소를 로컬로 캐시 관리에 대 한 XDK 명령 프롬프트에서 실행 하는 명령줄 도구입니다. 합니다 *xbstorage* 도구는 파일 경로에서 Xbox One XDK 찾을 수 있습니다:   **/program Files/Microsoft (x86) Durango XDK/bin/xbstorage.exe**
- *Gamesaveutil* UWP를 로컬로 관리 PC에 연결 된 저장소를 캐시 하는 명령줄 도구입니다. 합니다 *gamesaveutil* 도구와 함께 제공 되는 [Windows 10 SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) Fall Creators Update에 대 한 이상 (10.0.16299.15 빌드 이상). Windows 10 SDK의 적절 한 버전을 설치한 후 *gamesaveutil* 폴더에서 찾을 수 있습니다. **ProgramFiles(x86)/Windows Kits/10/bin/[SDK Version]/x64/gamesaveutil.exe**.
- *Xbox 장치 Portal(XDP)* 는 Xbox 1 본체 로컬로 캐시 된 저장소 UWP 연결 된 데이터를 관리할 수 있는 온라인 포털입니다. 이 문서에서는 XDP 사용을 다루지 않습니다. XDP 사용에 대해 알아보려면 [Xbox 용 장치 포털](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-xbox)합니다.

## <a name="xbstorage"></a>Xbstorage

*Xbstorage* 로컬로 캐시 된 데이터를 하드 드라이브에서 저장소 연결 된 데이터를 지울으로 내보내고 가져올 사용자 또는 연결 된 저장소 공간에서 컴퓨터에 대 한 데이터의 XML 파일을 사용 하 여 허용 합니다.

사용 하 여 로컬 데이터에서 작업이 수행 되는 경우는 *xbstorage* 도구인 시스템 처럼 작동 합니다를 로컬 파일에 연결 된 저장소 공간에서 데이터를 읽는 작업 하면 되므로 앱 자체에서 해당 작업이 수행 된 했습니다 복사 하기 전에 클라우드로 동기화 합니다.

마찬가지로, Xbox One dev 키트에서 연결 된 저장소 컨테이너에 개발 PC에 대 한 XML 파일에서 데이터의 복사본을 사용 하면 클라우드로 해당 데이터를 업로드 하려면 콘솔. 그러나 조건이는 발생 하지 것입니다: dev 키트 잠금을 가져올 수 없습니다 아니면 콘솔에서 충돌을 해결할 필요가 결정 했 고 사용자 처럼 동작 콘솔에서 컨테이너와 클라우드 간에 데이터 충돌이 발생 하는 경우 유지, 및 콘솔에 컨테이너의 버전을 선택 합니다. 제목 시작 될 때까지 사용자 오프 라인 재생 처럼 작동 합니다.

이러한 명령에는 한 가지 예외는 **재설정** **/force** 모든 SCIDs와 사용자가 저장 된 데이터의 로컬 저장소를 지웁니다 있지만 클라우드에 저장 된 데이터를 변경 하지 않습니다. 콘솔을는 것에 사용자를 콘솔로 로밍 되어 제목을 재생 시 클라우드에서 데이터를 다운로드 하는 경우 상태에 배치 하는 데 유용 합니다.

### <a name="xbstorage-commands"></a>Xbstorage 명령

Xbstorage는 다음과 같은 6 개의 명령을 개발자가 Xbox 하나의 개발 키트의 로컬 데이터를 관리 하려면 XDK 명령 프롬프트를 사용 하 여 사용할 수 있습니다.

<a id="xbstorage_reset"></a>

|명령  |설명  |
|---------|---------|
|다시 설정    |연결 된 저장소에서 공장 재설정을 수행 합니다.         |
|가져오기   |연결 된 저장소 공간에 지정된 된 XML 파일에서 데이터를 가져옵니다.         |
|export   |지정된 된 XML 파일에 연결 된 저장소 공간에서 데이터를 내보냅니다.         |
|삭제   |연결 된 저장소 공간에서 데이터를 삭제합니다.         |
|생성 |더미 데이터를 생성 하 고 지정된 된 XML 파일에 저장 합니다.         |
|시뮬레이션 |저장소 공간 부족 조건 시뮬레이션합니다.         |

### <a name="xbstorage-reset"></a>Xbstorage 재설정

`xbstorage reset [/force]`

출하 시 설정으로 복원 하는 로컬 콘솔에서 연결 된 저장소에 모든 로컬 데이터를 지웁니다. 클라우드로 유지 된 데이터 수정 되지 않으며 다시 다운로드 됩니다 필요에 따라 합니다.

|옵션  |설명  |
|---------|---------|
|/force   |연결 된 저장소를 다시 설정 해야 있는지 확인 합니다. 지정 하지 않고 다시 설정 명령을 실행 **/force** 하면 다음 메시지가 표시 됩니다.   저장소 연결 된 팩터리로 재설정은이 명령으로 수행할 다시 설정 하지 않으면 잠재적으로 안전 하지 않은 작업을 합니다 **/force** 플래그가 있습니다. |

<a id="xbstorage_import"></a>

### <a name="xbstorage-import"></a>Xbstorage 가져오기

`xbstorage import *file-name* [/scid:*SCID*] [/machine] [/msa:*account*] [/replace] [/verbose]`

에 지정 된 데이터를 가져옵니다 *filename* 연결 된 저장소 공간에 있습니다.
파일이 데이터를 포함 하는 XML 파일입니다. 예를 들어 참조 [xbstorage 생성](#xbstorage_generate)합니다. 파일의 XML 형식에 대 한 자세한 내용은 참조 하세요. [가져오기 및 내보내기 파일 형식](#xbstorage_fileformat)이 항목의 뒷부분에 나오는.
두 가지 방법으로 연결 된 저장소 공간을 지정할 수 있습니다.

- 입력된 파일에는 **ContextDescription** 섹션 올바르게 채워진 연결 된 저장소 공간을 지정 하려면 사용 됩니다.
- 저장소 공간 지정할 수도 있습니다 완전히 또는 부분적으로 입력된 파일에 지정 된 저장소 공간의 해당 요소 보다 우선적으로 적용 하는 명령줄 매개 변수를 통해.

사용 예제:

```cmd
  xbstorage import mydata.xml
  xbstorage import mydata.xml /replace
  xbstorage import mydata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /verbose 
```

> [!NOTE]
> 지정된 된 연결 된 저장소 공간에 가져오는 하기 전에 시스템을 실행 중인 앱에서 연결 된 저장소 공간을 획득할 때 실행 되는 동일한 논리를 사용 하 여 클라우드로 동기화 하려고 합니다.
>
> 동일한 기본 서비스 안내를 사용 하 여 응용 프로그램을 실행 중인 경우이 작업을 경합을 초래할 및 연결 된 저장소 공간의 내용을 확정 되지 않은 상태에 있을 수 있습니다.
>
> 하는 경우 **대체/** 지정 하지 않으면 입력된 파일에 지정 된 컨테이너는 입력된 파일에 지정 된 blob를 작성 하기 전에 초기화 됩니다. 입력된 파일에 지정 되지 않은 연결 된 저장소 공간에는 컨테이너는 그대로 유지 됩니다.

|옵션  |설명  |
|---------|---------|
|*file-name*     |가져올 데이터를 포함 하는 XML 파일을 지정 합니다.         |
|/scid:*SCID*    |서비스 구성 식별자 (서비스 안내)을 지정합니다.         |
|/machine        |컴퓨터별 연결 된 저장소 공간을 지정합니다.  사용 하 여이 옵션을 동시에 사용할 수 없습니다는 **/msa** 옵션입니다.         |
|/msa:*account*  |사용자별 연결 된 저장소에 사용할 계정을 지정 합니다. 사용자 사용할 공간에 대 한 콘솔에 로그인 해야 합니다.  사용 하 여이 옵션을 동시에 사용할 수 없습니다는 **/컴퓨터** 옵션입니다.         |
|/replace        |가져오기 전에 지정된 된 연결 된 저장소 공간에서 모든 컨테이너를 삭제합니다.         |
|/verbose        |가져오기 작업의 상태를 표시합니다.         |


 <a id="xbstorage_export"></a>

### <a name="xbstorage-export"></a>Xbstorage 내보내기

`xbstorage export *outfile* [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

지정 된 파일에 연결 된 저장소 공간에서 데이터를 내보냅니다 **outfile**합니다.    파일이 데이터를 포함 하는 XML 파일입니다. 참조 [xbstorage 생성](#xbstorage_generate) 예제를 생성 하는 방법을 확인 합니다. 파일의 XML 형식에 대 한 자세한 내용은 참조 하세요. [가져오기 및 내보내기 파일 형식](#xbstorage_fileformat)이 항목의 뒷부분에 나오는. 두 가지 방법으로 연결 된 저장소 공간을 지정할 수 있습니다.

- 경우는 **/context** 매개 변수를 사용 하 고 파일 이름을 지정 하 여 \<infile >에 **ContextDescription** 섹션 올바르게 채워진 해당 파일에서 지정 하는 데 사용 됩니다는 연결 된 저장소 공간입니다.
- 저장소 공간 지정할 수도 있습니다 완전히 또는 부분적으로 해당 요소에 지정 된 저장소 공간 보다 우선적으로 적용 하는 명령줄 매개 변수를 통해 합니다 **/context** 파일입니다.

사용 예제:

```cmd
  xbstorage export exporteddata.xml /context:space.xml
  xbstorage export exporteddata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /context:space.xml /verbose
```

> [!NOTE]
> 지정된 된 연결 된 저장소 공간을 내보내기 전에 시스템을 실행 중인 앱에서 연결 된 저장소 공간을 획득할 때 실행 되는 동일한 논리를 사용 하 여 클라우드로 동기화 하려고 합니다.
>
> 동일한 기본 서비스 안내를 사용 하 여 응용 프로그램을 실행 중인 경우이 작업을 경합을 초래할 및 연결 된 저장소 공간의 내용을 확정 되지 않은 상태에 있을 수 있습니다.

|옵션  |설명  |
|---------|---------|
|*outfile*             |XML 파일 데이터를 내보냅니다.         |
|/context:*input-file* |공간 정보를 읽을 수 있는 입력된 파일을 지정 합니다.         |
|/scid:*SCID*          |서비스 구성 식별자 (서비스 안내)을 지정합니다.         |
|/machine              |컴퓨터별 연결 된 저장소 공간을 지정합니다.  사용 하 여이 옵션을 동시에 사용할 수 없습니다는 **/msa** 옵션입니다.         |
|/msa:*account*        |사용자별 연결 된 저장소에 사용할 계정을 지정 합니다. 사용자 사용할 공간에 대 한 콘솔에 로그인 해야 합니다.  사용 하 여이 옵션을 동시에 사용할 수 없습니다는 **/컴퓨터** 옵션입니다.         |
|/verbose              |내보내기 작업의 상태를 표시합니다.         |

<a id="xbstorage_delete"></a>

### <a name="xbstorage-delete"></a>Xbstorage 삭제

`xbstorage delete [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

연결 된 저장소 공간에서 모든 데이터를 삭제합니다.
두 가지 방법으로 연결 된 저장소 공간을 지정할 수 있습니다.

- 경우는 **/context** 매개 변수를 사용 하 고 파일 이름을 지정 하 여 \<infile >에 **ContextDescription** 섹션 올바르게 채워진 해당 파일에서 지정 하는 데 사용 됩니다는 연결 된 저장소 공간입니다.
- 저장소 공간 지정할 수도 있습니다 완전히 또는 부분적으로 해당 요소에 지정 된 저장소 공간 보다 우선적으로 적용 하는 명령줄 매개 변수를 통해 합니다 **/context** 파일입니다.

사용 예제:

```cmd
  xbstorage delete /context:space.xml
  xbstorage delete /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /context:space.xml /verbose
```

> [!NOTE]
> 지정된 된 연결 된 저장소 공간을 삭제 하기 전에 시스템을 실행 중인 앱에서 연결 된 저장소 공간을 획득할 때 실행 되는 동일한 논리를 사용 하 여 클라우드로 동기화 하려고 합니다.
>> 동일한 기본 서비스 안내를 사용 하 여 응용 프로그램을 실행 중인 경우이 작업을 경합을 초래할 및 연결 된 저장소 공간의 내용을 확정 되지 않은 상태에 있을 수 있습니다.

|옵션  |설명 |
|---------|---------|
|/context:*input-file*     |공간 정보를 읽을 수 있는 입력된 파일을 지정 합니다.         |
|/scid:*SCID*              |서비스 구성 식별자 (서비스 안내)을 지정합니다.         |
|/machine                  |컴퓨터별 연결 된 저장소 공간을 지정합니다.  사용 하 여이 옵션을 동시에 사용할 수 없습니다는 **/msa** 옵션입니다.         |
|/msa:*account*            |사용자별 연결 된 저장소에 사용할 계정을 지정 합니다. 사용자 사용할 공간에 대 한 콘솔에 로그인 해야 합니다.  사용 하 여이 옵션을 동시에 사용할 수 없습니다는 **/컴퓨터** 옵션입니다.         |
|/verbose                  |삭제 작업의 상태를 표시합니다.         |

 <a id="xbstorage_generate"></a>

### <a name="xbstorage-generate"></a>Xbstorage 생성

`xbstorage generate *file-name* [/containers:*n*] [/blobs:*n*] [/blobsize:*n*]`

더미 데이터를 생성 하 고 지정 된 파일에 저장 \<파일 이름 >. 파일의 XML 형식에 대 한 자세한 내용은 참조 하세요. [가져오기 및 내보내기 파일 형식](#xbstorage_fileformat)이 항목의 뒷부분에 나오는.    서비스 구성 식별자 (서비스 안내) 00000000-0000-0000-0000-000000000000로 설정 됩니다 및 계정 컴퓨터별 연결 된 저장소 공간에 대 한 설정 됩니다. 이러한 값을 변경 하려는 경우 직접 편집할 수 있습니다 파일 생성 됩니다.

사용 예제:

```cmd
  xbstorage generate dummydata.xml
  xbstorage generate dummydata.xml /containers:4
  xbstorage generate dummydata.xml /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```

> [!NOTE]
> Byte 데이터는 간단한 오름차순 시퀀스; 예를 들어 5 바이트 blob을 사용 해야 다음 바이트: 00 01 02 03 04. >> 사용자별 연결 된 저장소 공간을 지정 하려는 경우 변경 합니다 **계정** 다음과 같이 XML 파일에는 노드:
>  ```
>    <Account msa="user@domain.com"/>
>  ```

|옵션  |설명  |
|---------|---------|
|*file-name*     | XML 파일 데이터를 쓸 수 됩니다. |
|/containers:*n* | 번호를 지정 *n*를 생성 하는 컨테이너입니다. 기본 수는 2입니다.  |
|/blobs:*n*      | 번호를 지정 *n*를 생성 하는 blob입니다. 기본 수는 3입니다.  |
|/blobsize:*n*   | 번호를 지정 *n*, blob 당 바이트. 기본 크기는 1024 바이트입니다.  |

 <a id="xbstorage_simulate"></a>

### <a name="xbstorage-simulate"></a>Xbstorage 시뮬레이션

`xbstorage simulate [/reserveremainingspace] [/forceoutoflocalstorage] [/stop] [/verbose]`

저장소 연결 서비스에 대 한 로컬 저장소 조건을 시뮬레이션합니다.

|옵션  |설명  |
|---------|---------|
|/reserveremainingspace | 연결 된 저장소의 모든 남은 공간을 예약합니다. ConnectedStorage에서 삭제 하는 데 사용할 수 있는 공간을 열립니다. |

| / forceoutoflocalstorage | 연결 된 저장소 서비스를 왼쪽에 사용할 수 있는 공간이 없습니다 것을 시뮬레이션 합니다. 연결 된 저장소에서 삭제 하는 연결 된 저장소 서비스에서 메모리가 부족 합니다. 보고 변경 되지 않습니다. |

| / 중지 | 모든 시뮬레이션을 중지합니다. |

| 자세한 정보 표시 / | 시뮬레이션 된 작업의 상태를 표시합니다. |

 <a id="ID4E4MAC"></a>

### <a name="common-options"></a>일반 옵션

`xbstorage [/?] [/X*:address* [*+accesskey*] ]`

|옵션  |설명  |
|---------|---------|
| /?                           |  Xbstorage.exe에 대 한 도움말을 표시 합니다 |
| /X *:address* [*+accesskey*]  | 호스트 이름 또는 주소 지정 (로 표시 **도구 IP** 콘솔에) 대상으로 지정 된 된 콘솔의 하지만 기본 콘솔을 변경 하지 않습니다. 이 옵션을 사용 하지 않는 경우 콘솔의 기본 사용 됩니다. *Accesskey* 는 액세스 키를 아는 사용자만 콘솔에 대 한 액세스를 제한 하는 데 사용할 수 있는 문자열입니다. 명령을 사용 하 여 액세스 키를 설정 **xbconfig** **accesskey = * * * 키 your*; 한 콘솔 액세스 키를 적용 하기 위해 다시 시작 합니다. 액세스 키를 사용 하 여 구성 된 콘솔에 액세스 하려면 콘솔의 IP 주소 또는 호스트 이름 뒤에 더하기 (+) 및 액세스 키를 포함 해야 합니다.
> [!NOTE]
> 기본 콘솔 xbconnect에 의해 설정 된 경우는 액세스 키가 제공 하는 경우에 액세스 키를 기본 콘솔 주소의 일부로 저장 됩니다.
|

## <a name="gamesaveutil"></a>Gamesaveutil

*Gamesaveutil* 앱의 모든 함수에 대 한 로컬에 캐시 된 연결 된 저장소를 관리할 수 있습니다 *xbstorage* 제공 합니다. 마찬가지로 다음 xbstorage 도구인 *gamesaveutil* 관리 동작에 몇 가지 차이점이 있지만 함수 같은 6 개의 데이터를 제공 합니다. 가져오기, 내보내기, 삭제 및 다시 설정 명령 Xbox Live 사용자 로그인에 필요 합니다. 보기 및 현재 사용자를 변경 하려면 Windows 10의 Xbox 앱을 사용할 수 있습니다.

### <a name="commands"></a>명령

|명령  |설명  |
|---------|---------|
|가져오기   |지정된 된 XML 파일에서 데이터를 가져옵니다.         |
|export   |지정 된 xml 파일에 데이터를 내보냅니다.         |
|삭제   |지정된 된 응용 프로그램에서 데이터를 삭제합니다.        |
|다시 설정    |지정된 된 응용 프로그램에 대해서만 로컬 데이터를 삭제합니다.         |
|생성 |더미 데이터를 생성 하 고 지정 된 xml 파일에 저장 합니다.         |
|시뮬레이션 |특수 한 조건을 테스트 하기 어려운 시뮬레이션         |

### <a name="gamesaveutil-import"></a>Gamesaveutil 가져오기

`gamesaveutil import <filename> [/pfn:<PFN>] [/scid:<SCID>] [/replace]`

에 지정 된 데이터를 가져오는 \<파일 이름 >

파일이 데이터를 포함 하는 XML 파일입니다. 형식 `gamesaveutil help generate` 예제를 생성 하는 방법을 확인 합니다.

앱을 지정 하는 방법은 두 가지 PFN 및 데이터를 저장 하는 서비스 안내 합니다.

입력된 파일은 채워진 올바르게 ContextDescription 섹션의 경우이 대상 앱을 지정 하 고 PFN 서비스 안내 합니다.

PFN 및 서비스 안내를 지정할 수 있습니다 또는 부분적으로 입력된 파일에서 지정 된 PFN와 서비스 안내의 해당 요소 보다 우선적으로 적용 하는 명령줄 매개 변수를 통해.

|옵션  |설명  |
|---------|---------|
|/pfn:\<PFN>       |에 대 한 가져오기를 수행 하는 앱에 대 한 패키지 패밀리 Name(PFN)를 지정 합니다. 앱을 설치 해야 합니다.         |
|/scid:\<SCID>     |서비스 구성 식별자 (서비스 안내)을 지정합니다. Xbox Live 구성에서입니다.         |
|/replace         |가져오기를 수행 하기 전에 모든 컨테이너를 삭제 합니다.         |

사용 예제:

```cmd
gamesaveutil import mydata.xml
gamesaveutil import mydata.xml /replace
gamesaveutil import mydata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 앱 설치 되어 있어야 하 고 가져오기 위해 데이터를 이미 동기화 된.
> 
> /Replace 지정 하지 않으면 입력된 파일에 지정 된 경우가 아니면 기존 컨테이너 수행 되지 않은 됩니다.

### <a name="gamesaveutil-export"></a>Gamesaveutil 내보내기

`gamesaveutil export <outfile> [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

지정 된 파일 데이터를 내보내는 \<outfile >.

파일이 데이터를 포함 하는 XML 파일입니다. Gamesaveutil help 입력 예제를 생성 하는 방법을 보려면 생성 합니다.

두 가지 방법으로 내보내려는 데이터의 위치를 지정할 수 있습니다.

/context 매개 변수를 사용 하 고 파일 이름을 지정 하 여 \<infile > 채워진 올바르게 ContextDescription 섹션에 있는 해당 파일에서 원본 데이터의 위치를 지정 하는 데 사용 됩니다.

/Context 파일에 지정 된 해당 요소 보다 우선적으로 적용 하는 명령줄 매개 변수를 통해 위치를 지정할 수도 있습니다.

|옵션  |설명  |
|---------|---------|
|/context:\<infile>     |지정된 된 파일을 사용 하 여 앱을 지정 하 고 PFN 서비스 안내 합니다.         |
|/pfn:\<PFN>            |내보내기를 수행 하려면 앱의 패키지 패밀리 Name(PFN)를 지정 합니다. 앱을 설치 해야 합니다.       |
|/scid:\<SCID>          |서비스 구성 식별자 (서비스 안내)을 지정합니다. Xbox Live 구성에서입니다.        |

사용 예제:

```cmd
gamesaveutil export exporteddata.xml /context:target.xml
gamesaveutil export exporteddata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 앱 설치 해야 하며 내보내려면 이미 데이터를 동기화 했습니다.

### <a name="gamesaveutil-delete"></a>Gamesaveutil 삭제

`gamesaveutil delete [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

지정 된 PFN 및 서비스 안내에 대 한 모든 데이터를 삭제합니다.

두 가지 방법으로 삭제할 데이터의 위치를 지정할 수 있습니다.

/context 매개 변수를 사용 하 고 파일 이름을 지정 하 여 \<infile > 채워진 올바르게 ContextDescription 섹션에 있는 해당 파일에서 원본 데이터의 위치를 지정 하는 데 사용 됩니다.

/Context 파일에 지정 된 해당 요소 보다 우선적으로 적용 하는 명령줄 매개 변수를 통해 위치를 지정할 수도 있습니다.

|옵션  |설명  |
|---------|---------|
|/context:\<infile>     |지정된 된 파일을 사용 하 여 앱을 지정 하 고 PFN 서비스 안내 합니다.         |
|/pfn:\<PFN>            |데이터를 삭제 하려면 앱의 패키지 패밀리 Name(PFN)를 지정 합니다. 앱을 설치 해야 합니다.       |
|/scid:\<SCID>          |서비스 구성 식별자 (서비스 안내)을 지정합니다. Xbox Live 구성에서입니다.        |

사용 예제:

```cmd
gamesaveutil delete /context:target.xml
gamesaveutil delete /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 앱은 컨테이너를 삭제 하기 위해 설치 해야 합니다.

### <a name="gamesaveutil-reset"></a>Gamesaveutil 재설정

`gamesaveutil reset [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

지정 된 PFN 및 서비스 안내에 대 한 모든 로컬 데이터를 삭제합니다.

두 가지 방법으로 삭제할 데이터의 위치를 지정할 수 있습니다.

/context 매개 변수를 사용 하 고 파일 이름을 지정 하 여 \<infile > 채워진 올바르게 ContextDescription 섹션에 있는 해당 파일에서 원본 데이터의 위치를 지정 하는 데 사용 됩니다.

/Context 파일에 지정 된 해당 요소 보다 우선적으로 적용 하는 명령줄 매개 변수를 통해 위치를 지정할 수도 있습니다.

|옵션  |설명  |
|---------|---------|
|/context:\<infile>     |지정된 된 파일을 사용 하 여 앱을 지정 하 고 PFN 서비스 안내 합니다.         |
|/pfn:\<PFN>            |데이터를 삭제 하려면 앱의 패키지 패밀리 Name(PFN)를 지정 합니다. 앱을 설치 해야 합니다.       |
|/scid:\<SCID>          |서비스 구성 식별자 (서비스 안내)을 지정합니다. Xbox Live 구성에서입니다.        |

사용 예제:

```cmd
gamesaveutil reset /context:target.xml
gamesaveutil reset /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 로컬 데이터를 삭제 하기 위해 앱을 설치 해야 합니다.

### <a name="gamesaveutil-generate"></a>Gamesaveutil 생성

`gamesaveutil generate <filename> [/containers:<n>] [/blobs:<n>] [/blobsize:<n>]`

더미 데이터를 생성 하 고 지정 된 파일에 저장 \<파일 이름 >.
서비스 구성 식별자 (서비스 안내) 00000000-0000-0000-0000-000000000000로 설정 됩니다. 원하는 경우 해당 값을 변경 하는 생성 후 파일을 수동으로 편집 합니다.

|옵션  |설명  |
|---------|---------|
|/containers:\<n>     |생성 하는 방법을 많은 컨테이너를 지정 합니다. 기본값은 2입니다.         |
|/blobs:\<n>          |생성 하는 컨테이너 당 얼마나 많은 blob을 지정 합니다. 기본값은 3입니다.        |
|/blobsize:\<n>       |Blob 당 바이트 수를 지정합니다. 기본값은 1024입니다.         |

사용 예제:

```cmd
gamesaveutil generate dummydata.xml
gamesaveutil generate dummydata.xml /containers:4
gamesaveutil generate dummydata.xml /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```


> [!NOTE]
> Byte 데이터는 간단한 오름차순 시퀀스, 즉 5 바이트 blob 바이트 00 01 02 03 04 해야 합니다.

### <a name="gamesaveutil-simulate"></a>Gamesaveutil 시뮬레이션

`gamesaveutil simulate [/markcontainerschanged] [/stop]`

특정 시나리오를 테스트 하는 것에 대 한 특별 한 조건을 시뮬레이션 합니다.

|옵션  |설명  |
|---------|---------|
|/markcontainerschanged     |강제로 모든 컨테이너가 app 바뀐 것 같습니다 일시 중단에서 다시 시작 하 고 새 공급자를 가져옵니다. /Stop을 사용 하 여 삭제 될 때까지 모든 앱을 영향을 줍니다.      |
|/stop                      |모든 시뮬레이션을 중지합니다.         |


 <a id="xbstorage_fileformat"></a>

## <a name="import-and-export-file-format"></a>가져오기 및 내보내기 파일 형식

사용 되는 XML 파일을 **가져올**, **내보내기**, 및 **생성** 명령을 사용 하 여를 *xbstorage* 도구에 표시 된 형식은 다음 예입니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
  <XbConnectedStorageSpace>
    <ContextDescription>
      <Account msa="user@domain.com" />
      <Title scid="00000000-0000-0000-0000-000000000000" />
    </ContextDescription>
    <Data>
      <Containers>
        <Container name="Container1" displayName="Optional Display Name">
          <Blobs>
            <Blob name="Blob1">
              <![CDATA[... ] ]>
            </Blob>
            ...
            <Blob name="BlobN">
              <![CDATA[... ] ]>
            </Blob>
          </Blobs>
        </Container>
        ...
        <Container name="ContainerN">
        ...
        </Container>
      </Containers>
    </Data>
  </XbConnectedStorageSpace>
```

이 xml에 대 한 서식을 지정 하는 데 필요한 유일한 변경 내용 **가져올**를 **내보내기**, 및 **생성** 에서 *gamesaveutil* 를대체하는\<계정 > 멤버 노드의 합니다 \<ContextDescription > 노드를 \<PackageFamilyName > 노드.
이 변경 됩니다는 \<ContextDescription >에서이 노드:

```xml
<ContextDescription>
    <Account msa="user@domain.com" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

다음과 같이 변경

```xml
<ContextDescription>
    <PackageFamilyName pfn="MyGame_xxxxxx" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

> [!NOTE]
> 이러한 XML 파일에서 데이터의 형식을 플랫폼에서 것 같지 않습니다. 이므로 가져오기에 대 한 목적 으로만 내보냅니다. 중간 또는 유틸리티 형식으로는 보관 형식이 아니라 처리 해야 하므로 이러한 XML 파일에 대 한 데이터 형식을 나중에 변경 될 수 있습니다.

합니다 **ContextDescription** 노드는 선택 사항입니다. 컴퓨터에 대 한 연결 된 저장소 공간을 하는 경우 사용할 수 있습니다 `<Account machine="true"/>` 대신 `<Account msa="user@domain.com"/>`합니다. 그렇지 않으면 가져오기에 대 한 명령줄에서 컨텍스트를 지정할 수 있습니다.
Blob 및 컨테이너 게임 또는 파일이 생성 되는 응용 프로그램에 지정 된 해당 이름이 있어야 합니다.
각 blob의 내용이 래핑됩니다 문자열 이어야 합니다는 **CDATA** 를 호출 하 여 생성 되는 태그 **CryptBinaryToStringW** 플래그로 **CRYPT_STRING_BASE64** 해당 blob에 대 한 원시 바이트 배열로 데이터를 제공합니다. 다시 호출 하 여 blob 데이터를 변환할 수 반대로 **CryptStringToBinary** 이전 암호화 된 문자열을 제공 합니다. 이 두 함수를 사용 하는 예제에 표시 됩니다 [CryptBinaryToString 잘못 된 바이트를 반환 합니다.](https://social.msdn.microsoft.com/Forums/vstudio/en-US/9acac904-c226-4ae0-9b7f-261993b9fda2/cryptbinarytostring-returns-invalid-bytes?forum=vcgeneral) Visual Studio에 대 한 MSDN 포럼에서.

<a id="ID4EWBAE"></a>