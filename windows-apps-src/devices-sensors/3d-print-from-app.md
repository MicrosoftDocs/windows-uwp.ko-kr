---
title: 앱에서 3D 인쇄
description: 유니버설 Windows 앱에 3D 인쇄 기능을 추가 하는 방법에 대해 알아봅니다. 이 항목에서는 3D 모델이 인쇄 가능 하 고 올바른 형식이 되도록 한 후 3D 인쇄 대화 상자를 시작 하는 방법을 설명 합니다.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 이상 인쇄, 3d 인쇄
ms.localizationpriority: medium
ms.openlocfilehash: b89fb14b8e554452674e0c7b0bc31b6314cce253
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175497"
---
# <a name="3d-printing-from-your-app"></a>앱에서 3D 인쇄

**중요 API**

-   [**Printing3D**](/uwp/api/Windows.Graphics.Printing3D)

유니버설 Windows 앱에 3D 인쇄 기능을 추가 하는 방법에 대해 알아봅니다. 이 항목에서는 3d 기 하 도형 데이터를 앱에 로드 하 고 3d 모델이 인쇄 가능 하 고 올바른 형식으로 유지 된 후 3d 인쇄 대화 상자를 시작 하는 방법을 설명 합니다. 이러한 절차의 작업 예제는 [3d 인쇄 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)을 참조 하세요.

> [!NOTE]
> 이 가이드의 샘플 코드에서는 간단한 설명을 위해 오류 보고 및 처리가 상당히 간소화 되었습니다.

## <a name="setup"></a>설치


3D 인쇄 기능을 사용할 수 있는 응용 프로그램 클래스에서 [**Printing3D**](/uwp/api/Windows.Graphics.Printing3D) 네임 스페이스를 추가 합니다.

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

이 가이드에서는 다음의 추가 네임 스페이스를 사용 합니다.

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

