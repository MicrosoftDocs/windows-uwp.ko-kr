---
author: mcleblanc
ms.assetid: A37ADD4A-2187-4767-9C7D-EDE8A90AA215
title: "성능 계획"
description: "사용자는 앱이 응답성을 유지하고 자연스러운 느낌을 주며 배터리를 소모하지 않기를 기대합니다."
translationtype: Human Translation
ms.sourcegitcommit: afb508fcbc2d4ab75188a2d4f705ea0bee385ed6
ms.openlocfilehash: 39d57811a07b4c404da4b7e369e3bf5441fa99c0

---
# 성능 계획

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


사용자는 앱이 응답성을 유지하고 자연스러운 느낌을 주며 배터리를 소모하지 않기를 기대합니다. 기술적으로 성능은 기능적 요구 사항이 아니지만 성능을 기능으로 간주하는 것이 사용자의 기대를 충족하는 데 도움이 됩니다. 목표 지정 및 측정이 중요한 요소입니다. 성능에 중요한 시나리오가 무엇인지 결정하고 우수한 성능이란 무엇인지 정의하세요. 그런 다음 초기에 측정하고 프로젝트의 수명 주기 동안 목표를 달성할 수 있는지 확인합니다.

## 목표 지정

우수한 성능을 정의하기 위한 기본적인 조건은 사용자 환경입니다. 앱 시작 시간은 성능에 대한 사용자의 인식에 영향을 미칠 수 있습니다. 사용자는 앱 실행 시간이 1초 미만인 경우 성능이 우수하고, 5초 미만인 경우 양호하고, 5초 이상인 경우 불량하다고 생각할 수 있습니다.

메모리 등의 기타 메트릭은 사용자 환경에 명확하지 않은 영향을 미칩니다. 일시 중단되거나 비활성화되어 있는 동안 앱이 종료될 가능성은 활성 앱에서 사용되는 메모리로 인해 증가합니다. 일반적으로 높은 메모리 사용량은 시스템의 모든 앱에 대한 경험을 저하시키므로 메모리 사용에 대한 목표가 적절해야 합니다. 사용자가 인식하는 앱의 대략적인 크기(작음, 보통 또는 큼)를 고려하세요. 성능과 관련된 기대는 이러한 인식과 상관 관계가 있습니다. 예를 들어 많은 미디어를 사용하지 않는 작은 앱에서 100MB 미만 메모리를 사용할 수 있습니다.

목표를 한 번에 설정하는 것보다 초기 목표를 설정한 다음 나중에 수정하는 것이 더 좋습니다. 앱의 성능 목표는 구체적이고 측정 가능해야 하며, 사용자 또는 앱이 작업을 완료하는 데 걸리는 시간(시간), 앱이 사용자 조작에 응답하여 자체적으로 다시 그리는 속도 및 연속성(유연성) 및 앱이 배터리 전원을 포함하여 시스템 리소스를 절약하는 정도(효율성)라는 세 가지 범주에 속해야 합니다.

## 시간

사용자가 앱에서 작업을 완료하는 데 걸리는 시간의 허용 범위(*조작 클래스*)를 고려합니다. 각 조작 클래스에 대해 레이블, 인지된 사용자 정서 및 적합한 기간과 최대 기간을 할당합니다. 다음은 몇 가지 제안 사항입니다.

| 조작 클래스 레이블 | 사용자 인식                 | 적합함            | 최대          | 예제                                                                     |
|-------------------------|---------------------------------|------------------|------------------|------------------------------------------------------------------------------|
| 빠름                    | 최소한의 인지할 수 있는 지연      | 100밀리초 | 200밀리초 | 앱 바 표시, 단추 누르기(첫 응답)                        |
| 일반                 | 적당히 빠름             | 300밀리초 | 500밀리초 | 크기 조정, 시맨틱 줌                                                        |
| 응답              | 신속하지 않지만 응답성 있음 | 500밀리초 | 1초         | 다른 페이지로 이동, 일시 중단 상태에서 앱 다시 시작          |
| 실행                  | 경쟁 환경          | 1초         | 3초        | 처음으로 또는 이전에 종료된 후 앱 실행 |
| 계속              | 더 이상 응답 없음      | 500밀리초 | 5초        | 인터넷에서 파일 다운로드                                            |
| 종속                 | 오래 걸리지만 사용자가 전환할 수 있음    | 500밀리초 | 10초       | 스토어에서 여러 앱 설치                                         |

 

