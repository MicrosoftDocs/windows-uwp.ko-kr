---
author: PatrickFarley
title: 앱에서 3D 인쇄
description: 유니버설 Windows 앱에 3D 인쇄 기능을 추가하는 방법을 알아봅니다. 이 항목에서는 3D 모델이 인쇄 가능하고 올바른 형식인지 확인한 다음 3D 인쇄 대화 상자를 시작하는 방법을 설명합니다.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 3dprinting, 3d 인쇄
ms.localizationpriority: medium
ms.openlocfilehash: 818e1338a1d36d24990f22316dc2072c2c0d7cc5
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5864834"
---
# <a name="3d-printing-from-your-app"></a>앱에서 3D 인쇄

**중요 API**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

유니버설 Windows 앱에 3D 인쇄 기능을 추가하는 방법을 알아봅니다. 이 항목에서는 3D 모델이 인쇄 가능하고 올바른 형식인지 확인한 다음 3D 기하 도형 데이터를 앱에 로드하고 3D 인쇄 대화 상자를 시작하는 방법을 설명합니다. 이러한 절차의 실제 예를 보려면 [3D 인쇄 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)을 참조하세요.

> [!NOTE]
> 이 가이드의 샘플 코드에서는 편의상 단순하게 오류 보고 및 처리를 보여 줍니다.

## <a name="setup"></a>설정


3D 인쇄 기능을 사용하게 될 응용 프로그램 클래스에 [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169) 네임스페이스를 추가합니다.

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

이 가이드에서는 다음 추가 네임스페이스가 사용됩니다.

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

