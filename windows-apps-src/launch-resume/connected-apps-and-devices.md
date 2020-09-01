---
title: 연결된 앱 및 디바이스(프로젝트 로마)
description: 이 섹션에서는 원격 시스템 플랫폼을 사용 하 여 원격 장치를 검색 하 고, 원격 장치에서 앱을 시작 하 고, 원격 장치에서 앱 서비스와 통신 하는 방법을 설명 합니다.
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp, 연결 된 장치, 원격 시스템, 로마, 프로젝트 로마
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: 08004f22575be69fff3f8d8017ea34327d9a0b7a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156037"
---
# <a name="connected-apps-and-devices-project-rome"></a>연결된 앱 및 디바이스(프로젝트 로마)

이 섹션에서는 [프로젝트 로마](https://developer.microsoft.com/windows/project-rome)를 사용 하 여 장치 및 플랫폼에서 앱을 연결 하는 방법을 설명 합니다. 플랫폼 간 시나리오에서 프로젝트 로마를 구현 하는 방법에 대해 알아보려면 [프로젝트 로마의 기본 문서 페이지](/windows/project-rome/)를 방문 하세요.

대부분의 사용자에 게는 장치가 여러 개 있으며 한 장치에서 작업을 시작 하 고 다른 장치에서 작업을 완료 하는 경우가 많습니다. 이를 수용 하기 위해 앱은 장치 및 플랫폼을 확장 해야 합니다. 프로젝트 로마를 사용 하면 원격 장치를 검색 하 고, 원격 장치에서 앱을 시작 하 고, 원격 장치에서 앱 서비스와 통신할 수 있습니다.

Windows 10, 버전 1607에 도입 된 [원격 시스템 api](/uwp/api/Windows.System.RemoteSystems) 를 사용 하면 사용자가 한 장치에서 작업을 시작 하 고 다른 장치에서 작업을 완료할 수 있도록 하는 앱을 작성할 수 있습니다. 작업은 중앙 집중 포커스를 유지 하 고 사용자가 가장 편리한 장치에서 작업을 수행할 수 있습니다. 예를 들어 사용자가 자동차에서 휴대폰의 라디오를 수신할 수 있지만 집에 있을 때 홈 스테레오 시스템으로 후크 된 Xbox에 재생을 전송 하려고 할 수 있습니다.

또한 동반 장치 또는 원격 제어 시나리오에 대해 프로젝트 로마를 사용할 수 있습니다. App service messaging Api를 사용 하 여 두 장치 간에 앱 채널을 만들어 사용자 지정 메시지를 보내고 받습니다. 예를 들어 TV에 대 한 재생을 제어 하는 휴대폰 또는 다른 앱을 통해 시청 하는 TV 쇼의 문자에 대 한 정보를 제공 하는 도우미 앱을 작성할 수 있습니다.  

장치는 Bluetooth 및 무선을 통해 proximally 또는 클라우드를 통해 원격으로 연결 될 수 있습니다. 이러한 파일은 해당 사용자를 사용 하는 사용자의 Microsoft 계정 (MSA)에 의해 연결 됩니다.

원격 시스템을 검색 하 고, 원격 시스템에서 앱을 시작 하 고, 앱 서비스를 사용 하 여 두 시스템에서 실행 되는 앱 간에 메시지를 보내는 방법에 대 한 예제는 [원격 시스템 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) 을 참조 하세요.

플랫폼 간 통합에 대 한 리소스를 비롯 하 여 일반적으로 프로젝트 로마에 대 한 자세한 내용은 [aka.ms/project-rome](https://developer.microsoft.com/windows/project-rome)를 참조 하세요.

| 항목 | 설명 |
|-------|-------------|
| [원격 디바이스에서 앱 시작](launch-a-remote-app.md) | 원격 디바이스에서 앱을 시작하는 방법을 알아봅니다. 이 항목에서는 가장 간단한 사용 사례 및 예비 설정에 대해 설명 합니다.  |
| [원격 디바이스 검색](discover-remote-devices.md)  | 연결할 수 있는 디바이스를 검색하는 방법을 알아봅니다. |
| [원격 앱 서비스와 통신](communicate-with-a-remote-app-service.md) | 원격 디바이스에서 앱을 조작하는 방법을 알아봅니다. |
| [원격 세션을 통해 디바이스 연결](remote-sessions.md) | 원격 세션에서 여러 디바이스를 연결하여 공유 환경을 만듭니다. |
| [디바이스 간에도 사용자 활동 계속 수행](useractivities.md)| 사용자가 앱에서 수행 하 던 작업을 여러 장치에 걸쳐 다시 시작 하도록 지원 합니다.|
| [사용자 활동 모범 사례](useractivities-best-practices.md)| 사용자 활동을 만들고 업데이트 하기 위한 권장 방법에 대해 알아봅니다.|