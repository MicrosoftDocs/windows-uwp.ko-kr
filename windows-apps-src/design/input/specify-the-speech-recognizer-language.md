---
author: Karl-Bridge-Microsoft
Description: Learn how to select an installed language to use for speech recognition.
title: 음성 인식기 언어 지정
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: 음성 명령, 목소리, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7e042a9bbedee3ded0601eda06da8e349c4b788c
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6835243"
---
# <a name="specify-the-speech-recognizer-language"></a>음성 인식기 언어 지정


음성 인식에 사용하기 위해 설치된 언어를 선택하는 방법에 대해 알아봅니다.

> **중요 API**: [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251), [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250), [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)


여기에서는 시스템에 설치된 언어를 열거하고 기본 언어를 확인하고 인식에 다른 언어를 선택합니다.

**필수 조건:**

이 항목은 [음성 인식](speech-recognition.md)을 기반으로 합니다.

음성 인식 및 인식 제약 조건에 대한 기본적인 지식이 있어야 합니다.

UWP(유니버설 Windows 플랫폼) 앱을 처음 개발하는 경우 다음 항목을 검토하여 여기서 설명하는 기술에 대해 알아보세요.

-   [첫 번째 앱 만들기](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   이벤트에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584)를 참조하세요.

**사용자 환경 지침:**

유용하고 매력적인 음성 사용 앱 디자인에 도움이 되는 팁은 [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)을 참조하세요.

## <a name="identify-the-default-language"></a>기본 언어 확인


음성 인식기는 해당 기본 인식 언어로 시스템 음성 언어를 사용합니다. 이 언어는 사용자가 장치의 설정 &gt; 시스템 &gt; 음성 &gt; 음성 언어 화면에서 설정합니다.

[**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252) 정적 속성을 확인하여 기본 언어를 식별합니다.

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>설치된 언어를 확인합니다.


설치된 언어는 장치마다 다를 수 있습니다. 특정 제약 조건 때문에 특정 언어에 의존하는 경우 해당 언어가 있는지 확인해야 합니다.

**참고**새 언어 팩을 설치한 후 부팅이 필요 합니다. 지정된 언어가 지원되지 않거나 설치가 완료되지 않은 경우 오류 코드 SPERR\_NOT\_FOUND(0x8004503a)의 예외가 발생합니다.

 

[**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 클래스의 다음 두 정적 속성 중 하나를 확인하여 장치에서 지원되는 언어를 확인합니다.

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251) - 미리 정의된 받아쓰기 및 웹 검색 문법에서 사용되는 [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 개체의 컬렉션

-   [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250) - 목록 제약 조건 또는 SRGS(Speech Recognition Grammar Specification) 파일에서 사용되는 [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 개체의 컬렉션

## <a name="specify-a-language"></a>언어 지정


언어를 지정하려면 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/br206804) 생성자의 [**Language**](https://msdn.microsoft.com/library/windows/apps/dn653226) 개체를 전달합니다.

여기에서는 인식 언어로 "en-US"를 지정합니다.


```CSharp
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>설명


항목 제약 조건은 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn631446)의 [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) 컬렉션에 [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653226)를 추가한 후 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240)를 호출하여 구성할 수 있습니다. 인식기가 지원되는 항목 언어와 함께 초기화되지 않은 경우 **TopicLanguageNotSupported**의 [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)가 반환됩니다.

목록 제약 조건은 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn631421)의 [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) 컬렉션에 [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653226)를 추가한 후 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240)를 호출하여 구성합니다. 사용자 지정 목록의 언어를 직접 지정할 수 없습니다. 대신 목록은 인식기의 언어를 사용하여 처리됩니다.

SRGS 문법은 [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412) 클래스에서 나타내는 개방형 표준 XML 형식입니다. 사용자 지정 목록과 달리 SRGS 태그에서 문법의 언어를 지정할 수 있습니다. 인식기가 SRGS 태그와 동일한 언어로 초기화되지 않은 경우 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240)는 **TopicLanguageNotSupported**의 [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)로 실패합니다.

## <a name="related-articles"></a>관련 문서

**개발자**

* [음성 조작](speech-interactions.md)

**디자이너**

* [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)

**샘플**

* [음성 인식 및 음성 합성 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




