---
title: 연결 된 로컬 저장소 관리
author: aablackm
description: 개발 환경에서 로컬 저장소 연결 된 데이터를 관리 하는 방법을 알아봅니다.
ms.assetid: 630cb5fc-5d48-4026-8d6c-3aa617d75b2e
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장소, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 5185ae50b428302c26b7a38389e4b925dcecd552
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5435724"
---
# <a name="managing-local-connected-storage"></a>연결 된 로컬 저장소 관리
연결 된 저장 게임 데이터가 클라우드에 저장을 사용 하는 동안은 연결 된 저장소 서비스는 로컬 저장소 구성 요소. PC 또는 콘솔에 인지 클라우드로 동기화 된 데이터를 포함 하는 연결 된 저장소 데이터의 로컬 캐시. XDK 또는 UWP 제목은 만들려는 인지 로컬 저장소 연결 된 데이터를 관리할 수 있도록 하는 도구입니다.

로컬에 캐시 된 연결 된 저장소 데이터를 관리 하려면 적절 한 도구를 찾으려면 다음 표를 참조 하세요.

|제목 분류  |장치  |로컬 저장소 도구  |
|---------|---------|---------|
|XDK     |Xbox One 콘솔     |*xbstorage*         |
|UWP     |PC         |*gamesaveutil*         |
|UWP     |Xbox One 콘솔     |Xbox 장치 포털 (XDP |)

- *Xbstorage* 에 Xbox One 콘솔에서 로컬에 캐시 된 연결 된 저장소를 관리 하기 위한 XDK 명령 프롬프트에서 실행, 명령줄 도구입니다. 파일 경로에서 Xbox One XDK에서 *xbstorage* 도구를 찾을 수 있습니다: **/Program 파일 (x86)/Microsoft Durango XDK/bin/xbstorage.exe**
- *Gamesaveutil* PC에 연결 된 저장소를 캐시 UWP를 로컬로 관리 하기 위한 명령줄 도구입니다. *Gamesaveutil* 도구는 [Windows 10 SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) Fall Creators Update 이상으로 패키지 (10.0.16299.15 빌드 이상). 적절 한 버전의 Windows 10 SDK를 설치한 후 *gamesaveutil* 폴더에서 찾을 수 있습니다: **ProgramFiles(x86)/Windows 키트/10/bin / [SDK Version]/x64/gamesaveutil.exe**.
- *Xbox 장치 Portal(XDP)* Xbox One 콘솔에서 로컬에 캐시 된 저장소 UWP 연결 된 데이터를 관리할 수 있도록 하는 온라인 포털입니다. 이 문서에서는 XDP 사용량을 다루지 않습니다. 자세한 XDP를 사용 하 여 [Xbox 용 디바이스 포털](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-xbox)을 읽습니다.

## <a name="xbstorage"></a>Xbstorage

*Xbstorage* 하드 드라이브의 연결 된 저장소 데이터를 로컬에 캐시 된 데이터 지우기 뿐만 아니라 가져오기 및 XML 파일을 사용 하 여 사용자 또는 컴퓨터에서 연결 된 저장소 공간에 대 한 데이터를 내보낼 수 있습니다.

로컬 파일에 연결 된 저장소 공간에서 데이터를 읽는 동작와 동기화 하면 되므로 해당 작업이 앱 자체에서 수행 되었는지 하는 경우 시스템 동작 *xbstorage* 도구를 사용 하 여 로컬 데이터에 대 한 작업을 수행 하는 경우 복사 하기 전에 클라우드 합니다.

마찬가지로, Xbox One 개발 키트의 연결 된 저장소 컨테이너에는 개발 PC에 XML 파일에서 데이터의 복사본을 사용 하면 클라우드 해당 데이터를 업로드 하려면 콘솔 합니다. 그러나 조건이 없는 동기화 되지 것입니다: 개발 키트, 잠금 얻을 수 없습니다 하거나 콘솔의 컨테이너와 클라우드에서 간의 데이터 충돌이 발생 하는 경우 사용자가 하기로 하 여 충돌을 해결 하는 경우 콘솔 동작 컨테이너를 유지 하 고 콘솔에의 한 버전을 선택 하면 제목 시작 될 때까지 사용자가 오프 라인 재생 하는 경우 작동 합니다.

