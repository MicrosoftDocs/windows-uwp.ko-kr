---
author: jwmsft
ms.assetid: E1943DCE-833F-48AE-8402-CD48765B24FC
title: 일시 중단/다시 시작 최적화
description: 프로세스 수명 시스템의 사용을 간소화하여 일시 중단 또는 종료 후 효율적으로 다시 시작하는 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: f9e045c381fc6c51a769be31403114ad15cf06bd
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/14/2017
ms.locfileid: "894687"
---
# <a name="optimize-suspendresume"></a>일시 중단/다시 시작 최적화

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

프로세스 수명 시스템의 사용을 간소화하여 일시 중단 또는 종료 후 효율적으로 다시 시작하는 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다.

## <a name="launch"></a>실행

일시 중단/종료 후에 앱을 다시 활성화할 때는 오랜 시간이 경과했는지 확인합니다. 시간이 오래 지났다면 오래된 사용자 데이터를 표시하는 대신 앱의 기본 방문 페이지로 돌아가는 것이 좋습니다. 그러면 시작 속도도 높아집니다.

정품 인증 도중에 항상 이벤트 인수 매개 변수의 PreviousExecutionState를 확인하세요(예: 정품 인증이 시작된 경우 LaunchActivatedEventArgs.PreviousExecutionState 확인). 값이 ClosedByUser 또는 NotRunning인 경우에는 이전에 저장된 상태를 복원하느라 시간을 낭비하지 마세요. 이 경우 적절한 조치는 새로운 환경을 제공하는 것이며, 그러면 시작 속도가 더 높아집니다.

이전에 저장된 상태를 적극적으로 복원하는 대신, 해당 상태를 추적하고 요청 시에만 복원하는 것이 좋습니다. 예를 들어 앱을 이전에 일시 중단하고 3페이지에 대한 상태를 저장한 다음 종료했다고 가정하겠습니다. 다시 시작할 때 사용자를 세 번째 페이지로 돌아가도록 하려면 처음 두 페이지에 대해 적극적으로 상태를 복원하지 마세요. 그 대신, 이 상태를 유지하고 필요하다고 판단될 때만 상태를 사용합니다.

## <a name="while-running"></a>실행하는 동안

일시 중단 이벤트를 대기했다가 많은 양의 상태를 유지하지 않는 것이 모범 사례입니다. 그 대신, 응용 프로그램에서 실행 시 소량의 상태를 증분식으로 유지해야 합니다. 이 방식은 일시 중단 도중에 모든 항목을 한꺼번에 저장하려고 하면 시간이 부족할 위험이 있는 큰 앱의 경우 특히 중요합니다.

그러나 실행하는 동안 증분 저장과 앱 성능 사이에 적절한 균형을 찾아야 합니다. 변경된(따라서 저장해야 하는) 데이터를 증분식으로 추적하는 동시에 일시 중단 이벤트를 사용하여 이 데이터를 실제로 저장(모든 데이터를 저장하거나 앱의 전체 상태를 검사해서 저장할 항목을 결정하는 작업보다 속도가 높음)하면 적절하게 균형이 잡힙니다.

창 Activated 또는 VisibilityChanged 이벤트를 사용하여 상태 저장 시기를 결정하지 마세요. 사용자가 앱 외부로 전환하는 경우 창은 비활성화되지만 시스템에서는 앱을 일시 중단하기 전에 잠시(약 10초) 기다립니다. 이런 방식 덕분에 사용자가 신속하게 다시 앱으로 전환하는 경우 응답성이 높은 경험이 제공됩니다. 일시 중단 논리를 실행하기 전에 일시 중단 이벤트를 대기하세요.

## <a name="suspend"></a>일시 중단

일시 중단 중에 앱의 공간을 줄입니다. 앱이 일시 중단된 동안 메모리를 더 적게 사용하는 경우 전체 시스템의 응답성이 높아지고 일시 중단된 앱(사용자 앱 포함) 중 종료되는 앱이 더 적어집니다. 그러나 이러한 특성과 다시 시작 속도 요구 사항 사이에 균형을 잡아야 합니다. 앱에서 대량 데이터를 메모리로 다시 로드하는 동안 다시 시작 속도가 현저히 떨어질 정도로 공간을 줄이지는 마세요.

