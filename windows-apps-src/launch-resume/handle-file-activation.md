---
title: 파일 활성화 처리
description: 앱은 특정 파일 형식에 대 한 기본 처리기가 되도록 등록할 수 있습니다.
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
ms.openlocfilehash: 4377db57b3cd713bae8f9c80a0116d016722be19
ms.sourcegitcommit: 90fe7a9a5bfa7299ad1b78bbef289850dfbf857d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2020
ms.locfileid: "84756539"
---
# <a name="handle-file-activation"></a>파일 활성화 처리

**중요 API**

-   [**FileActivatedEventArgs입니다.**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows. .Xaml. OnFileActivated 됨**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)

앱은 특정 파일 형식에 대 한 기본 처리기가 되도록 등록할 수 있습니다. UWP (Windows 데스크톱 응용 프로그램 및 유니버설 Windows 플랫폼) 앱은 모두 기본 파일 처리기로 등록할 수 있습니다. 사용자가 특정 파일 형식에 대 한 기본 핸들러로 앱을 선택 하면 해당 형식의 파일이 시작 될 때 앱이 활성화 됩니다.

해당 파일 형식에 대 한 모든 파일 시작을 처리 해야 하는 경우에만 파일 형식을 등록 하는 것이 좋습니다. 앱에서 내부적으로 파일 형식을 사용 해야 하는 경우에는를 기본 처리기로 등록 하지 않아도 됩니다. 파일 형식에 등록 하도록 선택 하는 경우 해당 파일 형식에 대해 앱이 활성화 될 때 예상 되는 기능을 최종 사용자에 게 제공 해야 합니다. 예를 들어 사진 뷰어 앱은 .jpg 파일을 표시 하도록 등록할 수 있습니다. 파일 연결에 대 한 자세한 내용은 [파일 형식 및 uri에 대 한 지침](https://docs.microsoft.com/windows/uwp/files/index)을 참조 하세요.

이러한 단계에서는 사용자가. alsdk를 등록 하는 방법과 사용자가 alsdk 파일을 시작할 때 앱을 활성화 하는 방법을 보여 줍니다.

> **참고**    UWP 앱에서 특정 Uri 및 파일 확장은 기본 제공 앱 및 운영 체제에서 사용 하도록 예약 되어 있습니다. 예약 된 URI 또는 파일 확장명을 사용 하 여 앱을 등록 하려고 하면 무시 됩니다. 자세한 내용은 [예약 된 파일 및 URI 체계 이름](reserved-uri-scheme-names.md)을 참조 하세요.

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>1 단계: 패키지 매니페스트에 확장 지점 지정

앱은 패키지 매니페스트에 나열 된 파일 확장명에 대해서만 활성화 이벤트를 받습니다. 앱이 확장명을 사용 하 여 파일을 처리 하도록 지정 하는 방법은 다음과 같습니다 `.alsdk` .

1.  **솔루션 탐색기**에서 appxmanifest.xml를 두 번 클릭 하 여 매니페스트 디자이너를 엽니다. **선언** 탭을 선택 하 고 **사용 가능한 선언** 드롭다운에서 **파일 형식 연결** 을 선택한 다음 **추가**를 클릭 합니다. 파일 연결에서 사용 하는 식별자에 대 한 자세한 내용은 [프로그래밍 방식 식별자](https://docs.microsoft.com/windows/desktop/shell/fa-progids) 를 참조 하세요.

    다음은 매니페스트 디자이너에서 채울 수 있는 각 필드에 대 한 간단한 설명입니다.

| 필드 | Description |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **표시 이름** | 파일 형식 그룹의 표시 이름을 지정 합니다. 표시 이름은 **제어판**의 [기본 프로그램 설정](https://docs.microsoft.com/windows/desktop/shell/default-programs) 에서 파일 형식을 식별 하는 데 사용 됩니다. |
| **로고** | 바탕 화면 및 **제어판**의 [기본 프로그램 설정](https://docs.microsoft.com/windows/desktop/shell/default-programs) 에서 파일 형식을 식별 하는 데 사용 되는 로고를 지정 합니다. 로고를 지정 하지 않으면 응용 프로그램의 작은 로고가 사용 됩니다. |
| **정보 팁** | 파일 형식 그룹에 대 한 [정보 팁](https://docs.microsoft.com/windows/desktop/shell/fa-progids) 을 지정 합니다. 이 도구 설명 텍스트는 사용자가이 형식의 파일 아이콘을 마우스로 가리킬 때 나타납니다. |
| **이름** | 동일한 표시 이름, 로고, 정보 팁 및 편집 플래그를 공유 하는 파일 형식 그룹의 이름을 선택 합니다. 앱 업데이트에서 동일 하 게 유지할 수 있는 그룹 이름을 선택 합니다. **참고**  이름은 모두 소문자 여야 합니다. |
| **콘텐츠 형식** | 특정 파일 형식에 대해 **이미지/jpeg**와 같은 MIME 콘텐츠 형식을 지정 합니다. **허용 되는 내용 유형에 대 한 중요 정보:** **응용 프로그램/강제 다운로드**, **응용 프로그램/8 진수 스트림**, **응용 프로그램/알 수**없음, **응용 프로그램/x**m l 다운로드 등 패키지 매니페스트에 입력할 수 없는 MIME 콘텐츠 형식의 사전순 목록은 다음과 같습니다. |
| **파일 형식** | ".Jpeg"와 같이 마침표 뒤에 등록할 파일 형식을 지정 합니다. **예약 및 금지 된 파일 형식:** UWP 앱이 예약 되었거나 사용할 수 없기 때문에 UWP 앱에 등록할 수 없는 기본 제공 앱에 대 한 파일 형식의 사전순 목록은 [예약 된 URI 체계 이름 및 파일 형식](reserved-uri-scheme-names.md) 을 참조 하세요. |

2.  `alsdk` **이름**으로을 입력 합니다.
3.  를 `.alsdk` **파일 형식**으로 입력 합니다.
4.  "Images \\Icon.png"을 로고로 입력 합니다.
5.  Ctrl + S를 눌러 변경 내용을 appxmanifest.xml에 저장 합니다.

위의 단계는 패키지 매니페스트에 이와 같은 [**확장**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) 요소를 추가 합니다. **Windows. Fil및 association** 범주는 앱이 확장명이 인 파일을 처리 함을 나타냅니다 `.alsdk` .

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

## <a name="step-2-add-the-proper-icons"></a>2 단계: 적절 한 아이콘 추가

파일 형식의 기본값이 되는 앱에는 시스템 전체의 다양한 위치에 표시되는 아이콘이 있습니다. 예를 들어 다음과 같은 아이콘이 표시 됩니다.

-   Windows 탐색기 항목 보기, 상황에 맞는 메뉴 및 리본
-   기본 프로그램 제어판
-   파일 선택기
-   시작 화면에서 결과 검색

로고를 해당 위치에 표시할 수 있도록 프로젝트에 44x44 아이콘을 포함 합니다. 앱 타일 로고의 모양을 일치 시키고 아이콘을 투명 하 게 만드는 대신 앱의 배경색을 사용 합니다. 로고가 패딩 되지 않고 가장자리로 확장 되도록 합니다. 흰색 배경의 아이콘을 테스트 합니다. 아이콘에 대 한 자세한 내용은 [타일 및 아이콘 자산에 대 한 지침](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets) 을 참조 하세요.

## <a name="step-3-handle-the-activated-event"></a>3 단계: 활성화 된 이벤트 처리

[**Onfileactivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated) 이벤트 처리기는 모든 파일 활성화 이벤트를 수신 합니다.

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
> 파일 계약을 통해 시작 하는 경우 뒤로 단추를 누르면 사용자가 앱의 이전 콘텐츠가 아닌 앱을 시작한 화면으로 다시 이동 해야 합니다.

새 페이지를 여는 각 활성화 이벤트에 대해 새 XAML **프레임** 을 만드는 것이 좋습니다. 이렇게 하면 새 XAML 프레임에 대 한 탐색 백 스택에 일시 중단 될 때 앱이 현재 창에 있을 수 있는 이전 콘텐츠가 포함 되지 않습니다. 시작 및 파일 계약에 단일 XAML **프레임** 을 사용 하기로 결정 한 경우에는 새 페이지로 이동 하기 전에 **프레임**의 탐색 저널에 있는 페이지를 지워야 합니다.

앱이 파일 활성화를 통해 시작 되는 경우 사용자가 앱의 맨 위 페이지로 돌아갈 수 있는 UI를 포함 하는 것을 고려해 야 합니다.

## <a name="remarks"></a>설명

수신 하는 파일은 신뢰할 수 없는 소스에서 가져올 수 있습니다. 작업을 수행 하기 전에 파일 내용의 유효성을 검사 하는 것이 좋습니다.

## <a name="related-topics"></a>관련 항목

### <a name="complete-example"></a>전체 예제

* [연결 시작 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>개념

* [기본 프로그램](https://docs.microsoft.com/windows/desktop/shell/default-programs)
* [파일 형식 및 프로토콜 연결 모델](https://docs.microsoft.com/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>작업

* [파일에 대한 기본 앱 시작](launch-the-default-app-for-a-file.md)
* [URI 활성화 처리](handle-uri-activation.md)

### <a name="guidelines"></a>지침

* [파일 형식 및 URI에 대한 지침](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>참조
* [FileActivatedEventArgs입니다.](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows. .Xaml. OnFileActivated 됨](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)