이러한 명령에의 한 가지 예외 **** 초기화/는 **** 강제로/저장 된 데이터의 로컬 저장소 모든 SCIDs 및 사용자를 해제 하지만 클라우드에 저장 된 데이터를 변경 하지 않습니다. 사용자가 콘솔에 로밍 되었으며 제목을 재생 시 클라우드에서 데이터를 다운로드 하는 경우에 것 상태로 콘솔을 배치 하는 데 유용 합니다.

### <a name="xbstorage-commands"></a>Xbstorage 명령

Xbstorage에 다음 6 개의 명령을 개발자는 자신의 Xbox One 개발 키트의 로컬 데이터 관리 XDK 명령 프롬프트를 사용 하 여 사용할 수 있습니다.

<a id="xbstorage_reset"></a>

|명령  |설명  |
|---------|---------|
|다시 설정    |연결 된 저장소 초기화 팩터리를 수행 합니다.         |
|가져오기   |연결 된 저장소 공간에 지정된 된 XML 파일에서 데이터를 가져옵니다.         |
|내보내기   |지정된 된 XML 파일에 연결 된 저장소 공간에서 데이터를 내보냅니다.         |
|삭제   |연결 된 저장소 공간에서 데이터를 삭제합니다.         |
|생성 |더미 데이터를 생성 하 고 지정된 된 XML 파일에 저장 합니다.         |
|시뮬레이션 |저장소 공간 부족 시뮬레이션합니다.         |

### <a name="xbstorage-reset"></a>Xbstorage 초기화

`xbstorage reset [/force]`

공장 설정으로 복원 로컬 콘솔에서 연결 된 저장소의 모든 로컬 데이터를 지웁니다. 클라우드로 유지 하는 데이터 수정 되지 않으며 다운로드할 수 다시 필요에 따라 합니다.

|옵션  |설명  |
|---------|---------|
|/force   |연결 된 저장소를 다시 설정 해야 한다는 것을 확인 합니다. 다음 메시지가 표시 될 수로 인해 **** 강제로/지정 하지 않고 재설정 명령을 실행: 연결 된 저장소 공장 초기화는 파괴적 작업을 **** 강제로/플래그 아닌 경우이 명령을 초기화를 수행 하지 않는 존재 합니다. |

<a id="xbstorage_import"></a>

### <a name="xbstorage-import"></a>Xbstorage 가져오기

`xbstorage import *file-name* [/scid:*SCID*] [/machine] [/msa:*account*] [/replace] [/verbose]`