이제 앱의 성능 시나리오에 조작 클래스를 할당할 수 있습니다. 앱의 지정 시간 참조, 사용자 환경의 일부, 조작 클래스 등을 각 시나리오에 할당할 수 있습니다. 다음은 예제 음식 및 식사 앱에 대한 몇 가지 제안 사항입니다.


<!-- DHALE: used HTML table here b/c WDCML src used rowspans -->
<table>
<tr><th>시나리오</th><th>시점</th><th>사용자 환경</th><th>조작 클래스</th></tr>
<tr><td rowspan="3">조리법 페이지로 이동 </td><td>첫 번째 응답</td><td>페이지 전환 애니메이션 시작됨</td><td>빠름(100 - 200밀리초)</td></tr>
<tr><td>응답</td><td>재료 목록 로드됨, 이미지 없음</td><td>응답(500밀리초 - 1초)</td></tr>
<tr><td>표시 완료</td><td>모든 콘텐츠 로드됨, 이미지 표시됨</td><td>연속(500밀리초 - 5초)</td></tr>
<tr><td rowspan="2">조리법 검색</td><td>첫 번째 응답</td><td>검색 단추 클릭함</td><td>빠름(100 - 200밀리초)</td></tr>
<tr><td>표시 완료</td><td>현지 조리법 제목 목록 표시됨</td><td>일반(300~500밀리초)</td></tr>
</table>

라이브 콘텐츠를 표시하려면 콘텐츠 최신 상태 목표를 고려합니다. 목표가 몇 초마다 콘텐츠를 새로 고치는 것입니까? 또는 몇 분마다, 몇 시간마다 또는 하루에 한 번 콘텐츠 새로 고침이 허용되는 사용자 환경입니까?

목표를 지정했으면 이제 앱을 보다 잘 테스트, 분석 및 최적화할 수 있습니다.

## 유연성

앱에 대해 측정 가능한 특정 유연성 목표에는 다음이 포함될 수 있습니다.

-   화면 다시 그리기가 중지되었다가 시작되지 않습니다(결함).
-   애니메이션이 초당 60프레임(FPS)에 렌더링됩니다.
-   사용자가 이동/스크롤할 때 앱에서 초당 3~6페이지의 콘텐츠를 제공합니다.

## 효율성

앱에 대해 측정 가능한 특정 효율성 목표에는 다음이 포함될 수 있습니다.

-   앱 프로세스의 경우 항상 CPU 백분율은 *N* 이하이고 메모리 사용량(MB)은 *M* 이하입니다.
-   앱이 비활성 상태인 경우 *N*과 *M*은 앱 프로세스에 대해 0입니다.
-   *X* 시간의 배터리 전원 동안 앱을 활발히 사용할 수 있습니다. 앱이 비활성 상태인 경우 디바이스는 *Y* 시간 동안 충전 상태를 유지합니다.

## 성능을 위한 앱 디자인

