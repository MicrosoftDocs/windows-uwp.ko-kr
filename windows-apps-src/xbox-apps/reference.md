---
title: Xbox 디바이스 포털 REST API
description: 콘솔을 원격으로 구성 하 고 관리 하는 데 사용 되는 REST API Xbox Device Portal에 대 한 참조 항목 목록을 참조 하세요.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: be8558daa39b126061caaa132fb134c8bdef15c1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172777"
---
# <a name="xbox-device-portal-rest-api"></a>Xbox 디바이스 포털 REST API

이 섹션에는 콘솔을 원격으로 구성 하 고 관리 하는 데 사용 되는 Xbox Device Portal REST API에 대 한 참조 항목이 포함 되어 있습니다.

| URI        | 설명 |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| 느슨한 폴더에 포함 된 앱을 등록 합니다. |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| 콘솔에 전체 폴더를 업로드 합니다. |
|[/ext/app/sshpins](uwp-sshpins-api.md)| 모든 신뢰할 수 있는 SSH pin을 원격으로 지웁니다. Visual Studio UWP 개발에 대 한 pin 페어링을 다시 수행 해야 합니다. |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| 하나 이상의 설치 된 패키지에 대 한 배포 정보를 요청 합니다. |
|[/ext/fiddler](wdp-fiddler-api.md)| Fiddler 네트워크 추적을 사용 하거나 사용 하지 않도록 설정 합니다. |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| Xbox의 집중 된 앱에서 HTTP 트래픽을 가져옵니다. |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| 네트워크 자격 증명을 추가, 제거 또는 업데이트 합니다. |
|[/ext/remoteinput](uwp-remoteinput-api.md)| Xbox에 원격으로 키보드, 마우스 또는 컨트롤러 입력을 보냅니다. |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| 연결 된 실제 컨트롤러의 수를 가져오거나 모든 실제 컨트롤러를 해제 합니다. |
|[/ext/screenshot](wdp-media-capture-api.md)| 콘솔에 현재 표시 된 화면의 PNG 표시를 캡처합니다. |
|[/ext/settings](wdp-xboxsettings-api.md)| Xbox One 개발자 설정에 액세스 합니다. |
|[/ext/smb/developerfolder](wdp-smb-api.md)| 개발 PC의 파일 탐색기를 통해 콘솔의 개발자 폴더에 액세스 합니다. |
|[/ext/user](wdp-user-management.md)| Xbox One 콘솔의 사용자를 관리 합니다. |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| Xbox One 장치에 대 한 정보를 제공 합니다. |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| Xbox Live 샌드박스를 관리 합니다. |

## <a name="see-also"></a>참고 항목

- [Xbox One의 UWP](index.md)
- [Windows Device Portal](../debug-test-perf/device-portal.md)