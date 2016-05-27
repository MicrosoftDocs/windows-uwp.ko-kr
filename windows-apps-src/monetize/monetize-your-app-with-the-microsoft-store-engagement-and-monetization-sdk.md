---
author: mcleanbyron
Description: Microsoft 스토어 참여 및 수익 창출 SDK는 더 많은 수익을 창출하고 고객을 확보할 수 있게 해주는 기능을 앱에 추가하는 데 사용할 수 있는 라이브러리 및 도구를 제공합니다.
title: Microsoft 스토어 참여 및 수익 창출 SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
---

# Microsoft 스토어 참여 및 수익 창출 SDK

Microsoft 스토어 참여 및 수익 창출 SDK는 앱에서 광고 표시, A/B 테스트로 실험 실행 등 더 많은 수익을 창출하고 고객을 확보하는 데 도움이 되는 라이브러리 및 도구를 제공합니다. 이 SDK는 Microsoft 유니버설 광고 클라이언트 SDK를 대신하여 사용되며, 시간이 지남에 따라 새 참여 및 수익 창출 기능을 포함하도록 진화합니다.


## SDK에서 사용할 수 있는 기능

Microsoft 스토어 참여 및 수익 창출 SDK는 다음 기능을 지원하는 라이브러리 및 도구를 제공합니다.

### UWP 앱에 대해 A/B 테스트로 실험 실행

UWP(유니버설 Windows 플랫폼) 앱에서 A/B 테스트를 실행하여, 모든 고객에게 기능을 릴리스하기 전에 일부 고객에 대한 기능의 효과를 측정합니다. 개발자 센터 대시보드에서 실험을 정의한 후 [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) 클래스를 사용하여 실험에 대한 변형을 가져오고, 이 데이터를 사용하여 테스트할 기능의 동작을 수정한 다음 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 메서드를 사용하여 보기 이벤트 및 전환 이벤트를 개발자 센터로 보냅니다. 마지막으로, 대시보드를 사용하여 결과를 보고 실험을 관리합니다.

자세한 내용은 [A/B 테스트로 실험 실행](run-app-experiments-with-a-b-testing.md)을 참조하세요.

### UWP 앱에 대한 앱 피드백

UWP 앱의 [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) 클래스를 사용하여 문제, 제안 및 좋아요를 제출할 수 있는 피드백 허브로 Windows 10 고객을 안내합니다. 그런 다음, 개발자 센터 대시보드의 [피드백 보고서](../publish/feedback-report.md)에서 이 피드백을 관리합니다.

자세한 내용은 [앱에서 피드백 허브 시작](launch-feedback-hub-from-your-app.md)을 참조하세요.

>**참고** **피드백** 보고서는 현재 [개발자 센터 참가자 프로그램](../publish/dev-center-insider-program.md)에 가입한 개발자 계정만 사용할 수 있습니다.

### 앱에서 광고 표시

UWP 앱, Windows 8.1 및 Windows Phone 8.x 앱에서 Microsoft의 배너 광고 또는 동영상 중간 광고를 표시하여 수익을 늘리세요. 또한 광고 조정을 사용하여 여러 광고 네트워크 공급자의 광고를 표시하면 광고 채우기 속도를 최대화할 수 있습니다.

자세한 내용은 [앱에서 광고 표시](display-ads-in-your-app.md)를 참조하세요.

>**참고** 이전 릴리스의 유니버설 광고 클라이언트 SDK, 광고 조정자 확장 및 Microsoft Advertising SDK에 있던 광고 기능이 이제 Microsoft 스토어 수익 창출 및 참여 SDK에 포함되었습니다.

### API 참조

SDK의 API에 대한 참조 설명서는 [Microsoft 스토어 참여 및 수익 창출 SDK API 참조](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)를 참조하세요.

## SDK 설치

Microsoft 스토어 수익 창출 및 참여 SDK를 설치하려면

