---
title: Xbox 개발 콘솔 구성
description: Xbox Live 개발을 지 원하는 개발 Xbox 본체를 구성 하는 방법에 알아봅니다.
ms.assetid: f8fd1caa-b1e9-4882-a01f-8f17820dfb55
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 479be2401e0c54801645ad1c0d91b11b7ffb6869
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649018"
---
# <a name="configure-your-xbox-development-console"></a>Xbox 개발 콘솔 구성

개발 콘솔을 구성 합니다.
- 사용자 Id를 가져오려면
- 프로그램 개발 키트 샌드박스에 설정
- 개발 계정으로 로그인

## <a name="get-your-ids"></a>사용자 Id를 가져오려면
샌드박스 및 Xbox Live 서비스를 사용 하려면 개발 키트 및 제목을 구성 하려면 여러 Id를 가져오는 해야 합니다. 이러한 동일한 프로세스를 사용 하 여 수행할 수 있습니다.

따릅니다 [Xbox Live 서비스 구성을](../xbox-live-service-configuration.md) 사용자 Id를 가져오려면

## <a name="set-your-sandbox-on-your-development-kits"></a>프로그램 개발 키트 샌드박스에 설정
Id입니다. 샌드박스를 설정 하지 않고 개발 키트를 부팅할 수 없습니다. 이렇게 하려면 "Xbox 하나 관리자" XDK, 하 여 PC에 설치 되어 있는 사용 하거나 XDK 명령 창을 열고 다음과 같이 구성 (xbconfig.exe) 명령을 사용 하 여 수 있습니다.

현재 샌드박스에 확인 합니다. 명령 프롬프트에서 xbconfig sandboxid를 입력 합니다.

기대와 다르면 인 경우에 샌드박스 id를 변경 합니다. Xbconfig sandboxid 입력 =<your sandbox id> 명령 프롬프트에서.

다시 부팅 (xbreboot.exe)를 사용 하 여 명령 프롬프트에서 콘솔을 다시 부팅 합니다.

샌드박스에 올바르게 다시 설정 되었는지 확인 합니다. 명령 프롬프트에서 xbconfig sandboxid를 입력 합니다.

## <a name="sign-in-with-a-development-account"></a>개발 계정으로 로그인

에 로그인 하는 데 사용 되는 개발 계정을 만들 수 있습니다 [Xbox 개발자 포털 (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) 또는 [파트너 센터](https://partner.microsoft.com/dashboard)
