---
description: Apple Boot 캠프, Oracle VirtualBox, VMware Fusion 및 도선 데스크톱과 같은 타사 솔루션과 함께 Mac을 사용 하 여 UWP 앱을 개발 하는 방법을 알아봅니다.
title: Windows 10에서 Mac 설정
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1f8b27eb9555633a9a092f3d324e0966d3ec453f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162227"
---
# <a name="setting-up-your-mac-with-windows-10"></a>Windows 10에서 Mac 설정


현재 Mac 컴퓨터를 사용 하 여 Windows 용 앱을 개발할 수 있습니다.

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Mac에서 Windows를 실행 하 고 Visual Studio를 사용 합니다.

유니버설 Windows 앱 개발을 시작할 준비가 되셨습니까? PC를 사용할 수 없나요? 이제 Mac을 사용할 수 있습니다. Apple Boot 캠프, Oracle VirtualBox, VMware Fusion 및 위 도선 데스크톱과 같은 인기 있는 타사 솔루션을 사용 하 여 Apple 컴퓨터에 Windows 10 및 Microsoft Visual Studio를 설치할 수 있습니다.

**참고**    디스크 또는 USB 플래시 드라이브에 Windows 10 부팅 가능 이미지가 필요 합니다. MSDN 구독자 인 경우 MSDN 구독자 다운로드 센터에서 설치 이미지를 다운로드할 수 있습니다. 구독자가 아닌 경우 [Microsoft Store](https://www.microsoft.com/store/apps)에서 설치 관리자를 구입할 수 있습니다. [이 위치](https://www.microsoft.com/software-download/windows10)에서 다운로드할 수도 있습니다 .이는 이미 Windows를 실행 중이 고 업그레이드 하려는 경우에 유용 합니다.

Windows를 실행 한 후 [에는 windows 10 용 개발자 다운로드](https://developer.microsoft.com/windows/downloads) 에서 최신 버전의 Visual Studio를 설치 하 고 앱 작성을 시작할 수 있습니다.

**참고**    Visual Studio 장치 에뮬레이터를 사용 하려는 경우 64 비트 (x64) 버전의 Windows 10 Pro 이상 버전을 설치 **해야** 합니다. 아쉽게도 일부 이전 Mac에서는 64 비트 Windows를 실행할 수 없습니다. 이[apple 지원 페이지](https://support.apple.com/kb/HT5634)에서 하드웨어가 호환 되는 경우 apple을 확인 하세요.

## <a name="apple-boot-camp"></a>Apple 부트 캠프

부팅 캠프 길잡이 앱은 모든 최신 Mac에 미리 설치 되어 있으며 시작 하면 Windows 10을 설치 하는 과정을 안내 합니다. 따라서 30GB 이상의 여유 디스크 공간을 준비하고 위에 나열된 원본에서 Windows를 복사하기만 하면 됩니다. 설치가 완료 되 면 Mac OSX 또는 Windows 10으로 부팅 하도록 선택할 수 있습니다. 자세한 내용은 Apple의 [부트 캠프 지침 페이지](https://support.apple.com/HT201468)를 참조 하세요.

## <a name="parallels-desktop"></a>위 도선 바탕 화면

병렬 데스크톱 11을 사용 하 여 Visual Studio 및 Cortana를 비롯 한 기존 Mac 응용 프로그램과 함께 Windows 앱을 함께 실행할 수 있습니다. 향상 된 디버깅, Docker 및 Jenkins 지원 등 개발자를 위한 추가 기능을 포함 하는 pro 버전을 사용할 수 있습니다. 자세한 내용과 무료 평가판 버전은 [위 도선 바탕 화면](https://www.parallels.com/download/desktop/)을 참조 하세요.

## <a name="vmware-fusion"></a>VMWare Fusion

VMWare의 Fusion 8을 사용 하면 Mac 바탕 화면에서 Visual Studio를 바로 실행할 수 있습니다. Pro 버전은 vSphere 지원 등의 몇 가지 고급 기능을 개발자에 게 제공 하는 데 사용할 수 있습니다. 자세한 내용과 무료 평가판 버전은 [VMware Fusion](http://www.vmware.com/products/fusion/)을 참조 하십시오.

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox는 컴퓨터에서 가상 컴퓨터를 실행 하기 위한 무료 응용 프로그램이 며 Mac에서 Windows를 실행할 수 있도록 지원 합니다. Frills 옵션 이지만 가격은 효과가 있습니다. 자세한 내용은 [Virtualbox](https://www.virtualbox.org/wiki/Downloads)를 참조 하세요.

