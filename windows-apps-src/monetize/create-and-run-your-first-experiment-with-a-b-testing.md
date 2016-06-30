---
author: mcleanbyron
Description: "이 연습에서는 A/B 테스트로 첫 번째 실험을 만들고 실행합니다."
title: "A/B 테스트로 첫 번째 실험 만들기 및 실행"
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 0f889c22b8999408341f4c12387602b344f49439

---

# A/B 테스트로 첫 번째 실험 만들기 및 실행

이 연습에서는 다음을 수행합니다.
* Windows 개발자 센터 대시보드에서 앱 단추의 배경색 변경이 단추 클릭 횟수를 성공적으로 증가시키는지 여부를 테스트하는 실험을 만듭니다.
* 개발자 센터에서 변형 설정을 검색하고, 이 데이터를 사용하여 단추의 배경색을 변경하고, 보기 및 전환 이벤트를 개발자 센터에 기록하는 앱을 만듭니다.
* 실험 데이터를 수집하는 앱을 실행합니다.
* 개발자 센터 대시보드에서 실험 결과를 검토하고, 앱의 모든 사용자가 사용할 수 있도록 설정할 변형을 선택하고, 실험을 완료합니다.

개발자 센터를 사용하여 A/B 테스트의 개요를 보려면 [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)을 참조하세요.

## 필수 조건

이 연습을 수행하려면 Windows 개발자 센터 계정이 있어야 하며 [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)에서 설명한 대로 개발 컴퓨터를 구성해야 합니다.

## Windows 개발자 센터에서 실험 만들기

