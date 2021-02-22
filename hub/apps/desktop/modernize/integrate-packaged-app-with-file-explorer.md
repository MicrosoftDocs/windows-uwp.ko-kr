---
description: 이 문서에서는 패키지 확장을 사용하여 패키지형 데스크톱 앱을 파일 탐색기와 통합하는 다양한 방법을 보여줍니다.
title: 패키지형 데스크톱 앱을 파일 탐색기와 통합
ms.date: 02/08/2021
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: e3e7aaffc86a152530c933291321c30c2e7c507d
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/12/2021
ms.locfileid: "100335199"
---
# <a name="integrate-a-packaged-desktop-app-with-file-explorer"></a>패키지형 데스크톱 앱을 파일 탐색기와 통합

일부 Windows 앱은 고객이 앱과 관련된 옵션을 사용할 수 있게 해주는 상황에 맞는 메뉴 항목을 추가하는 파일 탐색기 확장을 정의합니다. MSI이나 ClickOnce 같은 이전 Windows 앱 배포 기술은 레지스트리를 통해 파일 탐색기 확장을 정의합니다. 레지스트리에는 파일 탐색기 확장 및 다른 종류의 셸 확장을 제어하는 일련의 하이브가 포함되어 있습니다. 이러한 설치 관리자는 일반적으로 상황에 맞는 메뉴에 포함할 다양한 항목을 구성하는 일련의 레지스트리 키를 만듭니다.

[MSIX](/windows/msix/)를 사용하여 Windows 앱을 패키징하면 레지스트리가 가상화되므로 앱이 레지스트리를 통해 파일 탐색기 확장을 등록할 수 없습니다. 따라서 패키지 매니페스트에서 정의하는 패키지 확장을 통해 파일 탐색기 확장을 정의해야 합니다. 이 문서에서는 이 작업을 수행하는 여러 가지 방법을 설명합니다.

이 문서에 사용되는 전체 샘플 코드는 [GitHub](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample)에서 찾을 수 있습니다.

## <a name="add-a-context-menu-entry-that-supports-startup-parameters"></a>시작 매개 변수를 지원하는 상황에 맞는 메뉴 항목 추가

파일 탐색기와 통합하는 가장 간단한 방법 중 하나는 사용자가 파일 탐색기에서 특정 파일 형식을 마우스 오른쪽 단추로 클릭하면 상황에 맞는 메뉴의 사용 가능한 앱 목록에 앱을 추가하는 패키지 확장을 정의하는 것입니다. 사용자가 앱을 열면 확장이 앱에 매개 변수를 전달할 수 있습니다.

이 시나리오는 다음과 같은 여러 가지 제한 사항이 있습니다.

* [파일 형식 연결 기능](/windows/uwp/launch-resume/handle-file-activation)과 함께 사용해야만 작동합니다. 기본 앱과 연결된 파일 형식만(예: 파일 탐색기에서 파일을 두 번 클릭하여 파일을 열 수 있는 앱) 상황에 맞는 메뉴에 추가 옵션을 표시할 수 있습니다.
* 상황에 맞는 메뉴의 옵션은 앱이 해당 파일 형식의 기본값으로 설정된 경우에만 표시됩니다.
* 유일하게 지원되는 작업은 앱의 기본 실행 파일(즉, 시작 메뉴 항목에 연결된 것과 동일한 실행 파일)을 시작하는 것입니다. 그러나 작업마다 다른 매개 변수를 지정할 수 있으며, 앱이 실행을 트리거한 작업을 파악하고 다른 작업을 수행하기 시작할 때 이러한 매개 변수를 사용할 수 있습니다.

이러한 제한에도 불구하고 이 접근 방식은 여러 시나리오에 사용하기에 충분합니다. 예를 들어 이미지 편집기를 빌드하는 경우에는 이미지 크기를 조정하는 항목을 상황에 맞는 메뉴에 쉽게 추가할 수 있습니다. 이렇게 하면 마법사를 통해 직접 이미지 편집기를 시작하여 크기 조정 프로세스를 시작할 수 있습니다.

### <a name="implement-the-context-menu-entry"></a>상황에 맞는 메뉴 항목 구현

