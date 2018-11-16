---
author: QuinnRadich
Description: How to use thumbnail images to help users preview files in UWP apps.
title: UWP 앱의 미리 보기 이미지에 대한 지침
label: Thumbnail images
template: detail.hbs
ms.author: quradic
ms.date: 01/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8499f64e265cfeaa0a21c111c1aec1e2d3adb8ff
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6857497"
---
# <a name="thumbnail-images"></a>미리 보기 이미지

이러한 지침은 사용자가 UWP 앱에서 찾아볼 때 미리 보기 이미지를 사용하여 파일을 미리 볼 수 있는 방법에 대해 설명합니다. 

> **중요 API**: [ThumbnailMode enum](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)

## <a name="should-my-app-include-thumbnails"></a>내 앱에 미리 보기가 포함되어야 하나요?

앱에서 파일 검색이 허용되는 경우 사용자가 해당 파일을 빠르게 미리 볼 수 있도록 미리 보기 이미지를 표시할 수 있습니다. 

다음과 같은 경우 미리 보기를 사용하세요. 
- 갤러리 컬렉션의 여러 항목(예: 파일 및 폴더)에 대한 미리 보기를 표시합니다. 예를 들어, 사용자가 자신의 사진 파일을 찾아볼 때 사진 갤러리에서는 미리 보기를 사용하여 각 사진의 작은 보기를 제공해야 합니다.

    ![동영상 갤러리](images/thumbnail-gallery.png)

- 목록의 개별 항목(예: 특정 파일)에 대한 미리 보기를 표시합니다. 예를 들어, 사용자가 해당 파일을 열지 여부를 결정하기 전에 더 나은 미리 보기를 위해 더 큰 미리 보기를 포함하여 파일에 대한 자세한 내용을 보기를 원할 수 있습니다. 

    ![동영상 미리 보기](images/thumbnail-preview.png)

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항
- 미리 보기를 검색할 때 [미리 보기 모드](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)(PicturesView, VideosView, DocumentsView, MusicView, ListView 또는 SingleItem)를 지정하세요. 이렇게 하면 사용자가 보려는 파일 형식을 표시하기 위해 미리 보기 이미지가 최적화됩니다. 
    - SingleItem 모드를 사용하면 파일 형식에 관계 없이 SingleItem에 대한 미리 보기를 검색합니다. 다른 미리 보기 모드는 여러 파일의 미리 보기를 표시합니다. 

- 미리 보기가 로드되는 동안 미리 보기의 위치에 일반 자리 표시자 이미지가 표시됩니다. 자리 표시자를 사용하면 미리 보기가 로드되기 전에 사용자가 미리 보기와 상호 작용할 수 있으므로 앱의 응답성이 높아집니다. 

    자리 표시자 이미지는 다음과 같아야 합니다.
    * 대표하는 항목 종류에 특정해야 합니다. 예를 들어 폴더, 사진 및 동영상은 모두 특수한 자체 자리 표시자를 가지고 있어야 합니다. 
    * 대표하는 미리 보기 이미지와 크기 및 가로 세로 비율이 같아야 합니다. 
    * 미리 보기 이미지가 로드될 때까지 표시되어야 합니다. 

- 텍스트 레이블이 있는 자리 표시자 이미지를 사용하여 개별 파일과 구별할 폴더 및 파일 그룹을 표시합니다.

- 미리 보기를 검색할 수 없는 경우 자리 표시자 이미지를 표시합니다. 

- 문서 및 음악 파일의 미리 보기를 제공하는 경우 추가 파일 정보를 표시합니다. 사용자는 미리 보기 이미지만으로는 쉽게 사용할 결정을 내리지 못하는 파일에 대한 주요 정보를 식별할 수 있습니다. 예를 들어 음악 파일의 경우 앨범 이미지의 미리 보기와 함께 아티스트의 이름을 표시할 수 있습니다. 

- 사진과 동영상 파일에 대한 추가 파일 정보는 표시하지 마세요. 대부분의 경우 미리 보기 이미지만으로 사진과 동영상을 찾아보기에 충분합니다. 