관리되는 앱의 경우 시스템에서는 앱의 일시 중단 처리기가 완료되면 가비지 수집 단계를 실행합니다. 일시 중단된 동안 앱의 공간을 줄이는 데 도움이 되는 개체에 대한 참조를 해제하여 이를 활용해야 합니다.

이상적으로는 앱에서 일시 중단 논리를 완료하는 데 걸리는 시간이 1초 미만입니다. 더 신속하게 일시 중단할 수 있다면 더 좋습니다. 이렇게 되면 다른 앱 및 시스템 부분에 대한 사용자 환경이 더 빨라집니다. 필요한 경우에는 일시 중단 논리에 최대 5초(데스크톱 장치) 또는 10초(모바일 장치)까지 걸릴 수 있습니다. 이 시간이 초과되는 경우 앱이 갑자기 종료됩니다. 이런 상황은 바람직하지 않습니다. 만약 갑자기 종료된다면 사용자가 다시 앱으로 전환했을 때 새로운 프로세스가 시작되므로 일시 중단된 앱을 다시 시작하는 것에 비해 사용자 경험이 훨씬 더 느려집니다.

## <a name="resume"></a>다시 시작

대부분의 앱은 다시 시작 시 특별한 작업을 수행하지 않아도 됩니다. 따라서 일반적으로 이 이벤트를 처리하지 않습니다. 일부 앱에서는 일시 중단 중에 닫힌 연결을 복원하거나 오래된 데이터를 다시 고치기 위해 다시 시작을 사용합니다. 이런 종류의 작업을 적극적으로 수행하는 대신, 필요할 때 이러한 작업을 시작하도록 앱을 디자인하세요. 그러면 사용자가 일시 중단된 앱으로 다시 전환할 때 사용자 경험 속도가 높아지고 사용자가 실제로 필요로 하는 작업만 수행할 수 있습니다.

## <a name="avoid-unnecessary-termination"></a>불필요한 종료 방지

UWP 프로세스 수명 시스템에서는 다양한 이유로 앱을 일시 중단하거나 종료할 수 있습니다. 이 프로세스는 앱을 일시 중단되거나 종료되기 전 상태로 빠르게 복구하도록 디자인되어 있습니다. 제대로 작동할 경우 사용자가 앱 실행이 중지되었는지 인식하지 못합니다. 다음은 UWP 앱에서 앱 수명 동안 시스템이 전환을 간소화할 수 있도록 사용할 수 있는 몇 가지 트릭입니다.

사용자가 백그라운드로 앱을 이동하거나 시스템이 절전 상태가 될 때 앱이 일시 중단될 수 있습니다. 앱이 일시 중단되면 일시 중단 이벤트가 발생하고 데이터를 저장할 최대 5초의 시간이 주어집니다. 앱의 일시 중단 이벤트 처리기가 5초 내에 완료되지 않을 경우 앱에서 더 이상 응답하지 않는 것으로 간주하고 앱을 종료합니다. 종료된 앱은 사용자가 해당 앱으로 전환할 때 즉시 메모리로 로드되지 않고 긴 시작 프로세스를 다시 거쳐야 합니다.

### <a name="serialize-only-when-necessary"></a>필요한 경우에만 직렬화

많은 앱에서 일시 중단 시 모든 데이터를 직렬화합니다. 그러나 적은 양의 앱 설정 데이터만 저장해야 하는 경우에는 데이터를 직렬화하지 않고 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622) 저장소를 사용해야 합니다. 많은 양의 데이터 및 설정이 아닌 데이터에 대해 직렬화를 사용하세요.

데이터를 직렬화하는 경우에는 변경되지 않았으면 다시 직렬화하지 않아야 합니다. 데이터를 직렬화하고 저장하는 시간 그리고 앱이 다시 활성화될 때 데이터를 읽고 역직렬화하는 추가 시간이 소요됩니다. 그보다는 앱에서 실제로 상태가 변경되었는지 확인한 다음 변경된 데이터만 직렬화하고 역직렬화하는 것이 좋습니다. 이렇게 하는 좋은 방법은 데이터가 변경된 후 데이터를 정기적으로 백그라운드에서 직렬화하는 것입니다. 이 방법을 사용하면 일시 중단 시 직렬화해야 하는 모든 항목이 이미 저장되었으므로 수행할 작업이 없고 앱이 빠르게 일시 중단됩니다.

