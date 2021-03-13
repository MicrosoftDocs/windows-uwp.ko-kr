---
title: Windows 10 용 Powertoy Image 크기 조정 유틸리티
description: 대량 이미지 크기 조정을 위한 Windows 셸 확장
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e417a70e34ae1e5fdba95e2838a6221b3236e1c6
ms.sourcegitcommit: a1b251971f7ac574275d53bbe3e9ef4a3a9dc15c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2021
ms.locfileid: "103417081"
---
# <a name="image-resizer-utility"></a>Image 크기 조정 유틸리티

Image 크기 조정는 이미지를 대량으로 크기 조정 하는 Windows 셸 확장입니다. Powertoy를 설치한 후 파일 탐색기에서 하나 이상의 선택한 이미지 파일을 마우스 오른쪽 단추로 클릭 하 고 메뉴에서 **그림 크기 조정** 을 선택 합니다.

![Image 크기 조정 Demo](../images/powertoys-resize-images.gif)

## <a name="drag-and-drop"></a>끌어서 놓기

이미지 크기 조정을 사용 하면 **마우스 오른쪽 단추를 사용** 하 여 선택한 파일을 끌어서 놓아 이미지 크기를 조정할 수도 있습니다. 이렇게 하면 크기가 조정 되는 그림을 다른 폴더에 빠르게 저장할 수 있습니다.

![이미지 크기 조정 끌어서 놓기 데모](../images/powertoys-resize-drag-drop.gif)

## <a name="settings"></a>설정

Powertoy Image 크기 조정 탭 내에서 다음 설정을 구성할 수 있습니다.

![Powertoy 이미지 크기 조정 설정 메뉴](../images/powertoys-imageresize-settings.png)

### <a name="sizes"></a>크기

새 기본 설정 크기를 추가 합니다. 각 크기는 채우기, 맞춤 또는 늘이기로 구성할 수 있습니다. 크기를 조정 하는 데 사용할 차원을 센티미터, 인치, 백분율 및 픽셀로 구성할 수도 있습니다.

#### <a name="fill-vs-fit-vs-stretch"></a>채우기 vs 맞추기 vs Stretch

- **채우기:** 이미지를 사용 하 여 지정 된 전체 크기를 채웁니다. 이미지 크기를 비례적으로 조정 합니다. 필요에 따라 이미지를 자릅니다.
- **맞춤:** 전체 이미지를 지정 된 크기에 맞춥니다. 이미지 크기를 비례적으로 조정 합니다. 이미지를 자르지 않습니다.
- **스트레치:** 이미지를 사용 하 여 지정 된 전체 크기를 채웁니다. 필요에 따라 이미지를 비례적으로 확장 합니다. 이미지를 자르지 않습니다.

지정 된 크기의 너비와 높이는 현재 이미지의 방향 (세로/가로)과 일치 하도록 바꿀 수 있습니다. 항상 지정 된 너비와 높이를 사용 하려면 선택 취소: **그림의 방향을 무시** 합니다.


### <a name="fallback-encoding"></a>대체 인코딩

대체 (fallback) 인코더는 파일을 원래 형식으로 저장할 수 없을 때 사용 됩니다. 예를 들어 Windows 메타 파일 (.wmf) 이미지 형식에는 이미지를 읽는 디코더가 있지만 새 이미지를 작성 하는 인코더는 없습니다. 이 경우 이미지를 원래 형식으로 저장할 수 없습니다. 이미지 크기 조정를 사용 하 여 PNG, JPEG, TIFF, BMP, GIF 또는 WMPhoto 설정에서 사용할 수 있는 대체 인코더 형식을 지정할 수 있습니다. *이는 파일 형식 변환 도구가 아니라 지원 되지 않는 파일 형식에 대 한 대체 방식 으로만 작동 합니다.*

### <a name="file"></a>파일

다음 매개 변수를 사용 하 여 크기 조정 된 이미지의 파일 이름을 수정할 수 있습니다.

- `%1`: 원래 파일 이름
- `%2`: 크기 이름 (Powertoy Image 크기 조정 설정에서 구성 됨)
- `%3`: 선택한 너비
- `%4`: 선택한 높이
- `%5`: 실제 높이
- `%6`: 실제 너비

예를 들어 파일 이름 형식을로 설정 하 `%1 (%2)` `example.png` 고 파일 크기 설정을 선택 하면 `Small` 파일 이름이 생성 됩니다 `example (Small).png` .

파일에서 형식을로 설정 하 `%1_%4` `example.jpg` 고 크기 설정을 선택 하면 `Medium 1366 x 768px` 파일 이름이 `example_768.jpg` 로 설정 됩니다.

크기 조정 된 이미지의 원래 *마지막으로 수정한* 날짜를 유지 하도록 선택할 수도 있습니다.

### <a name="auto-widthheight"></a>자동 너비/높이

높이나 너비는 비워 둘 수 있습니다. 그러면 지정 된 차원이 적용 되 고 다른 차원이 원래 이미지 가로 세로 비율에 비례 하는 값으로 "잠금" 됩니다.

### <a name="sub-directories"></a>하위 디렉터리

파일 이름 형식의 디렉터리를 지정 하 여 크기 조정 된 이미지를 하위 디렉터리로 그룹화 할 수 있습니다. 예를 들어의 값은 `%2\%1` 크기 조정 된 이미지를로 저장 합니다. `Small\Sample.jpg`
