---
author: PatrickFarley
title: 연결된 앱 및 장치(프로젝트 로마)
description: 이 섹션에서는 원격 시스템 플랫폼을 사용하여 원격 디바이스를 검색하고, 원격 디바이스에서 앱을 실행하고, 원격 디바이스의 앱 서비스와 통신하는 방법을 설명합니다.
ms.author: pafarley
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp, 연결 된 장치, 원격 시스템, 로마, 프로젝트 로마
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: 8c462bb28de23db735b957870cf456d3649282ae
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6200928"
---
# <a name="connected-apps-and-devices-project-rome"></a>연결된 앱 및 장치(프로젝트 로마)

이 섹션에서는 프로젝트 로마를 사용하여 장치 및 플랫폼 간에 앱을 연결하는 방법을 설명합니다. 원격 디바이스를 검색하고, 원격 디바이스에서 앱을 실행하고, 원격 디바이스의 앱 서비스와 통신하는 방법을 알아봅니다.

대부분의 사용자는 여러 디바이스를 가지고 있으며 한 디바이스에서 작업을 시작한 후 다른 디바이스에서 완료하는 경우도 많습니다. 이러한 경우를 처리하려면 앱이 여러 디바이스와 플랫폼을 포괄해야 합니다.

Windows 10 버전 1607에서 도입된 [원격 시스템 API](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)를 통해 사용자가 한 디바이스에서 작업을 시작하고 다른 디바이스에서 완료할 수 있도록 하는 앱을 작성할 수 있습니다. 작업에 중앙 포커스가 유지되며, 사용자는 가장 편리한 장치에서 해당 작업을 수행할 수 있습니다. 예를 들어 사용자는 자동차에서 휴대폰을 통해 라디오를 듣지만 집에 도착하면 홈 스테레오 시스템에 연결된 Xbox One으로 재생을 전송할 수 있습니다.

도우미 장치나 원격 제어 시나리오에서 프로젝트 로마를 사용할 수도 있습니다. 앱 서비스 메시지 API를 사용하여 두 장치 간에 사용자 지정 메시지를 주고받을 앱 채널을 만들 수 있습니다. 예를 들어 TV의 재생을 제어하는 휴대폰용 앱이나 다른 앱에서 시청 중인 TV 프로그램의 등장 인물에 대한 정보를 제공하는 도우미 앱을 작성할 수 있습니다.  

Bluetooth 및 무선을 통해 근접해서 또는 클라우드를 통해 원격으로 디바이스를 연결할 수 있으며 사용자의 Microsoft 계정(MSA)으로 연결됩니다.

원격 시스템을 검색하고, 원격 시스템에서 앱을 실행하고, 앱 서비스를 사용하여 두 시스템에서 실행 중인 앱 간에 메시지를 보내는 방법의 예제는 [원격 시스템 UWP 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )을 참조하세요.

플랫폼 간 통합용 리소스를 비롯하여 일반적인 프로젝트 로마의 자세한 내용은 [aka.ms/project-rome](https://aka.ms/project-rome)에서 찾을 수 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [원격 장치에서 앱 실행](launch-a-remote-app.md) | 원격 장치에서 앱을 시작하는 방법을 알아봅니다. 이 항목은 간단한 사용 사례 및 사전 설정을 설명합니다.  |
| [원격 장치 검색](discover-remote-devices.md)  | 연결할 수 있는 장치를 검색하는 방법을 알아봅니다. |
| [원격 앱 서비스와 통신](communicate-with-a-remote-app-service.md) | 원격 장치에서 앱을 조작하는 방법을 알아봅니다. |
| [원격 세션을 통해 장치 연결](remote-sessions.md) | 원격 세션에서 여러 장치를 연결하여 공유되는 환경을 만듭니다. |
| [장치 간 사용자 활동 계속 수행](useractivities.md)| 사용자가 던 작업 앱에서 여러 장치에서 다시 시작 하는 데 도움이 됩니다.|
| [사용자 활동 모범 사례](useractivities-best-practices.md)| 만들고 사용자 활동을 업데이트 하기 위한 권장 되는 방법을 알아봅니다.|
