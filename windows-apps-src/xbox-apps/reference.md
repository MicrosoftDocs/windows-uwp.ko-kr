---
author: v-angraf
title: Xbox 장치 포털 REST API
description: Xbox One의 UWP에 대한 API 참조입니다.
ms.author: v-angraf
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: 894bc6f657f4a65072056a14171bf86b92cced38
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6657171"
---
# <a name="xbox-device-portal-rest-api"></a>Xbox 장치 포털 REST API

이 섹션은 원격으로 콘솔을 구성하고 관리하는 데 사용되는 Xbox 장치 포털 REST API에 대한 참조 항목을 포함합니다.

| URI        | 설명 |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| 느슨한 폴더에 포함된 앱을 등록합니다. |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| 전체 폴더를 콘솔에 업로드합니다. |
|[/ext/app/sshpins](uwp-sshpins-api.md)| 원격으로 모든 신뢰할 수 있는 SSH 핀을 지웁니다. Visual Studio UWP 개발을 위해 핀 페어링을 다시 수행해야 합니다. |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| 설치된 하나 이상의 패키지에 대한 배포 정보를 요청합니다. |
|[/ext/fiddler](wdp-fiddler-api.md)| Fiddler 네트워크 추적을 사용 및 사용하지 않도록 설정합니다. |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| Xbox의 포커스된 앱에서 HTTP 트래픽을 가져옵니다. |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| 네트워크 자격 증명을 추가, 제거 또는 업데이트합니다. |
|[/ext/remoteinput](uwp-remoteinput-api.md)| Xbox에 원격으로 키보드, 마우스 또는 컨트롤러 입력을 전송합니다. |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| 연결된 실제 컨트롤러의 수를 가져오거나 모든 실제 컨트롤러를 끕니다. |
|[/ext/screenshot](wdp-media-capture-api.md)| 현재 콘솔에 표시되는 화면의 PNG 표현을 캡처합니다. |
|[/ext/settings](wdp-xboxsettings-api.md)| Xbox One 개발자 설정에 액세스합니다. |
|[/ext/smb/developerfolder](wdp-smb-api.md)| 개발 PC에서 파일 탐색기를 통해 콘솔의 개발자 폴더에 액세스합니다. |
|[/ext/user](wdp-user-management.md)| Xbox One 본체에서 사용자를 관리합니다. |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| Xbox One 장치에 대한 정보를 제공합니다. |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| Xbox Live 샌드박스를 관리합니다. |

## <a name="see-also"></a>참고 항목

- [Xbox One의 UWP](index.md)
- [Windows 장치 포털](../debug-test-perf/device-portal.md)