---
author: mcleblanc
ms.assetid: 5A47301A-2291-4FC8-8BA7-55DB2A5C653F
title: SQLite 데이터베이스
description: SQLite는 서버 없는 포함된 데이터베이스 엔진입니다. 이 문서에서는 SDK에 포함된 SQLite 라이브러리를 사용하거나, 유니버설 Windows 앱에서 고유한 SQLite 라이브러리를 패키지하거나, 소스에서 빌드하는 방법을 설명합니다.
---
# SQLite 데이터베이스

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


SQLite는 서버 없는 포함된 데이터베이스 엔진입니다. 이 문서에서는 SDK에 포함된 SQLite 라이브러리를 사용하거나, 유니버설 Windows 앱에서 고유한 SQLite 라이브러리를 패키지하거나, 소스에서 빌드하는 방법을 설명합니다.

## SQLite 정의 및 사용해야 하는 경우

SQLite는 서버 없는 포함된 오픈 소스 데이터베이스입니다. 지난 몇 년 동안 다양한 플랫폼 및 디바이스의 데이터 저장소에 대한 주요 디바이스 쪽 기술로 대두되었습니다. UWP(유니버설 Windows 플랫폼)는 모든 Windows 10 디바이스 패밀리의 로컬 저장소에 대해 SQLite를 지원 및 권장합니다.

SQLite는 휴대폰 앱, Windows 10 IoT Core(IoT Core)용 포함된 응용 프로그램 및 엔터프라이즈 RDBS(관계형 데이터베이스 서버) 데이터의 캐시로 가장 적합합니다. 많은 동시 쓰기 또는 큰 데이터 크기 조정이 필요한 경우(대부분의 앱에서 거의 나타나지 않는 시나리오)가 아니면 대부분의 로컬 데이터 액세스 요구를 충족합니다.

미디어 재생 및 게임 응용 프로그램에서는 카탈로그나 웹 서버에서 현재 상태로 다운로드할 수 있는 게임 수준 등의 기타 자산을 저장할 파일 형식으로 SQLite를 사용할 수도 있습니다.

## UWP 앱 프로젝트에 SQLite 추가

UWP 프로젝트에 SQLite를 추가하는 방법에는 세 가지가 있습니다.

1.  [SDK SQLite 사용](#using-the-sdk-sqlite)
2.  [앱 패키지에 SQLite 포함](#including-sqlite-in-the-app-package)
3.  [Visual Studio에서 원본을 통해 SQLite 빌드](#building-sqlite-from-source-in-visual-studio)

### SDK SQLite 사용

UWP SDK에 포함된 SQLite 라이브러리를 사용하여 응용 프로그램 패키지의 크기를 줄이고 플랫폼을 사용하여 라이브러리를 정기적으로 업데이트할 수 있습니다. SDK SQLite를 사용하면 SQLite 라이브러리가 시스템 구성 요소에서 사용할 수 있도록 메모리에 이미 로드되어 있을 가능성이 크기 때문에 더 빠른 시작 시간 등의 성능 이점이 발생할 수도 있습니다.

SDK SQLite를 참조하려면 프로젝트에 다음 헤더를 포함합니다. 헤더에는 플랫폼에서 지원되는 SQLite 버전도 포함되어 있습니다.

`#include <winsqlite/winsqlite3.h>`

winsqlite3.lib에 연결하도록 프로젝트를 구성합니다. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**&gt;**링커**&gt;**입력**을 선택한 다음 winsqlite3.lib를 **추가 종속성**에 추가합니다.

### 2. 앱 패키지에 SQLite 포함

SDK 버전을 사용하는 대신 고유한 라이브러리를 패키지하려는 경우도 있습니다. 예를 들어 SDK에 포함된 SQLite 버전과 다른 특정 버전을 플랫폼 간 클라이언트에서 사용할 수 있습니다.

SQLite.org에서 또는 확장 및 업데이트 도구를 통해 사용할 수 있는 유니버설 Windows 플랫폼 Visual Studio 확장의 SQLite 라이브러리를 설치합니다.

![확장 및 업데이트 화면](./images/extensions-and-updates.png)

확장을 설치한 후 코드에서 다음 헤더 파일을 참조합니다.

`#include <sqlite3.h>`

### 3. Visual Studio에서 원본을 통해 SQLite 빌드

[다양한 컴파일러 옵션](http://www.sqlite.org/compile.html)을 사용하여 파일 크기를 줄이거나, 라이브러리 성능을 조정하거나, 응용 프로그램에 맞게 기능 집합을 조정하기 위해 사용자 고유의 SQLite 이진 파일을 컴파일하려는 경우도 있습니다. SQLite는 Windows에서 플랫폼 구성, 기본 매개 변수 값 설정, 크기 제한 설정, 작동 특성 제어, 일반적으로 꺼져 있는 기능 사용, 일반적으로 켜져 있는 기능 사용 안 함, 기능 생략, 분석 및 디버깅 사용, 메모리 할당 동작 관리 등을 수행하기 위한 옵션을 제공합니다.

*Visual Studio 프로젝트에 소스 추가*

SQLite 소스 코드는 [SQLite.org 다운로드 페이지](https://www.sqlite.org/download.html)에서 다운로드할 수 있습니다. SQLite를 사용하려는 응용 프로그램의 Visual Studio 프로젝트에 이 파일을 추가합니다.

*전처리기 구성*

다른 [컴파일 시간 옵션](http://www.sqlite.org/compile.html) 외에 SQLITE\_OS\_WINRT 및 SQLITE\_API=\_\_declspec(dllexport)을 항상 사용합니다.

![SQLite 속성 페이지 화면](./images/property-pages.png)

## SQLite 데이터베이스 관리

SQLite C API를 사용하여 SQLite 데이터베이스를 만들고 업데이트 및 삭제할 수 있습니다. SQLite C API의 세부 정보는 SQLite.org [SQLite C/C++ 인터페이스 소개](http://www.sqlite.org/cintro.html) 페이지에서 확인할 수 있습니다.

SQLite 작동 방식을 잘 이해하려면 SQL 문을 평가하는 SQL 데이터베이스의 주요 작업에서 뒤로 작업합니다. 다음 두 가지 개체에 유의해야 합니다.

-   [데이터베이스 연결 핸들](https://www.sqlite.org/c3ref/sqlite3.html)
-   [준비된 문 개체](https://www.sqlite.org/c3ref/stmt.html)

이러한 개체에 대해 데이터베이스 작업을 수행하기 위한 다음 6가지 인터페이스가 있습니다.

-   [sqlite3\_open()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/open.html)
-   [sqlite3\_prepare()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/prepare.html)
-   [sqlite3\_step()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/step.html)
-   [sqlite3\_column()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/column_blob.html)
-   [sqlite3\_finalize()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/finalize.html)
-   [sqlite3\_close()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/close.html)

 

 






<!--HONumber=May16_HO2-->