그런 다음 클래스에 유용한 멤버 필드를 제공 합니다. 인쇄 드라이버로 전달 될 인쇄 작업을 나타내는 [**Print3DTask**](/uwp/api/Windows.Graphics.Printing3D.Print3DTask) 개체를 선언 합니다. [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 개체를 선언 하 여 앱에 로드 되는 원래 3d 데이터 파일을 저장 합니다. 필요한 모든 메타 데이터가 포함 된 인쇄 준비 3D 모델을 나타내는 [**Printing3D3MFPackage**](/uwp/api/Windows.Graphics.Printing3D.Printing3D3MFPackage) 개체를 선언 합니다.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>간단한 UI 만들기

이 샘플은 파일을 프로그램 메모리로 가져오는 로드 단추, 필요에 따라 파일을 수정 하는 수정 단추 및 인쇄 작업을 시작 하는 인쇄 단추와 같은 세 가지 사용자 정의 컨트롤을 제공 합니다. 다음 코드에서는 .cs 클래스의 해당 XAML 파일에 이러한 단추 (온-클릭 이벤트 처리기 포함)를 만듭니다.

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

UI 피드백을 위한 **TextBlock** 을 추가 합니다.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>3D 데이터 가져오기


앱에서 3D 기 하 도형 데이터를 획득 하는 방법은 다양 합니다. 앱은 3D 검색에서 데이터를 검색 하거나, 웹 리소스에서 모델 데이터를 다운로드 하거나, 수학 수식이 나 사용자 입력을 사용 하 여 프로그래밍 방식으로 3D 메시를 생성할 수 있습니다. 간단히 하기 위해이 가이드에서는 몇 가지 일반적인 파일 형식의 3D 데이터 파일을 장치 저장소의 프로그램 메모리로 로드 하는 방법을 보여 줍니다. [3D 작성기 모델 라이브러리](https://developer.microsoft.com/windows/hardware/3d-print/windows-3d-printing) 는 장치에 쉽게 다운로드할 수 있는 다양 한 모델을 제공 합니다.

`OnLoadClick`메서드에서 [**Fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 클래스를 사용 하 여 단일 파일을 앱의 메모리에 로드 합니다.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>3D 작성기를 사용 하 여 3D 제조 형식 (. 3mf)으로 변환

이 시점에서 응용 프로그램의 메모리에 3D 데이터 파일을 로드할 수 있습니다. 그러나 3D 기 하 도형 데이터는 다양 한 형식으로 제공 될 수 있으며, 일부는 3D 인쇄에 효율적이 지 않습니다. Windows 10에서는 모든 3D 인쇄 작업에 3D 제조 형식 (. 3mf) 파일 형식을 사용 합니다.

> [!NOTE]  
> .3mf 파일 형식은이 자습서에서 설명 하는 것 보다 더 많은 기능을 제공 합니다. 3MF 및 3D 제품의 생산자와 소비자에 게 제공 하는 기능에 대해 자세히 알아보려면 [3Mf 사양을](https://3mf.io/what-is-3mf/3mf-specification/)참조 하세요. Windows 10 Api에서 이러한 기능을 활용 하는 방법을 알아보려면 [3MF 패키지 생성](./generate-3mf.md) 자습서를 참조 하세요.

[3D 작성기](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) 앱은 가장 인기 있는 3d 형식의 파일을 열고. 3mf 파일로 저장할 수 있습니다. 파일 형식이 다를 수 있는이 예제에서 매우 간단한 솔루션은 3D 작성기 앱을 열고 사용자에 게 가져온 데이터를. 3mf 파일로 저장 하 고 다시 로드 하 라는 메시지를 표시 하는 것입니다.

> [!NOTE]  
> 3D 작성기는 파일 형식 변환 외에도 모델을 편집 하 고 색 데이터를 추가 하 고 다른 인쇄 관련 작업을 수행할 수 있는 간단한 도구를 제공 하므로 3D 인쇄를 처리 하는 앱으로 통합 하는 것이 좋습니다.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>3D 인쇄용 모델 데이터 복구

.3mf 형식 에서도 모든 3D 모델 데이터를 인쇄할 수 있는 것은 아닙니다. 프린터에서 채울 공간을 정확 하 게 결정 하 고 빈 채로 둘 수 있도록 하려면 인쇄 될 모델은 단일 원활한 메시, 바깥쪽 표면 법선 및 다 기관의 geometry가 있어야 합니다. 이러한 영역에서 발생 하는 문제는 다양 한 형식에서 발생할 수 있으며 복잡 한 모양에서 매우 어려울 수 있습니다. 그러나 최신 소프트웨어 솔루션은 주로 원시 geometry를 인쇄 가능한 3D 모양으로 변환 하는 데 적합 합니다. 이를 모델 *복구* 라고 하며 메서드에서 수행 됩니다 `OnFixClick` .

3D 데이터 파일은 [**Printing3DModel**](/uwp/api/Windows.Graphics.Printing3D.Printing3DModel) 개체를 생성 하는 데 사용할 수 있는 [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream)을 구현 하도록 변환 해야 합니다.

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

이제 **Printing3DModel** 개체가 복구 되 고 인쇄 가능 합니다. [**SaveModelToPackageAsync**](/uwp/api/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) 를 사용 하 여 클래스를 만들 때 선언한 **Printing3D3MFPackage** 개체에 모델을 할당 합니다.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>인쇄 작업 실행: TaskRequested 처리기 만들기


나중에 3D 인쇄 대화 상자가 사용자에 게 표시 되 고 사용자가 인쇄를 시작할 때 앱은 3D 인쇄 파이프라인에 원하는 매개 변수를 전달 해야 합니다. 3D 인쇄 API가 **[Taskrequested](/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** 이벤트를 발생 시킵니다. 이 이벤트를 적절 하 게 처리 하는 메서드를 작성 해야 합니다. 항상 처리기 메서드는 이벤트와 동일한 형식 이어야 합니다. **Taskrequested** 이벤트에는 매개 변수 [**Print3DManager**](/uwp/api/Windows.Graphics.Printing3D.Print3DManager) (보낸 사람 개체에 대 한 참조)와 대부분의 관련 정보를 포함 하는 [**Print3DTaskRequestedEventArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequestedEventArgs) 개체가 있습니다.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

이 메서드의 핵심 용도는 *args* 매개 변수를 사용 하 여 파이프라인 **Printing3D3MFPackage** 을 보내는 것입니다. **Print3DTaskRequestedEventArgs** 형식에는 [**Request**](/uwp/api/windows.graphics.printing3d.print3dtaskrequestedeventargs.request)속성이 하나 있습니다. [**Print3DTaskRequest**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequest) 형식 이며 하나의 인쇄 작업 요청을 나타냅니다. 이러한 메서드 [**createtask**](/uwp/api/windows.graphics.printing3d.print3dtaskrequest.createtask) 를 사용 하면 프로그램에서 인쇄 작업에 대 한 올바른 정보를 제출할 수 있으며, 3d 인쇄 파이프라인으로 전송 된 **Print3DTask** 개체에 대 한 참조를 반환 합니다.

**Createtask** 에는 다음과 같은 입력 매개 변수가 있습니다. 인쇄 작업 이름에 대 한 문자열, 사용할 프린터 ID의 문자열 및 [**Print3DTaskSourceRequestedHandler**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedhandler) 대리자입니다. 대리자는 **3DTaskSourceRequested** 이벤트가 발생할 때 자동으로 호출 됩니다 (API 자체에서 수행 됨). 중요 한 점은 인쇄 작업이 시작 될 때이 대리자가 호출 되 고 올바른 3D 인쇄 패키지를 제공 해야 한다는 것입니다.

**Print3DTaskSourceRequestedHandler** 는 전송할 데이터를 제공 하는 [**Print3DTaskSourceRequestedArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskSourceRequestedArgs) 개체인 매개 변수 하나를 사용 합니다. 이 클래스의 공용 메서드인 [**SetSource**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource)은 인쇄 될 패키지를 허용 합니다. 다음과 같이 **Print3DTaskSourceRequestedHandler** 대리자를 구현 합니다.

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

그런 다음 새로 정의 된 대리자를 사용 하 여 **createtask**호출 `sourceHandler` 합니다.

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

반환 된 **Print3DTask** 는 처음에 선언 된 클래스 변수에 할당 됩니다. 이제이 참조를 사용 하 여 작업에서 throw 되는 특정 이벤트를 처리할 수 있습니다 (옵션).

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> `Task_Submitting` `Task_Completed` 이러한 이벤트에 등록 하려는 경우 및 메서드를 구현 해야 합니다.

## <a name="execute-printing-task-open-3d-print-dialog"></a>인쇄 작업 실행: 3D 인쇄 대화 상자를 엽니다.


필요한 코드의 마지막 부분은 3D 인쇄 대화 상자를 시작 하는 것입니다. 기존 인쇄 대화 상자 창과 마찬가지로 3D 인쇄 대화 상자에는 몇 분 분량의 인쇄 옵션을 제공 하 고 사용자가 사용할 프린터를 선택할 수 있습니다 (USB 또는 네트워크를 통해 연결 됨).

`MyTaskRequested` **Taskrequested** 이벤트를 사용 하 여 메서드를 등록 합니다.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

**Taskrequested** 이벤트 처리기를 등록 한 후에는 현재 응용 프로그램 창에서 3d 인쇄 대화 상자를 표시 하는 [**Showprintuiasync**](/uwp/api/windows.graphics.printing3d.print3dmanager.showprintuiasync)메서드를 호출할 수 있습니다.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

마지막으로, 앱이 다시 시작 되 면 이벤트 처리기를 등록 취소 하는 것이 좋습니다.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>관련 항목

[3MF 패키지 생성](./generate-3mf.md)  
[3D 인쇄 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 