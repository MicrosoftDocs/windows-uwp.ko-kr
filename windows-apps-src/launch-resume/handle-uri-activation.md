---
title: URI 활성화 처리
description: URI (Uniform Resource Identifier) 구성표 이름에 대 한 기본 처리기가 되도록 앱을 등록 하는 방법에 대해 알아봅니다.
ms.assetid: 92D06F3E-C8F3-42E0-A476-7E94FD14B2BE
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9a9d3c073bdfc23511728825a0defac790ae6623
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167887"
---
# <a name="handle-uri-activation"></a>URI 활성화 처리

**중요 API**

-   [**ProtocolActivatedEventArgs입니다.**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs)
-   [**Windows. .Xaml. 응용 프로그램을 활성화 합니다.**](/uwp/api/windows.ui.xaml.application.onactivated)

URI (Uniform Resource Identifier) 구성표 이름에 대 한 기본 처리기가 되도록 앱을 등록 하는 방법에 대해 알아봅니다. Windows 데스크톱 앱 및 유니버설 Windows 플랫폼 (UWP) 앱은 모두 URI 체계 이름에 대 한 기본 처리기로 등록할 수 있습니다. 사용자가 응용 프로그램을 URI 스키마 이름에 대 한 기본 처리기로 선택 하는 경우 URI 형식이 시작 될 때마다 앱이 활성화 됩니다.

Uri 체계의 해당 형식에 대 한 모든 URI 시작을 처리할 것으로 간주 되는 경우에만 URI 체계 이름을 등록 하는 것이 좋습니다. URI 체계 이름을 등록 하도록 선택 하는 경우 해당 URI 체계에 대해 앱이 활성화 될 때 예상 되는 기능을 최종 사용자에 게 제공 해야 합니다. 예를 들어 mailto: URI 체계 이름을 등록 하는 앱은 사용자가 새 전자 메일을 작성할 수 있도록 새 전자 메일 메시지를 열어야 합니다. URI 연결에 대 한 자세한 내용은 [파일 형식 및 uri에 대 한 지침 및 검사 목록](../files/index.md)을 참조 하세요.

이러한 단계에서는 사용자 지정 URI 체계 이름을 등록 하는 방법 `alsdk://` 및 사용자가 URI를 시작할 때 앱을 활성화 하는 방법을 보여 줍니다 `alsdk://` .

> [!NOTE]
> UWP 앱에서 특정 Uri 및 파일 확장은 기본 제공 앱 및 운영 체제에서 사용 하도록 예약 되어 있습니다. 예약 된 URI 또는 파일 확장명을 사용 하 여 앱을 등록 하려고 하면 무시 됩니다. UWP 앱은 예약 되었거나 금지 되어 있으므로 등록할 수 없는 Uri 체계의 사전순 목록은 [예약 된 uri 체계 이름 및 파일 형식](reserved-uri-scheme-names.md) 을 참조 하세요.

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>1 단계: 패키지 매니페스트에 확장 지점 지정

앱은 패키지 매니페스트에 나열 된 URI 체계 이름에 대해서만 활성화 이벤트를 받습니다. 앱이 URI 체계 이름을 처리 하도록 지정 하는 방법은 다음과 같습니다 `alsdk` .

1. **솔루션 탐색기**에서 appxmanifest.xml를 두 번 클릭 하 여 매니페스트 디자이너를 엽니다. **선언** 탭을 선택 하 고 **사용 가능한 선언** 드롭다운에서 **프로토콜** 을 선택한 다음 **추가**를 클릭 합니다.

    다음은 프로토콜에 대 한 매니페스트 디자이너에서 채울 수 있는 각 필드에 대 한 간략 한 설명입니다 (자세한 내용은 [**AppX 패키지 매니페스트**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-extension) 참조).

