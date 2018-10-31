---
author: Karl-Bridge-Microsoft
Description: Learn how to capture and recognize long-form, continuous dictation speech input.
title: 연속 받아쓰기 사용
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: 음성 명령, 음성, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ea7c0b92c5900e468023dd5b972942a89c2833c3
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5861825"
---
# <a name="continuous-dictation"></a>연속 받아쓰기

긴 형식의 연속 받아쓰기 음성 입력을 캡처 및 인식하는 방법을 알아봅니다.

> **중요 API**: [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896), [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)

[음성 인식](speech-recognition.md)에서 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653244) 개체의 [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 또는 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653226) 메서드를 사용하여 상대적으로 짧은 음성 입력을 캡처 및 인식하는 방법을 알아보았습니다. 예를 들어 SMS(Short Message Service) 메시지를 작성하거나 질문을 할 경우입니다.

더 긴, 연속 음성 인식 세션(예: 세션 받아쓰기 또는 메일)의 경우 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn913913)의 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn653226) 속성을 사용하여 [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896) 개체를 가져오세요.

> [!NOTE]
> 받아쓰기 언어 지원 앱 실행 중인 [장치](https://docs.microsoft.com/windows/uwp/design/devices/) 에 따라 달라 집니다. Pc 및 노트북에 대 한만 EN-US 인식 되, Xbox, 휴대폰 음성 인식에서 지 원하는 모든 언어를 인식 하는 동안 됩니다. 자세한 내용은 [음성 인식기 언어 지정](specify-the-speech-recognizer-language.md)을 참조 하세요.

## <a name="set-up"></a>설정

연속 받아쓰기 세션을 관리하려면 몇 가지 개체가 앱에 필요합니다.

- [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 개체의 인스턴스
- 받아쓰는 동안 UI를 업데이트하기 위한 UI 디스패처 참조
- 사용자가 말한 누적된 단어를 추적하는 방법

여기서는 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 인스턴스를 코드 숨김 클래스의 전용 필드로 선언합니다. 단일 XAML(Extensible Application Markup Language) 페이지 이후에도 연속 받아쓰기를 유지하려는 경우 앱이 다른 곳에 참조를 저장해야 합니다.

```CSharp
private SpeechRecognizer speechRecognizer;
```

받아쓰기를 하는 동안 인식기는 백그라운드 스레드에서 이벤트를 발생시킵니다. 백그라운드 스레드가 XAML에서 UI를 직접 업데이트할 수 없으므로 앱은 디스패처를 사용하여 인식 이벤트에 대한 응답으로 UI를 업데이트해야 합니다.

여기서는 UI 디스패처를 통해 나중에 초기화되는 전용 필드를 선언합니다.

```CSharp
// Speech events may originate from a thread other than the UI thread.
// Keep track of the UI thread dispatcher so that we can update the
// UI in a thread-safe manner.
private CoreDispatcher dispatcher;
```

사용자가 말하는 내용을 추적하려면 음성 인식기에서 발생한 인식 이벤트를 처리해야 합니다. 이러한 이벤트는 사용자 말하기의 청크에 대한 인식 결과를 제공합니다.

여기서는 [**StringBuilder**](https://msdn.microsoft.com/library/system.text.stringbuilder.aspx) 개체를 사용하여 세션 중에 가져온 모든 인식 결과를 저장합니다. 새로운 결과는 처리될 때 **StringBuilder**에 추가됩니다.

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>초기화

연속 음성 인식을 초기화하는 동안 다음 작업을 수행해야 합니다.

- 연속 인식 이벤트 처리기에서 앱의 UI를 업데이트하는 경우 UI 스레드 디스패처를 가져옵니다.
- 음성 인식기를 초기화합니다.
- 기본 제공 받아쓰기 문법을 컴파일합니다.
    **참고**  음성 인식에 인식할 수 있는 어휘를 정의 하려면 이상의 제약 조건이 필요 합니다. 제약 조건을 지정하지 않으면 미리 정의된 받아쓰기 문법이 사용됩니다. [음성 인식](speech-recognition.md)을 참조하세요.
- 인식 이벤트에 대한 이벤트 수신기를 설정합니다.

이 예제에서는 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 페이지 이벤트에서 음성 인식을 초기화합니다.

1. 음성 인식기에 의해 발생한 이벤트가 백그라운드 스레드에서 발생하므로 UI 스레드 업데이트를 위해 디스패처에 대한 참조를 만듭니다. [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508)는 항상 UI 스레드에서 호출됩니다.
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  그런 다음 [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 인스턴스를 초기화합니다.
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)에서 인식할 수 있는 모든 단어와 구를 정의하는 문법을 추가하고 컴파일합니다.

    문법을 명시적으로 지정하지 않으면 미리 정의된 받아쓰기 문법이 기본적으로 사용됩니다. 일반적으로 기본 문법이 일반 받아쓰기에 가장 적합합니다.

    여기서는 문법을 추가하지 않고 즉시 [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240)를 호출합니다.

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>인식 이벤트 처리

[**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) 또는 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245)를 호출하여 짧은 한 마디 말이나 구를 캡처할 수 있습니다. 

그러나 보다 길고 연속적인 인식 세션을 캡처하기 위해 사용자가 말할 때 백그라운드에서 실행하는 이벤트 수신기를 지정하고 받아쓰기 문자열을 빌드하는 처리기를 정의합니다.

그런 다음 인식기의 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) 속성을 사용하여 연속 인식 세션을 관리하기 위한 메서드 및 이벤트를 제공하는 [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896) 개체를 가져옵니다.

특히 두 가지 이벤트가 중요합니다.

- [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) - 인식기에서 몇 가지 결과를 생성할 때 발생합니다.
- [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899) - 연속 인식 세션이 종료 될 때 발생합니다.

[**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 이벤트는 사용자가 말할 때 발생합니다. 인식기는 지속적으로 사용자의 말을 수신 대기하고 음성 입력의 청크를 전달하는 이벤트를 주기적으로 발생시킵니다. 이벤트 인수의 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) 속성을 사용하여 음성 입력을 검사하고, 이벤트 처리기에서 적절한 작업(예: StringBuilder 개체에 텍스트 추가)을 수행해야 합니다.

[**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432)의 인스턴스인 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) 속성은 음성 입력을 허용할 것인지 여부를 확인하는 데 유용합니다. [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432)는 이를 위해 다음과 같은 두 가지 속성을 제공합니다.