연결 된 저장소 공간에 *파일 이름* 지정 된 데이터를 가져옵니다.
파일은 데이터가 포함 된 XML 파일. 예를 들어 [xbstorage 생성](#xbstorage_generate)을 참조 하세요. 파일의 XML 형식에 대 한 자세한 내용은이 항목의 뒷부분에 [가져오기 및 내보내기 파일 형식](#xbstorage_fileformat)을 참조 하세요.
연결 된 저장소 공간을 지정 하는 방법은 두 가지가 있습니다.

- 입력된 파일이 올바르게 채워지는 **ContextDescription** 섹션에 있는 경우 다음이 연결 된 저장소 공간을 지정 합니다.
- 저장소 공간 또는 부분적으로 지정할 수도 입력된 파일에 지정 된 저장소 공간의 각 요소 보다 우선 명령줄 매개 변수를 통해 합니다.

사용 예:

```cmd
  xbstorage import mydata.xml
  xbstorage import mydata.xml /replace
  xbstorage import mydata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /verbose 
```

> [!NOTE]
> 지정 된 연결 된 저장소 공간을 가져오기 전에 시스템은 실행 중인 앱 연결 된 저장소 공간을 확보 때 실행 되는 동일한 논리를 사용 하 여 클라우드와 동기화 하려고 합니다.
>
> 동일한 기본 서비스 안내를 사용 하 여 응용 프로그램을 실행 하는 경우에이 작업으로 인해는 경합 상태 및 연결 된 저장소 공간의 내용을 확정 되지 않은 상태에서 수 있습니다.
>
> **** 바꾸기/지정 하지 않으면 입력된 파일에 지정 된 컨테이너는 입력된 파일에 지정 된 blob을 작성 하기 전에 지워집니다. 입력된 파일에 지정 되지 않은 연결 된 저장소 공간에서 컨테이너는 그대로 유지 됩니다.

|옵션  |설명  |
|---------|---------|
|*파일 이름*     |가져오려는 데이터가 포함 된 XML 파일을 지정 합니다.         |
|/scid:*서비스 안내*    |서비스 구성 Id (서비스 안내)을 지정합니다.         |
|/machine        |컴퓨터별 연결 된 저장소 공간을 지정합니다.  이 옵션은 **/msa** 옵션을 사용 하 여 동시에 사용할 수 없습니다.         |
|/msa:*계정*  |사용자 당 연결 된 저장소에 사용할 계정을 지정 합니다. 사용자가 콘솔에 사용할 공간 서명 해야 합니다.  이 옵션은 **/machine** 옵션을 사용 하 여 동시에 사용할 수 없습니다.         |
|/replace        |가져오기 전에 지정 된 연결 된 저장소 공간에서 모든 컨테이너를 삭제합니다.         |
|/verbose        |가져오기 작업의 상태를 표시합니다.         |


 <a id="xbstorage_export"></a>

### <a name="xbstorage-export"></a>Xbstorage 내보내기

`xbstorage export *outfile* [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

연결 된 저장소 공간에서 데이터를 **출력**하 여 지정 된 파일을 내보냅니다.    파일은 데이터가 포함 된 XML 파일. 예를 생성 하는 방법을 보려면 [xbstorage 생성](#xbstorage_generate) 을 참조 하세요. 파일의 XML 형식에 대 한 자세한 내용은이 항목의 뒷부분에 [가져오기 및 내보내기 파일 형식](#xbstorage_fileformat)을 참조 하세요. 연결 된 저장소 공간을 지정 하는 방법은 두 가지가 있습니다.

- **/Context** 매개 변수를 사용 하 여 파일 이름을 지정 하는 경우 \ < infile > 올바르게 채워지는 **ContextDescription** 섹션에 다음 해당 파일 연결 된 저장소 공간을 지정 하는 데 사용 됩니다.
- 저장소 공간 또는 부분적으로 지정할 수도 **/context** 파일에 지정 된 저장소 공간의 각 요소 보다 우선 명령줄 매개 변수를 통해 합니다.

사용 예:

```cmd
  xbstorage export exporteddata.xml /context:space.xml
  xbstorage export exporteddata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /context:space.xml /verbose
```

> [!NOTE]
> 지정 된 연결 된 저장소 공간을 내보내기 전에 시스템은 실행 중인 앱 연결 된 저장소 공간을 확보 때 실행 되는 동일한 논리를 사용 하 여 클라우드와 동기화 하려고 합니다.
>
> 동일한 기본 서비스 안내를 사용 하 여 응용 프로그램을 실행 하는 경우에이 작업으로 인해는 경합 상태 및 연결 된 저장소 공간의 내용을 확정 되지 않은 상태에서 수 있습니다.

|옵션  |설명  |
|---------|---------|
|*출력*             |XML 파일 데이터를 내보냅니다.         |
|/context:*입력 파일* |공간 정보를 읽을 수 있는 입력된 파일을 지정 합니다.         |
|/scid:*서비스 안내*          |서비스 구성 id (서비스 안내)을 지정합니다.         |
|/machine              |컴퓨터별 연결 된 저장소 공간을 지정합니다.  이 옵션은 **/msa** 옵션을 사용 하 여 동시에 사용할 수 없습니다.         |
|/msa:*계정*        |사용자 당 연결 된 저장소에 사용할 계정을 지정 합니다. 사용자가 콘솔에 사용할 공간 서명 해야 합니다.  이 옵션은 **/machine** 옵션을 사용 하 여 동시에 사용할 수 없습니다.         |
|/verbose              |내보내기 작업의 상태를 표시합니다.         |

<a id="xbstorage_delete"></a>

### <a name="xbstorage-delete"></a>Xbstorage 삭제

`xbstorage delete [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

연결 된 저장소 공간에서 모든 데이터를 삭제합니다.
연결 된 저장소 공간을 지정 하는 방법은 두 가지가 있습니다.

- **/Context** 매개 변수를 사용 하 여 파일 이름을 지정 하는 경우 \ < infile > 올바르게 채워지는 **ContextDescription** 섹션에 다음 해당 파일 연결 된 저장소 공간을 지정 하는 데 사용 됩니다.
- 저장소 공간 또는 부분적으로 지정할 수도 **/context** 파일에 지정 된 저장소 공간의 각 요소 보다 우선 명령줄 매개 변수를 통해 합니다.

사용 예:

```cmd
  xbstorage delete /context:space.xml
  xbstorage delete /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /context:space.xml /verbose
```

> [!NOTE]
> 지정 된 연결 된 저장소 공간을 삭제 하기 전에 시스템은 실행 중인 앱 연결 된 저장소 공간을 확보 때 실행 되는 동일한 논리를 사용 하 여 클라우드와 동기화 하려고 합니다.
>> 동일한 기본 서비스 안내를 사용 하 여 응용 프로그램을 실행 하는 경우에이 작업으로 인해는 경합 상태 및 연결 된 저장소 공간의 내용을 확정 되지 않은 상태에서 수 있습니다.

|옵션  |설명 |
|---------|---------|
|/context:*입력 파일*     |공간 정보를 읽을 수 있는 입력된 파일을 지정 합니다.         |
|/scid:*서비스 안내*              |서비스 구성 id (서비스 안내)을 지정합니다.         |
|/machine                  |컴퓨터별 연결 된 저장소 공간을 지정합니다.  이 옵션은 **/msa** 옵션을 사용 하 여 동시에 사용할 수 없습니다.         |
|/msa:*계정*            |사용자 당 연결 된 저장소에 사용할 계정을 지정 합니다. 사용자가 콘솔에 사용할 공간 서명 해야 합니다.  이 옵션은 **/machine** 옵션을 사용 하 여 동시에 사용할 수 없습니다.         |
|/verbose                  |삭제 작업의 상태를 표시 합니다.         |

 <a id="xbstorage_generate"></a>

### <a name="xbstorage-generate"></a>Xbstorage 생성

`xbstorage generate *file-name* [/containers:*n*] [/blobs:*n*] [/blobsize:*n*]`

더미 데이터를 생성 하 고 지정 된 파일에 저장 \ < 이름 >. 파일의 XML 형식에 대 한 자세한 내용은이 항목의 뒷부분에 [가져오기 및 내보내기 파일 형식](#xbstorage_fileformat)을 참조 하세요.    서비스 구성 id (서비스 안내) 00000000-0000-0000-0000-000000000000를 놓고 컴퓨터별 연결 된 저장소 공간에 대 한 계정 설정 됩니다. 이러한 값을 변경 하려는 경우 직접 편집할 수 있습니다 파일 생성 됩니다.

사용 예:

```cmd
  xbstorage generate dummydata.xml
  xbstorage generate dummydata.xml /containers:4
  xbstorage generate dummydata.xml /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```

> [!NOTE]
> 바이트 데이터는 간단한 오름차순; 예를 들어 5 비트 blob에 다음 바이트: 00 01 02 03 04 합니다. >> 사용자별 연결 된 저장소 공간을 지정 하려면 **계정** 노드는 XML 파일에 다음과 같이 변경 합니다.
>  ```
>    <Account msa="user@domain.com"/>
>  ```

|옵션  |설명  |
|---------|---------|
|*파일 이름*     | XML 파일 데이터에 기록 됩니다. |
|/containers:*n* | 숫자를 *n*을 생성 하는 컨테이너를 지정 합니다. 기본 개수는 2입니다.  |
|blob /:*n*      | 숫자를 *n*을 생성 하는 blob을 지정 합니다. 기본 횟수는 3입니다.  |
|/blobsize:*n*   | 숫자를 *n*, blob 당 바이트를 지정합니다. 기본 크기가 1024 바이트입니다.  |

 <a id="xbstorage_simulate"></a>

### <a name="xbstorage-simulate"></a>Xbstorage 시뮬레이션

`xbstorage simulate [/reserveremainingspace] [/forceoutoflocalstorage] [/stop] [/verbose]`

연결 된 저장소 서비스에 대 한 로컬 저장소 부족 시뮬레이션합니다.

|옵션  |설명  |
|---------|---------|
|/reserveremainingspace | 연결 된 저장소의 모든 나머지 공간을 예약 합니다. 삭제 ConnectedStorage에서 사용할 수 있는 공간을 열립니다. |

| / forceoutoflocalstorage | 왼쪽 공간이 필요 하는 연결 된 저장소 서비스를 시뮬레이트합니다. 연결 된 저장소에서 삭제 메모리 부족 보고 연결 된 저장소 서비스는 변경 되지 않습니다. |

| 중지 / | 모든 시뮬레이션을 중지합니다. |

| /verbose | 시뮬레이트된 작업의 상태를 표시합니다. |

 <a id="ID4E4MAC"></a>

### <a name="common-options"></a>일반적인 옵션

`xbstorage [/?] [/X*:address* [*+accesskey*] ]`

|옵션  |설명  |
|---------|---------|
| /?                           |  Xbstorage.exe에 대 한 도움말을 표시 |
| /X *: 주소* [*+ accesskey*]  | 호스트 이름 또는 대상된 콘솔의 주소 (콘솔에서 **도구 IP** 로 표시 됨)를 지정 하지만 기본 콘솔 변경 되지 않습니다. 이 옵션을 사용 하면 기본 콘솔 사용 됩니다. *Accesskey* 은 선택 키를 알고 있는 사용자만 콘솔 액세스를 제한 하는 데 사용할 수 있는 문자열입니다. **Xbconfig**명령을 사용 하 여 선택 키를 설정 **accesskey = * * * your 키*; 선택 키를 적용 하려면 콘솔을 다시 시작 합니다. 선택 키를 사용 하 여 구성 된 콘솔에 액세스 하려면 콘솔의 IP 주소 또는 호스트 이름 뒤에 더하기 (+)와 선택 키를 포함 해야 합니다.
> [!NOTE]
> Xbconnect 하 여 콘솔의 기본 설정 된 경우 선택 키 제공 하는 경우 선택 키 기본 콘솔의 주소의 일부로 저장 됩니다.
|

## <a name="gamesaveutil"></a>Gamesaveutil

*Gamesaveutil* *xbstorage* 해당 기능을 모두 동일한를 사용 하 여 앱에 대 한 로컬에 캐시 된 연결 된 저장소를 관리할 수 있도록 제공 합니다. Xbstorage 도구와 마찬가지로 *gamesaveutil* 함수 동작에 몇 가지 차이점이 관리 동일한 6 개까지 데이터를 제공 합니다. 가져오기, 내보내기, 삭제 하 고 초기화 명령은 로그인 하는 Xbox Live 사용자 필요 합니다. 확인 하 고 현재 사용자를 변경할 Windows 10의 Xbox 앱을 사용할 수 있습니다.

### <a name="commands"></a>명령

|명령  |설명  |
|---------|---------|
|가져오기   |지정된 된 XML 파일에서 데이터를 가져옵니다.         |
|내보내기   |지정 된 xml 파일에 데이터를 내보냅니다.         |
|삭제   |지정된 된 앱에서 데이터를 삭제합니다.        |
|다시 설정    |지정된 된 앱에 대 한 로컬 데이터를 삭제합니다.         |
|생성 |더미 데이터를 생성 하 고 지정 된 xml 파일을 저장 합니다.         |
|시뮬레이션 |테스트 하기 어려운 특수 조건이 시뮬레이션         |

### <a name="gamesaveutil-import"></a>Gamesaveutil 가져오기

`gamesaveutil import <filename> [/pfn:<PFN>] [/scid:<SCID>] [/replace]`

에 지정 된 데이터를 가져오는 \ < 이름 >

파일은 데이터가 포함 된 XML 파일. 유형 `gamesaveutil help generate` 예를 생성 하는 방법을 확인 합니다.

앱을 지정 하는 방법은 두 가지가 PFN 및 데이터가 저장 되는 서비스 안내 합니다.

입력된 파일에 올바르게 채워지는 ContextDescription 섹션 경우 대상 앱을 지정 하려면 사용할이 PFN 및 서비스 안내 합니다.

PFN 및 서비스 안내 또는 부분적으로 지정할 수 입력된 파일에서 지정 된 PFN 및 서비스 안내의 각 요소 보다 우선 명령줄 매개 변수를 통해 합니다.

|옵션  |설명  |
|---------|---------|
|/pfn: \ < PFN >       |가져오기에 대해 수행 하는 앱에 대 한 패키지 패밀리 Name(PFN)를 지정 합니다. 앱을 설치 해야 합니다.         |
|/scid: \ < 서비스 > 안내     |서비스 구성 Id (서비스 안내)을 지정합니다. Xbox Live 구성과에서입니다.         |
|/replace         |가져오기 수행 하기 전에 모든 컨테이너를 삭제 합니다.         |

예제 사용법:

```cmd
gamesaveutil import mydata.xml
gamesaveutil import mydata.xml /replace
gamesaveutil import mydata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 앱 설치 해야 하 고 가져오기 위해 데이터를 이미 동기화 않은.
> 
> /Replace를 지정 하지 않으면 입력된 파일에서 지정 하지 않는 한 기존 컨테이너 터치 하지 않습니다.

### <a name="gamesaveutil-export"></a>Gamesaveutil 내보내기

`gamesaveutil export <outfile> [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

지정 된 파일에 데이터를 내보냅니다. \ < 출력 > 합니다.

파일은 데이터가 포함 된 XML 파일. Gamesaveutil 도움말을 입력 합니다. 예를 생성 하는 방법을 보려면 생성 합니다.

내보낼 데이터의 위치를 지정 하는 방법은 두 가지가 있습니다.

/Context 매개 변수를 사용 하 고 파일 이름을 지정 하 여 \ < infile > 올바르게 채워지는 ContextDescription 섹션에 다음 해당 파일을 사용 하 여 원본 데이터의 위치를 지정 합니다.

/Context 파일에 지정 된 각 요소 보다 우선 명령줄 매개 변수를 통해 위치를 지정할 수도 있습니다.

|옵션  |설명  |
|---------|---------|
|/context: \ < infile >     |지정 된 파일을 사용 하 여 앱을 지정 PFN 및 서비스 안내 합니다.         |
|/pfn: \ < PFN >            |내보내기에서 수행 하는 앱에 대 한 패키지 패밀리 Name(PFN)를 지정 합니다. 앱을 설치 해야 합니다.       |
|/scid: \ < 서비스 > 안내          |서비스 구성 Id (서비스 안내)을 지정합니다. Xbox Live 구성과에서입니다.        |

예제 사용법:

```cmd
gamesaveutil export exporteddata.xml /context:target.xml
gamesaveutil export exporteddata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 앱 설치 해야 하 고 내보내기 하기 위해 데이터를 이미 동기화 않은.

### <a name="gamesaveutil-delete"></a>Gamesaveutil 삭제

`gamesaveutil delete [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

지정 된 PFN 및 서비스 안내 모든 데이터가 삭제 됩니다.

삭제할 데이터의 위치를 지정 하는 방법은 두 가지가 있습니다.

/Context 매개 변수를 사용 하 고 파일 이름을 지정 하 여 \ < infile > 올바르게 채워지는 ContextDescription 섹션에 다음 해당 파일을 사용 하 여 원본 데이터의 위치를 지정 합니다.

/Context 파일에 지정 된 각 요소 보다 우선 명령줄 매개 변수를 통해 위치를 지정할 수도 있습니다.

|옵션  |설명  |
|---------|---------|
|/context: \ < infile >     |지정 된 파일을 사용 하 여 앱을 지정 PFN 및 서비스 안내 합니다.         |
|/pfn: \ < PFN >            |데이터를 삭제 하는 앱에 대 한 패키지 패밀리 Name(PFN)를 지정 합니다. 앱을 설치 해야 합니다.       |
|/scid: \ < 서비스 > 안내          |서비스 구성 Id (서비스 안내)을 지정합니다. Xbox Live 구성과에서입니다.        |

예제 사용법:

```cmd
gamesaveutil delete /context:target.xml
gamesaveutil delete /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 컨테이너를 삭제 하기 위해 앱을 설치 해야 합니다.

### <a name="gamesaveutil-reset"></a>Gamesaveutil 초기화

`gamesaveutil reset [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

지정 된 PFN 및 서비스 안내에 대 한 모든 로컬 데이터를 삭제합니다.

삭제할 데이터의 위치를 지정 하는 방법은 두 가지가 있습니다.

/Context 매개 변수를 사용 하 고 파일 이름을 지정 하 여 \ < infile > 올바르게 채워지는 ContextDescription 섹션에 다음 해당 파일을 사용 하 여 원본 데이터의 위치를 지정 합니다.

/Context 파일에 지정 된 각 요소 보다 우선 명령줄 매개 변수를 통해 위치를 지정할 수도 있습니다.

|옵션  |설명  |
|---------|---------|
|/context: \ < infile >     |지정 된 파일을 사용 하 여 앱을 지정 PFN 및 서비스 안내 합니다.         |
|/pfn: \ < PFN >            |데이터를 삭제 하는 앱에 대 한 패키지 패밀리 Name(PFN)를 지정 합니다. 앱을 설치 해야 합니다.       |
|/scid: \ < 서비스 > 안내          |서비스 구성 Id (서비스 안내)을 지정합니다. Xbox Live 구성과에서입니다.        |

예제 사용법:

```cmd
gamesaveutil reset /context:target.xml
gamesaveutil reset /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> 로컬 데이터를 삭제 하기 위해 앱을 설치 해야 합니다.

### <a name="gamesaveutil-generate"></a>Gamesaveutil 생성

`gamesaveutil generate <filename> [/containers:<n>] [/blobs:<n>] [/blobsize:<n>]`

더미 데이터를 생성 하 고 지정 된 파일에 저장 \ < 이름 >.
서비스 구성 식별자 (서비스 안내) 00000000-0000-0000-0000-000000000000로 설정 됩니다. 원하는 경우 해당 값을 변경 하는 생성 후 파일을 수동으로 편집 합니다.

|옵션  |설명  |
|---------|---------|
|/containers: \ < n >     |생성에 얼마나 많은 컨테이너를 지정 합니다. 기본값은 2입니다.         |
|blob /: \ < n >          |컨테이너를 생성 당 얼마나 많은 blob을 지정 합니다. 기본값은 3입니다.        |
|/blobsize: \ < n >       |Blob 당 바이트 수를 지정합니다. 기본값은 1024입니다.         |

예제 사용법:

```cmd
gamesaveutil generate dummydata.xml
gamesaveutil generate dummydata.xml /containers:4
gamesaveutil generate dummydata.xml /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```


> [!NOTE]
> 바이트 데이터 간단한 오름차순 이면 5 바이트 blob 한 바이트가 00 01 02 03 04 즉.

### <a name="gamesaveutil-simulate"></a>Gamesaveutil 시뮬레이션

`gamesaveutil simulate [/markcontainerschanged] [/stop]`

특정 시나리오를 테스트 하기 위한 특별 한 조건을 시뮬레이트합니다.

|옵션  |설명  |
|---------|---------|
|/markcontainerschanged     |힘 때 앱 바뀐 것 처럼 보이게 하는 모든 컨테이너에서 일시 중단 다시 시작 하 고 새 공급자를 가져옵니다. /Stop을 사용 하 여 선택이 취소 될 때까지 모든 앱을 영향을 줍니다.      |
|정지 /                      |모든 시뮬레이션을 중지합니다.         |


 <a id="xbstorage_fileformat"></a>

## <a name="import-and-export-file-format"></a>가져오기 및 내보내기 파일 형식

XML 파일 **가져오기**, **내보내기**및 *xbstorage* 도구를 사용 하 여 명령을 **생성** 을 사용한 다음 예제에 표시 된 형식은.

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

대체 하는 **가져오기**, **내보내기**및 *gamesaveutil* **생성** 에 대 한이 xml 서식을 지정 하는 데 필요한 변경은 \ < 계정 > 구성원 노드는 \ < ContextDescription > 노드는 \ < PackageFamilyName > 노드.
이렇게 하면 변경 됩니다는 \ < ContextDescription > 노드가:

```xml
<ContextDescription>
    <Account msa="user@domain.com" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

다음과 같이 변경 하려면

```xml
<ContextDescription>
    <PackageFamilyName pfn="MyGame_xxxxxx" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

> [!NOTE]
> XML 파일에 데이터 형식은 플랫폼에서 동일한, 가져오기 위한 것이 목적 으로만 내보내기 및 합니다. 중간 또는 유틸리티 형식, 보관 된 형식의 하지으로 처리 해야 하므로 이러한 XML 파일에 대 한 데이터 형식을 나중에 변경 될 수 있습니다.

**ContextDescription** 노드 선택 사항입니다. 사용할 수는 컴퓨터에 대 한 연결 된 저장소 공간을 만들고 `<Account machine="true"/>` 대신 `<Account msa="user@domain.com"/>`. 그렇지 않으면 컨텍스트 가져오기에 대 한 명령줄에서 지정할 수 있습니다.
Blob 및 컨테이너에서 게임이 나 응용 프로그램 생성 되는 파일에 게 부여 된 해당 이름을 사용 해야 합니다.
각 blob의 콘텐츠는 **CryptBinaryToStringW** **CRYPT_STRING_BASE64** 해당 blob raw 바이트 배열에 대 한 데이터를 제공 하는 플래그를 사용 하 여 호출 하 여 생성 된 **CDATA** 태그에 래핑된 문자열 이어야 합니다. 반대로, 데이터 blob **CryptStringToBinary** 를 호출 하 고 이전의 암호화 된 문자열을 제공 하 여 다시 변환할 수 있습니다. 이 두 함수를 사용 하는 예제는 Visual Studio에 대 한 MSDN 포럼에서 [잘못 된 바이트를 반환 하는 CryptBinaryToString](http://social.msdn.microsoft.com/Forums/vstudio/en-US/9acac904-c226-4ae0-9b7f-261993b9fda2/cryptbinarytostring-returns-invalid-bytes?forum=vcgeneral) 에 표시 됩니다.

<a id="ID4EWBAE"></a>