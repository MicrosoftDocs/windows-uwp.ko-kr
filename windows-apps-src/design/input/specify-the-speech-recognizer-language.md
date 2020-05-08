---
Description: 음성 인식에 사용할 설치 된 언어를 선택 하는 방법을 알아봅니다.
title: 음성 인식기 언어 지정
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: 음성, 음성, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9cd347b115a920c71ca1eb9b5f466adf05c69c64
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968248"
---
# <a name="specify-the-speech-recognizer-language"></a>음성 인식기 언어 지정


음성 인식에 사용할 설치 된 언어를 선택 하는 방법을 알아봅니다.

> **중요 한 api**: [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages), [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages), [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)


여기에서 시스템에 설치 된 언어를 열거 하 고, 기본 언어를 식별 하 고, 인식 하기 위해 다른 언어를 선택 합니다.

**사전 요구 사항:**

이 항목은 [음성 인식을](speech-recognition.md)기반으로 합니다.

음성 인식 및 인식 제약 조건에 대 한 기본적인 지식이 있어야 합니다.

Windows 앱 앱을 처음 개발 하는 경우 여기에 설명 된 기술에 익숙해질 수 있도록 다음 항목을 참조 하세요.

-   [첫 번째 앱 만들기](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
-   [이벤트 및 라우트된 이벤트](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview) 를 사용 하 여 이벤트에 대해 알아보기 개요

**사용자 환경 지침:**

유용 하 고 매력적인 음성 지원 앱을 디자인 하는 방법에 대 한 유용한 팁은 [음성 디자인 지침](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions) 을 참조 하세요.

## <a name="identify-the-default-language"></a>기본 언어 식별


음성 인식기는 시스템 음성 언어를 기본 인식 언어로 사용 합니다. 이 언어는 장치 설정 &gt; 시스템 &gt; 음성 &gt; 음성 언어 화면에서 사용자가 설정 합니다.

[**SystemSpeechLanguage**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage) 정적 속성을 확인 하 여 기본 언어를 식별 합니다.

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>설치 된 언어 확인


설치 된 언어는 장치 마다 다를 수 있습니다. 특정 제약 조건에 종속 된 언어가 있는지 확인 해야 합니다.

**참고**  새 언어 팩을 설치한 후에는 다시 부팅 해야 합니다. 지정 된 언어가 지원 되지 않거나 설치가\_완료\_되지 않은 경우 오류 코드 sperr을 찾을 수 없는 예외가 발생 합니다 (0x8004503a).

 

[**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 클래스의 두 정적 속성 중 하나를 확인 하 여 장치에서 지원 되는 언어를 확인 합니다.

-   [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)-미리 정의 된 받아쓰기 및 웹 검색 문법에 사용 되는 [**언어**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) 개체의 컬렉션입니다.

-   [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)— 목록 제약 조건 또는 SRGS (음성 인식 문법 사양) 파일에 사용 되는 [**언어**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) 개체의 컬렉션입니다.

## <a name="specify-a-language"></a>언어 지정


언어를 지정 하려면 [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 생성자에서 [**언어**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) 개체를 전달 합니다.

여기서는 "en-us"를 인식 언어로 지정 합니다.


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>설명


[**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 의 [**제약 조건**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) 컬렉션에 [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) 를 추가 하 고 [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)를 호출 하 여 토픽 제약 조건을 구성할 수 있습니다. 지원 되는 토픽 언어로 인식기가 초기화 되지 않으면 [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) 의 **TopicLanguageNotSupported** 이 반환 됩니다.

[**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 의 [**제약 조건**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) 컬렉션에 [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) 를 추가 하 고 [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)를 호출 하 여 목록 제약 조건을 구성 합니다. 사용자 지정 목록의 언어는 직접 지정할 수 없습니다. 대신 목록은 인식기의 언어를 사용하여 처리됩니다.

SRGS 문법은 [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint) 클래스가 나타내는 오픈 표준 XML 형식입니다. 사용자 지정 목록과 달리 SRGS 태그에서 문법의 언어를 지정할 수 있습니다. 인식기가 SRGS 태그와 동일한 언어로 초기화 되지 않은 경우 [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) 는 [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) 의 **TopicLanguageNotSupported** 와 함께 실패 합니다.

## <a name="related-articles"></a>관련된 문서

**개발자**

* [음성 조작](speech-interactions.md)

**디자이너**

* [음성 디자인 지침](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)

**샘플**

* [음성 인식 및 음성 합성 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 