| 필드 | Description |
|-------|-------------|
| **로고** | **제어판**의 [기본 프로그램 설정](/windows/desktop/shell/default-programs) 에서 URI 체계 이름을 식별 하는 데 사용 되는 로고를 지정 합니다. 로고를 지정 하지 않으면 앱에 대 한 작은 로고가 사용 됩니다. |
| **표시 이름** | **제어판**의 [기본 프로그램 설정](/windows/desktop/shell/default-programs) 에서 URI 체계 이름을 식별할 표시 이름을 지정 합니다. |
| **이름** | Uri 체계의 이름을 선택 합니다. |
|  | **참고**  이름은 모두 소문자 여야 합니다. |
|  | **예약 및 금지 된 파일 형식** UWP 앱은 예약 되었거나 금지 되어 있으므로 등록할 수 없는 Uri 체계의 사전순 목록은 [예약 된 uri 체계 이름 및 파일 형식](reserved-uri-scheme-names.md) 을 참조 하세요. |
| **실행 파일** | 프로토콜에 대 한 기본 시작 실행 파일을 지정 합니다. 지정 하지 않으면 앱의 실행 파일이 사용 됩니다. 지정 된 경우 문자열의 길이는 1 ~ 007e; 256 자 여야 하 고,는 ".exe"로 끝나야 하며 &gt; ,, &lt; :, ", &#124;,? 또는와 같은 문자를 포함할 \* 수 없습니다. 지정 된 경우 **진입점** 도 사용 됩니다. **진입점** 을 지정 하지 않으면 앱에 대해 정의 된 진입점이 사용 됩니다. |
| **진입점** | 프로토콜 확장을 처리 하는 작업을 지정 합니다. 이는 일반적으로 Windows 런타임 형식의 전체 네임 스페이스로 한정 된 이름입니다. 지정 하지 않으면 앱에 대 한 진입점이 사용 됩니다. |
| **시작 페이지** | 확장성 지점을 처리 하는 웹 페이지입니다. |
| **리소스 그룹** | 리소스 관리를 위해 확장 활성화를 함께 그룹화 하는 데 사용할 수 있는 태그입니다. |
| **원하는 보기** (Windows 전용) | URI 체계 이름에 대해 앱이 시작 될 때 필요한 **보기** 필드를 지정 하 여 앱의 창이 필요한 공간을 표시 합니다. **원하는 뷰에** 사용할 수 있는 값은 **기본값**, **쓸모**없는, **Usehalf**, **usemore**또는 **UseMinimum**입니다.<br/>**참고**  Windows는 대상 앱의 최종 창 크기를 결정할 때 여러 가지 요인을 고려 합니다. 예를 들어 원본 앱의 기본 설정, 화면에 있는 앱의 수, 화면 방향 등을 확인할 수 있습니다. **원하는 뷰** 를 설정 하면 대상 앱에 대 한 특정 창 고 동작이 보장 되지 않습니다.<br/>**모바일 장치 제품군: 원하는 보기** 는 모바일 장치 제품군에서 지원 되지 않습니다. |

2. `images\Icon.png` **로고**로을 입력 합니다.
3. `SDK Sample URI Scheme` **표시 이름** 으로를 입력 합니다.
4. `alsdk` **이름**으로을 입력 합니다.
5. Ctrl + S를 눌러 변경 내용을 appxmanifest.xml에 저장 합니다.

    이렇게 하면 이와 같은 [**확장**](/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) 요소가 패키지 매니페스트에 추가 됩니다. **Windows. protocol** 범주는 응용 프로그램이 `alsdk` URI 체계 이름을 처리 함을 나타냅니다.

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

## <a name="step-2-add-the-proper-icons"></a>2 단계: 적절 한 아이콘 추가

URI 체계 이름에 대 한 기본값이 되는 앱의 경우 기본 프로그램 제어판에서와 같이 시스템 전체에서 다양 한 위치에 아이콘이 표시 됩니다. 이 목적을 위해 프로젝트에 44x44 아이콘을 포함 합니다. 앱 타일 로고의 모양을 일치 시키고 아이콘을 투명 하 게 만드는 대신 앱의 배경색을 사용 합니다. 로고가 패딩 되지 않고 가장자리로 확장 되도록 합니다. 흰색 배경의 아이콘을 테스트 합니다. 아이콘에 대 한 자세한 내용은 [앱 아이콘 및 로고](../design/style/app-icons-and-logos.md) 를 참조 하세요.

## <a name="step-3-handle-the-activated-event"></a>3 단계: 활성화 된 이벤트 처리

[**Onactivated**](/uwp/api/windows.ui.xaml.application.onactivated) 이벤트 처리기는 모든 활성화 이벤트를 수신 합니다. **Kind** 속성은 활성화 이벤트의 유형을 나타냅니다. 이 예제는 [**프로토콜**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) 활성화 이벤트를 처리 하도록 설정 되어 있습니다.

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
> 프로토콜 계약을 통해 시작 하는 경우 뒤로 단추를 누르면 사용자가 앱의 이전 콘텐츠가 아닌 앱을 시작한 화면으로 다시 이동 해야 합니다.