- [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440)는 인식에 성공했는지 여부를 나타냅니다. 인식은 다양한 이유로 실패할 수 있습니다.
- [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434)는 인식기에서 올바른 단어를 이해한 상대적인 신뢰도를 나타냅니다.

연속적인 인식을 지원하기 위한 기본 단계는 다음과 같습니다.  

1. 여기서는 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/dn913900) 페이지 이벤트에서 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/br227508) 연속 인식 이벤트에 대한 처리기를 등록합니다.
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  그런 다음 [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) 속성을 확인합니다. 신뢰도 값이 [**Medium**](https://msdn.microsoft.com/library/windows/apps/dn631409) 이상인 경우 StringBuilder에 텍스트를 추가합니다. 또한 입력을 수집할 때 UI를 업데이트합니다.

    **참고** [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 이벤트가 백그라운드 스레드에서 UI를 직접 업데이트할 수 없습니다. 처리기가 UI를 업데이트해야 하는 경우(\[음성 및 TTS 샘플\]에서와 마찬가지로) 디스패처의 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 메서드를 통해 UI 스레드 업데이트를 디스패치해야 합니다.
```csharp
private async void ContinuousRecognitionSession_ResultGenerated(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionResultGeneratedEventArgs args)
      {

        if (args.Result.Confidence == SpeechRecognitionConfidence.Medium ||
          args.Result.Confidence == SpeechRecognitionConfidence.High)
          {
            dictatedTextBuilder.Append(args.Result.Text + " ");

            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
              btnClearText.IsEnabled = true;
            });
          }
        else
        {
          await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
        }
      }
```

3.  그런 다음 연속 받아쓰기의 끝을 나타내는 [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899) 이벤트를 처리합니다.

    이 세션은 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) 또는 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) 메서드(다음 섹션에 설명된)를 호출할 때 종료됩니다. 또한 이 세션은 오류가 발생하거나 사용자가 말하기를 중지하는 경우에 종료될 수 있습니다. 이벤트 인수의 [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) 속성을 확인하여 세션이 종료된 이유를 확인합니다([**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)).

    여기서는 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/dn913899) 페이지 이벤트에서 [**Completed**](https://msdn.microsoft.com/library/windows/apps/br227508) 연속 인식 이벤트에 대한 처리기를 등록합니다.
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  이벤트 처리기는 Status 속성을 확인하여 인식에 성공했는지 여부를 알아봅니다. 또한 사용자가 말하기를 중지하는 경우도 처리합니다. [**TimeoutExceeded**](https://msdn.microsoft.com/library/windows/apps/dn631433)는 사용자가 말하기를 마친 것을 의미하므로, 대개 인식에 성공한 것으로 간주됩니다. 한층 뛰어난 환경을 구현하기 위해 코드에서 이런 경우를 처리해야 합니다.

    **참고** [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 이벤트가 백그라운드 스레드에서 UI를 직접 업데이트할 수 없습니다. 처리기가 UI를 업데이트해야 하는 경우(\[음성 및 TTS 샘플\]에서와 마찬가지로) 디스패처의 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 메서드를 통해 UI 스레드 업데이트를 디스패치해야 합니다.
```csharp
private async void ContinuousRecognitionSession_Completed(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionCompletedEventArgs args)
      {
        if (args.Status != SpeechRecognitionResultStatus.Success)
        {
          if (args.Status == SpeechRecognitionResultStatus.TimeoutExceeded)
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Automatic Time Out of Dictation",
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
          }
          else
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Continuous Recognition Completed: " + args.Status.ToString(),
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
            });
          }
        }
      }
```

## <a name="provide-ongoing-recognition-feedback"></a>진행 중인 인식 피드백 제공


사람들이 말할 때 말하는 내용을 완전히 이해하기 위해 보통 컨텍스트를 사용합니다. 마찬가지로, 음성 인식기의 경우에도 높은 신뢰도의 인식 결과를 제공하기 위해 컨텍스트가 필요합니다. 예를 들어 "무게" 및 "대기" 단어는 주변 단어에서 더 많은 컨텍스트를 얻을 때까지 그 자체로 구별할 수 없습니다. 인식기에서 단어가 올바르게 인식되었다는 신뢰도가 어느 수준에 도달할 때까지 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 이벤트를 발생시키지 않습니다.

따라서 사용자가 계속 말할 때에는 그다지 좋지 않은 환경이 구현되며 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 이벤트를 발생시킬 수 있을 정도로 인식기의 신뢰도가 높아질 때까지 어떤 결과도 제공되지 않습니다.

[**HypothesisGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913914) 이벤트를 처리하여 이처럼 명백한 응답성 부족을 개선합니다. 이 이벤트는 인식기가 처리 중인 단어와 일치할 수 있는 새 집합을 생성할 때마다 발생합니다. 이벤트 인수는 현재 일치 항목을 포함하는 [**Hypothesis**](https://msdn.microsoft.com/library/windows/apps/dn913911) 속성을 제공합니다. 사용자가 계속 말할 때 해당 속성을 표시하여 처리가 진행되고 있음을 알려 줍니다. 신뢰도가 높고 인식 결과가 결정되면 임시 **Hypothesis** 결과를 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913895) 이벤트에 제공된 최종 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913900)로 바꿉니다.

여기서는 출력 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)의 현재 값에 가설 텍스트 및 줄임표("...")를 추가합니다. 새로운 가설이 생성될 때 그리고 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 이벤트에서 최종 결과를 가져올 때까지 텍스트 상자의 내용이 업데이트됩니다.

```CSharp
private async void SpeechRecognizer_HypothesisGenerated(
  SpeechRecognizer sender,
  SpeechRecognitionHypothesisGeneratedEventArgs args)
  {

    string hypothesis = args.Hypothesis.Text;
    string textboxContent = dictatedTextBuilder.ToString() + " " + hypothesis + " ...";

    await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
      dictationTextBox.Text = textboxContent;
      btnClearText.IsEnabled = true;
    });
  }
```

## <a name="start-and-stop-recognition"></a>인식 시작 및 중지


인식 세션을 시작하기 전에 음성 인식기 [**State**](https://msdn.microsoft.com/library/windows/apps/dn913915) 속성의 값을 확인합니다. 음성 인식기는 [**Idle**](https://msdn.microsoft.com/library/windows/apps/dn653227) 상태여야 합니다.

음성 인식기의 상태를 확인한 후 음성 인식기의 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913901) 속성에 대한 [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn913913) 메서드를 호출하여 세션을 시작합니다.

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

다음 두 가지 방법으로 인식을 중지할 수 있습니다.

-   [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908)를 통해 보류 중인 모든 인식 이벤트가 완료됩니다. 보류 중인 모든 인식 작업이 완료될 때까지 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)가 계속 발생합니다.
-   [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898)에서 즉시 인식 세션을 종료하고 보류 중인 모든 결과를 삭제합니다.

음성 인식기의 상태를 확인한 후 음성 인식기의 [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913898) 속성에 대한 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913913) 메서드를 호출하여 세션을 중지합니다.

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 이벤트는 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) 호출 후에 발생할 수 있습니다.  
> 다중 스레딩 때문에 [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913900)가 호출될 때 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913898) 이벤트가 계속 스택에 남아 있을 수 있습니다. 그런 경우 **ResultGenerated** 이벤트가 계속 발생합니다.  
> 인식 세션을 취소할 때 전용 필드를 설정한 경우 [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) 처리기에서 해당 값을 항상 확인하세요. 예를 들어 세션을 취소할 때 해당 값을 null로 설정한 경우 필드가 처리기에서 초기화된다고 가정하지 마세요.

 

## <a name="related-articles"></a>관련 문서


* [음성 조작](speech-interactions.md)

**샘플**
* [음성 인식 및 음성 합성 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




