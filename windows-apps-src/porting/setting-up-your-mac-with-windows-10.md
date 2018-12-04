---
description: 현재 Mac 컴퓨터를 사용하여 Windows용 앱을 개발합니다.
title: Windows 10에서 Mac 설정
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 61918f8f81137dad4db9bf627b7c87e0c05ace8d
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8471651"
---
# <a name="setting-up-your-mac-with-windows-10"></a>Windows 10에서 Mac 설정


현재 Mac 컴퓨터를 사용하여 Windows용 앱을 개발합니다.

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Mac에서 Windows를 실행하고 Visual Studio를 사용합니다.

유니버설 Windows 앱 개발을 시작할 준비가 되었지만 편리한 PC가 없나요? 상관없습니다. Mac을 사용할 수 있으니까요! Apple Boot Camp, Oracle VirtualBox, VMware Fusion 및 Parallels Desktop 같은 인기 있는 타사 솔루션을 사용 하 여 Windows10 및 Microsoft Visual Studio Apple 컴퓨터에 설치할 수 있습니다.

**참고**디스크 또는 USB 플래시 드라이브에 Windows10 부팅 가능 이미지가 필요 합니다. MSDN 구독자인 경우 MSDN 구독자 다운로드 센터에서 설치 이미지를 다운로드할 수 있습니다. 구독자가 아닌 경우 [Microsoft Store](http://apps.microsoft.com/windows/app)에서 설치 프로그램을 구입할 수 있습니다. 이미 Windows를 실행하고 있으며 업그레이드하려는 경우에는 [이 위치](http://go.microsoft.com/fwlink/?LinkId=623906)에서 다운로드하는 것이 유용할 수도 있습니다.

실행 중인 Windows가 있으면 다음 [Windows10을 위한 개발자 다운로드](https://developer.microsoft.com/en-us/windows/downloads) 에서 Visual Studio의 최신 버전 설치 및 앱을 작성할 수 있습니다!

**참고**Visual Studio 장치 에뮬레이터를 사용 하려는 경우 64 비트 (x64) 버전의 Windows10 Pro 이상을 설치 **해야 합니다** . 하지만 일부 이전 Mac은 64비트 Windows를 실행할 수 없습니다. Apple에 문의하여 이 [Apple 지원 페이지](http://go.microsoft.com/fwlink/p/?LinkID=397959)에서 하드웨어가 호환되는지 확인하세요.

## <a name="apple-boot-camp"></a>Apple Boot Camp

Boot Camp 앱은 모든 최근 Mac에 미리 설치 되어 및 시작 안내 Windows10를 설치 하는 프로세스입니다. 따라서 30GB 이상의 여유 디스크 공간을 준비하고 위에 나열된 원본에서 Windows를 복사하기만 하면 됩니다. 설치되면 Mac OSX 또는 Windows 10으로 부팅하도록 선택할 수 있습니다. 자세한 내용은 Apple의 [Boot Camp 지침 페이지](http://go.microsoft.com/fwlink/?LinkId=623912)를 참조하세요.

## <a name="parallels-desktop"></a>Parallels Desktop

Parallels Desktop 11을 사용하여 Visual Studio 및 Cortana를 비롯한 Windows 앱을 기존 Mac 응용 프로그램과 나란히 실행할 수 있습니다. 향상된 디버깅, Docker 및 Jenkins 지원을 비롯한 개발자용 추가 기능이 포함된 Pro 버전을 사용할 수 있습니다. 자세한 내용을 보고 무료 평가판 버전을 다운로드하려면 [Parallels Desktop](http://go.microsoft.com/fwlink/p/?LinkId=281827)을 참조하세요.

## <a name="vmware-fusion"></a>VMWare Fusion

VMWare의 Fusion 8을 사용하면 Mac 바탕 화면에서 바로 Visual Studio를 실행할 수 있습니다. Pro 버전은 개발자에게 vSphere 지원과 같은 일부 고급 기능을 제공합니다. 자세한 내용을 보고 무료 평가판 버전을 다운로드하려면 [VMware Fusion](http://go.microsoft.com/fwlink/p/?LinkId=281826)을 참조하세요.

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox는 컴퓨터에서 가상 컴퓨터를 실행하기 위한 무료 응용 프로그램으로, Mac에서 Windows를 실행하도록 지원합니다. 꼭 필요한 요소를 포함하지만 가격은 적절합니다. 자세한 내용은 [VirtualBox](http://go.microsoft.com/fwlink/p/?LinkId=280599)를 참조하세요.