이 시나리오를 지원하려면 범주가 `windows.fileTypeAssociation`인 [Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-extension) 요소를 패키지 매니페스트에 추가합니다. 이 요소는 [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 요소 아래에 [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 요소의 자식으로 추가해야 합니다.

다음 예제에서는 확장명이 `.foo`인 파일에 상황에 맞는 메뉴를 사용하도록 설정하는 앱의 등록을 보여줍니다. 이 예제에서는 일반적으로 지정된 컴퓨터의 다른 앱에 등록되지 않는 가짜 확장인 `.foo` 확장을 지정합니다. 이미 사용 중일 수도 있는 파일 형식(예: .txt 또는 .jpg)을 관리해야 하는 경우 앱을 해당 파일 형식의 기본값으로 설정하기 전에는 옵션을 볼 수 없다는 점에 주의해야 합니다. 이 예제는 GitHub의 관련 샘플에 들어 있는 [Package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) 파일에서 발췌한 것입니다.

```xml
<Extensions>
  <uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="foo" Parameters="&quot;%1&quot;">
      <uap:SupportedFileTypes>
        <uap:FileType>.foo</uap:FileType>
      </uap:SupportedFileTypes>
      <uap2:SupportedVerbs>
        <uap3:Verb Id="Resize" Parameters="&quot;%1&quot; /p">Resize file</uap3:Verb>
      </uap2:SupportedVerbs>
    </uap3:FileTypeAssociation>
  </uap3:Extension>
</Extensions>
```

이 예제에서는 매니페스트의 루트 `<Package>` 요소에 다음 네임스페이스와 별칭이 선언된 것으로 가정합니다.

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap uap2 uap3 rescap">
  ...
</Package>
```

[FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 요소는 앱을 지원하려는 파일 형식과 연결합니다. 자세한 내용은 [패키지된 애플리케이션을 파일 형식 세트와 연결](desktop-to-uwp-extensions.md#associate-your-packaged-application-with-a-set-of-file-types)을 참조하세요. 다음은 이 요소와 관련된 가장 중요한 항목입니다.

| 특성 또는 요소 | 설명 |
|----------------------|-------------|
| `Name` 특성 | 등록하려는 확장의 이름에서 점을 뺀 이름(이전 예제에서는 `foo`)을 매칭합니다. |
| `Parameters` 특성 | 사용자가 이러한 확장명을 가진 파일을 두 번 클릭할 때 애플리케이션에 전달하려는 매개 변수를 포함합니다. 적어도 `%1` 매개 변수를 전달하는 것이 일반적입니다. 이 매개 변수는 선택한 파일의 경로를 포함합니다. 이러한 방식으로 파일을 두 번 클릭하면 애플리케이션에서 전체 경로를 인식하고 로드할 수 있습니다. |
| [SupportedFileTypes](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-supportedfiletypes) 요소 | 등록하려는 확장의 이름을 점까지 포함하여 지정합니다(이 예제에서는 `.foo`). `<FileType>` 항목을 여러 개 지정하여 더 많은 파일 형식을 지원할 수 있습니다. |

상황에 맞는 메뉴 통합을 정의하려면 [SupportedVerbs](/uwp/schemas/appxpackage/uapmanifestschema/element-uap2-supportedverbs) 자식 요소도 추가해야 합니다. 이 요소는 사용자가 파일 탐색기에서 확장명이 .foo인 파일을 마우스 오른쪽 단추로 클릭할 때 표시되는 옵션을 정의하는 [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 요소를 하나 이상 포함합니다. 자세한 내용은 [특정 형식의 파일 상황에 맞는 메뉴에 옵션 추가](desktop-to-uwp-extensions.md#add-options-to-the-context-menus-of-files-that-have-a-certain-file-type)를 참조하세요. 다음은 [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 요소와 관련된 가장 중요한 항목입니다.

| 특성 또는 요소 | 설명 |
|----------------------|-------------|
| `Id` 특성 | 작업의 고유 식별자를 지정합니다.|
| `Parameters` 특성 | [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 요소와 마찬가지로 [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 요소에는 사용자가 상황에 맞는 메뉴 항목을 클릭할 때 애플리케이션에 전달되는 매개 변수가 포함됩니다. 일반적으로 선택한 파일의 경로를 가져오는 특수 매개 변수 `%1` 외에는 매개 변수를 하나 이상 전달하여 컨텍스트를 가져옵니다. 이렇게 하면 상황에 맞는 메뉴 항목에서 열렸다는 것을 앱이 알 수 있습니다.  |
| 요소 값 | [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 요소의 값에는 상황에 맞는 메뉴 항목에 표시할 레이블(이 예제에서는 **파일 크기 조정**)이 포함됩니다. |

### <a name="access-the-startup-parameters-in-your-app-code"></a>앱 코드에서 시작 매개 변수에 액세스

앱이 매개 변수를 받는 방법은 개발자가 만든 앱 유형에 따라 달라집니다. 예를 들어 WPF 앱은 일반적으로 `OnStartup` 클래스의 `App` 메서드에서 시작 이벤트 인수를 처리합니다. 시작 매개 변수가 있는지 확인하고 그 결과에 따라 가장 적절한 작업을 수행하면 됩니다(예: 기본 애플리케이션 대신 애플리케이션의 특정 창 열기).

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        if (e.Args.Contains("Resize"))
        {
            // Open a specific window of the app.
        }
        else
        {
            MainWindow main = new MainWindow();
            main.Show();
        }
    }
}
```

다음 스크린샷은 이전 예제에서 만든 **파일 크기 조정** 상황에 맞는 메뉴 항목을 보여줍니다.

![바로 가기 메뉴의 파일 크기 조정 명령 스크린샷](images/file-explorer/resize-file.png)

## <a name="support-generic-files-or-folders-and-perform-complex-tasks"></a>일반 파일 또는 폴더를 지원하고 복잡한 작업 수행

이전 섹션에서 설명한 대로 대부분의 시나리오에서는 패키지 매니페스트에 [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 확장을 사용하는 것으로 충분하지만, 약간의 제한이 따를 수 있습니다. 가장 큰 두 가지 문제는 다음과 같습니다.

* 연결된 파일 형식만 처리할 수 있습니다. 예를 들어 일반 폴더를 처리할 수 없습니다.
* 일련의 매개 변수로 앱을 시작하는 것만 가능합니다. 기본 앱을 열지 않고 다른 실행 파일을 시작하거나 작업을 수행하는 등의 고급 작업을 수행할 수 없습니다.

이러한 과제를 해결하려면 파일 탐색기와 통합하는 보다 강력한 방법을 제공하는 [셸 확장](/windows/win32/shell/shell-exts)을 만들어야 합니다. 이 시나리오에서는 레이블, 아이콘, 상태 및 수행할 작업을 포함하여 파일 상황에 맞는 메뉴를 관리하는 데 필요한 모든 것이 포함된 DLL을 만들어야 합니다. 이 기능은 DLL에서 구현되므로 일반 앱으로 수행할 수 있는 거의 모든 것을 할 수 있습니다. DLL을 구현한 후에는 패키지 매니페스트에 정의하는 확장을 통해 등록해야 합니다.

> [!NOTE]
> 이 섹션에서 설명하는 프로세스에는 한 가지 제한이 있습니다. 확장이 포함된 MSIX 패키지를 대상 컴퓨터에 설치한 후에는 셸 확장을 로드하기 전에 파일 탐색기를 다시 시작해야 합니다. 이렇게 하려면 사용자는 컴퓨터를 다시 시작하거나 **작업 관리자** 를 사용하여 **explorer.exe** 프로세스를 다시 시작하면 됩니다.

### <a name="implement-the-shell-extension"></a>셸 확장 구현

셸 확장은 [COM(구성 요소 개체 모델)](/windows/win32/com/component-object-model--com--portal)을 기반으로 합니다. DLL은 시스템 레지스트리에 등록된 하나 이상의 COM 개체를 공개합니다. Windows는 이러한 COM 개체를 탐색하고 확장을 파일 탐색기와 통합합니다. 우리는 코드를 Windows 셸과 통합할 것이므로 성능 및 메모리 공간이 매우 중요합니다. 따라서 이러한 종류의 확장은 일반적으로 C++로 빌드합니다.

셸 확장을 구현하는 방법을 보여주는 샘플 코드는 GitHub의 관련 샘플에 제공된 [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb) 프로젝트를 참조하세요. 이 프로젝트는 Windows 데스크톱 샘플의 [이 샘플](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb)을 기반으로 하며, 최신 버전의 Visual Studio에서 샘플을 보다 쉽게 사용할 수 있도록 여러 수정 버전이 있습니다.

이 프로젝트에는 동적/정적 메뉴, DLL 수동 등록과 같은 여러 작업을 위한 다양한 상용구 코드가 포함되어 있습니다. MSIX를 사용하여 앱을 패키징하는 경우 패키징 지원에서 작업을 알아서 처리하므로 이 코드는 대부분 필요 없습니다. [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/ExplorerCommandVerb.cpp) 파일은 상황에 맞는 메뉴의 구현을 포함하고 있으며, 이 연습의 주요 코드 파일입니다.

핵심 함수는 `CExplorerCommandVerb::Invoke`입니다. 이 함수는 사용자가 상황에 맞는 메뉴의 항목을 클릭할 때 호출됩니다. 이 샘플에서는 성능에 미치는 영향을 최소화하기 위해 작업을 다른 스레드에서 수행하므로, 실제 구현은 `CExplorerCommandVerb::_ThreadProc`에서 찾을 수 있습니다.

```cpp
DWORD CExplorerCommandVerb::_ThreadProc()
{
    IShellItemArray* psia;
    HRESULT hr = CoGetInterfaceAndReleaseStream(_pstmShellItemArray, IID_PPV_ARGS(&psia));
    _pstmShellItemArray = NULL;
    if (SUCCEEDED(hr))
    {
        DWORD count;
        psia->GetCount(&count);

        IShellItem2* psi;
        HRESULT hr = GetItemAt(psia, 0, IID_PPV_ARGS(&psi));
        if (SUCCEEDED(hr))
        {
            PWSTR pszName;
            hr = psi->GetDisplayName(SIGDN_DESKTOPABSOLUTEPARSING, &pszName);
            if (SUCCEEDED(hr))
            {
                WCHAR szMsg[128];
                StringCchPrintf(szMsg, ARRAYSIZE(szMsg), L"%d item(s), first item is named %s", count, pszName);

                MessageBox(_hwnd, szMsg, L"ExplorerCommand Sample Verb", MB_OK);

                CoTaskMemFree(pszName);
            }

            psi->Release();
        }
        psia->Release();
    }

    return 0;
}
```

사용자가 파일 또는 폴더를 마우스 오른쪽 단추로 클릭하면 이 함수는 선택한 파일 또는 폴더의 전체 경로가 포함된 메시지 상자를 표시합니다. 다른 방법으로 셸 확장을 사용자 지정하려면 샘플의 다음 함수를 확장하면 됩니다.

- [GetTitle](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettitle) 함수를 변경하여 상황에 맞는 메뉴의 항목의 레이블을 사용자 지정할 수 있습니다.
- [GetIcon](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-geticon) 함수를 변경하여 상황에 맞는 메뉴의 항목 근처에 표시되는 아이콘을 사용자 지정할 수 있습니다.
- [GetTooltip](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettooltip) 함수를 변경하여 상황에 맞는 메뉴의 항목을 가리킬 때 표시되는 도구 설명을 사용자 지정할 수 있습니다.

### <a name="register-the-shell-extension"></a>셸 확장 등록

셸 확장은 COM 기반이므로 구현 DLL을 COM 서버로 공개해야만 Windows에서 파일 탐색기와 통합할 수 있습니다. 일반적으로 이 작업은 COM 서버에 고유 ID(CLSID)를 할당하고 시스템 레지스트리의 특정 하이브에 등록하여 수행합니다. [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb) 프로젝트에서 `CExplorerCommandVerb` 확장의 CLSID는 [Dll.h](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/Dll.h) 파일에 정의됩니다.

```cpp
class __declspec(uuid("CC19E147-7757-483C-B27F-3D81BCEB38FE")) CExplorerCommandVerb;
```

셸 확장 DLL을 MSIX 패키지에 패키징하는 경우 비슷한 방식을 따릅니다. 그러나 [여기](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)에 설명된 대로 GUID는 레지스트리 대신 패키지 매니페스트 내부에 등록되어야 합니다.

패키지 매니페스트에서 **Package** 요소에 다음 네임스페이스를 추가하는 것부터 시작합니다.

```xml
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:desktop5="http://schemas.microsoft.com/appx/manifest/desktop/windows10/5"
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10" 
  IgnorableNamespaces="desktop desktop4 desktop5 com">
    
    ...
</Package>
```

CLSID를 등록하려면 범주가 `windows.comServer`인 [com.Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) 요소를 패키지 매니페스트에 추가합니다. 이 요소는 [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 요소 아래에 [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 요소의 자식으로 추가해야 합니다. 이 예제는 GitHub의 관련 샘플에 들어 있는 [Package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) 파일에서 발췌한 것입니다.

```xml
<com:Extension Category="windows.comServer">
  <com:ComServer>
    <com:SurrogateServer DisplayName="ContextMenuSample">
      <com:Class Id="CC19E147-7757-483C-B27F-3D81BCEB38FE" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
    </com:SurrogateServer>
  </com:ComServer>
</com:Extension>
```

[com:Class](/uwp/schemas/appxpackage/uapmanifestschema/element-com-surrogateserver-class) 요소에서 구성해야 하는 두 가지 중요한 특성이 있습니다.

| attribute | 설명 |
|----------------------|-------------|
| `Id` 특성 | 등록하려는 개체의 CLSID와 일치해야 합니다. 이 예제에서는 `CExplorerCommandVerb` 클래스와 연결된 `Dll.h` 파일에 선언된 CLSID입니다. |
| `Path` 특성 | COM 개체를 공개하는 DLL의 이름을 포함해야 합니다. 이 예제에서는 패키지의 루트에 DLL이 포함되어 있으므로 `ExplorerCommandVerb` 프로젝트에서 생성한 DLL 이름만 지정할 수 있습니다. |

다음으로 파일 상황에 맞는 메뉴를 등록하는 또 다른 확장을 추가합니다. 이렇게 하려면 범주가 `windows.fileExplorerContextMenus`인 [desktop4:Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension) 요소를 패키지 매니페스트에 추가합니다. 이 요소 역시 [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 요소 아래에 [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 요소의 자식으로 추가해야 합니다.

```xml
<desktop4:Extension Category="windows.fileExplorerContextMenus">
  <desktop4:FileExplorerContextMenus>
    <desktop5:ItemType Type="Directory">
      <desktop5:Verb Id="Command1" Clsid="CC19E147-7757-483C-B27F-3D81BCEB38FE" />
    </desktop5:ItemType>
  </desktop4:FileExplorerContextMenus>
</desktop4:Extension>
```

[desktop4:Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension) 요소에서 구성해야 하는 두 가지 중요한 특성이 있습니다.

| 특성 또는 요소 | 설명 |
|----------------------|-------------|
| [desktop5:ItemType](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-itemtype)의 `Type` 특성 | 상황에 맞는 메뉴와 연결할 항목의 형식을 정의합니다. 모든 파일에 대해 표시하도록 별표(`*`)로 할 수도 있고, 특정 파일 확장명(`.foo`)으로 할 수도 있고, 폴더(`Directory`)에 사용할 수도 있습니다. |
| [desktop5:Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-verb)의 `Clsid` 특성 | 이전에 패키지 매니페스트 파일에 COM 서버로 등록한 CLSID와 일치해야 합니다. |

### <a name="configure-the-dll-in-the-package"></a>패키지의 DLL 구성

MSIX 패키지의 루트에서 셸 확장을 구현하는 DLL(이 샘플에서는 **ExplorerCommandVerb.dll**)을 포함시킵니다. [Windows 애플리케이션 패키징 프로젝트](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 사용하는 경우 가장 쉬운 방법은 DLL을 복사하여 프로젝트에 붙여넣고, DLL 파일 속성의 **출력 디렉터리로 복사** 옵션을 **변경된 내용만 복사** 로 설정하는 것입니다.

패키지에 항상 최신 버전의 DLL이 포함되도록 하려면 셸 확장 프로젝트에 [빌드 후 이벤트](/visualstudio/ide/specifying-custom-build-events-in-visual-studio)를 추가하면 됩니다. 그러면 빌드할 때마다 DLL이 Windows 애플리케이션 패키징 프로젝트에 복사됩니다.

### <a name="restart-file-explorer"></a>파일 탐색기 다시 시작

셸 확장 패키지를 설치한 후에는 셸 확장을 로드하기 전에 파일 탐색기를 다시 시작해야 합니다. 이는 MSIX 패키지를 통해 배포 및 등록되는 셸 확장의 제한 사항입니다.

셸 확장을 테스트하려면 PC를 다시 시작하거나 **작업 관리자** 를 사용하여 **explorer.exe** 프로세스를 다시 시작합니다. 그러면 상황에 맞는 메뉴에 항목이 표시됩니다.

![사용자 지정 상황에 맞는 메뉴 항목의 스크린샷](images/file-explorer/folder-context-menu.png)

항목을 클릭하면 선택한 폴더의 경로를 사용하여 메시지 상자를 표시하는 `CExplorerCommandVerb::_ThreadProc` 함수가 호출됩니다.

![사용자 지정 팝업의 스크린샷](images/file-explorer/popup.png)
