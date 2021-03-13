---
title: Windows 10 용 Powertoy PowerRename 유틸리티
description: 파일의 대량 이름을 바꾸기 위한 windows 셸 확장
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 26eee9fcb954a0a97ba6f30fae8a9d09395403a3
ms.sourcegitcommit: a1b251971f7ac574275d53bbe3e9ef4a3a9dc15c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2021
ms.locfileid: "103417104"
---
# <a name="powerrename-utility"></a>PowerRename 유틸리티

PowerRename은 다음 작업을 수행할 수 있도록 하는 대량 이름 바꾸기 도구입니다.

- 파일의 이름을 *모두 같은 이름으로 바꾸지 않고* 많은 수의 파일 이름을 수정 합니다.
- 파일 이름의 대상 섹션에서 검색 및 바꾸기를 수행 합니다.
- 여러 파일에서 정규식 이름 바꾸기를 수행 합니다.
- 대량 이름 바꾸기를 완료 하기 전에 미리 보기 창에서 필요한 이름 바꾸기 결과를 확인 합니다.
- 작업을 완료 한 후에 이름 바꾸기 작업을 실행 취소 합니다.

## <a name="demo"></a>데모

이 데모에서는 파일 이름 "Pampalona"의 모든 인스턴스가 "Pamplona"로 바뀝니다. 모든 파일의 이름을 고유 하 게 지정 했으므로 수동으로 하나씩을 완료 하는 데 시간이 오래 걸릴 수 있습니다. PowerRename는 단일 대량 이름 바꾸기를 사용 하도록 설정 합니다. "실행 취소 이름 바꾸기" (Ctrl + Z) 명령을 사용 하면 변경 내용을 실행 취소할 수 있습니다.

![PowerRename 데모](../images/powerrename-demo.gif)

## <a name="powerrename-menu"></a>PowerRename 메뉴

Windows 파일 탐색기에서 일부 파일을 선택한 후 마우스 오른쪽 단추로 클릭 하 고 *PowerRename* (powertoy에서 사용 하도록 설정 된 경우에만 표시 됨)를 선택 하면 PowerRename 메뉴가 표시 됩니다. 선택한 항목 (파일)의 수, 검색 및 바꾸기 값, 옵션 목록 및 입력 한 검색 및 바꾸기 값을 표시 하는 미리 보기 창이 표시 됩니다.

![PowerRename 메뉴 스크린샷](../images/powerrename-menu.png)

### <a name="search-for"></a>검색 대상

