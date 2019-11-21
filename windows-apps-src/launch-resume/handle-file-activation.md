---
title: 파일 활성화 처리
description: 앱을 특정 파일 형식의 기본 처리기로 등록할 수 있습니다.
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 079746d3c1619fe940ba243410f0247b7b850ed9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259452"
---
# <a name="handle-file-activation"></a>파일 활성화 처리

**중요 API**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)

Your app can register to become the default handler for a certain file type. Windows 데스크톱 응용 프로그램과 UWP(유니버설 Windows 플랫폼) 앱 모두 기본 파일 처리기로 등록할 수 있습니다. 사용자가 앱을 특정 파일 형식의 기본 처리기로 선택하면 해당 형식의 파일을 시작할 때 앱이 활성화됩니다.

해당 형식의 파일에 대해 모든 파일 시작을 처리하려는 경우에만 파일 형식을 등록하는 것이 좋습니다. 앱에서 파일 형식을 내부적으로만 사용해야 할 경우에는 기본 처리기로 등록할 필요가 없습니다. 파일 형식을 등록할 경우에는 앱이 해당 파일 형식에 대해 활성화될 때 기대되는 기능을 최종 사용자에게 제공해야 합니다. 예를 들어 사진 뷰어 앱은 .jpg 파일을 표시하도록 등록할 수 있습니다. 파일 연결에 대한 자세한 내용은 [파일 형식 및 URI에 대한 지침](https://docs.microsoft.com/windows/uwp/files/index)을 참조하세요.

다음 단계에서는 사용자 지정 파일 형식인 .alsdk를 등록하는 방법 및 사용자가 .alsdk 파일을 시작할 때 앱을 활성화하는 방법을 보여 줍니다.

> **Note**  In UWP apps, certain URIs and file extensions are reserved for use by built-in apps and the operating system. 예약된 URI 또는 파일 확장명에 앱을 등록하려고 하면 무시됩니다. 자세한 내용은 [예약된 파일 및 URI 스키마 이름](reserved-uri-scheme-names.md)을 참조하세요.

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>1단계: 패키지 매니페스트에서 확장점 지정

앱은 패키지 매니페스트에 나열된 파일 확장명에 대해서만 활성화 이벤트를 받습니다. 다음은 앱이 `.alsdk` 확장명을 가진 파일을 처리하도록 지정하는 방법입니다.

1.  **솔루션 탐색기**에서 package.appxmanifest를 두 번 클릭하여 매니페스트 디자이너를 엽니다. **선언** 탭을 선택하고 **사용 가능한 선언** 드롭다운 목록에서 **파일 형식 연결**을 선택한 다음 **추가**를 클릭합니다. 파일 연결에서 사용하는 식별자에 대한 자세한 내용은 [프로그래밍 ID](https://docs.microsoft.com/windows/desktop/shell/fa-progids)를 참조하세요.

    다음은 매니페스트 디자이너에서 입력할 수 있는 각 필드에 대한 간략한 설명입니다.

| 필드 | 설명 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Display Name** | 파일 형식 그룹에 대한 표시 이름을 지정합니다. 표시 이름은 **제어판**의 [기본 프로그램 설정](https://docs.microsoft.com/windows/desktop/shell/default-programs)에서 파일 형식을 식별하는 데 사용됩니다. |
| **Logo** | 데스크톱 및 **제어판**의 [기본 프로그램 설정](https://docs.microsoft.com/windows/desktop/shell/default-programs)에서 파일 형식을 식별하는 데 사용되는 로고를 지정합니다. 로고를 지정하지 않으면 응용 프로그램의 작은 로고가 사용됩니다. |
| **Info Tip** | 파일 형식 그룹에 대한 [정보 팁](https://docs.microsoft.com/windows/desktop/shell/fa-progids)을 지정합니다. 이 도구 설명 텍스트는 사용자가 이 파일 형식의 아이콘을 가리키면 표시됩니다. |
| **이름** | 동일한 표시 이름, 로고, 정보 팁 및 편집 플래그를 공유하는 파일 형식 그룹의 이름을 선택합니다. 앱 업데이트 간에 동일하게 유지될 수 있는 그룹 이름을 선택합니다. **참고** 이름은 모두 소문자여야 합니다. |
| **Content Type** | 특정 파일 형식에 대해 **image/jpeg** 같은 MIME 콘텐츠 형식을 지정합니다. **허용된 콘텐츠 형식에 대한 중요 정보:** 다음은 예약되거나 금지되어 있기 때문에 패키지 매니페스트에 입력할 수 없는 MIME 콘텐츠 형식의 사전순 목록입니다. **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **File type** | 앞에 마침표를 추가하여 등록할 파일 형식을 지정합니다(예제: ".jpeg"). **예약되거나 금지된 파일 형식** 예약되거나 금지되어 UWP 앱에 등록할 수 없는 기본 제공 앱의 파일 형식에 대한 사전순 목록은 [예약된 URI 체계 이름 및 파일 형식](reserved-uri-scheme-names.md)을 참조하세요. |

2.  **이름**으로 `alsdk`를 입력합니다.
3.  **파일 형식**으로 `.alsdk`를 입력합니다.
4.  Enter “images\\Icon.png” as the Logo.
5.  Ctrl+S를 눌러 package.appxmanifest에 변경 사항을 저장합니다.

위 단계는 이와 같은 [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) 요소를 패키지 매니페스트에 추가합니다. **windows.fileTypeAssociation** 범주는 앱이 `.alsdk` 확장명을 가진 파일을 처리한다는 것을 나타냅니다.

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## <a name="step-2-add-the-proper-icons"></a>2단계: 적절한 아이콘 추가

파일 형식의 기본값이 되는 앱에는 시스템 전체의 다양한 위치에 표시되는 아이콘이 있습니다. 예를 들어 다음과 같은 아이콘이 표시됩니다.

-   Windows 탐색기 항목 보기, 상황에 맞는 메뉴 및 리본
-   기본 프로그램 제어판
-   파일 선택기
-   시작 화면의 검색 결과

이러한 위치에 로고가 표시될 수 있도록 프로젝트에 44x44 아이콘을 포함합니다. 앱 타일 로고의 모양을 일치시키고 아이콘을 투명으로 설정하는 대신 앱의 배경색을 사용합니다. 로고를 안쪽 여백 없이 가장자리로 확장합니다. 흰색 배경에서 아이콘을 테스트합니다. 아이콘에 대한 자세한 내용은 [타일 및 아이콘 자산에 대한 지침](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets)을 참조하세요.

## <a name="step-3-handle-the-activated-event"></a>3단계: 활성화된 이벤트 처리

[  **OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated) 이벤트 처리기는 모든 파일 활성화 이벤트를 받습니다.

```csharp
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```

```cppwinrt
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs const& args)
{
    // TODO: Handle file activation.
    auto numberOfFilesReceived{ args.Files().Size() };
    auto nameOfTheFirstFile{ args.Files().GetAt(0).Name() };
}
```

```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
    // TODO: Handle file activation
    // The number of files received is args->Files->Size
    // The name of the first file is args->Files->GetAt(0)->Name
}
```

> [!NOTE]
> When launched via File Contract, make sure that Back button takes the user back to the screen that launched the app and not to the app's previous content.

We recommend that you create a new XAML **Frame** for each activation event that opens a new page. That way, the navigation backstack for the new XAML Frame doesn't contain any previous content that the app might have on the current window when suspended. If you decide to use a single XAML **Frame** for Launch and for File Contracts, then you should clear the pages in the **Frame**'s navigation journal before navigating to a new page.

When your app is launched via File activation, you should consider including UI that allows the user to go back to the top page of the app.

## <a name="remarks"></a>설명

받게 되는 파일은 신뢰할 수 없는 원본에서 올 수 있으므로 파일에 대한 작업을 수행하기 전에 파일 내용의 유효성을 검사하는 것이 좋습니다.

## <a name="related-topics"></a>관련 항목

### <a name="complete-example"></a>전체 예제

* [Association launching sample](https://code.msdn.microsoft.com/windowsapps/Association-Launching-535d2cec)

### <a name="concepts"></a>개념

* [Default Programs](https://docs.microsoft.com/windows/desktop/shell/default-programs)
* [File Type and Protocol Associations Model](https://docs.microsoft.com/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>작업

* [파일에 대한 기본 앱 시작](launch-the-default-app-for-a-file.md)
* [URI 활성화 처리](handle-uri-activation.md)

### <a name="guidelines"></a>지침

* [Guidelines for file types and URIs](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>참고자료
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows.UI.Xaml.Application.OnFileActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)
