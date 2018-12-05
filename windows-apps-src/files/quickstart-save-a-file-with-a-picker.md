---
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: 선택기를 사용하여 파일 저장
description: 사용자가 앱에서 파일을 저장할 이름과 위치를 지정할 수 있도록 하려면 FileSavePicker를 사용합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a7e278df29a531e5bf1d0d92946cd0199f85515d
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8729935"
---
# <a name="save-a-file-with-a-picker"></a>선택기를 사용하여 파일 저장

**중요 API**

-   [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)
-   [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)

사용자가 앱에서 파일을 저장할 이름과 위치를 지정할 수 있도록 하려면 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용합니다.

> [!NOTE]
> [파일 선택기 샘플](http://go.microsoft.com/fwlink/p/?linkid=619994)도 참조하세요.

 

## <a name="prerequisites"></a>필수 조건


-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C# 또는 Visual Basic에서 비동기식 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

-   **위치에 대한 액세스 권한**

    [파일 액세스 권한](file-access-permissions.md)을 참조하세요.

## <a name="filesavepicker-step-by-step"></a>FileSavePicker: 단계별

사용자가 이름, 형식 및 파일 저장 위치를 지정할 수 있도록 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용합니다. 파일 선택기 개체를 만들고, 사용자 지정하고, 표시한 다음, 선택된 파일을 나타내는 반환된 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 통해 데이터를 저장합니다.

1.  **FileSavePicker 만들기 및 사용자 지정**

```cs
var savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.SuggestedStartLocation =
    Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
// Dropdown of file types the user can save the file as
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
// Default file name if the user does not type one in or select a file to replace
savePicker.SuggestedFileName = "New Document";
```

사용자 및 앱과 관련된 파일 선택기 개체에서 속성을 설정합니다.

이 예제에서는 세 가지 속성 [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880), [**FileTypeChoices**](https://msdn.microsoft.com/library/windows/apps/br207875) 및 [**SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878)을 설정합니다.

> [!NOTE]
>[**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 개체는 [**PickerViewMode.List**](https://msdn.microsoft.com/library/windows/apps/br207891)를 사용하여 파일 선택기를 표시합니다.
     
- 사용자가 문서나 텍스트 파일을 저장하고 있으므로 이 샘플에서는 [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621)를 사용하여 [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880)를 앱의 로컬 폴더로 설정합니다. [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854)을 저장하려는 파일 형식(예: 음악, 사진, 동영상 또는 문서)에 적절한 위치로 설정합니다. 사용자는 시작 위치에서 다른 위치로 이동할 수 있습니다.

- 파일이 저장된 후 앱에서 파일을 열 수 있는지 확인하려고 하므로 [**FileTypeChoices**](https://msdn.microsoft.com/library/windows/apps/br207875)를 사용하여 샘플에서 지원하는 파일 형식(Microsoft Word 문서 및 텍스트 파일)을 지정합니다. 지정하는 모든 파일 형식이 앱에서 지원되는지 확인합니다. 사용자는 자신의 파일을 지정한 파일 형식으로 저장할 수 있습니다. 또한 지정된 다른 파일 형식을 선택하여 파일 형식을 변경할 수 있습니다. 목록에서 첫 번째 파일 형식 항목은 기본적으로 선택됩니다. 이를 제어하려면 [**DefaultFileExtension**](https://msdn.microsoft.com/library/windows/apps/br207873) 속성을 설정합니다.

> [!NOTE]
> 파일 선택기는 현재 선택된 파일 형식을 사용하여 표시되는 파일을 필터링하기도 하므로 선택한 파일 형식과 일치하는 파일 형식만 사용자에게 표시됩니다.

- 사용자 입력을 저장하기 위해 예제에서는 [**SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878)을 설정합니다. 제안된 파일 이름을 저장하려는 파일과 연관되게 만듭니다. 예를 들어, Word처럼, 사용자가 이름이 아직 지정되지 않은 파일을 저장하려 하면 문서의 첫 줄을 이름으로 제안하거나 이름이 있으면 기존 파일 이름을 제안할 수 있습니다.

2.  **FileSavePicker 표시 및 선택한 파일에 저장**

    [**PickSaveFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207876)를 호출하여 파일 선택기를 표시합니다. 사용자가 이름, 파일 형식 및 위치를 지정한 후 파일 저장을 확인하면 **PickSaveFileAsync**는 저장된 파일을 나타내는 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 반환합니다. 이제 읽기 및 쓰기 액세스 권한이 있으므로 이 파일을 캡처하고 처리할 수 있습니다.

    ```cs
    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
        if (file != null)
        {
            // Prevent updates to the remote version of the file until
            // we finish making changes and call CompleteUpdatesAsync.
            Windows.Storage.CachedFileManager.DeferUpdates(file);
            // write to file
            await Windows.Storage.FileIO.WriteTextAsync(file, file.Name);
            // Let Windows know that we're finished changing the file so
            // the other app can update the remote version of the file.
            // Completing updates may require Windows to ask for user input.
            Windows.Storage.Provider.FileUpdateStatus status =
                await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
            if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
            {
                this.textBlock.Text = "File " + file.Name + " was saved.";
            }
            else
            {
                this.textBlock.Text = "File " + file.Name + " couldn't be saved.";
            }
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
    ```

예제에서는 파일이 유효한지 확인하고 자체 파일 이름을 씁니다. [파일 만들기, 쓰기 및 읽기](quickstart-reading-and-writing-files.md)도 참조하세요.

**팁**항상 저장 된 파일을 다른 처리를 수행 하기 전에 유효 있는지 확인 해야 합니다. 그런 다음 파일을 앱에 맞게 저장하고 선택한 파일이 잘못된 경우 적절한 동작을 제공할 수 있습니다.