다음 코드는 해당 URI를 통해 프로그래밍 방식으로 앱을 시작 합니다.

```csharp
   // Launch the URI
   var uri = new Uri("alsdk:");
   var success = await Windows.System.Launcher.LaunchUriAsync(uri)
```

URI를 통해 응용 프로그램을 시작 하는 방법에 대 한 자세한 내용은 [uri에 대 한 기본 앱 시작](launch-default-app.md)을 참조 하세요.

앱에서 새 페이지를 여는 각 활성화 이벤트에 대해 새 XAML [**프레임**](/uwp/api/Windows.UI.Xaml.Controls.Frame) 을 만드는 것이 좋습니다. 이러한 방식으로 새 XAML **프레임** 에 대 한 탐색 백 스택은 일시 중단 될 때 현재 창에서 앱이 있을 수 있는 이전 콘텐츠를 포함 하지 않습니다. 시작 및 파일 계약에 단일 XAML **프레임** 을 사용 하기로 결정 한 앱은 새 페이지로 이동 하기 전에 **프레임** 탐색 저널의 페이지를 지워야 합니다.

프로토콜 활성화를 통해 시작된 경우 앱은 사용자가 앱의 최상위 페이지로 다시 이동할 수 있도록 하는 UI를 포함해야 합니다.

## <a name="remarks"></a>설명

모든 앱 또는 웹 사이트는 악의적 사용자를 포함 하 여 URI 체계 이름을 사용할 수 있습니다. 따라서 URI에 표시 되는 모든 데이터는 신뢰할 수 없는 소스에서 가져올 수 있습니다. URI에서 수신 하는 매개 변수를 기반으로 영구 작업을 수행 하지 않는 것이 좋습니다. 예를 들어 URI 매개 변수를 사용 하 여 사용자의 계정 페이지로 앱을 시작할 수 있지만 사용자 계정을 직접 수정 하는 데 사용 하지 않는 것이 좋습니다.

> [!NOTE]
> 앱에 대 한 새 URI 체계 이름을 만드는 경우 [RFC 4395](https://tools.ietf.org/html/rfc4395)의 지침을 따라야 합니다. 이렇게 하면 이름이 URI 체계에 대 한 표준을 충족 합니다.

> [!NOTE]
> 프로토콜 계약을 통해 시작 하는 경우 뒤로 단추를 누르면 사용자가 앱의 이전 콘텐츠가 아닌 앱을 시작한 화면으로 다시 이동 해야 합니다.

앱에서 새 Uri 대상을 여는 각 활성화 이벤트에 대해 새 XAML [**프레임**](/uwp/api/Windows.UI.Xaml.Controls.Frame) 을 만드는 것이 좋습니다. 이러한 방식으로 새 XAML **프레임** 에 대 한 탐색 백 스택은 일시 중단 될 때 현재 창에서 앱이 있을 수 있는 이전 콘텐츠를 포함 하지 않습니다.

앱에서 시작 및 프로토콜 계약에 단일 XAML [**프레임**](/uwp/api/Windows.UI.Xaml.Controls.Frame) 을 사용 하도록 결정 한 경우 새 페이지로 이동 하기 전에 **프레임** 탐색 저널에서 페이지의 선택을 취소 합니다. 프로토콜 계약을 통해 시작 하는 경우 사용자가 앱의 맨 위로 돌아갈 수 있도록 하는 UI를 앱에 포함 하는 것이 좋습니다.

## <a name="related-topics"></a>관련 항목

### <a name="complete-sample-app"></a>전체 샘플 앱

- [연결 시작 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>개념

- [기본 프로그램](/windows/desktop/shell/default-programs)
- [파일 형식 및 URI 연결 모델](/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>작업

- [URI에 대한 기본 앱 실행](launch-default-app.md)
- [파일 활성화 처리](handle-file-activation.md)

### <a name="guidelines"></a>지침

- [파일 형식 및 URI에 대한 지침](../files/index.md)

### <a name="reference"></a>참조

- [AppX 패키지 매니페스트](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-extension)
- [ProtocolActivatedEventArgs입니다.](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs)
- [Windows. .Xaml. 응용 프로그램을 활성화 합니다.](/uwp/api/windows.ui.xaml.application.onactivated)