---
title: 컨테이너를 사용하는 원격 개발을 위한 Docker 시작
description: Windows 또는 WSL에서 Docker Desktop을 시작하는 방법에 대한 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, Docker, WSL, 원격 개발, 컨테이너, Docker Desktop, Windows 및 WSL
ms.date: 09/24/2020
ms.openlocfilehash: b3fc2509aa6a623bebd9f4566f3b8e75301251db
ms.sourcegitcommit: c65f62bda57563f6196691e7b9c25cbf5a8b16e5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91780585"
---
# <a name="overview-of-docker-remote-development-on-windows"></a>Windows의 Docker 원격 개발에 대한 개요

원격 개발에 컨테이너를 사용하고 Docker 플랫폼을 통해 애플리케이션을 배포하는 것은 많은 이점이 있는 매우 인기 있는 솔루션입니다. WSL(Linux용 Windows 하위 시스템), Visual Studio, Visual Studio Code, .NET 및 다양한 Azure 서비스를 포함한 Microsoft 도구 및 서비스에서 제공하는 다양한 지원에 대해 자세히 알아봅니다.

## <a name="docker-on-windows-10"></a>Windows 10의 Docker

:::row:::
    :::column:::
       [![Docker 문서 아이콘](../../images/docker-docs-icon.png)](https://docs.docker.com/docker-for-windows/install/)<br>
        **[Windows용 Docker Desktop 설치](https://docs.docker.com/docker-for-windows/install/)**<br>
        설치 단계, 시스템 요구 사항, 설치 관리자에 포함된 기능, 제거 방법, 안정적 버전과 에지 버전 간의 차이점, Windows 및 Linux 컨테이너 간의 전환 방법을 알아봅니다.
    :::column-end:::
    :::column:::
       [![Docker 실행 스크린샷](../../images/docker-running-screenshot.png)](https://docs.docker.com/get-started/)<br>
        **[Docker 시작](https://docs.docker.com/get-started/)**<br>
        비디오 연습을 포함하여 시작하는 방법에 대한 단계별 지침이 포함된 Docker 예비 교육 및 설정 문서입니다.
    :::column-end:::
    :::column:::
       [![Microsoft Learn Docker 과정 스크린샷](../../images/docker-learn-course.png)](/learn/modules/intro-to-docker-containers/)<br>
        **[MS Learn 과정: Docker 컨테이너 소개](/learn/modules/intro-to-docker-containers/)**<br>
        Microsoft Learn은 Docker 시작 및 Azure 서비스 연결에 대한 [다양한 과정](/learn/browse/?terms=docker) 외에도 Docker 컨테이너에 대한 무료 소개 과정을 제공합니다.
    :::column-end:::
    :::column:::
       [![Docker Desktop WSL2 메뉴 스크린샷](../../images/docker-wsl2.png)](/windows/wsl/tutorials/wsl-containers)<br>
        **[WSL 2에서 Docker 원격 컨테이너 시작](/windows/wsl/tutorials/wsl-containers)**<br>
        WSL 2(Linux용 Windows Subsystem, 버전 2)를 사용하여 Linux 명령줄(Ubuntu, Debian, SUSE 등)에서 사용하도록 Windows용 Docker Desktop을 설정하는 방법을 알아봅니다.
    :::column-end:::
:::row-end:::

## <a name="vs-code-and-docker"></a>VS Code 및 Docker

:::row:::
    :::column:::
       [![VS Code 원격 컨테이너 그래픽](../../images/vscode-remote-containers.png)](https://code.visualstudio.com/docs/remote/create-dev-container)<br>
        **[VS Code를 사용하여 Docker 컨테이너 만들기](https://code.visualstudio.com/docs/remote/containers-tutorial)**<br>
        [원격 - 컨테이너 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)을 사용하여 컨테이너 내에서 완전한 기능을 갖춘 개발 환경을 설정하고, [NodeJS 컨테이너](https://code.visualstudio.com/docs/containers/quickstart-node), [Python 컨테이너](https://code.visualstudio.com/docs/containers/quickstart-python) 또는 [ASP.NET Core 컨테이너](https://code.visualstudio.com/docs/containers/quickstart-aspnet-core)를 설정하는 방법에 대한 자습서를 찾습니다.
    :::column-end:::
    :::column:::
       [![VSCode 연결 Docker 스크린샷](../../images/vscode-attach-docker.png)](https://code.visualstudio.com/docs/remote/attach-container)<br>
        **[Docker 컨테이너에 VS Code 연결](https://code.visualstudio.com/docs/remote/attach-container)**<br>
        Visual Studio Code를 이미 실행하고 있는 Docker 컨테이너 또는 [Kubernetes 클러스터의 컨테이너](https://code.visualstudio.com/docs/remote/attach-container#_attach-to-a-container-in-a-kubernetes-cluster)에 연결하는 방법을 알아봅니다.
    :::column-end:::
    :::column:::
       [![VSCode 컨테이너 메뉴 스크린샷](../../images/vscode-advanced-docker.png)](https://code.visualstudio.com/docs/remote/containers-advanced)<br>
        **[고급 컨테이너 구성](https://code.visualstudio.com/docs/remote/containers-advanced)**<br>
        Visual Studio Code에서 Docker 컨테이너를 사용하는 방법에 대한 고급 설정 시나리오에 대해 알아보거나, VS Code를 사용하여 디버깅하기 위해 이 문서에서 [컨테이너를 검사](https://code.visualstudio.com/blogs/2019/10/31/inspecting-containers)하는 방법을 알아봅니다.
    :::column-end:::
    :::column:::
       [![WSL을 사용하는 VSCode Docker Desktop 스크린샷](../../images/vscode-docker-wsl.png)](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)<br>
        **[WSL 2에서 원격 컨테이너 사용](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)**<br>
        WSL 2(Linux용 Windows 하위 시스템 버전 2)에서 Docker 컨테이너를 사용하는 방법 및 VS Code를 사용하여 모든 항목을 설정하는 방법을 알아봅니다. [작동 방식](https://code.visualstudio.com/blogs/2020/03/02/docker-in-wsl2#_how-it-works)도 알아볼 수 있습니다.
    :::column-end:::
:::row-end:::

## <a name="visual-studio-and-docker"></a>Visual Studio 및 Docker

:::row:::
    :::column:::
       [![Visual Studio 아이콘](../../images/visualstudio.png)](/visualstudio/containers/overview#docker-support-in-visual-studio-1)<br>
        **[Visual Studio의 Docker 지원](/visualstudio/containers/overview#docker-support-in-visual-studio-1)**<br>
        Visual Studio에서 컨테이너 오케스트레이션 지원 외에도 ASP.NET 프로젝트, ASP.NET Core 프로젝트, .NET Core 및 .NET Framework 콘솔 프로젝트에 사용할 수 있는 Docker 지원에 대해 알아봅니다.
    :::column-end:::
    :::column:::
       [![Visual Studio Docker 메뉴](../../images/visualstudio-docker-menu.png)](/visualstudio/containers/container-tools)<br>
        **[빠른 시작: Visual Studio의 Docker](/visualstudio/containers/container-tools)**<br>
        Visual Studio를 사용하여 컨테이너화된 .NET, ASP.NET 및 ASP.NET Core 앱을 빌드, 디버그 및 실행하고, ACR(Azure Container Registry), Docker Hub, Azure App Service 또는 사용자 고유의 컨테이너 레지스트리에 게시하는 방법을 알아봅니다.
    :::column-end:::
    :::column:::
       [![VS 자습서 스크린샷](../../images/visualstudio-tutorial.png)](/visualstudio/containers/tutorial-multicontainer)<br>
        **[자습서: Docker Compose를 사용하여 다중 컨테이너 앱 만들기](/visualstudio/containers/tutorial-multicontainer)**<br>
        Visual Studio에서 컨테이너 도구를 사용할 때 둘 이상의 컨테이너를 관리하고 서로 간에 통신하는 방법을 알아봅니다. [React 단일 페이지 앱에서 Docker를 사용](/visualstudio/containers/container-tools-react)하는 방법과 같은 자습서에 대한 링크도 확인할 수 있습니다.
    :::column-end:::
    :::column:::
       [![VS 컨테이너 링크](../../images/visualstudio-container-links.png)](/visualstudio/containers)<br>
        **[Visual Studio의 컨테이너 도구](/visualstudio/containers)**<br>
        컨테이너에서 빌드 도구를 실행하고, [Docker 앱을 디버그](/visualstudio/containers/edit-and-refresh)하고, 개발 도구 문제를 해결하며, Docker 컨테이너를 배포하고, Visual Studio와 Kubernetes를 연결하는 방법을 설명하는 항목을 확인합니다.
    :::column-end:::
:::row-end:::

![컨테이너, 이미지 및 레지스트리에 대한 기본 Docker 분류 인포그래픽](../../images/taxonomy-of-docker-terms-and-concepts.png)

## <a name="net-core-and-docker"></a>.NET Core 및 Docker

:::row:::
    :::column:::
       [![.NET 마이크로서비스 가이드 표지](../../images/dotnet-microservice-guide.png)](/dotnet/architecture/microservices/)<br>
        **[.NET 가이드: 마이크로서비스 앱 및 컨테이너](/dotnet/architecture/microservices/)**<br>
        컨테이너를 사용하여 관리되는 마이크로서비스 기반 앱에 대한 소개 가이드입니다.
    :::column-end:::
    :::column:::
       [![Docker 인포그래픽](../../images/dotnet-docker-infographic.png)](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)<br>
        **[Docker란 무엇인가요?](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)**<br>
        Docker 컨테이너에 대한 기본 설명입니다([가상 머신과 Docker 컨테이너 비교](/dotnet/architecture/microservices/container-docker-introduction/docker-defined#comparing-docker-containers-with-virtual-machines) 및 컨테이너, 이미지 및 레지스트리 간의 차이점을 설명하는 [Docker 용어 및 개념에 대한 기본 분류](/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries) 포함).
    :::column-end:::
    :::column:::
       [![Docker 분류 인포그래픽](../../images/taxonomy-of-docker-terms-and-concepts.png)](/dotnet/core/docker/build-container?tabs=windows)<br>
        **[자습서: .NET Core 앱 컨테이너화](/dotnet/core/docker/build-container?tabs=windows)**<br>
        Docker 파일 만들기, 필수 명령, 리소스 정리를 포함하여 Docker에서 .NET Core 애플리케이션을 컨테이너화하는 방법을 알아봅니다.
    :::column-end:::
    :::column:::
       [![Docker를 사용하는 내부 루프 개발 워크플로 인포그래픽](../../images/dotnet-docker-workflow.png)](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)<br>
        **[Docker 앱에 대한 개발 워크플로](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)**<br>
        Docker 컨테이너 기반 애플리케이션에 대한 내부 루프 개발 워크플로를 설명합니다.
    :::column-end:::
:::row-end:::

## <a name="azure-container-services"></a>Azure 컨테이너 서비스

:::row:::
    :::column:::
       [![Azure Container Instances 스크린샷](../../images/azure-container-instances.png)](/azure/container-instances/)<br>
        **[Azure Container Instances](/azure/container-instances/)**<br>
        관리형 서버리스 Azure 환경에서 Docker 컨테이너를 주문형 방식으로 실행하는 방법을 알아봅니다. 여기에는 Docker CLI, ARM, Azure Portal을 사용하여 배포하고, 다중 컨테이너 그룹을 만들고, 컨테이너 간에 데이터를 공유하고, 가상 네트워크에 연결하는 방법 등이 포함됩니다.
    :::column-end:::
    :::column:::
       [![Azure Container Registry 스크린샷](../../images/azure-container-registry-icon.png)](/azure/container-registry)<br>
        **[Azure Container Registry](/azure/container-registry)**<br>
        모든 유형의 컨테이너 배포를 위해 컨테이너 이미지와 아티팩트를 프라이빗 레지스트리에 작성, 저장 및 관리하는 방법을 알아봅니다. 기존 컨테이너 개발 및 배포 파이프라인에 대한 Azure 컨테이너 레지스트리를 만들고, 자동화 작업을 설정하고, 지역 복제 및 모범 사례를 포함하여 레지스트리를 관리하는 방법을 알아봅니다.
    :::column-end:::
    :::column:::
       [![Azure Service Fabric 스크린샷](../../images/azure-service-fabric.png)](/azure/service-fabric)<br>
        **[Azure Service Fabric](/azure/service-fabric)**<br>
        확장 가능하고 안정적인 마이크로서비스 및 컨테이너를 패키지, 배포 및 관리하기 위한 분산 시스템 플랫폼인 Azure Service Fabric에 대해 알아봅니다.
    :::column-end:::
    :::column:::
       [![Azure App Service 스크린샷](../../images/azure-app-service.png)](/azure/app-service)<br>
        **[Azure App Service](/azure/app-service)**<br>
        인프라를 관리하지 않고 원하는 프로그래밍 언어로 웹앱, 모바일 백 엔드 및 RESTful API를 빌드하고 호스팅하는 방법을 알아봅니다. Docker 이미지를 기반으로 하여 웹앱을 배포하고 지속적인 배포를 구성하려면 [MS Learn의 Azure App Service 과정](/learn/modules/deploy-run-container-app-service)을 사용해 보세요.
    :::column-end:::
:::row-end:::

[컨테이너를 지원하는 Azure 서비스](https://azure.microsoft.com/overview/containers/)에 대해 자세히 알아보세요.

## <a name="docker-containers-explainer-video"></a>Docker 컨테이너 설명 비디오

> [!VIDEO https://www.youtube.com/embed/0oEsMwSxBsk]

## <a name="kubernetes-and-container-orchestration-explainer-video"></a>Kubernetes 및 컨테이너 오케스트레이션 설명 비디오

> [!VIDEO https://www.youtube.com/embed/3RTvoI-A7UQ]

## <a name="containers-on-windows"></a>Windows의 컨테이너

:::row:::
    :::column:::
       [![Windows 서버 컨테이너 아이콘](../../images/windows-server-containers.png)](/virtualization/windowscontainers)<br>
        **[Windows의 컨테이너 문서](/virtualization/windowscontainers)**<br>
        종속성이 있는 앱을 패키지하고, 운영 체제 수준 가상화를 빠르고 완전히 격리된 단일 시스템의 환경에 활용합니다. 빠른 시작, 배포 가이드 및 샘플을 포함하여 [Windows 컨테이너](/virtualization/windowscontainers/about)에 대해 알아봅니다.
    :::column-end:::
    :::column:::
       [![FAQ 아이콘](../../images/faq.png)](/virtualization/windowscontainers/about/faq)<br>
        **[Windows 컨테이너에 대한 FAQ](/virtualization/windowscontainers/about/faq)**<br>
        컨테이너에 대한 질문과 대답을 확인합니다. 또한 이 설명은 StackOverflow의 "[Windows용 Docker와 Windows의 Docker 간의 차이점은 무엇인가요?](https://stackoverflow.com/questions/38464724/whats-the-difference-between-docker-for-windows-and-docker-on-windows/40320748)"에서도 확인할 수 있습니다.
    :::column-end:::
    :::column:::
       [![Windows 컨테이너 아이콘](../../images/windows-container.png)](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)<br>
        **[환경 설정](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)**<br>
        필수 구성 요소, Docker 설치, [Windows 컨테이너 기본 이미지](/virtualization/windowscontainers/manage-containers/container-base-images) 작업을 포함하여 컨테이너를 만들고 실행하고 배포하도록 Windows 10 또는 Windows Server를 설정하는 방법을 알아봅니다.
    :::column-end:::
    :::column:::
       [![AKS 아이콘](../../images/kubernettes.png)](/azure/aks/windows-container-cli)<br>
        **[AKS(Azure Kubernetes Service)에서 Windows Server 컨테이너 만들기](/azure/aks/windows-container-cli)**<br>
        Azure CLI를 사용하여 Windows Server 컨테이너의 ASP.NET 샘플 앱을 AKS 클러스터에 배포하는 방법을 알아봅니다.
    :::column-end:::
:::row-end:::