1.  Visual Studio 2013 또는 Visual Studio 2015의 모든 인스턴스를 닫고 유니버설 광고 클라이언트 SDK, 광고 중재자 확장 또는 Microsoft Advertising SDK의 이전 버전을 모두 제거합니다.
2.  [SDK](http://aka.ms/store-em-sdk)를 다운로드하고 설치합니다. 설치하는 데 몇 분 정도 걸릴 수 있습니다. 프로세스가 완료되었는지 확인하고 완료될 때까지 기다립니다.
3.  Visual Studio를 다시 시작합니다.

Microsoft는 성능 향상과 새로운 기능을 추가하여 주기적으로 새 버전의 Microsoft 스토어 수익 창출 및 참여 SDK를 릴리스합니다. Microsoft 스토어 수익 창출 및 참여 SDK를 사용하는 기존 프로젝트가 있고 최신 버전을 사용하려는 경우 최신 버전의 SDK를 다운로드하고 설치하면 됩니다.

이전 릴리스의 유니버설 광고 클라이언트 SDK, 광고 조정자 확장 및 Microsoft Advertising SDK에 있던 광고 기능이 이제 Microsoft 스토어 수익 창출 및 참여 SDK에 포함되었습니다. 이러한 이전 릴리스 중 하나의 광고 기능을 사용하는 기존 Visual Studio 2015 또는 Visual Studio 2013 프로젝트가 있는 경우 Microsoft 스토어 수익 창출 및 참여 SDK를 설치한 후 변경하지 않고도 프로젝트에서 계속 작업할 수 있습니다.

>**참고** Visual Studio 2015와 함께 Microsoft 스토어 참여 및 수익 창출 SDK를 설치하려면 유니버설 Windows 앱용 Visual Studio Tools 1.1 버전 이상이 설치되어 있어야 합니다. 유니버설 Windows 앱용 Visual Studio Tools의 이 업데이트에 대한 자세한 내용은 [릴리스 정보](http://go.microsoft.com/fwlink/?LinkID=624516)를 참조하세요.

## SDK의 프레임워크 패키지

Microsoft 스토어 수익 창출 및 참여 SDK의 다음 라이브러리는 *프레임워크 패키지*로 구성되어 있습니다.

* Microsoft.Advertising.dll(UWP 앱 프로젝트에만 해당). 이 라이브러리에는 **Microsoft.Advertising** 및 **Microsoft.Advertising.WinRT.UI** 네임스페이스의 광고 API가 포함되어 있습니다.

즉, 개발 컴퓨터에 SDK를 설치하면 수정 및 성능 향상이 포함된 새 버전의 라이브러리를 게시할 때마다 이 라이브러리가 Windows 업데이트를 통해 자동으로 업데이트됩니다. 이렇게 하면 개발 컴퓨터에 항상 사용 가능한 최신 버전의 라이브러리가 설치되도록 할 수 있습니다.

또한 사용자가 이 라이브러리를 사용하는 앱 버전을 설치하면 수정 및 성능 향상이 포함된 새 버전의 라이브러리를 게시할 때마다 디바이스의 라이브러리도 자동으로 업데이트됩니다. 따라서 업데이트된 버전의 앱을 스토어에 게시하지 않아도 사용자가 항상 최신 버전의 라이브러리를 사용할 수 있습니다.

그러나 이 라이브러리에 새로운 API 또는 기능을 도입하는 새 버전의 SDK를 릴리스하는 경우 이러한 기능을 사용하려면 최신 버전의 SDK를 설치해야 합니다. 이 시나리오에서는 업데이트된 앱도 스토어에 게시해야 합니다.

다른 대상 플랫폼에 대한 SDK의 기타 라이브러리(예: Microsoft.Advertising.dll) 및 광고 조정용 라이브러리는 현재 프레임워크 라이브러리로 구성되어 있지 않습니다.

## 관련 항목

* [A/B 테스트로 실험 실행](run-app-experiments-with-a-b-testing.md)
* [Microsoft 스토어 참여 및 수익 창출 SDK API 참조](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [앱에서 피드백 허브 시작](launch-feedback-hub-from-your-app.md)


<!--HONumber=May16_HO2-->