그런 다음 클래스에 유용한 멤버 필드를 제공합니다. 인쇄 드라이버로 전달되는 인쇄 작업을 보여줄 [**Print3DTask**](https://msdn.microsoft.com/library/windows/apps/dn998044) 개체를 선언합니다. 앱에 로드되는 원본 3D 데이터 파일을 저장할 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 선언합니다. 모든 필수 메타데이터와 함께 인쇄 가능한 3D 모델을 나타내는 [**Printing3D3MFPackage**](https://msdn.microsoft.com/library/windows/apps/dn998063) 개체를 선언합니다.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>간단한 UI 만들기

이 샘플에서는 세 가지 사용자 컨트롤인 로드 단추(프로그램 메모리에 파일 저장), 수정 단추(필요에 따라 파일 수정) 및 인쇄 단추(인쇄 작업 시작)가 작동합니다. 다음 코드에서는 이러한 단추와 이들의 온클릭 이벤트 처리기를 .cs 클래스의 해당 XAML 파일에 만듭니다.

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

UI 피드백에 대한 **TextBlock**을 추가합니다.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>3D 데이터 가져오기


앱에서 3D 기하 도형 데이터를 얻는 방법은 다양합니다. 앱에서 3D 스캔을 통해 데이터를 검색하거나, 웹 리소스에서 모델 데이터를 다운로드하거나, 수학 공식이나 사용자 입력을 사용하여 프로그래밍 방식으로 3D 메시를 생성합니다. 이 가이드에서는 편의상 디바이스 저장소에서 일반적인 몇 가지 파일 형식의 3D 데이터 파일을 프로그램 메모리로 로드하는 방법을 보여 줍니다. [3D Builder 모델 라이브러리](https://developer.microsoft.com/windows/hardware/3d-builder-model-library)는 디바이스에 쉽게 다운로드할 수 있는 다양한 모델을 제공합니다.

`OnLoadClick` 메서드에서 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 클래스를 사용하여 단일 파일을 앱의 메모리에 로드합니다.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>3D Builder를 사용하여 3D 제조 형식(.3mf)으로 변환합니다.

이 시점에서 3D 데이터 파일을 앱의 메모리에 로드할 수 있습니다. 그러나 3D 기하 도형 데이터의 형식은 다양할 수 있으므로 일부 형식은 3D 인쇄에 적합하지 않을 수 있습니다. Windows 10에서는 모든 3D 인쇄 작업에 3D 제조 형식(.3mf) 파일 형식을 사용합니다.

> [!NOTE]  
> .3mf 파일 형식은 이 자습서에 나와 있는 추가 기능을 제공합니다. 3MF 및 이 형식이 3D 제품 제작자와 소비자에게 제공하는 기능에 대한 자세한 내용은 [3MF 사양](http://3mf.io/what-is-3mf/3mf-specification/)을 참조하세요. Windows 10 API를 사용하여 이러한 기능을 활용하는 방법을 알아보려면 [3MF 패키지 생성](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf) 자습서를 참조하세요.

[3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) 앱에서는 가장 많이 사용하는 3D 형식의 파일을 열고 이를 .3mf 파일로 저장할 수 있습니다. 이 예제에서 파일 형식은 달라질 수 있지만 가장 간단한 솔루션은 3D Builder 앱을 열고 사용자에게 가져온 데이터를 .3mf 파일로 저장한 다음 다시 로드하라는 메시지를 표시하는 것입니다.

> [!NOTE]  
> 3D Builder는 파일 형식을 변환하는 것 외에도 모델을 편집하고, 색상 데이터를 추가하고, 기타 인쇄 관련 작업을 수행하는 간단한 도구를 제공하므로 3D 인쇄를 처리하는 앱에 통합되면 편리합니다.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>3D 인쇄를 위해 모델 데이터 복구

.3mf 형식에서도 모든 3D 모델 데이터 인쇄가 가능한 것은 아닙니다. 프린터에서 채워야 할 공간과 비워 두어야 할 공간을 정확하게 판단하려면 인쇄할 각각의 모델이 이음새가 없는 메시로서, 바깥쪽 면이 수직이고 다중 형상으로 되어야 합니다. 이러한 영역의 문제들은 다양한 형태로 발생하기 때문에 복잡한 셰이프에서는 발견이 어려울 수 있습니다. 하지만 원시 기하 도형을 인쇄 가능한 3D 셰이프로 변환할 수 있는 최신 소프트웨어 솔루션이 많이 나와 있습니다. 모델 *복구*라는 이 과정은 `OnFixClick` 메서드에서 수행됩니다.

[**Printing3DModel**](https://msdn.microsoft.com/library/windows/apps/mt203679) 개체를 생성하는 데 사용할 수 있는 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731)을 구현하려면 3D 데이터 파일을 변환해야 합니다.

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

이제 **Printing3DModel** 개체를 복구 및 인쇄할 수 있습니다. [**SaveModelToPackageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync)를 사용하여 클래스 생성 시 선언한 **Printing3D3MFPackage** 개체에 모델을 할당합니다.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>인쇄 작업 실행: TaskRequested 처리기 만들기


나중에 3D 인쇄 대화 상자가 사용자에게 표시되고 사용자가 인쇄를 시작하면 앱에서 원하는 매개 변수를 3D 인쇄 파이프라인에 전달해야 합니다. 3D 인쇄 API는 **[TaskRequested](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** 이벤트를 발생시킵니다. 이 이벤트를 적절하게 처리하는 메서드를 작성해야 합니다. 처리기 메서드는 항상 해당 이벤트와 동일한 형식이어야 합니다. **TaskRequested** 이벤트에는 [**Print3DManager**](https://msdn.microsoft.com/library/windows/apps/dn998029) 매개 변수(해당 송신자 개체에 대한 참조) 및 대부분의 관련 정보가 포함된 [**Print3DTaskRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn998051) 개체가 있습니다.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

이 메서드의 주요 목적은 *args* 매개 변수를 사용하여 **Printing3D3MFPackage**를 파이프라인 아래쪽으로 보내는 것입니다. **Print3DTaskRequestedEventArgs** 형식에는 [**Request**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtaskrequestedeventargs.request.aspx) 속성 한 가지가 있습니다. 이 속성의 형식은 [**Print3DTaskRequest**](https://msdn.microsoft.com/library/windows/apps/dn998050)이며 하나의 인쇄 작업 요청을 나타냅니다. 해당 메서드 [**CreateTask**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtaskrequest.createtask.aspx)를 사용하면 프로그램에서 인쇄 작업에 대한 올바른 정보를 제출할 수 있으며 3D 인쇄 파이프라인 아래쪽으로 **Print3DTask** 개체에 대한 참조를 반환합니다.

**CreateTask**의 입력 매개 변수로 인쇄 작업 이름에 대한 string 매개 변수, 사용할 프린터 ID에 대한 string 및 [**Print3DTaskSourceRequestedHandler**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtasksourcerequestedhandler.aspx) 대리자가 있습니다. **3DTaskSourceRequested** 이벤트가 발생하면(API에 의해 자동으로 수행됨) 대리자가 자동으로 호출됩니다. 이 대리자는 인쇄 작업이 시작될 때 호출되며 올바른 3D 인쇄 패키지를 제공하는 역할을 한다는 점을 기억해야 합니다.

**Print3DTaskSourceRequestedHandler**에서 사용하는 매개 변수는 전송할 데이터를 제공하는 [**Print3DTaskSourceRequestedArgs**](https://msdn.microsoft.com/library/windows/apps/dn998056) 개체입니다. 이 클래스의 공용 메서드인 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource.aspx)는 인쇄할 패키지를 허용합니다. **Print3DTaskSourceRequestedHandler** 대리자는 다음과 같이 구현됩니다.

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

그런 다음 새로 정의된 대리자 `sourceHandler`를 사용하여 **CreateTask**를 호출합니다.

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

반환된 **Print3DTask**가 시작 부분에 선언된 클래스 변수에 할당됩니다. 이제 이 참조를 사용하여 작업에서 발생한 특정 이벤트를 처리할 수 있습니다(옵션).

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> 이러한 이벤트에 등록하려면 `Task_Submitting` 및 `Task_Completed` 메서드를 구현해야 합니다.

## <a name="execute-printing-task-open-3d-print-dialog"></a>인쇄 작업 실행: 3D 인쇄 대화 상자 열기


필요한 코드의 마지막 부분은 3D 인쇄 대화 상자를 시작합니다. 기존의 인쇄 대화 상자 창과 마찬가지로 3D 인쇄 대화 상자는 다양한 최신 인쇄 옵션을 제공하며 사용자가 사용할 프린터를 선택할 수 있도록 해줍니다(USB 또는 네트워크 연결 여부에 관계없이).

`MyTaskRequested` 메서드를 **TaskRequested** 이벤트에 등록합니다.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

**TaskRequested** 이벤트 처리기를 등록한 후 현재 응용 프로그램 창에 3D 인쇄 대화 상자를 표시하는 [**ShowPrintUIAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.print3dmanager.showprintuiasync.aspx) 메서드를 호출할 수 있습니다.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

마지막으로 앱이 제어를 다시 시작하면 이벤트 처리기의 등록을 취소하는 것이 좋습니다.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>관련 항목

[3MF 패키지 생성](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)  
[3D 인쇄 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 
