---
description: 태그와 코드 숨김으로 partial 클래스를 조인 하도록 XAML 컴파일을 구성 합니다. 코드 partial 클래스는 별도의 코드 파일에 정의 되 고, 태그 partial 클래스는 XAML 컴파일 중에 코드 생성에 의해 생성 됩니다.
title: xClass 특성
ms.assetid: 40A7C036-133A-44DF-9D11-0D39232C948F
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: be238be3414fb17ff64a5c6d5da713f614c297be
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169087"
---
# <a name="xclass-attribute"></a>x:Class 특성


태그와 코드 숨김으로 partial 클래스를 조인 하도록 XAML 컴파일을 구성 합니다. 코드 partial 클래스는 별도의 코드 파일에 정의 되 고, 태그 partial 클래스는 XAML 컴파일 중에 코드 생성에 의해 생성 됩니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용


``` syntax
<object x:Class="namespace.classname"...>
  ...
</object>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | Description |
|------|-------------|
| namespace | 선택 사항입니다. _Classname_으로 식별 되는 partial 클래스를 포함 하는 네임 스페이스를 지정 합니다. _Namespace_ 를 지정 하면 점 (.)은 _네임 스페이스_ 와 _classname_을 구분 합니다. _Namespace_ 를 생략 하면 _classname_ 에는 네임 스페이스가 없는 것으로 간주 됩니다. |
| classname | 필수 요소. 해당 XAML에 대해 로드 된 XAML 및 코드를 연결 하는 partial 클래스의 이름을 지정 합니다. | 

## <a name="remarks"></a>설명

**x:Class** 는 XAML 파일/개체 트리의 루트인 모든 요소에 대 한 특성으로 선언 될 수 있으며, 빌드 작업 또는 컴파일된 응용 프로그램에 대 한 응용 프로그램 정의의 [**응용**](/uwp/api/Windows.UI.Xaml.Application) 프로그램 루트에 대해 컴파일됩니다. 루트 노드가 아닌 모든 요소에 대해 **x:Class** 를 선언 하 고, **페이지** 빌드 작업으로 컴파일되지 않은 XAML 파일의 경우에는 컴파일 시간 오류가 발생 합니다.

**X:Class** 로 사용 된 클래스는 중첩 된 클래스가 될 수 없습니다.

**X:Class** 특성의 값은 클래스의 정규화 된 이름을 지정 하는 문자열 이어야 합니다. 코드 숨김이 구조화 된 방법 (클래스 정의가 클래스 수준에서 시작 됨) 인 경우에만 네임 스페이스 정보를 생략할 수 있습니다. 페이지 또는 응용 프로그램 정의에 대 한 코드 숨겨진 파일은 프로젝트의 일부로 포함 된 코드 파일 내에 있어야 합니다. 코드 숨김이 클래스는 public 이어야 합니다. 코드 숨김이 클래스는 부분 이어야 합니다.

## <a name="clr-language-rules"></a>CLR 언어 규칙

코드 숨김이 파일은 c + + 파일 일 수 있지만 XAML 구문에 차이가 없도록 CLR 언어 폼을 따르는 특정 규칙도 있습니다. 특히, 모든 **x:Class** 값의 네임 스페이스와 classname 구성 요소 사이에 있는 구분 기호는 항상 점 (".") 이며,이는 XAML과 연결 된 c + + 코드 파일에서 네임 스페이스와 classname 사이의 구분 기호가 "::"입니다. C + +에서 중첩 된 네임 스페이스를 선언 하는 경우 **x:Class** 값의 *네임 스페이스* 부분을 지정 하는 경우 연속 된 중첩 된 네임 스페이스 문자열 간의 구분 기호가 "::"이 아닌 "." 여야 합니다.