항목과 일치 하는 조건을 포함 하는 선택 영역에서 파일을 찾으려면 텍스트 또는 [정규식](https://wikipedia.org/wiki/Regular_expression) 을 입력 합니다. [ *미리 보기]* 창에 일치 하는 항목이 표시 됩니다.

### <a name="replace-with"></a>다음 항목으로 교체

선택한 파일에 일치 하는 이전에 입력 *한 값에 대 한 검색* 을 대체할 텍스트를 입력 합니다. *미리 보기* 창에서 원래 파일 이름과 이름이 바뀐 파일을 볼 수 있습니다.

### <a name="options---use-regular-expressions"></a>옵션-정규식 사용

이 확인란을 선택 하면 검색 값이 [정규식](https://wikipedia.org/wiki/Regular_expression) (regex)로 해석 됩니다. Replace 값에는 regex 변수가 포함 될 수도 있습니다 (아래 예제 참조).  선택 하지 않으면 검색 값이 일반 텍스트로 해석 되어 Replace 필드의 텍스트로 바뀝니다.

`Use Boost library`확장 regex 기능의 설정 메뉴에 있는 옵션에 대 한 자세한 내용은 [정규식 섹션](#regular-expressions)을 참조 하십시오.

### <a name="options---case-sensitive"></a>옵션-대/소문자 구분

이 확인란을 선택 하면 텍스트가 동일한 경우에만 검색 필드에 지정 된 텍스트가 항목의 텍스트와 일치 하 게 됩니다. 대/소문자 일치는 기본적으로 대/소문자 구분을 인식 하지 않고 구분 됩니다.

### <a name="options---match-all-occurrences"></a>옵션-모든 항목 일치

선택 하는 경우 검색 필드에 있는 모든 텍스트의 일치 항목은 바꾸기 텍스트로 바뀝니다.  그렇지 않으면 파일 이름에 있는 텍스트 검색의 첫 번째 인스턴스만 대체 됩니다 (왼쪽에서 오른쪽으로).

예를 들어, 다음과 같은 파일 이름을 지정 `powertoys-powerrename.txt` 합니다.

- 검색: `power`
- 이름 바꾸기: `super`

이름이 바뀐 파일의 값은 다음을 반환 합니다.

- 모든 항목 일치 (선택 하지 않음): `supertoys-powerrename.txt`
- 모든 항목 일치 (선택): `supertoys-superrename.txt`

### <a name="options---exclude-files"></a>옵션-파일 제외

파일은 작업에 포함 되지 않습니다. 폴더만 포함 됩니다.

### <a name="options---exclude-folders"></a>옵션-폴더 제외

폴더는 작업에 포함 되지 않습니다. 파일만 포함 됩니다.

### <a name="options---exclude-subfolder-items"></a>옵션-하위 폴더 항목 제외

폴더 내의 항목은 작업에 포함 되지 않습니다. 기본적으로 모든 하위 폴더 항목이 포함 되어 있습니다.

### <a name="options---enumerate-items"></a>옵션-항목 열거

작업에서 수정 된 파일 이름에 숫자 접미사를 추가 합니다. 예: `foo.jpg` -> `foo (1).jpg`

### <a name="options---item-name-only"></a>옵션-항목 이름만

파일 확장명이 아닌 파일 이름 부분만 작업에 의해 수정 됩니다. 예: `txt.txt` ->  `NewName.txt`

### <a name="options---item-extension-only"></a>옵션-항목 확장만

파일 이름 이외의 파일 확장명 부분만 작업에 의해 수정 됩니다. 예: `txt.txt` -> `txt.NewExtension`

## <a name="replace-using-file-creation-date-and-time"></a>파일 생성 날짜 및 시간을 사용 하 여 바꾸기

파일의 만든 날짜 및 시간 특성은 아래 테이블에 따라 변수 패턴을 입력 하 여 텍스트로 *바꾸기* 에서 사용할 수 있습니다.

변수 패턴 |설명
|:---|:---|
|`$YYYY`|사용 된 달력에 따라 전체 4 자리 또는 5 자리로 표시 되는 연도입니다.
|`$YY`|마지막 두 자리로만 표시 되는 연도입니다. 한 자리 연도에 대해 앞에 오는 0이 추가 됩니다.
|`$Y`|마지막 자릿수로 표시 되는 연도입니다.
|`$MMMM`|월 이름
|`$MMM`|월의 약식 이름입니다.
|`$MM`|한 자리 월의 경우 앞에 0을 사용 하는 월입니다.
|`$M`|한 자리 월의 경우 앞에 0이 없는 월입니다.
|`$DDDD`|요일 이름
|`$DDD`|요일의 약식 이름입니다.
|`$DD`|1 자리 일의 경우 앞에 0을 사용 하는 숫자로 된 월의 일입니다.
|`$D`|한 자리 일의 경우 앞에 0이 없는 월의 일입니다.
|`$hh`|한 자리 시간의 경우 앞에 0이 있는 시간
|`$h`|한 자리 시간의 경우 앞에 0이 없는 시간
|`$mm`|1 자리 분의 경우 앞에 0이 포함 된 분입니다.
|`$m`|1 자리 분의 경우 앞에 0이 없는 분입니다.
|`$ss`|한 자리 초의 경우 앞에 0이 있는 초
|`$s`|한 자리 초의 경우 앞에 0이 없는 시간 (초)입니다.
|`$fff`|전체 3 자리로 표시 되는 시간 (밀리초)입니다.
|`$ff`|처음 두 자리로만 표시 되는 시간 (밀리초)입니다.
|`$f`|첫 번째 자릿수로만 표시 되는 시간 (밀리초)입니다.

예를 들어 다음과 같은 파일 이름을 지정 합니다.

- `powertoys.png`, 11/02/2020에 만듦
- `powertoys-menu.png`, 11/03/2020에 만듦

항목의 이름을 바꿀 조건을 입력 합니다.

- 검색: `powertoys`
- 이름 바꾸기: `$MMM-$DD-$YY-powertoys`

이름이 바뀐 파일의 값은 다음을 반환 합니다.

- `Nov-02-20-powertoys.png`
- `Nov-03-20-powertoys-menu.png`

## <a name="regular-expressions"></a>정규식

대부분의 사용 사례에서 간단한 검색 및 바꾸기를 사용 하면 충분 합니다. 그러나 복잡 한 이름 바꾸기 작업을 수행 하 여 더 많은 제어가 필요한 경우도 있습니다. [정규식](https://wikipedia.org/wiki/Regular_expression) 은 유용할 수 있습니다.

정규식은 텍스트에 대 한 검색 패턴을 정의 합니다. 텍스트를 검색, 편집 및 조작 하는 데 사용할 수 있습니다. 정규식에 의해 정의 된 패턴은 지정 된 문자열에 대해 한 번 또는 여러 번 일치할 수 있습니다. PowerRename는 최신 프로그래밍 언어에서 일반적으로 사용 되는 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 문법을 사용 합니다.

정규식을 사용 하도록 설정 하려면 "정규식 사용" 확인란을 선택 합니다.

**참고:** 정규식을 사용 하는 동안 "모든 항목 일치"를 확인 하는 것이 좋습니다.

표준 라이브러리 대신 [부스트 라이브러리](https://www.boost.org/doc/libs/1_74_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html) 를 사용 하려면 `Use Boost library` powertoy 설정에서 옵션을 선택 합니다. 표준 라이브러리에서 지원 하지 않는 [좌측](https://www.boost.org/doc/libs/1_74_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html#boost_regex.syntax.perl_syntax.lookbehind)과 같은 확장 된 기능을 사용할 수 있습니다.

### <a name="examples-of-regular-expressions"></a>정규식의 예

#### <a name="simple-matching-examples"></a>간단한 일치 예제

| 검색 대상       | Description                                           |
| ---------------- | ------------- |
| `^`              | 파일 이름 시작 부분 일치                   |
| `$`              | 파일 이름의 끝을 찾습니다.                         |
| `.*`             | 이름에 있는 모든 텍스트를 찾습니다.                        |
| `^foo`           | "Foo"로 시작 하는 텍스트 일치                     |
| `bar$`           | "Bar"로 끝나는 텍스트 일치                       |
| `^foo.*bar$`     | "Foo"로 시작 하 고 "bar"로 끝나는 텍스트를 찾습니다. |
| `.+?(?=bar)`     | 모든 항목을 "가로 막대형"과 일치                          |
| `foo[\s\S]*bar`  | "Foo"와 "bar" 사이의 모든 항목 일치              |

#### <a name="matching-and-variable-examples"></a>일치 및 변수 예

*변수를 사용 하는 경우 "모든 항목 일치" 옵션을 사용 하도록 설정 해야 합니다.*

| 검색 대상   | 바꿀 항목    | Description                                |
| ------------ | --------------- |--------------------------------------------|
| `(.*).png`   | `foo_$1.png`   | \_기존 파일 이름 앞에 "foo"를 추가할 수 있습니다. |
| `(.*).png`   | `$1_foo.png`   | \_기존 파일 이름에 "foo"를 추가 합니다.  |
| `(.*)`       | `$1.txt`        | 기존 파일 이름에 ".txt" 확장명을 추가 합니다. |
| `(^\w+\.$)|(^\w+$)` | `$2.txt` | 확장명이 없는 경우에만 기존 파일 이름에 ".txt" 확장명을 추가 합니다. |
|  `(\d\d)-(\d\d)-(\d\d\d\d)` | `$3-$2-$1` | 파일 이름에서 이동 번호: "29-03-2020"는 "2020-03-29"이 됩니다. |

### <a name="additional-resources-for-learning-regular-expressions"></a>정규식 학습을 위한 추가 리소스

온라인에서 제공 되는 유용한 예제/참고 자료 시트가 온라인으로 제공 됩니다.

[Regex 자습서-빠른 참고 자료 예제](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)

[ECMAScript 정규식 자습서](https://o7planning.org/en/12219/ecmascript-regular-expressions-tutorial)

## <a name="file-list-filters"></a>파일 목록 필터

PowerRename에서 필터를 사용 하 여 이름 바꾸기의 결과 범위를 좁힐 수 있습니다. *미리 보기* 창을 사용 하 여 예상 결과를 확인할 수 있습니다. 필터 간에 전환할 열 머리글을 선택 합니다.

- **원본**, *미리 보기* 창의 첫 번째 열은 다음 사이를 순환 합니다.
  - 선택 됨: 파일의 이름이 변경 되었습니다.
  - Unchecked: 검색 조건에 입력 된 값에 맞는 파일의 이름을 변경 하지 않은 경우에도 파일의 이름을 바꿀 수 없습니다.

- **이름을 바꿔서** *미리 보기* 창의 두 번째 열을 전환할 수 있습니다.
  - 기본 미리 보기에는 선택한 모든 파일이 표시 됩니다 .이 파일에는 업데이트 된 이름 바꾸기 값을 표시 하 *는 조건과 일치* 하는 파일만 표시 됩니다.
  - *이름을 바꾼* 헤더를 선택 하면 이름이 변경 될 파일만 표시 하도록 미리 보기가 전환 됩니다. 원본 선택에서 선택한 다른 파일은 표시 되지 않습니다.

![Powertoy PowerRename 필터 데모](../images/powerrename-demo2.gif)