이제 성능 목표를 사용하여 앱의 디자인에 영향을 줄 수 있습니다. 예제 음식 및 식사 앱을 사용하는 경우 사용자가 조리법 페이지로 이동하면 조리법 이름이 먼저 렌더링된 다음 재료 표시가 지연되고 이미지 표시가 더 지연되도록 [항목을 증분식으로 업데이트](optimize-gridview-and-listview.md#update-items-incrementally)할 수 있습니다. 이렇게 하면 이동/스크롤하는 동안 응답성 및 유연한 UI가 유지되며, UI 스레드가 따라갈 수 있을 정도로 조작 속도가 느려진 후 완전한 충실도가 실현됩니다. 다음은 고려해야 할 몇 가지 다른 측면입니다.

**UI**

-   [XAML 태그를 최적화](optimize-xaml-loading.md)하여 앱 UI의 각 페이지(특히 초기 페이지)에 대한 구문 분석 및 로드 시간과 메모리 효율성을 극대화합니다. nutshell에서는 필요할 때까지 UI 및 코드 로드를 지연합니다.
-   [
            **ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 및 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)의 경우 모든 항목을 같은 크기로 설정하고 최대한 많은 [ListView 및 GridView 최적화 기술](optimize-gridview-and-listview.md)을 사용합니다.
-   프레임워크가 코드로 작성하는 대신 청크로 로드하고 다시 사용할 수 있는 태그 형식으로 UI를 선언합니다.
-   사용자가 필요로 할 때까지 UI 요소를 축소합니다. [
            **Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 속성을 참조하세요.
-   테마 전환 및 애니메이션이 스토리보드 애니메이션보다 좋습니다. 자세한 내용은 [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/Mt187350)를 참조하세요. 스토리보드 애니메이션은 화면을 지속적으로 업데이트해야 하고 CPU 및 그래픽 파이프라인을 활성 상태로 유지해야 합니다. 배터리를 절약하기 위해 사용자가 앱을 조작하지 않는 경우 애니메이션을 실행하지 마세요.
-   로드하는 이미지는 [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210) 메서드를 사용하여 이미지를 표시할 뷰에 적합한 크기로 로드되어야 합니다.

**CPU, 메모리 및 전원**

-   우선 순위가 낮은 작업은 우선 순위가 낮은 스레드 및/또는 코어에서 실행되도록 예약합니다. [비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/Mt187335), [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR209054) 속성 및 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) 클래스를 참조하세요.
-   일시 중단 시 비용이 많이 드는 리소스(예: 미디어)를 해제하여 앱의 메모리 공간을 최소화합니다.
-   코드의 작업 집합을 최소화합니다.
-   가능하면 언제나 이벤트 처리기 등록을 취소하고 UI 요소를 역참조하여 메모리 누수를 방지합니다.
-   배터리를 절약하기 위해 데이터 폴링 빈도, 센서 쿼리 빈도 또는 유휴 상태일 때의 CPU 작업 예약 빈도를 보수적으로 설정합니다.

**데이터 액세스**

-   가능한 경우 콘텐츠를 프리페치합니다. 자동 프리페치의 경우 [**ContentPrefetcher**](https://msdn.microsoft.com/library/windows/apps/Dn279042) 클래스를 참조하세요. 수동 프리페치의 경우 [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/BR224847) 네임스페이스 및 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Hh700517) 클래스를 참조하세요.
-   가능한 경우 액세스 비용이 많이 드는 콘텐츠를 캐시합니다. [
            **LocalFolder**](https://msdn.microsoft.com/library/windows/apps/BR241621) 및 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622) 속성을 참조하세요.
-   캐시 누락 시 앱이 여전이 콘텐츠를 로드하고 있음을 나타내는 자리 표시자 UI를 가급적 빨리 표시합니다. 사용자에게 방해가 되지 않은 방법으로 자리 표시자 콘텐츠에서 라이브 콘텐츠로 전환합니다. 예를 들어 앱이 라이브 콘텐츠를 로드할 때는 사용자 손가락 또는 마우스 포인터 아래의 콘텐츠 위치를 변경하지 않습니다.

**앱 시작 및 다시 시작**

-   앱의 시작 화면을 지연시키고 필요한 경우에 아니면 앱의 시작 화면을 확장하지 않습니다. 자세한 내용은 [빠르고 유연한 앱 실행 환경 만들기](http://go.microsoft.com/fwlink/p/?LinkId=317595) 및 [오랫동안 시작 화면 표시](https://msdn.microsoft.com/library/windows/apps/Mt187309)를 참조하세요.
-   시작 화면이 해제된 직후에 발생하는 애니메이션은 앱 시작 시간이 지연되는 인상만 주기 때문에 사용하지 않도록 설정합니다.

**적응 UI 및 방향**

-   [
            **VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021) 클래스를 사용합니다.
-   필수 작업만 즉시 완료하고 부하가 큰 앱 작업을 나중까지 지연합니다. 앱이 200~800밀리초 내에 작업을 완료하지 못하면 앱의 UI가 잘린 상태로 표시됩니다.

성능 관련 디자인을 사용하여 앱 코딩을 시작할 수 있습니다.

## 성능 계측

코딩할 때 앱이 실행되는 동안 특정 지점에서 메시지 및 이벤트를 기록하는 코드를 추가합니다. 나중에 앱을 테스트할 때 Windows Performance Recorder 및 Windows Performance Analyzer(둘 다 [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx)에 포함되어 있음)와 같은 프로파일링 도구를 사용하여 앱의 성능에 대한 보고서를 만들고 볼 수 있습니다. 이 보고서에서 이러한 메시지 및 이벤트를 검색하면 보고서 결과를 보다 쉽게 분석할 수 있습니다.

UWP(유니버설 Windows 플랫폼)는 풍부한 이벤트 로깅 및 추적 솔루션을 함께 제공하는 로깅 API([ETW(Windows용 이벤트 추적)](https://msdn.microsoft.com/library/windows/desktop/Bb968803)에서 지원)를 제공합니다. [
            **Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/BR206677) 네임스페이스의 일부인 API에는 [**FileLoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264138), [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195), [**LoggingChannel**](https://msdn.microsoft.com/library/windows/apps/Dn264202) 및 [**LoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264217) 클래스가 포함됩니다.

앱이 실행되는 동안 특정 지점에서 보고서에 메시지를 로깅하려면 다음과 같이 **LoggingChannel** 개체를 만든 다음 개체의 [**LogMessage**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingchannel.logmessage.aspx) 메서드를 호출합니다.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingChannel myLoggingChannel = new LoggingChannel("MyLoggingChannel");

myLoggingChannel.LogMessage(LoggingLevel.Information, "Here' s my logged message.");

// ...
```

앱이 실행되는 일정 기간 동안 보고서에 시작 및 중지 이벤트를 로깅하려면 다음과 같이 **LoggingActivity** 개체를 만든 다음 개체의 [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingactivity.loggingactivity.aspx) 생성자를 호출합니다.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingActivity myLoggingActivity;

// myLoggingChannel is defined and initialized in the previous code example.
using (myLoggingActivity = new LoggingActivity("MyLoggingActivity"), myLoggingChannel))
{   // After this logging activity starts, a start event is logged.
    
    // Add code here to do something of interest.
    
}   // After this logging activity ends, an end event is logged.

// ...
```

[로깅 샘플](http://go.microsoft.com/fwlink/p/?LinkId=529576)도 참조하세요.

앱을 계측한 후에는 앱의 성능을 테스트하고 측정할 수 있습니다.

## 성능 목표 테스트 및 측정

계획의 일부로 개발하는 동안 성능을 측정할 모든 지점을 정의합니다. 이는 프로토타입 제작, 개발 또는 배포 중에 측정할지에 따라 여러 가지 목적에 사용됩니다. 프로토타입 제작 초기 단계 중에 성능을 측정하는 것이 매우 유용할 수 있으므로 의미 있는 작업을 수행하는 코드를 만드는 즉시 이렇게 하는 것이 좋습니다. 앱에 중요한 비용이 있고 디자인 결정을 알리는 경우 초기에 측정하는 것이 좋습니다. 이렇게 하면 잘 확장되는 성능이 우수한 앱을 만들 수 있습니다. 나중보다 초기에 디자인을 변경하는 것이 일반적으로 비용이 적게 됩니다. 제품 주기에서 늦게 성능을 측정하면 마지막 순간 해킹이 발생하거나 성능이 저하될 수 있습니다.

다음 기술 및 도구를 사용하여 앱이 원래 성능 목표에 대해 어떻게 스택되는지를 테스트합니다.

-   올인원 및 데스크톱 PC, 랩톱 및 울트라북, 태블릿 및 기타 모바일 디바이스 등 광범위한 하드웨어 구성을 테스트합니다.
-   다양한 화면 크기에 대해 테스트합니다. 화면 크기가 클수록 더 많은 콘텐츠를 표시할 수 있지만 추가 콘텐츠를 모두 표시하면 성능에 부정적인 영향을 줄 수 있습니다.
-   테스트 변수를 최대한 많이 제거합니다.
    -   테스트 디바이스에서 백그라운드 앱을 끕니다. 이렇게 하려면 Windows의 시작 메뉴에서 **설정**을 선택한 후 &gt;**개인 설정**&gt;**화면 잠금**을 선택합니다. 각 활성 앱을 선택하고 **없음**을 선택합니다.
    -   테스트 디바이스에 배포하기 전에 릴리스 구성에서 작성하여 네이티브 코드로 앱을 컴파일합니다.
    -   자동 유지 관리가 테스트 디바이스의 성능에 영향을 주지 않도록 하려면 수동으로 트리거하고 완료될 때까지 기다립니다. Windows의 시작 메뉴에서 **보안 및 유지 관리**를 검색합니다. **유지 관리** 영역의 **자동 유지 관리** 아래에서 **유지 관리 시작**을 선택하고 상태가 **유지 관리 진행 중**에서 변경될 때까지 기다립니다.
    -   앱을 여러 번 실행하여 임의 테스트 변수를 제거하고 일관된 측정을 보장합니다.
-   절전 사용 여부 테스트 사용자의 디바이스는 개발 컴퓨터보다 전원이 훨씬 적을 수 있습니다. Windows는 모바일 디바이스와 같은 절전 디바이스를 염두에 두고 설계되었습니다. 플랫폼에서 실행되는 앱은 이러한 디바이스에서 제대로 작동해야 합니다. 경험적으로 절전 디바이스는 데스크톱 컴퓨터보다 4배 정도 더 느릴 것으로 예상되므로 이에 따라 목표를 설정하세요.
-   Microsoft Visual Studio 및 Windows Performance Analyzer와 같은 도구를 함께 사용하여 앱 성능을 측정합니다. Visual Studio는 소스 코드 링크와 같은 앱 중심 분석을 제공하도록 설계되었습니다. Windows Performance Analyzer는 시스템 정보 제공, 터치 조작 이벤트에 대한 정보, 디스크 I/O(입출력) 및 GPU(그래픽 처리 디바이스) 비용에 대한 정보 등 시스템 중심 분석을 제공하도록 설계되었습니다. 두 도구 모두 추적 캡처 및 내보내기를 제공하며 공유된 추적 및 사후 추적을 다시 열 수 있습니다.
-   인증을 위해 스토어에 앱을 제출하려면 먼저 [Windows 앱 인증 키트 테스트](windows-app-certification-kit-tests.md)의 "성능 테스트" 섹션 및 [Windows 스토어 앱 테스트 사례](https://msdn.microsoft.com/library/windows/apps/Dn275879)의 "성능 및 안정성" 섹션에 설명된 대로 테스트 계획 및 성능 관련 테스트 사례에 통합해야 합니다.

자세한 내용은 다음과 같은 리소스 및 프로파일링 도구를 참조하세요.

-   [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/apps/xaml/hh448170.aspx)
-   [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx)
-   [Visual Studio 진단 도구를 사용하여 성능 분석](https://msdn.microsoft.com/library/windows/apps/xaml/hh696636.aspx)
-   //Build/ 세션 [XAML 성능](https://channel9.msdn.com/Events/Build/2015/3-698)
-   //Build/ 세션 [Visual Studio 2015의 새로운 XAML 도구](https://channel9.msdn.com/Events/Build/2015/2-697)

## 성능 테스트 결과에 응답

성능 테스트 결과를 분석한 후 변경이 필요한지 여부를 결정합니다. 예를 들면 다음과 같습니다.

-   앱 디자인 의사 결정을 변경하거나 코드를 최적화해야 하나요?
-   코드에서 계측을 추가, 제거 또는 변경해야 하나요?
-   성능 목표를 수정해야 하나요?

변경이 필요한 경우 변경한 후 계측 또는 테스트로 돌아가 반복하세요.

## 최적화

앱에서 성능에 중요한 코드 경로만 최적화하세요. 이는 대부분 시간이 소요되는 경로입니다. 프로파일링을 통해 확인할 수 있습니다. 종종 좋은 디자인 사례에 따라 소프트웨어를 만드는 작업과 최고로 최적화된 성능을 제공하는 코드를 작성하는 작업은 서로 상충되는 점이 있습니다. 일반적으로 성능이 중요하지 않은 영역에서는 개발자 생산성과 양호한 소프트웨어 디자인을 우선적으로 처리하는 것이 더 좋습니다.




<!--HONumber=Jun16_HO4-->