### <a name="serializing-data-in-c-and-visual-basic"></a>C# 및 Visual Basic에서 데이터 직렬화

.NET 앱에 대해 선택할 수 잇는 직렬화 기술은 [**System.Xml.Serialization.XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx), [**System.Runtime.Serialization.DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) 및 [**System.Runtime.Serialization.Json.DataContractJsonSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.json.datacontractjsonserializer.aspx) 클래스입니다.

성능적 관점에서 [**XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx) 클래스를 사용하는 것이 좋습니다. **XmlSerializer**의 경우 직렬화 및 역직렬화 시간이 가장 짧고 메모리 사용 공간을 적게 유지합니다. **XmlSerializer**의 경우 일부 .NET Framework에 종속되므로 다른 직렬화 기술에 비해 **XmlSerializer**를 사용하기 위해 앱으로 로드해야 하는 모듈 수가 더 적습니다.

[**DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx)를 사용하면 사용자 지정 클래스를 더 쉽게 직렬화할 수 있지만 **XmlSerializer**보다 성능에 미치는 영향은 큽니다. 향상된 성능이 필요한 경우 전환을 고려하세요. 일반적으로 직렬 변환기를 2개 이상 로드하지 않아야 하며 다른 직렬 변환기의 기능이 필요하지 않은 경우 **XmlSerializer**를 사용하는 것이 좋습니다.

### <a name="reduce-memory-footprint"></a>메모리 사용 공간 감소

시스템에서는 일시 중단된 앱을 최대한 많이 메모리에 저장하여 사용자가 신속하고 확실하게 앱으로 전환할 수 있게 합니다. 일시 중단된 앱이 시스템의 메모리에 남아 있으면 시작 화면을 표시하거나 오랜 로드 작업을 수행할 필요 없이 신속하게 포그라운드로 다시 불러와 사용자가 조작하도록 할 수 있습니다. 메모리에 앱을 저장하기에 충분한 리소스가 없을 경우 앱이 종료됩니다. 이로 인해 다음과 같은 두 가지 이유로 메모리 관리가 중요합니다.

-   일시 중단 시 가능한 많은 메모리를 해제하면 앱이 일시 중단된 동안 리소스 부족으로 인해 종료될 가능성이 최소화됩니다.
-   앱에서 사용하는 전체 메모리 양을 줄이면 다른 앱이 일시 중단된 동안 종료될 가능성이 감소합니다.

### <a name="release-resources"></a>리소스 해제

파일, 장치와 같은 일부 개체는 상당량의 메모리를 차지합니다. 앱이 일시 중단될 때 그 개체에 대한 핸들을 해제하고 필요할 때 다시 핸들을 만드는 것이 좋습니다. 앱이 다시 시작될 때 유효하지 않은 모든 캐시를 제거하는 것도 좋습니다. C# 및 Visual Basic 앱에 대해 XAML 프레임워크에서 필요한 경우 자동으로 실행하는 추가 단계는 가비지 수집입니다. 이 단계를 통해 앱 코드에서 더 이상 참조되지 않는 모든 개체가 해제됩니다.

## <a name="resume-quickly"></a>신속하게 다시 시작

일시 중단된 앱은 사용자가 포그라운드로 가져오거나 시스템의 절전 상태가 해제될 때 다시 시작할 수 있습니다. 일시 중단된 앱이 다시 시작하면 일시 중단되었을 때의 지점부터 시작합니다. 앱이 오랜 시간 동안 일시 중단된 경우에도 메모리에 저장되어 있으므로 앱 데이터가 손실되지 않습니다.

대부분의 앱에서 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/BR205859) 이벤트를 처리할 필요가 없습니다. 앱이 다시 시작할 때 변수 및 개체는 일시 중단되었을 시점과 정확하게 동일한 상태가 됩니다. 앱이 일시 중단된 시점과 다시 시작한 시점의 사이에 변경되었을 데이터 또는 개체(예: 업데이트 피드 데이터와 같은 오래된 콘텐츠나 네트워크 연결)를 업데이트해야 하는 경우 또는 webcam과 같은 디바이스에 대한 액세스 권한을 다시 얻어야 하는 경우에만 **Resuming** 이벤트를 처리하세요.

## <a name="related-topics"></a>관련 항목

* [앱 일시 중단 및 다시 시작에 대한 지침](https://msdn.microsoft.com/library/windows/apps/Hh465088)
 

 



