---
Description: Long 형식의 연속 받아쓰기 음성 입력을 캡처하고 인식 하는 방법에 대해 알아봅니다.
title: 연속 받아쓰기 사용
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: 음성, 음성, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eff53ac21be290315ea020a820c718f69019d71d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172527"
---
# <a name="continuous-dictation"></a>연속 받아쓰기

Long 형식의 연속 받아쓰기 음성 입력을 캡처하고 인식 하는 방법에 대해 알아봅니다.

> **중요 한 api**: [**SpeechContinuousRecognitionSession**](/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession), [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession)

[음성 인식](speech-recognition.md)에서 [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 개체의 [**RecognizeAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) 또는 [**RecognizeWithUIAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) 메서드 (예: SMS (short message service) 메시지를 작성 하거나 질문을 할 때)를 사용 하 여 비교적 짧은 음성 입력을 캡처하고 인식 하는 방법을 배웠습니다.

더 긴 경우 받아쓰기 또는 전자 메일과 같은 연속 음성 인식 세션에서는 [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 의 [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) 속성을 사용 하 여 [**SpeechContinuousRecognitionSession**](/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession) 개체를 가져옵니다.

> [!NOTE]
> 받아쓰기 언어 지원은 앱이 실행 되는 [장치](../devices/index.md) 에 따라 다릅니다. Pc와 노트북의 경우 en-us만 인식 되 고 Xbox 및 휴대폰은 음성 인식에서 지원 되는 모든 언어를 인식할 수 있습니다. 자세한 내용은 [음성 인식기 언어 지정](specify-the-speech-recognizer-language.md)을 참조 하세요.

## <a name="set-up"></a>설정

연속 받아쓰기 세션을 관리 하려면 앱에 몇 개의 개체가 필요 합니다.

- [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 개체의 인스턴스입니다.
- 받아쓰기 중 UI를 업데이트 하기 위한 UI 디스패처에 대 한 참조입니다.
- 사용자가 말한 누적 단어를 추적 하는 방법입니다.

여기에서는 [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 인스턴스를 코드 숨김이 클래스의 전용 필드로 선언 합니다. 연속 받아쓰기를 단일 Extensible Application Markup Language (XAML) 페이지 이상으로 유지 하려면 앱은 다른 곳에 참조를 저장 해야 합니다.

```CSharp
private SpeechRecognizer speechRecognizer;
```

받아쓰기를 수행 하는 동안 인식기는 백그라운드 스레드에서 이벤트를 발생 시킵니다. 백그라운드 스레드는 XAML에서 UI를 직접 업데이트할 수 없으므로 앱에서 디스패처를 사용 하 여 인식 이벤트에 대 한 응답으로 UI를 업데이트 해야 합니다.

여기서는 나중에 UI 디스패처를 사용 하 여 초기화 되는 전용 필드를 선언 합니다.

```CSharp
// Speech events may originate from a thread other than the UI thread.
// Keep track of the UI thread dispatcher so that we can update the
// UI in a thread-safe manner.
private CoreDispatcher dispatcher;
```

사용자의 의견을 추적 하려면 음성 인식기에서 발생 하는 인식 이벤트를 처리 해야 합니다. 이러한 이벤트는 사용자 길이 발언의 청크에 대 한 인식 결과를 제공 합니다.

여기서는 [**StringBuilder**](/dotnet/api/system.text.stringbuilder) 개체를 사용 하 여 세션 중에 얻은 모든 인식 결과를 저장 합니다. 새 결과는 처리 될 때 **StringBuilder** 에 추가 됩니다.

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>초기화

연속 음성 인식을 초기화 하는 동안 다음을 수행 해야 합니다.

- 연속 인식 이벤트 처리기에서 응용 프로그램의 UI를 업데이트 하는 경우 UI 스레드에 대 한 디스패처를 인출 합니다.
- 음성 인식기를 초기화 합니다.
- 기본 제공 받아쓰기 문법을 컴파일합니다.
    **참고**    음성 인식은 인식할 수 있는 어휘를 정의 하는 하나 이상의 제약 조건이 필요 합니다. 제약 조건을 지정 하지 않으면 미리 정의 된 받아쓰기 문법이 사용 됩니다. [음성 인식](speech-recognition.md)을 참조 하세요.
- 인식 이벤트에 대 한 이벤트 수신기를 설정 합니다.

이 예제에서는 [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) page 이벤트에서 음성 인식을 초기화 합니다.

1. 음성 인식기에서 발생 하는 이벤트는 백그라운드 스레드에서 발생 하므로 UI 스레드에 대 한 업데이트에 대 한 디스패처 참조를 만듭니다. [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) 는 항상 UI 스레드에서 호출 됩니다.
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  그런 다음 [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 인스턴스를 초기화 합니다.
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  그런 다음 [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)에서 인식할 수 있는 모든 단어와 구를 정의 하는 문법을 추가 하 고 컴파일합니다.

    문법을 명시적으로 지정 하지 않으면 기본적으로 미리 정의 된 받아쓰기 문법이 사용 됩니다. 일반적으로 기본 문법이 일반 받아쓰기에 가장 적합합니다.

    여기서는 문법을 추가 하지 않고 즉시 [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) 를 호출 합니다.

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>인식 이벤트 처리

[**RecognizeAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) 또는 [**RecognizeWithUIAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync)를 호출 하 여 단일 brief utterance 또는 구를 캡처할 수 있습니다. 

그러나 더 긴 연속 인식 세션을 캡처하려면 사용자가 받아쓰기 문자열을 작성 하는 처리기를 지정 하 고 정의할 때 백그라운드에서 실행할 이벤트 수신기를 지정 합니다.

그런 다음 인식기의 [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) 속성을 사용 하 여 연속 인식 세션을 관리 하기 위한 메서드 및 이벤트를 제공 하는 [**SpeechContinuousRecognitionSession**](/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession) 개체를 가져옵니다.

특히 중요 한 두 가지 이벤트는 다음과 같습니다.

- 인식기에서 일부 결과를 생성할 때 발생 하는 [**Resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated)입니다.
- 연속 인식 세션이 종료 될 때 발생 하는 [**완료**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed)된입니다.

사용자가 말하는 대로 [**Resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 이벤트가 발생 합니다. 인식기는 지속적으로 사용자를 수신 하 고 음성 입력의 청크를 전달 하는 이벤트를 주기적으로 발생 시킵니다. 이벤트 인수의 [**Result**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) 속성을 사용 하 여 음성 입력을 검사 하 고, StringBuilder 개체에 텍스트를 추가 하는 것과 같이 이벤트 처리기에서 적절 한 작업을 수행 해야 합니다.

[**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult)의 인스턴스인 [**Result**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) 속성은 음성 입력을 수락할지 여부를 결정 하는 데 유용 합니다. [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 는 다음과 같은 두 가지 속성을 제공 합니다.

- [**상태**](/uwp/api/windows.media.speechrecognition.speechrecognitionresult.status) 인식이 성공 했는지 여부를 나타냅니다. 다양 한 이유로 인식이 실패할 수 있습니다.
- [**신뢰도**](/uwp/api/windows.media.speechrecognition.speechrecognitionresult.confidence) 는 인식기가 올바른 단어를 이해 했다는 상대적 신뢰도를 나타냅니다.

연속 인식을 지원 하기 위한 기본 단계는 다음과 같습니다.  

1. 여기서는 [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) page 이벤트에서 [**resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 연속 인식 이벤트에 대 한 처리기를 등록 합니다.
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  그런 다음 [**신뢰도**](/uwp/api/windows.media.speechrecognition.speechrecognitionresult.confidence) 속성을 확인 합니다. 신뢰도 값이 [**Medium**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionConfidence) 이상이 면 StringBuilder에 텍스트를 추가 합니다. 또한 입력을 수집할 때 UI를 업데이트 합니다.

    **참고**    [**Resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 이벤트는 UI를 직접 업데이트할 수 없는 백그라운드 스레드에서 발생 합니다. 처리기가 UI를 업데이트 해야 하는 경우 ( \[ 음성 및 TTS 샘플에서 \] ) 디스패처의 [**runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 메서드를 통해 ui 스레드에 대 한 업데이트를 발송 해야 합니다.
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

3.  그런 다음 연속 받아쓰기의 끝을 나타내는 [**Completed**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed) 이벤트를 처리 합니다.

    다음 섹션에 설명 된 [**Stopasync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.stopasync) 또는 [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) 메서드를 호출 하면 세션이 종료 됩니다. 오류가 발생 하거나 사용자가 말하기를 중지 한 경우에도 세션이 종료 될 수 있습니다. 이벤트 인수의 [**Status**](/uwp/api/windows.media.speechrecognition.speechrecognitionresult.status) 속성을 확인 하 여 세션이 종료 된 이유를 확인 합니다 ([**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)).

    여기에서는 [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) page 이벤트에서 [**완료**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed) 된 연속 인식 이벤트에 대 한 처리기를 등록 합니다.
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  이벤트 처리기는 Status 속성을 확인 하 여 인식이 성공 했는지 여부를 확인 합니다. 또한 사용자가 말하기를 중지 한 경우를 처리 합니다. 대부분의 경우에는 사용자가 말하기를 완료 했다는 것을 의미 하기 때문에 [**timeoutexceeded**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) 성공적으로 인식 된 것으로 간주 됩니다. 한층 뛰어난 환경을 구현하기 위해 코드에서 이런 경우를 처리해야 합니다.

    **참고**    [**Resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 이벤트는 UI를 직접 업데이트할 수 없는 백그라운드 스레드에서 발생 합니다. 처리기가 UI를 업데이트 해야 하는 경우 ( \[ 음성 및 TTS 샘플에서 \] ) 디스패처의 [**runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 메서드를 통해 ui 스레드에 대 한 업데이트를 발송 해야 합니다.
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

## <a name="provide-ongoing-recognition-feedback"></a>지속적인 인식 피드백 제공


사용자가 많은 경우에는 컨텍스트를 사용 하 여 무엇을 하는지 완전히 이해 하는 경우가 많습니다. 마찬가지로, 음성 인식기는 높은 신뢰도 인식 결과를 제공 하기 위해 컨텍스트가 필요할 수 있습니다. 예를 들어, "가중치" 및 "대기" 라는 단어는 주변 단어에서 더 많은 컨텍스트를 통해 수집할 수 있을 때까지 구별할 수 없습니다. 인식기가 단어 또는 단어가 올바르게 인식 되었다는 확신을 가질 때까지 [**Resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 이벤트를 발생 시 키 지 않습니다.

이로 인해 사용자가 계속 해 서 사용자에 게 적합 한 환경이 생성 될 수 있으며, 인식기가 [**Resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 이벤트를 발생 시킬 수 있을 정도로 충분히 높아질 때까지 결과가 제공 되지 않습니다.

[**HypothesisGenerated**](/uwp/api/windows.media.speechrecognition.speechrecognizer.hypothesisgenerated) 이벤트를 처리 하 여 이러한 명백한 응답성 부족을 향상 시킵니다. 인식기가 처리 중인 단어에 대 한 잠재적 일치 항목의 새 집합을 생성할 때마다 발생 하는 이벤트입니다. 이벤트 인수는 현재 일치 항목을 포함 하는 [**가설**](/uwp/api/windows.media.speechrecognition.speechrecognitionhypothesisgeneratedeventargs.hypothesis) 속성을 제공 합니다. 사용자가 계속 해 서 사용자에 게이를 표시 하 여 처리가 여전히 활성 상태를 reassure 합니다. 신뢰가 높고 인식 결과가 확인 되 면 중간 **가설** 결과를 [**resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 이벤트에 제공 된 최종 [**결과로**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) 바꿉니다.

여기서는 가상 텍스트와 줄임표 ("...")를 출력 [**텍스트 상자의**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)현재 값에 추가 합니다. 새 가설 생성 되 고 [**resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 이벤트에서 최종 결과가 반환 될 때까지 텍스트 상자의 내용이 업데이트 됩니다.

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


인식 세션을 시작 하기 전에 음성 인식기 [**상태**](/uwp/api/windows.media.speechrecognition.speechrecognizer.state) 속성의 값을 확인 합니다. 음성 인식기는 [**유휴**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerState) 상태 여야 합니다.

음성 인식기의 상태를 확인 한 후 음성 인식기의 [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) 속성의 [**StartAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.startasync) 메서드를 호출 하 여 세션을 시작 합니다.

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

인식은 다음과 같은 두 가지 방법으로 중지할 수 있습니다.

-   [**Stopasync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.stopasync) 는 보류 중인 모든 인식 이벤트를 완료할 수 있도록 합니다. ([**resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 는 보류 중인 모든 인식 작업이 완료 될 때까지 계속 발생 합니다).
-   [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) 는 인식 세션을 즉시 종료 하 고 보류 중인 결과를 모두 삭제 합니다.

음성 인식기의 상태를 확인 한 후 음성 인식기의 [**ContinuousRecognitionSession**](/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) 속성의 [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) 메서드를 호출 하 여 세션을 중지 합니다.

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> [**Resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 이벤트는 [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync)를 호출한 후에 발생할 수 있습니다.  
> 다중 스레딩을 통해 [**CancelAsync**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) 가 호출 될 때 [**resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 이벤트가 여전히 스택에 남아 있을 수 있습니다. 그렇다면 **Resultgenerated** 이벤트가 여전히 발생 합니다.  
> 인식 세션을 취소할 때 전용 필드를 설정 하는 경우 항상 [**Resultgenerated**](/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) 처리기에서 해당 값을 확인 합니다. 예를 들어, 세션을 취소할 때 null로 설정 하는 경우 처리기에서 필드를 초기화 한다고 가정 하지 마십시오.

 

## <a name="related-articles"></a>관련된 문서


* [음성 조작](speech-interactions.md)

**샘플**
* [음성 인식 및 음성 합성 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 