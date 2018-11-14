---
author: laurenhughes
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: 파일, 폴더 및 라이브러리
description: 앱 설정 읽기/쓰기, 파일 및 폴더 선택기, 비디오/음악 라이브러리와 같은 특수 샌드박스가 적용된 위치에 대해 알아봅니다.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 80c6292c12568d7f1017ca67707689ce9539c804
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6448348"
---
 # <a name="files-folders-and-libraries"></a>파일, 폴더 및 라이브러리


[Windows.Storage](https://msdn.microsoft.com/library/windows/apps/br227346), [Windows.Storage.Streams](https://msdn.microsoft.com/library/windows/apps/br241791) 및 [Windows.Storage.Pickers](https://msdn.microsoft.com/library/windows/apps/br207928) 네임스페이스의 API를 사용하여 텍스트 및 다른 데이터 형식을 읽고 파일에 쓸 수 있으며, 파일 및 폴더를 관리할 수 있습니다. 이 섹션에서는 앱 설정 읽기/쓰기, 파일 및 폴더 선택기, 비디오/음악 라이브러리와 같은 특수 샌드박스가 적용된 위치에 대해서도 알아봅니다.

| 항목 | 설명  |
|-------|--------------|
| [파일 및 폴더 열거 및 쿼리](quickstart-listing-files-and-folders.md) | 폴더, 라이브러리, 디바이스 또는 네트워크 위치에 있는 파일 및 폴더에 액세스합니다. 파일 및 폴더 쿼리를 작성하여 위치에 있는 파일 및 폴더를 쿼리할 수도 있습니다. |
| [파일 만들기, 쓰기 및 읽기](quickstart-reading-and-writing-files.md) | [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 사용하여 파일을 읽고 씁니다. |
| [파일 속성 가져오기](quickstart-getting-file-properties.md) | [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) 개체로 표시되는 파일의 최상위, 기본 및 확장 속성을 가져옵니다. |
| [선택기를 사용하여 파일 및 폴더 열기](quickstart-using-file-and-folder-pickers.md) | 사용자가 선택기를 조작할 수 있도록 하여 파일 및 폴더에 액세스합니다. [FolderPicker](https://msdn.microsoft.com/library/windows/apps/br207881)를 사용하여 폴더에 액세스할 수 있습니다. |
| [선택기를 사용하여 파일 저장](quickstart-save-a-file-with-a-picker.md) | 사용자가 앱에서 파일을 저장할 이름과 위치를 지정할 수 있도록 하려면 [FileSavePicker](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용합니다. |
| [홈 그룹 콘텐츠 액세스](quickstart-accessing-homegroup-content.md) | 사진, 음악, 비디오 등 사용자의 홈 그룹 폴더에 저장된 콘텐츠에 액세스합니다. |
| [Microsoft OneDrive 파일의 가용성 확인](quickstart-determining-availability-of-microsoft-onedrive-files.md) | [StorageFile.IsAvailable](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) 속성을 사용하여 Microsoft OneDrive 파일의 사용 가능 여부를 확인합니다. |
| [음악, 사진 및 비디오 라이브러리의 파일 및 폴더](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | 음악, 사진 또는 비디오의 기존 폴더를 해당 라이브러리에 추가합니다. 라이브러리에서 폴더를 제거하고, 라이브러리에 폴더 목록을 가져오고, 저장된 사진, 음악 및 동영상을 검색할 수도 있습니다. |
| [최근에 사용한 파일 및 폴더 추적](how-to-track-recently-used-files-and-folders.md) | 사용자가 자주 액세스하는 파일을 앱의 MRU(최근에 사용한 목록)에 추가하여 추적할 수 있습니다. 플랫폼은 마지막으로 액세스한 시간을 기반으로 항목을 정렬하고 목록의 25개 항목 제한에 도달한 경우 가장 오래된 항목을 제거하여 MRU를 자동으로 관리합니다. 모든 앱에는 자체 MRU가 있습니다. |
| [SD 카드에 액세스](access-the-sd-card.md) | 특히 내부 저장 용량이 제한적인 저가대의 디바이스에서는 중요하지 않은 데이터를 선택적 microSD 카드에 저장하고 액세스할 수 있습니다. |
| [파일 액세스 권한](file-access-permissions.md) | 앱은 기본적으로 특정 파일 시스템 위치에 액세스할 수 있습니다. 또한 앱은 파일 선택기를 통해서나 접근 권한 값을 선언하여 추가 위치에 액세스할 수도 있습니다. |
| [UWP의 파일 속성에 빠르게 액세스](fast-file-properties.md) | UWP 앱에서 사용할 라이브러리에서 파일 및 속성 목록을 효율적으로 수집합니다. |

## <a name="related-samples"></a>관련 샘플
[폴더 열거 샘플](http://go.microsoft.com/fwlink/p/?linkid=619993)

[파일 액세스 샘플](http://go.microsoft.com/fwlink/p/?linkid=619995)

[파일 선택기 샘플](http://go.microsoft.com/fwlink/p/?linkid=619994)
 

 
