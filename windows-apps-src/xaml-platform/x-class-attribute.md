---
author: jwmsft
description: XAML 컴파일을 구성하여 태그와 코드 숨김 사이에 partial 클래스를 연결합니다. 코드 partial 클래스는 별도의 코드 파일에 정의되고, 태그 partial 클래스는 XAML 컴파일 시 코드 생성을 통해 만들어집니다.
title: xClass 특성
ms.assetid: 40A7C036-133A-44DF-9D11-0D39232C948F
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6746969b1b717183894d6b941be41c9aca452960
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6994779"
---
# <a name="xclass-attribute"></a>x:Class 특성


XAML 컴파일을 구성하여 태그와 코드 숨김 사이에 partial 클래스를 조인합니다. 코드 partial 클래스는 별도의 코드 파일에 정의되고, 태그 partial 클래스는 XAML 컴파일 시 코드 생성을 통해 만들어집니다.

## <a name="xaml-attribute-usage"></a>XAML 특성 사용


``` syntax
<object x:Class="namespace.classname"...>
  ...
</object>
```

## <a name="xaml-values"></a>XAML 값

| 용어 | 설명 |
|------|-------------|
| 네임스페이스 | 옵션. _classname_에서 지정한 partial 클래스가 포함된 네임스페이스를 지정합니다. _namespace_를 지정한 경우 점(.)은 _namespace_와 _classname_을 분리합니다. _namespace_를 생략한 경우 _classname_은 네임스페이스가 없는 것으로 간주됩니다. |
| 클래스이름 | 필수. 로드된 XAML 및 해당 XAML의 코드 숨김을 연결하는 partial 클래스의 이름을 지정합니다. | 

## <a name="remarks"></a>설명

**x:Class**는 XAML 파일/개체 트리의 루트이고 빌드 작업이 컴파일 중인 모든 요소의 특성으로 선언하거나, 컴파일된 응용 프로그램의 응용 프로그램 정의에서 [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 루트의 특성으로 선언할 수 있습니다. **x:Class**를 루트 노드 이외의 다른 요소에 선언하거나 XAML 파일이 **페이지** 빌드 작업과 컴파일되지 않은 경우에 선언하면 컴파일 시간 오류가 발생합니다.

**x:Class**로 사용되는 클래스는 중첩된 클래스가 될 수 없습니다.

**x:Class** 특성의 값은 클래스의 정규화된 이름을 지정하는 문자열이어야 합니다. 네임스페이스 정보를 생략할 수도 있는데 이는 코드 숨김에서도 구조가 동일한 경우, 즉 클래스 수준에서 클래스 정의가 시작되는 경우에만 가능합니다. 페이지 또는 응용 프로그램 정의에 대한 코드 숨김 파일은 프로젝트의 일부로 포함된 코드 파일에 들어 있어야 합니다. 코드 숨김 클래스는 public이어야 합니다. 코드 숨김 클래스는 partial이어야 합니다.

## <a name="clr-language-rules"></a>CLR 언어 규칙

코드 숨김 파일이 C++ 파일이어도 CLR 언어 폼을 계속 따르는 특정 규칙이 있어서 XAML 구문에서 차이가 없습니다. 특히 XAML과 연관된 C++ 코드 파일에서 네임스페이스와 클래스이름 사이의 구분 기호가 "::"일지라도 **x:Class** 값의 네임스페이스와 클래스 이름 구성 요소 사이의 구분 기호는 항상 점(".")입니다. C++에서 중첩 네임스페이스를 선언하는 경우 **x:Class** 값의 *namespace* 부분을 지정할 때 연속된 중첩 네임스페이스 문자열 사이의 구분 기호도 "::"이 아닌 "."이어야 합니다.