1. [개발자 센터 대시보드](https://dev.windows.com/overview)에 로그인합니다.
2. 개발자 센터에 실험을 만드는 데 사용할 앱이 이미 있는 경우 **앱**에서 앱을 선택하세요. 대시보드에 아직 앱이 없는 경우 [이름을 예약하여 새 앱을 만든](../publish/create-your-app-by-reserving-a-name.md) 다음 대시보드에서 해당 앱을 선택합니다.
3. 탐색 창에서 **서비스**를 클릭한 다음 **실험**을 클릭합니다.
4. **API 키** 섹션에서 새 API 키를 생성하는 **새 API 키**를 선택하고 API 키의 이름을 **My First Experiment**로 입력합니다. 이 연습의 다음 섹션에서 이 API 키를 사용합니다.
5. **실험** 섹션에서 **새 실험**을 클릭합니다. **실험 이름** 필드에서 이름을 **Optimize Button Clicks**로 입력합니다.
6. **보기 이벤트 이름** 필드에서 이름을 **userViewedButton**으로 입력합니다. 이 연습의 뒷부분에서는 앱의 기본 페이지가 초기화되고 단추가 사용자에게 표시되는 경우 이 보기 이벤트를 기록하는 코드를 추가합니다.
7. **목표 및 전환 이벤트** 섹션에서 다음 값을 입력합니다.
  * **목표 이름** 필드에서 **Increase Button Clicks**를 입력합니다.
  * **전환 이벤트 이름** 필드에서 이름 **userClickedButton**을 입력합니다. 이 연습의 뒷부분에서는 단추의 클릭 이벤트 처리기에 이 전환 이벤트를 기록하는 코드를 추가합니다.
  * **목표** 필드에서 **최대화**를 선택합니다.
8. **변형 및 설정** 섹션에서 **설정 추가**를 세 번 클릭합니다. 이제 네 개의 빈 설정 행이 표시됩니다.
  * 첫 번째 행에서 설정 이름을 **buttonText**로 입력하고, **변형 A** 열에 **Grey Button**을 입력하고, **변형 B** 열에 **Blue Button**을 입력합니다.
  * 두 번째 행에서 설정 이름을 **r**로 입력하고, **변형 A** 열에 **128**을 입력하고, **변형 B** 열에 **1**을 입력합니다.
  * 세 번째 행에서 설정 이름을 **g**로 입력하고, **변형 A** 열에 **128**을 입력하고, **변형 B** 열에 **1**을 입력합니다.
  * 네 번째 행에서 설정 이름을 **b**로 입력하고, **변형 A** 열에 **128**을 입력하고, **변형 B** 열에 **255**를 입력합니다.  
9. 변형이 앱에 고르게 배포되도록 **고르게 분배** 확인란이 선택되어 있는지 확인합니다.
10. **저장**을 클릭한 다음 **활성화**를 클릭합니다.

> **중요** 실험을 활성화한 후 테스트 실험이 아닌 경우(실험을 만들 때 **테스트 실험** 확인란을 클릭하지 않은 경우) 더 이상 실험 매개 변수를 수정할 수 없습니다. 일반적으로 실험을 활성화하기 전에 앱에서 실험을 코딩하는 것이 좋습니다. 편의상 이 연습에서는 지금 실험을 활성화할 수 있습니다.

## 앱에서 실험 코딩

1. Visual Studio 2015에서 Visual C#을 사용하여 새 유니버설 Windows 플랫폼 프로젝트를 만듭니다. 프로젝트 이름을 **SampleExperiment**로 지정합니다.
2. 솔루션 탐색기에서 프로젝트 노드를 확장하여 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.
3. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
4. SDK 목록에서 **Microsoft 스토어 참여 SDK** 옆의 확인란을 선택하고 **확인**을 클릭합니다.
5. **솔루션 탐색기**에서 MainPage.xaml을 두 번 클릭하여 앱에서 기본 페이지에 대한 디자이너를 엽니다.
6. **도구 상자**에서 페이지로 **단추**를 끌어다 놓습니다.
7. 디자이너에서 단추를 두 번 클릭하여 코드 파일을 열고 **Click** 이벤트에 대한 이벤트 처리기를 추가합니다.  
8. 코드 파일의 전체 내용을 다음 코드로 바꿉니다.

  ```CSharp
  using System;
  using Windows.UI.Xaml;
  using Windows.UI.Xaml.Controls;
  using Windows.UI.Xaml.Media;
  using System.Threading.Tasks;
  using Windows.UI;
  using Windows.UI.Core;

  // Namespace for A/B testing.
  using Microsoft.Services.Store.Engagement;

  namespace SampleExperiment
  {  
    public sealed partial class MainPage : Page
    {
        private readonly ExperimentClient experiment;
        private ExperimentVariation variation;

        // Assign this variable to your API key from Dev Center. The API key
        // shown below is for example purposes only.
        private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";    

        public MainPage()
        {
            this.InitializeComponent();

            // Initialize the ExperimentClient for A/B testing.
            experiment = new ExperimentClient(apiKey);

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
            #pragma warning disable CS4014
            Initialize();
            #pragma warning restore CS4014
        }

        private async Task Initialize()
        {
            // Get the current cached variation assignment for the experiment.
            ExperimentVariationResult result = await experiment.GetVariationAsync();
            variation = result.Variation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
            {
                result = await experiment.RefreshVariationAsync();

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == EngagementErrorCode.Success)
                {
                    variation = result.Variation;
                }
            }

            // Get settings named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the settings default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInteger("r", 128);
            var g = (byte)variation.GetInteger("g", 128);
            var b = (byte)variation.GetInteger("b", 128);

            // Assign button text and color.
            await button.Dispatcher.RunAsync(
                CoreDispatcherPriority.Normal,
                () =>
                {
                    button.Background = new SolidColorBrush(Color.FromArgb(255, r, g, b));
                    button.Content = buttonText;
                    button.Visibility = Visibility.Visible;
                });

            // Log the view event named "userViewedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userViewedButton", variation);
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userClickedButton", variation);
        }
     }
  }
  ```
9. 다음 코드 줄에서 *apiKey* 변수를 이전 섹션에서 개발자 센터 대시보드로부터 얻은 API 키에 할당합니다. 아래에 표시되는 API 키는 예제용으로만 사용됩니다.
```CSharp
private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";
```
10. 코드 파일을 저장하고 프로젝트를 빌드합니다.

## 실험 데이터를 수집하는 앱 실행

1. 이전 섹션에서 만든 **SampleExperiment** 앱을 실행합니다.
2. 회색이나 파란색 단추가 표시되는지 확인합니다. 단추를 클릭한 다음 앱을 닫습니다.
3. 동일한 컴퓨터에서 위의 단계를 여러 번 반복하여 앱에 동일한 단추 색이 표시되는지 확인합니다.

## 결과 검토 및 실험 완료

이전 섹션을 완료한 후 몇 시간 동안 기다린 후 다음 단계를 수행하여 실험의 결과를 검토하고 실험을 완료합니다.

> **참고** 실험을 활성화하자마자 개발자 센터에서 즉시 실험에 대한 데이터를 기록하도록 계측되는 모든 앱에서 데이터를 수집하기 시작합니다. 그러나 실험 데이터를 대시보드에 표시하려면 몇 시간이 걸릴 수 있습니다.

1. 개발자 센터에서 앱의 **실험** 페이지로 돌아갑니다.
2. **실험** 섹션에서 **활성** 필터를 클릭한 다음 **단추 클릭 최적화**를 클릭하여 이 실험 페이지로 이동합니다.
3. **결과 요약** 및 **결과 세부 정보** 섹션에 표시되는 결과가 예상과 일치하는지 확인합니다. 이러한 섹션에 대한 자세한 내용은 [개발자 센터 대시보드에서 실험 관리](manage-your-experiment.md#review-the-results-of-your-experiment)를 참조하세요.

  >**참고** 개발자 센터에서는 24시간 내의 각 사용자에 대한 첫 번째 전환 이벤트만 보고합니다. 사용자가 24시간 내에 앱에서 여러 전환 이벤트를 트리거하는 경우 첫 번째 전환 이벤트만 보고됩니다. 이렇게 하면 많은 전환 이벤트를 트리거하는 단일 사용자가 샘플 그룹 사용자의 실험 결과를 왜곡시키지 않을 수 있습니다.

4. 이제 실험을 끝낼 준비가 완료되었습니다. **결과 요약** 섹션의 **변형 B** 열에서 **스위치**를 클릭합니다. 이렇게 하면 앱의 모든 사용자가 파란색 단추로 전환됩니다.
5. **확인**을 클릭하여 실험을 종료할 것인지 확인합니다.
6. 이전 섹션에서 만든 **SampleExperiment** 앱을 실행합니다.
7. 파란색 단추가 표시되는지 확인합니다. 앱에서 업데이트된 변형 할당을 받으려면 최대 2분이 걸릴 수 있습니다.

## 관련 항목

  * [개발자 센터 대시보드에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
  * [실험용 앱 코딩](code-your-experiment-in-your-app.md)
  * [개발자 센터 대시보드에서 실험 관리](manage-your-experiment.md)
  * [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


