---
author: TylerMSFT
title: URI 활성화 처리
description: 앱을 URI(Uniform Resource Identifier) 체계 이름의 기본 처리기로 등록하는 방법을 알아봅니다.
ms.assetid: 92D06F3E-C8F3-42E0-A476-7E94FD14B2BE
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 808242fd7e179c225b3119dad146e7f05d72ffd4
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5883992"
---
# <a name="handle-uri-activation"></a>URI 활성화 처리

**중요 API**

-   [**Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)
-   [**Windows.UI.Xaml.Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)

앱을 URI(Uniform Resource Identifier) 스키마 이름의 기본 처리기로 등록하는 방법을 알아봅니다. Windows 데스크톱 앱과 UWP(유니버설 Windows 플랫폼) 앱 모두 URI 스키마 이름의 기본 처리기로 등록할 수 있습니다. 사용자가 앱을 URI 스키마 이름의 기본 처리기로 선택하면 해당 형식의 URI를 시작할 때마다 앱이 활성화됩니다.

해당 형식의 URI 스키마에 대해 모든 URI 시작을 처리하려는 경우에만 URI 스키마 이름을 등록하는 것이 좋습니다. URI 스키마 이름을 등록할 경우에는 앱이 해당 URI 스키마에 대해 활성화될 때 기대되는 기능을 최종 사용자에게 제공해야 합니다. 예를 들어 mailto: URI 스키마 이름에 대해 등록된 앱은 사용자가 새 메일을 작성할 수 있도록 새 메일 메시지로 열려야 합니다. URI 연결에 대한 자세한 내용은 [파일 형식 및 URI에 대한 지침 및 검사 목록](https://msdn.microsoft.com/library/windows/apps/hh700321)을 참조하세요.

다음 단계에서는 사용자 지정 URI 스키마 이름인 `alsdk://`를 등록하는 방법 및 사용자가 `alsdk://` URI를 시작할 때 앱을 활성화하는 방법을 보여 줍니다.

> [!NOTE]
> UWP 앱에서 특정 Uri 및 파일 확장명은 기본 제공 앱과 운영 체제에서 사용 하기 위해 예약 되어 있습니다. 예약된 URI 또는 파일 확장명에 앱을 등록하려고 하면 무시됩니다. 예약되거나 금지되어 UWP 앱에 등록할 수 없는 URI 스키마의 사전순 목록은 [예약된 URI 스키마 이름 및 파일 형식](reserved-uri-scheme-names.md)을 참조하세요.

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>1단계: 패키지 매니페스트에서 확장점 지정

앱은 패키지 매니페스트에 나열된 URI 스키마 이름에 대해서만 활성화 이벤트를 받습니다. 다음은 앱이 `alsdk` URI 스키마 이름을 처리하도록 지정하는 방법입니다.

1. **솔루션 탐색기**에서 package.appxmanifest를 두 번 클릭하여 매니페스트 디자이너를 엽니다. **선언** 탭을 선택하고 **사용 가능한 선언** 드롭다운 목록에서 **프로토콜**을 선택한 다음 **추가**를 클릭합니다.

    다음은 프로토콜에 대한 매니페스트 디자이너에서 입력할 수 있는 각 필드에 대한 간략한 설명입니다(자세한 내용은 [**AppX Package Manifest**](https://msdn.microsoft.com/library/windows/apps/dn934791) 참조).

| 필드 | 설명 |
|-------|-------------|
| **로고** | **제어판**의 [기본 프로그램 설정](https://msdn.microsoft.com/library/windows/desktop/cc144154)에서 URI 스키마 이름을 식별하는 데 사용되는 로고를 지정합니다. 로고를 지정하지 않으면 앱의 작은 로고가 사용됩니다. |
| **표시 이름** | **제어판**의 [기본 프로그램 설정](https://msdn.microsoft.com/library/windows/desktop/cc144154)에서 URI 스키마 이름을 식별하는 표시 이름을 지정합니다. |
| **이름** | Uri 스키마 이름을 선택합니다. |
|  | **참고** 이름은 모두 소문자여야 합니다. |
|  | **예약되거나 금지된 파일 형식** 예약되거나 금지되어 UWP 앱에 등록할 수 없는 URI 체계의 사전순 목록은 [예약된 URI 체계 이름 및 파일 형식](reserved-uri-scheme-names.md)을 참조하세요. |
| **실행 파일** | 프로토콜에 대한 기본 시작 실행 파일을 지정합니다. 지정하지 않으면 앱의 실행 파일이 사용됩니다. 지정하는 경우 문자열의 길이는 1-256자여야 하고 ".exe"로 끝나야 하며 &gt;, &lt;, :, ", &#124;, ?, \*를 포함할 수 없습니다. 지정하는 경우 **진입점**도 사용됩니다. **진입점**을 지정하지 않으면 앱에 대해 정의한 진입점을 사용합니다. |
| **진입점** | 프로토콜 확장명을 처리하는 작업을 지정합니다. 이는 일반적으로 Windows 런타임 형식의 정규화된 네임스페이스 이름입니다. 지정하지 않으면 앱에 대한 진입점이 사용됩니다. |
| **시작 페이지** | 확장성 지점을 처리하는 웹 페이지입니다. |
| **리소스 그룹** | 리소스 관리 목적으로 확장명 활성화를 그룹화하기 위해 사용할 수 있는 태그입니다. |
| **원하는 보기**(Windows만 해당) | **Desired View** 필드를 지정하여 URI 체계 이름에 대해 시작을 처리할 때 앱의 창에 필요한 공간을 나타냅니다. **Desired View**에 사용 가능한 값은 **Default**, **UseLess**, **UseHalf**, **UseMore** 또는 **UseMinimum**입니다.<br/>**참고** Windows는 대상 앱의 최종 창 크기를 결정할 때 원본 앱의 기본 설정, 화면의 앱 수, 화면 방향 같은 여러 가지 요소를 고려합니다. **원하는 보기**를 설정해도 대상 앱에 대한 특정 창 관리 동작이 보장되지 않습니다.<br/>**모바일 디바이스 패밀리: Desired View**는 모바일 디바이스 패밀리에서 지원되지 않습니다. |

2. **로고**로 `images\Icon.png`를 입력합니다.
3. **표시 이름**으로 `SDK Sample URI Scheme`를 입력합니다.
4. **이름**으로 `alsdk`를 입력합니다.
5. Ctrl+S를 눌러 package.appxmanifest에 변경 사항을 저장합니다.

    이렇게 하면 이와 같은 [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) 요소가 패키지 매니페스트에 추가됩니다. **windows.protocol** 범주는 앱이 `alsdk` URI 체계 이름을 처리함을 나타냅니다.

```xml
    <Applications>
        <Application Id= ... >
            <Extensions>
                <uap:Extension Category="windows.protocol">
                  <uap:Protocol Name="alsdk">
                    <uap:Logo>images\icon.png</uap:Logo>
                    <uap:DisplayName>SDK Sample URI Scheme</uap:DisplayName>
                  </uap:Protocol>
                </uap:Extension>
          </Extensions>
          ...
        </Application>
   <Applications>
```

## <a name="step-2-add-the-proper-icons"></a>2단계: 적절한 아이콘 추가

URI 스키마 이름의 기본값이 되는 앱에는 시스템 전체의 다양한 위치(예: 기본 프로그램 제어판)에 표시되는 아이콘이 있습니다. 이 목적을 위해 프로젝트에 44x44 아이콘을 포함합니다. 앱 타일 로고의 모양을 일치시키고 아이콘을 투명으로 설정하는 대신 앱의 배경색을 사용합니다. 로고를 안쪽 여백 없이 가장자리로 확장합니다. 흰색 배경에서 아이콘을 테스트합니다. 아이콘에 대한 자세한 내용은 [타일 및 아이콘 자산에 대한 지침](https://docs.microsoft.com/windows/uwp/shell/tiles-and-notifications/app-assets)을 참조하세요.

## <a name="step-3-handle-the-activated-event"></a>3단계: 활성화된 이벤트 처리

[**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 이벤트 처리기는 모든 활성화 이벤트를 받습니다. **Kind** 속성은 활성화 이벤트의 형식을 나타냅니다. 이 예제는 [**Protocol**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.activation.activationkind.aspx#Protocol) 활성화 이벤트를 처리하도록 설정되었습니다.

```csharp
public partial class App
{
   protected override void OnActivated(IActivatedEventArgs args)
  {
      if (args.Kind == ActivationKind.Protocol)
      {
         ProtocolActivatedEventArgs eventArgs = args as ProtocolActivatedEventArgs;
         // TODO: Handle URI activation
         // The received URI is eventArgs.Uri.AbsoluteUri
      }
   }
}
```

```vb
Protected Overrides Sub OnActivated(ByVal args As Windows.ApplicationModel.Activation.IActivatedEventArgs)
   If args.Kind = ActivationKind.Protocol Then
      ProtocolActivatedEventArgs eventArgs = args As ProtocolActivatedEventArgs
      
      ' TODO: Handle URI activation
      ' The received URI is eventArgs.Uri.AbsoluteUri
 End If
End Sub
```

```cppwinrt
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
    {
        auto protocolActivatedEventArgs{ args.as<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs>() };
        // TODO: Handle URI activation  
        auto receivedURI{ protocolActivatedEventArgs.Uri().RawUri() };
    }
}
```

```cpp
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ args)
{
   if (args->Kind == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
   {
      Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^ eventArgs =
          dynamic_cast<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^>(args);
      
      // TODO: Handle URI activation  
      // The received URI is eventArgs->Uri->RawUri
   }
}
```

> [!NOTE]
> 프로토콜 계약을 통해 시작 하는 경우 다시 돌아가도록 해야 뒤로 단추는 사용자가 앱의 이전 콘텐츠가 아닌 앱이 시작 화면에 있습니다.

다음 코드는 프로그래밍 방식으로 URI를 통해 앱을 시작합니다.

```csharp
   // Launch the URI
   var uri = new Uri("alsdk:");
   var success = await Windows.System.Launcher.LaunchUriAsync(uri)
```

URI를 통해 앱을 실행하는 방법에 대한 자세한 내용은 [URI에 대한 기본 앱 실행](launch-default-app.md)을 참조하세요.

앱이 새 페이지를 여는 각 활성화 이벤트에 대해 새 XAML [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)을 만들도록 하는 것이 좋습니다. 이런 식으로 새 XAML **Frame**에 대한 탐색 백 스택에는 앱이 일시 중단될 때 현재 창에 포함될 수 있는 이전 콘텐츠가 포함되지 않습니다. 시작 및 파일 계약에 단일 XAML **Frame**을 사용하도록 결정한 앱은 새 페이지를 탐색하기 전에 **Frame**의 탐색 저널에서 페이지를 지워야 합니다.

프로토콜 활성화를 통해 시작된 경우 앱은 사용자가 앱의 최상위 페이지로 다시 이동할 수 있도록 하는 UI를 포함해야 합니다.

## <a name="remarks"></a>설명

악의적인 경우를 비롯하여 어떤 앱이나 웹 사이트도 URI 스키마 이름을 사용할 수 있습니다. 따라서 URI를 통해 가져오는 데이터는 신뢰할 수 없는 원본에서 온 것일 수 있으므로 URI를 통해 받은 매개 변수를 기반으로 영구 작업을 수행하지 않는 것이 좋습니다. 예를 들어 사용자의 계정 페이지로 앱을 실행하는 데 URI 매개 변수를 사용할 수 있지만 사용자의 계정을 직접 수정하는 데는 사용하지 않는 것이 좋습니다.

> [!NOTE]
> 앱에 대 한 새 URI 체계 이름을 만들려는 경우 [RFC 4395](http://go.microsoft.com/fwlink/p/?LinkID=266550)의 지침을 수행 해야 합니다. 그러면 이름이 URI 스키마에 대한 표준을 충족하게 됩니다.

> [!NOTE]
> 프로토콜 계약을 통해 시작 하는 경우 다시 돌아가도록 해야 뒤로 단추는 사용자가 앱의 이전 콘텐츠가 아닌 앱이 시작 화면에 있습니다.

앱이 새 URI 대상을 여는 각 활성화 이벤트에 대해 새 XAML [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)을 만들도록 하는 것이 좋습니다. 이런 식으로 새 XAML **Frame**에 대한 탐색 백 스택에는 앱이 일시 중단될 때 현재 창에 포함될 수 있는 이전 콘텐츠가 포함되지 않습니다.

앱에서 시작 및 프로토콜 계약에 단일 XAML [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)을 사용하도록 결정하는 경우 새 페이지를 탐색하기 전에 **Frame**의 탐색 저널에서 페이지를 지워야 합니다. 프로토콜 계약을 통해 시작된 경우 사용자가 앱의 맨 위로 다시 이동할 수 있도록 하는 UI를 앱에 포함하는 것이 좋습니다.

## <a name="related-topics"></a>관련 항목

### <a name="complete-sample-app"></a>전체 샘플 앱

- [연결 시작 예제](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>개념

- [기본 프로그램](https://msdn.microsoft.com/library/windows/desktop/cc144154)
- [파일 형식 및 URI 연결 모델](https://msdn.microsoft.com/library/windows/desktop/hh848047)

### <a name="tasks"></a>작업

- [URI에 대한 기본 앱 실행](launch-default-app.md)
- [파일 활성화 처리](handle-file-activation.md)

### <a name="guidelines"></a>지침

- [파일 형식 및 URI에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>참조

- [AppX 패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/dn934791)
- [Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/br224742)
- [Windows.UI.Xaml.Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330)