## <a name="additional-usage-guidelines"></a>추가 사용법 지침
권장 [미리 보기 모드](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) 및 해당 기능:

<table>
<tr>
<th> 다음에 대한 미리 보기를 표시합니다.</th>
<th> 미리 보기 모드 </th>
<th> 검색된 미리 보기 이미지의 기능 </th>
</tr>
<tr>
<td> 사진<br /> 동영상 </td>
<td> PicturesView <br />VideosView </td>
<td> <b>크기</b>: 중간, 가급적 190 이상(이미지 크기가 190x130인 경우) <br />
<b>가로 세로 비율</b>: 약 .7의 균일한 와이드 가로 세로 비율(190인 경우 190x130) <br />
미리 보기를 위해 잘라냅니다. <br /> 
균일한 가로 세로 비율로 인해 격자에서 이미지를 정렬하는 데 적합합니다.  </td>
</tr>
<tr>
<td> 문서<br />음악 </td>
<td> DocumentsView <br />MusicView <br /> ListView</td>
<td> <b>크기</b>: 작은, 가급적 40 x 40 픽셀 이상 <br />
<b>가로 세로 비율</b>:  균일한 사각형 가로 세로 비율  <br />
사각형 가로 세로 비율로 인해 앨범 이미지를 미리 보는 데 적합합니다. <br /> 
문서는 파일 선택기 창에서 볼 때와 동일하게 표시됩니다(동일한 아이콘 사용). </td>
</tr>
</tr>
<tr>
<td> 단일 항목(파일 형식에 관계 없음) </td>
<td> SingleItem </td>
<td> <b>크기</b>: 작은, 가급적 40 x 40 픽셀 이상 <br />
<b>가로 세로 비율</b>:  균일한 사각형 가로 세로 비율  <br />
사각형 가로 세로 비율로 인해 앨범 이미지를 미리 보는 데 적합합니다. <br /> 
문서는 파일 선택기 창에서 볼 때와 동일하게 표시됩니다(동일한 아이콘 사용). </td>
</tr>
</table>

