---
title: Xbox 개발 콘솔 구성
author: KevinAsgari
description: Xbox Live 개발을 지원 하기 위해 Xbox 개발 콘솔을 구성 하는 방법을 알아봅니다.
ms.assetid: f8fd1caa-b1e9-4882-a01f-8f17820dfb55
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 53126e185b9d94c911abab8999e3ca1da8691c7c
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5476748"
---
# <a name="configure-your-xbox-development-console"></a>Xbox 개발 콘솔 구성

개발 콘솔을 구성 하려면
- 사용자 id
- 개발 키트에 샌드박스를 설정 합니다.
- 개발 계정으로 로그인

## <a name="get-your-ids"></a>사용자 id
샌드박스 및 Xbox Live 서비스를 사용 하려면 개발 키트 및 타이틀을 구성 하는 몇 가지 Id를 가져오는 해야 합니다. 이러한 동일한 프로세스를 사용 하 여 수행할 수 있습니다.

[Xbox Live 서비스 구성](../xbox-live-service-configuration.md) id에 따라

## <a name="set-your-sandbox-on-your-development-kits"></a>개발 키트에 샌드박스를 설정 합니다.
샌드박스 id. 설정 하지 않고 개발 키트를 부팅할 수 없습니다. 이렇게 하려면 "Xbox One 관리자는" XDK, 하 여 PC에 설치 되어 있는 사용 하거나 XDK 명령 창을 열고 다음과 같이 구성 (xbconfig.exe) 명령을 사용 하 여 수 있습니다.

에 있는 현재 샌드박스를 확인 합니다. 명령 프롬프트에서 xbconfig sandboxid를 입력 합니다.

들지 인 경우 샌드박스 id를 변경 합니다. 입력 xbconfig sandboxid =<your sandbox id> 명령 프롬프트.

명령 프롬프트에서 재부팅이 (xbreboot.exe)를 사용 하 여 콘솔을 다시 부팅 합니다.

샌드박스에 올바르게 다시 설정 되었는지 확인 합니다. 명령 프롬프트에서 xbconfig sandboxid를 입력 합니다.

## <a name="sign-in-with-a-development-account"></a>개발 계정으로 로그인

에서는 [Xbox 개발자 포털 (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) 또는 [Windows 개발자 센터](https://developer.microsoft.com/en-us/windows) 에서 로그인 하는 데 사용 하는 개발 계정을 만들 수 있습니다.
