---
author: Karl-Bridge-Microsoft
Description: "음성 인식기에서 무음 또는 인식할 수 없는 소리(왁자지껄)를 무시하고 계속해서 음성 입력에 대해 수신 대기하는 시간을 설정합니다."
title: "음성 인식 시간 제한 설정"
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 99ead02e8886ae79b8e8423ac0f78609b4fa1f6f

---

# 음성 인식 시간 제한 설정
음성 인식기에서 무음 또는 인식할 수 없는 소리(왁자지껄)를 무시하고 계속해서 음성 입력에 대해 수신 대기하는 시간을 설정합니다.

**중요 API**

-   [**시간 제한**](https://msdn.microsoft.com/library/windows/apps/dn653253)
-   [**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)


## 시간 제한 설정


여기에서는 다양한 [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253) 값을 지정합니다.

-   InitialSilenceTimeout - SpeechRecognizer가 (인식 결과가 생성되기 전에) 침묵을 감지하고 음성 입력이 들어오지 않는다는 것을 가정하는 시간입니다.
-   BabbleTimeout - SpeechRecognizer가 음성 입력이 종료되었다고 가정하고 인식 작업을 종료하기 전에 인식 불가능한 소리(왁자지껄)를 계속 수신하는 시간입니다.
-   EndSilenceTimeout - SpeechRecognizer가 (인식 결과가 생성된 후에) 침묵을 감지하고 음성 입력이 종료되었다고 가정하는 시간입니다.

**참고** 시간 제한은 인식기별로 설정할 수 있습니다.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## 관련 문서


* [음성 조작](speech-interactions.md) 
           **샘플**
* [음성 인식 및 음성 합성 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Jun16_HO5-->


