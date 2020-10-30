---
description: 음성 인식기에서 침묵 또는 인식할 때 소리를 무시 하는 기간 (babble)을 설정 하 고 음성 입력 수신 대기를 계속 합니다.
title: 음성 인식 시간 제한 설정
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: 음성, 음성, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 78c351941b1b6703c28f249afcd119cf267f3d0a
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031356"
---
# <a name="set-speech-recognition-timeouts"></a>음성 인식 시간 제한 설정


음성 인식기에서 침묵 또는 인식할 때 소리를 무시 하는 기간 (babble)을 설정 하 고 음성 입력 수신 대기를 계속 합니다.

> **중요 한 api** : [**시간 제한**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts), [**SpeechRecognizerTimeouts**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerTimeouts)

## <a name="set-a-timeout"></a>시간 제한 설정


여기서는 다양 한 [**시간 제한**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts) 값을 지정 합니다.

-   InitialSilenceTimeout-인식 결과를 생성 하기 전에 SpeechRecognizer에서 침묵를 검색 하 고 음성 입력을 사용할 수 없다고 가정 하는 시간입니다.
-   BabbleTimeout-음성 입력이 종료 된 것으로 가정 하 고 인식 작업을 마무리 하기 전에 SpeechRecognizer가 인식할 수 없는 소리 (babble)를 계속 수신 대기 하는 시간입니다.
-   EndSilenceTimeout-SpeechRecognizer에서 소리를 검색 하 고 (인식 결과가 생성 된 후) 음성 입력이 종료 된 것으로 가정 하는 시간입니다.

**참고**  시간 제한은 인식기 별로 설정할 수 있습니다.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>관련된 문서

* [음성 조작](speech-interactions.md)

**샘플**

* [음성 인식 및 음성 합성 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
