---
Description: Windows 개발자 센터 대시보드를 사용하여 A/B 테스트로 UWP(유니버설 Windows 플랫폼) 앱에 대한 실험을 실행할 수 있습니다.
title: A/B 테스트로 앱 실험 실행
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
---

# A/B 테스트로 앱 실험 실행

Windows 개발자 센터 대시보드를 사용하여 A/B 테스트로 UWP(유니버설 Windows 플랫폼) 앱에 대한 실험을 실행할 수 있습니다.

A/B 테스트에서 개발자 센터를 통한 앱의 프로그램 변수 할당을 실험합니다. A/B 테스트 가능 프로그램 변수를 중심으로 앱의 논리를 작성하면 사용자층의 무작위 세그먼트에 대해 앱의 변형을 사용하도록 설정할 수 있습니다. A/B 테스트의 목적은 전환율 향상(예: 앱에서 바로 구매 증가) 가능성이 큰 변형을 식별하는 것입니다.

비즈니스 목표에 가장 부합되는 변형을 식별한 후 즉시 실험을 종료하고, 앱을 다시 게시할 필요 없이 개발자 센터 대시보드에서 전체 사용자층에 대해 해당 변형을 사용하도록 설정할 수 있습니다.

## A/B 테스트 만들기 및 실행

A/B 테스트를 만들고 실행하려면 다음 단계를 따르세요.

1. [개발자 센터 대시보드에서 실험을 정의](define-your-experiment-in-the-dev-center-dashboard.md)합니다. 각 실험은 다음과 같이 이루어져 있습니다.
  * 사용자가 실험의 일부인 변형을 보기 시작하는 경우를 나타내는 *보기 이벤트*
  * 목표에 도달한 경우를 나타내는 *전환 이벤트*가 있는 하나 이상의 목표
  * 실험에서 사용되는 설정을 정의하는 하나 이상의 *변형*.
2. [실험용 앱을 코딩합니다](code-your-experiment-in-your-app.md). Microsoft 스토어 참여 및 수익 창출 SDK의 API를 사용하여 실험에 대한 변형 설정을 가져오고, 이 데이터를 사용하여 테스트할 기능의 동작을 수정하고, 개발자 센터에 보기 이벤트 및 전환 이벤트를 전송합니다.
3. [개발자 센터 대시보드에서 실험을 실행하고 관리합니다](manage-your-experiment.md). 대시보드를 사용하여 실험의 결과를 검토하고 실험을 완료합니다.

종단 간 프로세스를 보여 주는 연습은 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

## 요구 사항

Windows 개발자 센터의 A/B 테스트는 UWP 앱에 대해서만 지원됩니다.

A/B 테스트로 실험을 실행하려면 먼저 개발 컴퓨터를 설정해야 합니다.

* [여기](../get-started/get-set-up.md)에 있는 지침에 따라 UWP 개발용으로 개발 컴퓨터를 설정합니다.
* [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk)를 설치합니다. 실험에 대한 API 외에, 이 SDK는 광고 표시, 앱에 대한 피드백을 수집하기 위해 고객을 피드백 허브로 보내기 등의 다른 기능을 위한 API도 제공합니다. 이 SDK에 대한 자세한 내용은 [스토어 참여 및 수익 창출 SDK를 사용하여 앱으로 수익 창출](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)을 참조하세요.

## 모범 사례

가장 유용한 결과를 얻으려면 A/B 테스트로 실험을 실행할 때 다음과 같은 권장 사항을 따르는 것이 좋습니다.

* 변형 할당에 무작위 50/50 분할 분포를 사용하여 두 가지 변형만으로 실험을 실행하는 것이 좋습니다.
* 통계적으로 의미가 있고 작업 가능한 충분한 데이터를 수집하기 위해 최소 2-4주 동안 실험을 실행합니다.

## 관련 항목

* [개발자 센터 대시보드에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [실험용 앱 코딩](code-your-experiment-in-your-app.md)
* [개발자 센터 대시보드에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)


<!--HONumber=Mar16_HO5-->


