---
title: Node.js에서 Docker 컨테이너 사용 시작
description: Node.js 앱에서 Docker 컨테이너 사용을 시작하는 데 도움이 되는 단계별 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a1bd1b0f2916ccf44cc79d83f0335f55cf3863e4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166627"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Node.js에서 Docker 컨테이너 사용 시작

Node.js 앱에서 Docker 컨테이너 사용을 시작하는 데 도움이 되는 단계별 가이드입니다.

## <a name="prerequisites"></a>필수 구성 요소

이 가이드에서는 다음을 포함하여 [WSL 2를 사용하여 Node.js 개발 환경 설치](./setup-on-wsl2.md) 단계를 이미 완료했다고 가정합니다.

- Windows 10 Insider Preview 빌드 18932 이상 설치
- Windows에서 WSL 2 기능을 사용하도록 설정
- Linux 배포판(이 예제에서는 Ubuntu 18.04) 설치. `wsl lsb_release -a`를 사용하여 확인할 수 있습니다.
- Ubuntu 18.04 배포판이 WSL 2 모드로 실행되는지 확인. (WSL은 배포판을 v1 또는 v2 모드로 실행할 수 있습니다.) PowerShell을 열고 `wsl -l -v`를 입력하여 확인할 수 있습니다.
- PowerShell에서 `wsl -s ubuntu 18.04`를 사용하여 Ubuntu 18.04를 기본 배포로 설정

## <a name="overview-of-docker-containers"></a>Docker 컨테이너 개요

**Docker**는 컨테이너를 사용하여 애플리케이션을 만들고, 배포하고, 실행하는 데 사용되는 도구입니다. 개발자는 컨테이너를 사용하여 필요한 모든 파트(라이브러리, 프레임워크, 종속성 등)와 함께 앱을 패키징하여 하나의 패키지로 제공할 수 있습니다. 컨테이너를 사용하면 앱을 실행하는 컴퓨터(코드를 작성하고 테스트하는 데 사용된 머신과 다를 수도 있음)의 사용자 지정된 설정 또는 이전에 설치한 라이브러리와 상관없이 앱이 동일하게 실행됩니다. 따라서 개발자는 코드가 실행될 시스템에 대해 신경 쓰지 않고 코드 작성에 집중할 수 있습니다.

Docker 컨테이너는 가상 머신과 비슷하지만 전체 가상 운영 체제를 만들지는 않습니다. 대신 Docker를 사용하면 앱이 실행 중인 시스템과 동일한 Linux 커널을 사용할 수 있습니다. 따라서 호스트 컴퓨터에 아직 없는 파트만 앱 패키지에 필요하므로 패키지 크기를 줄이고 성능을 높일 수 있습니다.

컨테이너가 인기 있는 또 다른 이유는 [Kubernetes](/azure/aks/) 같은 도구와 함께 Docker 컨테이너를 사용하면 지속적인 가용성을 얻을 수 있다는 점입니다. 이렇게 하면 여러 버전의 앱 컨테이너를 서로 다른 시간에 만들 수 있습니다. 업데이트 또는 유지 관리를 위해 전체 시스템을 중단하는 대신, 각 컨테이너(및 해당 마이크로서비스)를 필요할 때 바꿀 수 있습니다. 모든 업데이트가 포함된 새 컨테이너를 준비하고, 프로덕션용 컨테이너를 설정하고, 준비가 되면 새 컨테이너를 가리킬 수 있습니다. 컨테이너를 사용하여 여러 버전의 앱을 보관하고, 필요한 경우 안전 대비책으로 앱을 계속 실행할 수도 있습니다.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Docker Desktop WSL 2 Tech Preview 설치

이전에는 WSL 1에서 Docker 디먼을 직접 실행할 수 없었지만, WSL 2에서는 이것이 변경되어 WSL 2용 Docker Desktop의 속도와 성능이 크게 향상되었습니다.

Docker Desktop WSL 2 Tech Preview를 설치하고 실행하는 방법은 다음과 같습니다.

1. [Docker Desktop WSL 2 Tech Preview 설치 관리자](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe)를 다운로드합니다. (필요한 경우 [설치 관리자 문서](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)를 참조할 수 있습니다.)

2. 방금 다운로드한 Docker 설치 관리자를 엽니다. 설치 마법사에서 "Linux 컨테이너 대신 Windows 컨테이너 사용" 여부를 묻는 메시지가 표시되면 확인란을 선택하지 말고 그냥 둡니다. 우리는 Linux 하위 시스템을 사용할 것입니다. Docker는 기본 WSL 2 배포의 관리형 디렉터리에 설치되며 Docker 디먼, CLI 및 Compose CLI를 포함할 것입니다.

    ![Docker Desktop 시작](../images/install-docker-1.png)

