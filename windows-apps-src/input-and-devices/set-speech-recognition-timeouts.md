---
author: Karl-Bridge-Microsoft
Description: "음성 인식기에서 무음 또는 인식할 수 없는 소리(왁자지껄)를 무시하고 계속해서 음성 입력에 대해 수신 대기하는 시간을 설정합니다."
title: "음성 인식 시간 제한 설정"
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: "음성 명령, 목소리, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 93314c29de926b988d01da09a6ce7feb21fdcf20
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-speech-recognition-timeouts"></a>음성 인식 시간 제한 설정
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

음성 인식기에서 무음 또는 인식할 수 없는 소리(왁자지껄)를 무시하고 계속해서 음성 입력에 대해 수신 대기하는 시간을 설정합니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**시간 제한**](https://msdn.microsoft.com/library/windows/apps/dn653253)</li>
<li>[**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)</li>
</ul>
</div>

## <a name="set-a-timeout"></a>시간 제한 설정


여기에서는 다양한 [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253) 값을 지정합니다.

-   InitialSilenceTimeout - SpeechRecognizer가 (인식 결과가 생성되기 전에) 침묵을 감지하고 음성 입력이 들어오지 않는다는 것을 가정하는 시간입니다.
-   BabbleTimeout - SpeechRecognizer가 음성 입력이 종료되었다고 가정하고 인식 작업을 종료하기 전에 인식 불가능한 소리(왁자지껄)를 계속 수신하는 시간입니다.
-   EndSilenceTimeout - SpeechRecognizer가 (인식 결과가 생성된 후에) 침묵을 감지하고 음성 입력이 종료되었다고 가정하는 시간입니다.

**참고**  시간 제한은 인식기별로 설정할 수 있습니다.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>관련 문서


* [음성 조작](speech-interactions.md)
**샘플**
* [음성 인식 및 음성 합성 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