파일 형식과 미리 보기 모드에 따라 검색되는 미리 보기 이미지가 어떻게 다른지를 보여주는 예는 다음과 같습니다.
<div class="mx-responsive-img">
<table>
<tr>
<th>항목 유형</th>
<th>다음을 사용하여 검색하는 경우: <ul><li>PicturesView <li>VideosView</ul></th>
<th>다음을 사용하여 검색하는 경우: <ul><li>DocumentsView <li>MusicView <li>ListView</ul></th>
<th>다음을 사용하여 검색하는 경우: <ul><li>SingleItem</ul></th>
<tr>
<tr>
<td>사진</td>
<td>미리 보기 이미지가 파일의 원래 가로 세로 비율을 사용합니다. <br />
<img src="images/thumbnail-pic-picvidmode.png" alt="Picture thumbnail in picture or video mode"/></td>
<td>미리 보기가 사각형 가로 세로 비율로 잘립니다. <br />
<img src="images/thumbnail-pic-doclistmusic-modes.png" alt="Picture thumbnail in documents, music, or list modes"/></td>
<td>미리 보기 이미지가 파일의 원래 가로 세로 비율을 사용합니다.<br />
<img src="images/thumbnail-pic-single-mode.png" alt="Picture thumbnail in single mode"/> </td>
</tr>
<tr>
<td>동영상</td>
<td>미리 보기에 사진과 구별되는 아이콘이 있습니다. <br />
<img src="images/thumbnail-vid-picvid-modes.png" alt="Video thumbnail in picture or video mode"/></td>
<td>미리 보기가 사각형 가로 세로 비율로 잘립니다. <br />
<img src="images/thumbnail-vid-doclistmusic-modes.png" alt="Video thumbnail in documents, music, or list mode"/> </td>
<td>미리 보기 이미지가 파일의 원래 가로 세로 비율을 사용합니다. <br />
<img src="images/thumbnail-vid-single-mode.png" alt="Video thumbnail in single mode"/></td>
</tr>
<tr>
<td>음악</td>
<td>미리 보기는 적절한 크기의 배경에 있는 아이콘입니다. 배경색은 앱의 타일 배경색에 의해 결정됩니다. <br />
<img src="images/thumbnail-music-picvid-modes.png" alt="Music thumbnail in picture or video mode"/></td>
<td>파일에 앨범 이미지가 있는 경우 미리 보기는 앨범 이미지입니다.  <br />
<img src="images/thumbnail-music-doclistmusic-modes.png" alt="Music thumbnail in documents, music, or list mode"/> <br />
그렇지 않은 경우 미리 보기는 적절한 크기의 배경에 있는 아이콘입니다.</td>
<td>파일에 앨범 이미지가 있는 경우 미리 보기는 파일의 원래 가로 세로 비율을 사용하는 앨범 이미지입니다.  <br />
<img src="images/thumbnail-music-single-mode.png" alt="Music thumbnail in single mode"/> <br />
그렇지 않은 경우 미리 보기는 아이콘입니다. </td>
</tr>
<tr>
<td>문서</td>
<td>미리 보기는 적절한 크기의 배경에 있는 아이콘입니다. 배경색은 앱의 타일 배경색에 의해 결정됩니다. <br />
<img src="images/thumbnail-docs-picvid-modes.png" alt="Document thumbnail in picture or video mode"/></td>
<td>미리 보기는 적절한 크기의 배경에 있는 아이콘입니다. 배경색은 앱의 타일 배경색에 의해 결정됩니다. <br />
<img src="images/thumbnail-doc-doclistmusic-modes.png" alt="Document thumbnail in documents, music, or list mode"/></td>
<td>미리 보기가 있는 경우 문서 미리 보기입니다. <br />
<img src="images/thumbnail-doc1-single-mode.png" alt="Document thumbnail in single mode"/><br />
그렇지 않은 경우 미리 보기는 아이콘입니다. <br />
<img src="images/thumbnail-doc2-single-mode.png" alt="Document thumbnail icon in single mode"/></td>
</tr>
<tr>
<td>폴더</td>
<td>폴더에 사진 파일이 있는 경우 사진 미리 보기가 사용됩니다.  <br />
<img src="images/thumbnail-dir-picvid-modes.png" alt="Folder thumbnail in picture or video mode"/> <br />
그렇지 않은 경우 미리 보기가 검색되지 않습니다.</td>
<td>미리 보기 이미지가 검색되지 않습니다.</td>
<td>미리 보기가 폴더 아이콘입니다.<br />
<img src="images/thumbnail-dir-single-mode.png" alt="Folder icon thumbnail in single mode"/></td>
</tr>
<tr>
<td>파일 그룹</td>
<td>폴더에 사진 파일이 있는 경우 사진 미리 보기가 사용됩니다.<br />
<img src="images/thumbnail-grp-picvid-modes.png" alt="File group thumbnail in picture or video mode"/> <br /> 그렇지 않은 경우 미리 보기가 검색되지 않습니다. </td>
<td>그룹의 파일 중에 앨범 이미지가 있는 파일이 있으면 미리 보기는 앨범 이미지입니다. <br />
<img src="images/thumbnail-grp-doclistmusic-modes.png" alt="File group thumbnail in documents, music or list mode"/> <br />그렇지 않은 경우 미리 보기가 검색되지 않습니다. </td>
<td>그룹의 파일 중에 앨범 이미지가 있는 파일이 있으면 미리 보기는 앨범 이미지이고 파일의 원래 가로 세로 비율을 사용합니다. <br />
<img src="images/thumbnail-grp1-single-mode.png" alt="File group thumbnail in picture or video mode"/> <br />그렇지 않은 경우 미리 보기는 파일 그룹을 표시하는 아이콘입니다. <br />
<img src="images/thumbnail-grp2-single-mode.png" alt="File group icon in single mode"/> 
</td>
</tr>
</table>
</div>

## <a name="related-topics"></a>관련 항목
- [ThumbnailMode enum](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)
- [StorageItemThumbnail 클래스](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemThumbnail)
- [StorageFile 클래스](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)
- [파일 및 폴더 미리 보기 샘플(GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileThumbnails)
- [목록 및 그리드 보기](../design/controls-and-patterns/lists.md)