3. Docker ID가 아직 없는 경우 [https://hub.docker.com/signup](https://hub.docker.com/signup)을 방문하여 설정해야 합니다. ID는 모두 소문자 영숫자여야 합니다.

4. 설치가 완료되면 바탕 화면에서 바로 가기 아이콘을 선택하거나 Windows 시작 메뉴에서 찾아 Docker Desktop을 시작합니다. 작업 표시줄의 숨겨진 아이콘 메뉴에 Docker 아이콘이 표시됩니다. 이 아이콘을 마우스 오른쪽 단추로 클릭하여 Docker 명령 메뉴를 표시하고 "WSL 2 Tech Preview"를 선택합니다.

5. Tech Preview 창이 열리면 **시작**을 선택하여 WSL 2에서 Docker 디먼(백그라운드 프로세스)을 실행합니다. WSL 2 docker 디먼이 시작되면 이에 대한 docker CLI 컨텍스트가 자동으로 만들어집니다.

    ![Docker Desktop 시작](../images/start-docker.gif)

6. Docker가 설치되었는지 확인하고 버전 번호를 표시하려면 명령줄(WSL 또는 PowerShell)을 열고 `docker --version`을 입력합니다.

7. `docker run hello-world` 명령으로 간단한 기본 제공 Docker 이미지를 실행하여 올바르게 설치되었는지 테스트합니다.

다음은 알아 두어야 하는 몇 가지 Docker 명령입니다.

- Docker CLI에서 사용할 수 있는 명령 나열: `docker`
- 특정 명령에 대한 정보 나열: `docker <COMMAND> --help`
- 머신의 docker 이미지 나열(현재는 hello-world 이미지만 나열됨): `docker image ls --all`
- 머신의 컨테이너 나열: `docker container ls --all`
- WSL 2 컨텍스트에서 사용할 수 있는 Docker 시스템 통계 및 리소스(CPU 및 메모리) 나열: `docker info`
- Docker가 현재 실행되고 있는지 표시: `docker context ls`

Docker가 두 가지 컨텍스트 `default`(클래식 Docker 디먼) 및 `wsl`(Tech Preview를 사용하는 권장 컨텍스트)에서 실행되는 것을 볼 수 있습니다. (또한 `ls` 명령은 `list`의 축약형이므로 서로 바꿔서 사용할 수 있습니다.)

![Powershell의 Docker 표시 컨텍스트](../images/docker-context.png)

> [!TIP]
> [Docker Hub의 자습서](https://hub.docker.com/?overlay=onboarding)를 사용하여 예제 Docker 이미지를 만들어 보세요. Docker Hub에는 개발자들이 컨테이너화하려는 앱 형식과 일치할 수도 있는 수천 개의 오픈 소스 이미지가 포함되어 있습니다. [Gatsby.js 프레임워크 컨테이너](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) 또는 [Nuxt.js 프레임워크 컨테이너](https://hub.docker.com/r/hobord/nuxtexpress)와 같은 이미지를 다운로드하고, 개발자 고유의 애플리케이션 코드를 사용하여 확장할 수 있습니다. [명령줄의 Docker](https://docs.docker.com/engine/reference/commandline/search/) 또는 [Docker Hub 웹 사이트](https://hub.docker.com/search/?type=image)를 사용하여 레지스트리를 검색할 수 있습니다.

## <a name="install-the-docker-extension-on-vs-code"></a>VS Code에 Docker 확장 설치

Docker 확장을 사용하면 Visual Studio Code에서 컨테이너화된 애플리케이션을 쉽게 빌드, 관리 및 배포할 수 있습니다.

1. VS Code에서 **확장** 창을 열고(Ctrl+Shift+X) **Docker**를 검색합니다.

2. [Microsoft Docker 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)을 선택하고 **설치**를 선택합니다. 확장을 사용하려면 설치 후 VS Code를 다시 로드해야 합니다.

    ![VS Code에 설치된 원격 WSL의 Docker 확장](../images/docker-vscode-extension.png)

VS Code에 Docker 확장을 설치했으므로, 이제 바로 가기(`Ctrl+Space`)를 사용하여 다음 섹션에서 사용할 `Dockerfile` 명령 목록을 표시할 수 있습니다.

[VS Code에서 Docker를 작업하는 방법](https://code.visualstudio.com/docs/azure/docker)을 자세히 알아보세요.

## <a name="create-a-container-image-with-dockerfile"></a>DockerFile을 사용하여 컨테이너 이미지 만들기

**컨테이너 이미지**는 애플리케이션 코드, 라이브러리, 구성 파일, 환경 변수 및 런타임을 저장합니다. 이미지를 사용하면 컨테이너의 환경이 표준화되며, 애플리케이션을 빌드하고 실행하는 데 필요한 항목만 포함됩니다.

**DockerFile**에는 새 컨테이너 이미지를 빌드하는 데 필요한 명령만 포함됩니다. 다시 말해서, 이 파일은 어디서나 재현할 수 있도록 앱 환경을 정의하는 컨테이너 이미지를 빌드합니다.

[웹 프레임워크](./web-frameworks.md) 가이드에서 설정한 Next.js 앱을 사용하여 컨테이너 이미지를 빌드하겠습니다.

1. VS Code에서 Next.js 앱을 엽니다(원격 WSL 확장이 왼쪽 아래 녹색 탭에 표시된 대로 실행되는지 확인). VS Code에 통합된 WSL 터미널을 열고(**보기 > 터미널**), 터미널 경로가 Next.js 프로젝트 디렉터리(예: `~/NextProjects/my-next-app$`)를 가리키는지 확인합니다.

2. Next.js 프로젝트의 루트에 `Dockerfile`이라는 새 파일을 만들고 다음을 추가합니다.

    ```docker
    # Specifies where to get the base image (Node v12 in our case) and creates a new container for it
    FROM node:12
    
    # Set working directory. Paths will be relative this WORKDIR.
    WORKDIR /usr/src/app
    
    # Install dependencies
    COPY package*.json ./
    RUN npm install
    
    # Copy source files from host computer to the container
    COPY . .
    
    # Build the app
    RUN npm run build
    
    # Specify port app runs on
    EXPOSE 3000

    # Run the app
    CMD [ "npm", "start" ]
    ```

3. docker 이미지를 빌드하려면 프로젝트의 루트에서 `docker build -t <your_docker_username>/my-nextjs-app .` 명령을 실행합니다(`<your_docker_username>`을 Docker Hub에서 만든 사용자 이름으로 대체).

> [!NOTE]
> 이 명령이 작동하려면 Docker가 WSL Tech Preview와 함께 실행 중이어야 합니다. Docker를 시작하는 방법은 설치 섹션의 [4단계](#install-docker-desktop-wsl-2-tech-preview)를 참조하세요. `-t` 플래그는 생성할 이미지의 이름을 지정합니다. 여기서는 "my-nextjs-app:v1"입니다. 이미지를 만들 때 항상 태그 이름에 [버전 번호를 사용](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375)하는 것이 좋습니다. 명령의 끝에 마침표를 넣어야 합니다. 마침표는 현재 작업 디렉터리를 사용하여 Next.js 앱에 대한 빌드 파일을 찾아서 복사하도록 지정합니다.

4. 컨테이너에서 Next.js 앱의 새 docker 이미지를 실행하려면 `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1` 명령을 입력합니다.

5. `-p` 플래그는 포트 '3000'(앱이 컨테이너 내부에서 실행되는 포트)을 머신의 로컬 포트 '3333'에 바인딩합니다. 따라서 이제 웹 브라우저에서 [http://localhost:3333](http://localhost:3333)을 가리키고 서버 쪽의 렌더링된 Next.js 애플리케이션이 Docker 컨테이너 이미지로 실행되는 것을 볼 수 있습니다.

> [!TIP]
> `FROM node:12`를 사용하여 Docker Hub에 저장된 Node.js 버전 12 기본 이미지를 참조하는 컨테이너 이미지를 빌드했습니다. 이 기본 Node.js 이미지는 Debian/Ubuntu Linux 시스템을 기반으로 하며 여러 Node.js 이미지 중에서 선택할 수 있습니다. 하지만 좀 더 가볍거나 요구 사항에 맞게 조정된 이미지를 사용할 수도 있습니다. [Docker Hub의 Node.js 이미지 레지스트리](https://hub.docker.com/_/node/)에서 자세히 알아보세요.

## <a name="upload-your-container-image-to-a-repository"></a>리포지토리에 컨테이너 이미지 업로드

**컨테이너 리포지토리**는 컨테이너 이미지를 클라우드에 저장합니다. 종종 컨테이너 리포지토리에는 여러 버전처럼 간편하게 설정하고 신속하게 배포하는 데 사용할 수 있는 관련 이미지 컬렉션이 포함되기도 합니다. 일반적으로 보안 HTTP 엔드포인트를 통해 컨테이너 리포지토리의 이미지에 액세스할 수 있으며 아무 시스템, 하드웨어 또는 VM 인스턴스를 통해 이미지를 끌어오거나 푸시하거나 관리할 수 있습니다.

반면 **컨테이너 레지스트리**는 인덱스, 액세스 제어 규칙 및 API 경로뿐 아니라 리포지토리 컬렉션을 저장합니다. 이러한 항목을 공개적으로 또는 비공개적으로 호스팅할 수 있습니다. [Docker Hub](https://hub.docker.com/)는 오픈 소스 Docker 레지스트리이며 `docker push` 및 `docker pull` 명령을 실행할 때 사용되는 기본값입니다. 퍼블릭 리포지토리에는 무료로 사용할 수 있고 프라이빗 리포지토리에 사용하려면 요금을 지불해야 합니다.

Docker Hub에 호스팅되는 리포지토리에 새 컨테이너 이미지를 업로드하려면 다음을 수행합니다.

1. Docker Hub에 로그인합니다. 설치 단계에서 Docker Hub 계정을 만드는 데 사용한 사용자 이름 및 암호를 입력하라는 메시지가 표시됩니다. 터미널에서 Docker에 로그인하려면 `docker login`을 입력합니다.

2. 머신에서 만든 docker 컨테이너 이미지 목록을 가져오려면 `docker image ls --all`을 입력합니다.

3. `docker push <your_docker_username>/my-nextjs-app:v1` 명령을 사용하여 컨테이너 이미지를 Docker Hub에 푸시하고, 해당 이미지에 대한 새 리포지토리를 Docker Hub에 만듭니다.

4. 이제 https://cloud.docker.com/repository/list 를 방문하여 Docker Hub의 리포지토리를 살펴보고, 설명을 입력하고, GitHub 계정을 연결할 수 있습니다(원하는 경우).

5. `docker container ls`(또는 `docker ps`)를 사용하여 활성 Docker 컨테이너 목록을 볼 수도 있습니다.

6. "my-nextjs-app:v1" 컨테이너가 포트 3333 ->3000/tcp에서 활성화된 것을 볼 수 있습니다. 또한 여기에 "컨테이너 ID"가 나열됩니다. 컨테이너 실행을 중지하려면 `docker stop <container ID>` 명령을 입력합니다.

7. 일반적으로 컨테이너를 중지하면 해당 컨테이너를 제거해야 합니다. 컨테이너를 제거하면 남아 있는 리소스가 정리됩니다. 컨테이너를 제거하면 해당 컨테이너의 이미지 파일 시스템 내에서 변경한 내용이 영구적으로 손실됩니다. 변경 내용을 나타내려면 새 이미지를 작성해야 합니다. 컨테이너를 제거하려면 `docker rm <container ID>` 명령을 사용합니다.

[Docker를 사용하여 컨테이너화된 웹 애플리케이션 빌드](/learn/modules/intro-to-containers/)에 대해 자세히 알아보세요.

## <a name="deploy-to-azure-container-registry"></a>Azure Container Registry에 배포

[**ACR(Azure Container Registry**](https://azure.microsoft.com/services/container-registry/))을 사용하면 컨테이너 이미지를 인증된 프라이빗 리포지토리에 안전하게 저장, 관리 및 유지할 수 있습니다. 표준 Docker 명령과 호환되는 ACR은 컨테이너 상태 모니터링이나 유지 관리 같은 중요한 작업을 자동으로 처리하고, [Kubernetes](/azure/aks/intro-kubernetes)와 페어링하여 확장 가능한 오케스트레이션 시스템을 만들 수 있습니다. 주문형으로 빌드할 수도 있고 소스 코드 커밋, 기본 이미지 업데이트 등의 트리거를 사용하여 빌드를 완전히 자동화할 수도 있습니다. 또한 ACR은 방대한 Azure 클라우드 네트워크를 활용하여 네트워크 대기 시간 및 글로벌 배포를 관리하고, [Azure App Service](/azure/app-service/)(웹 호스팅, 모바일 백 엔드, REST API의 경우) 또는 [다른 Azure 클라우드 서비스](https://azure.microsoft.com/product-categories/containers/)를 사용하는 모든 사용자를 위해 원활한 기본 환경을 만듭니다.

> [!IMPORTANT]
> Azure에 자체 컨테이너를 배포하려면 고유한 Azure 구독이 필요하며, 요금이 발생할 수 있습니다. Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정을 만드세요](https://azure.microsoft.com/free/).

Azure Container Registry를 만들고 앱 컨테이너 이미지를 배포하는 방법에 대한 도움말은 연습: [Azure Container Instance에 Docker 이미지 배포](/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure의 Node.js](https://azure.microsoft.com/develop/nodejs/)
- 빠른 시작: [Azure에서 Node.js 웹앱 만들기](/azure/app-service/app-service-web-get-started-nodejs)
- 온라인 과정: [Azure의 관리자 컨테이너](/learn/paths/administer-containers-in-azure/)
- VS Code 사용: [Docker 작업](https://code.visualstudio.com/docs/azure/docker)
- Docker 문서: [Docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)