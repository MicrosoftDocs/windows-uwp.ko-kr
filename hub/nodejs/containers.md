---
title: Node.js에서 Docker 컨테이너를 사용 하 여 시작
description: Node.js 앱에서 Docker 컨테이너를 사용 하 여 시작 하는 데 도움이 되는 단계별 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 16b1421606d3c8271141256b80ae2600ec9ca49d
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315127"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Node.js에서 Docker 컨테이너를 사용 하 여 시작

Node.js 앱에서 Docker 컨테이너를 사용 하 여 시작 하는 데 도움이 되는 단계별 가이드입니다.

## <a name="prerequisites"></a>사전 요구 사항

이 가이드에서는 다음을 포함 하 여 [WSL 2를 사용 하 여 node.js 개발 환경을 설정](./setup-on-wsl2.md)하는 단계를 이미 완료 했다고 가정 합니다.

- Windows 10 Insider Preview 빌드 18932 이상 버전을 설치 합니다.
- Windows에서 WSL 2 기능을 사용 하도록 설정 합니다.
- Linux 배포 (Ubuntu 18.04)를 설치 합니다. @No__t-0을 사용 하 여이를 확인할 수 있습니다.
- Ubuntu 18.04 배포가 WSL 2 모드로 실행 되 고 있는지 확인 합니다. WSL은 v1 또는 v2 모드에서 배포를 실행할 수 있습니다. PowerShell을 열고 `wsl -l -v`을 입력 하 여이를 확인할 수 있습니다.
- PowerShell을 사용 하 여 `wsl -s ubuntu 18.04`을 사용 하 여 Ubuntu 18.04을 기본 배포로 설정 합니다.

## <a name="overview-of-docker-containers"></a>Docker 컨테이너 개요

**Docker** 는 컨테이너를 사용 하 여 응용 프로그램을 만들고 배포 하 고 실행 하는 데 사용 되는 도구입니다. 개발자는 컨테이너를 사용 하 여 필요한 모든 파트 (라이브러리, 프레임 워크, 종속성 등)로 앱을 패키지 한 다음 하나의 패키지로 제공할 수 있습니다. 컨테이너를 사용 하면 응용 프로그램 코드를 작성 하 고 테스트 하는 데 사용 된 컴퓨터와 다를 수 있는 사용자 지정 된 설정 또는 이전에 설치한 컴퓨터의 라이브러리와 상관 없이 앱이 동일 하 게 실행 됩니다. 이를 통해 개발자는 코드가 실행 되는 시스템에 대해 걱정 하지 않고 코드 작성에 집중할 수 있습니다.

Docker 컨테이너는 가상 머신과 비슷하지만 전체 가상 운영 체제를 만들지는 않습니다. 대신 Docker를 사용 하면 앱이 실행 중인 시스템과 동일한 Linux 커널을 사용할 수 있습니다. 이를 통해 응용 프로그램 패키지는 호스트 컴퓨터에 아직 없는 부품만 필요로 하 여 패키지 크기를 줄이고 성능을 향상 시킬 수 있습니다.

[Kubernetes](https://docs.microsoft.com/azure/aks/)같은 도구와 함께 Docker 컨테이너를 사용 하는 지속적인 가용성은 컨테이너의 인기를 위한 또 다른 이유입니다. 이렇게 하면 여러 버전의 앱 컨테이너를 서로 다른 시간에 만들 수 있습니다. 업데이트 또는 유지 관리를 위해 전체 시스템을 중단 하는 대신 각 컨테이너 (및 특정 마이크로 서비스)를 즉석에서 바꿀 수 있습니다. 모든 업데이트를 사용 하 여 새 컨테이너를 준비 하 고, 프로덕션을 위해 컨테이너를 설정 하 고, 준비가 되 면 새 컨테이너를 가리킬 수 있습니다. 컨테이너를 사용 하 여 여러 버전의 앱을 보관 하 고 필요한 경우 안전 대체로 계속 실행할 수도 있습니다.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Docker Desktop WSL 2 Tech Preview 설치

이전에는 WSL 1이 Docker 디먼을 직접 실행할 수 없지만 WSL 2를 사용 하 여 변경 되었으며 WSL 2 용 Docker 데스크톱의 속도와 성능이 크게 향상 되었습니다.

Docker Desktop WSL 2 Tech Preview를 설치 하 고 실행 하려면:

1. [Docker Desktop WSL 2 Tech Preview 설치 관리자](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe)를 다운로드 합니다. 필요한 경우 [설치 관리자 문서](https://docs.docker.com/docker-for-windows/wsl-tech-preview/) 를 참조할 수 있습니다.

2. 방금 다운로드 한 Docker 설치 관리자를 엽니다. 설치 마법사에서 "Linux 컨테이너 대신 Windows 컨테이너 사용"을 선택 하 라는 메시지가 표시 되 면 Linux 하위 시스템을 사용 하 게 되므로이 확인란을 선택 취소 합니다. Docker는 기본 WSL 2 배포의 관리 되는 디렉터리에 설치 되며 Docker 데몬, CLI 및 작성 CLI를 포함 합니다.

    ![Docker 데스크톱 시작](../images/install-docker-1.png)

3. Docker ID가 아직 없는 경우에는 [https://hub.docker.com/signup](https://hub.docker.com/signup)을 방문 하 여 설정 해야 합니다. ID는 모두 소문자 영숫자 여야 합니다.

4. 설치 된 후 바탕 화면에서 바로 가기 아이콘을 선택 하거나 Windows 시작 메뉴에서 검색 하 여 Docker Desktop을 시작 합니다. 작업 표시줄의 숨겨진 아이콘 메뉴에 Docker 아이콘이 표시 됩니다. 아이콘을 마우스 오른쪽 단추로 클릭 하 여 Docker 명령 메뉴를 표시 하 고 "WSL 2 Tech Preview"를 선택 합니다.

5. Tech preview 창이 열리면 **시작** 을 선택 하 여 wsl 2에서 Docker 디먼 (백그라운드 프로세스) 실행을 시작 합니다. WSL 2 docker 디먼이 시작 되 면이에 대 한 docker CLI 컨텍스트가 자동으로 만들어집니다.

    ![Docker 데스크톱 시작](../images/start-docker.gif)

6. Docker가 설치 되어 있는지 확인 하 고 버전 번호를 표시 하려면 명령줄 (WSL 또는 PowerShell)을 열고 다음을 입력 합니다. `docker --version`

7. 간단한 기본 제공 Docker 이미지를 실행 하 여 설치가 제대로 작동 하는지 테스트 합니다. `docker run hello-world`

다음은 알아야 할 몇 가지 Docker 명령입니다.

- 다음을 입력 하 여 Docker CLI에서 사용할 수 있는 명령을 나열 `docker`
- 다음을 사용 하 여 특정 명령에 대 한 정보 나열 `docker <COMMAND> --help`
- 다음을 사용 하 여 컴퓨터의 docker 이미지를 나열 합니다 (이 시점에서 hello-세계 이미지). `docker image ls --all`
- 다음을 사용 하 여 컴퓨터의 컨테이너 나열: `docker container ls --all`
- 다음을 사용 하 여 WSL 2 컨텍스트에서 사용할 수 있는 Docker 시스템 통계 및 리소스 (CPU & 메모리)를 나열 합니다. `docker info`
- Docker가 현재 실행 되 고 있는 위치를 표시 합니다. `docker context ls`

Docker를 실행 하는 두 개의 컨텍스트가--`default` (클래식 Docker 디먼) 및 `wsl` (tech preview를 사용 하는 권장 사항)으로 표시 될 수 있습니다. @No__t-0 명령은 `list`에 대해 짧고 서로 바꿔 사용할 수 있습니다.

![Powershell의 Docker 표시 컨텍스트](../images/docker-context.png)

> [!TIP]
> [Docker 허브에서이 자습서](https://hub.docker.com/?overlay=onboarding)를 사용 하 여 예제 docker 이미지를 작성해 보세요. 또한 Docker 허브에는 컨테이너 화 앱의 유형과 일치 하는 수천 개의 오픈 소스 이미지가 포함 되어 있습니다. 이 [Gatsby framework 컨테이너](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) 또는이 [Nuxt 프레임 워크 컨테이너](https://hub.docker.com/r/hobord/nuxtexpress)와 같은 이미지를 다운로드 하 고 사용자 고유의 응용 프로그램 코드로 확장할 수 있습니다. 명령줄 또는 [Docker 허브 웹 사이트](https://hub.docker.com/search/?type=image) [에서 docker를](https://docs.docker.com/engine/reference/commandline/search/) 사용 하 여 레지스트리를 검색할 수 있습니다.

## <a name="install-the-docker-extension-on-vs-code"></a>VS Code에서 Docker 확장을 설치 합니다.

Docker 확장을 사용 하면 Visual Studio Code에서 컨테이너 화 된 응용 프로그램을 쉽게 빌드, 관리 및 배포할 수 있습니다.

1. VS Code에서 **확장** 창 (Ctrl + Shift + X)을 열고 **Docker**를 검색 합니다.

2. [Microsoft Docker 확장](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) 을 선택 하 고 **설치**합니다. 확장을 사용 하도록 설정 하려면를 설치한 후 VS Code를 다시 로드 해야 합니다.

    ![원격-WSL의 VS Code에 대 한 Docker 확장](../images/docker-vscode-extension.png)

VS Code에서 Docker 확장을 설치 하면 다음 섹션에서 사용 되는 `Dockerfile` 명령 목록을 가져올 수 있습니다. `Ctrl+Space`

[VS Code에서 Docker로 작업 하는](https://code.visualstudio.com/docs/azure/docker)방법에 대해 자세히 알아보세요.

## <a name="create-a-container-image-with-dockerfile"></a>DockerFile을 사용 하 여 컨테이너 이미지 만들기

**컨테이너 이미지** 는 응용 프로그램 코드, 라이브러리, 구성 파일, 환경 변수 및 런타임을 저장 합니다. 이미지를 사용 하면 컨테이너의 환경이 표준화 되 고 응용 프로그램을 빌드하고 실행 하는 데 필요한 항목만 포함 됩니다.

**Dockerfile** 에는 새 컨테이너 이미지를 빌드하는 데 필요한 지침이 포함 되어 있습니다. 즉,이 파일은 앱 환경을 정의 하는 컨테이너 이미지를 빌드하여 어디에서 나 재현할 수 있습니다.

[웹 프레임 워크](./web-frameworks.md) 가이드에 설정 된 다음 .js 앱을 사용 하 여 컨테이너 이미지를 빌드 해 보겠습니다.

1. VS Code에서 다음 .js 앱을 엽니다 (원격 WSL 확장이 왼쪽 아래 녹색 탭에 표시 된 대로 실행 되 고 있는지 확인). VS Code ( **> 터미널 보기**)에 통합 된 wsl 터미널을 열고 터미널 경로가 다음 .js 프로젝트 디렉터리 (`~/NextProjects/my-next-app$`)를 가리키는지 확인 합니다.

2. 다음 .js 프로젝트의 루트에 `Dockerfile` 이라는 새 파일을 만들고 다음을 추가 합니다.

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

3. Docker 이미지를 빌드하려면 프로젝트의 루트에서 다음 명령을 실행 합니다 (`<your_docker_username>`을 Docker 허브에서 만든 사용자 이름으로 대체). `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> 이 명령이 작동 하려면 Docker가 WSL Tech Preview와 함께 실행 중 이어야 합니다. Docker를 시작 하는 방법에 대 한 미리 알림은 설치 섹션의 [#4 단계](#install-docker-desktop-wsl-2-tech-preview) 를 참조 하세요. @No__t-0 플래그는 생성 될 이미지의 이름을 지정 합니다 .이 경우 "내 nextjs-앱: v1"입니다. 이미지를 만들 때 [태그 이름에 항상 버전 #을 사용](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) 하는 것이 좋습니다. 명령의 끝에 마침표를 포함 해야 합니다 .이는 현재 작업 디렉터리를 사용 하 여 다음 .js 앱에 대 한 빌드 파일을 찾고 복사 하도록 지정 합니다.

4. 컨테이너에서 다음 .js 앱의 새 docker 이미지를 실행 하려면 다음 명령을 입력 합니다. `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. @No__t-0 플래그는 포트 ' 3000 ' (앱이 컨테이너 내에서 실행 되는 포트)을 (를) 컴퓨터의 로컬 포트 ' 3333 '에 바인딩합니다. 이제 웹 브라우저를 [http://localhost:3333](http://localhost:3333) 로 지정 하 여 Docker로 실행 되는 다음 js 응용 프로그램을 볼 수 있습니다. 컨테이너 이미지입니다.

> [!TIP]
> Docker Hub에 저장 된 node.js 버전 12 기본 이미지를 참조 하는 `FROM node:12`을 사용 하 여 컨테이너 이미지를 빌드 했습니다. 이 기본 node.js 이미지는 Debian/Ubuntu Linux 시스템을 기반으로 하며 다양 한 node.js 이미지를 선택할 수 있으며, 더 경량 또는 요구에 맞게 조정 된 항목을 사용 하는 것이 좋습니다. [Docker 허브의 Node.js 이미지 레지스트리에서](https://hub.docker.com/_/node/)자세히 알아보세요.

## <a name="upload-your-container-image-to-a-repository"></a>컨테이너 이미지를 리포지토리에 업로드

**컨테이너 리포지토리** 는 컨테이너 이미지를 클라우드에 저장 합니다. 일반적으로 컨테이너 리포지토리에는 쉽게 설정 하 고 신속 하 게 배포 하는 데 사용할 수 있는 서로 다른 버전 등의 관련 이미지 컬렉션이 실제로 포함 되어 있습니다. 일반적으로 보안 HTTPs 끝점을 통해 컨테이너 리포지토리의 이미지에 액세스할 수 있으므로 시스템, 하드웨어 또는 VM 인스턴스를 통해 이미지를 끌어오거나 푸시 하거나 관리할 수 있습니다.

반면 **컨테이너 레지스트리**는 인덱스, 액세스 제어 규칙 및 API 경로 뿐만 아니라 리포지토리 컬렉션을 저장 합니다. 공개적으로 또는 개인적으로 호스팅될 수 있습니다. [Docker 허브](https://hub.docker.com/) 는 `docker push` 및 `docker pull` 명령을 실행할 때 사용 되는 오픈 소스 Docker 레지스트리 및 기본값입니다. 공개 리포지토리 무료 이며 개인 리포지토리 요금을 지불 해야 합니다.

Docker 허브에서 호스트 되는 리포지토리에 새 컨테이너 이미지를 업로드 하려면 다음을 수행 합니다.

1. Docker 허브에 로그인 합니다. 설치 단계에서 Docker 허브 계정을 만드는 데 사용한 사용자 이름 및 암호를 입력 하 라는 메시지가 표시 됩니다. 터미널에서 Docker에 로그인 하려면 다음을 입력 합니다. `docker login`

2. 컴퓨터에서 만든 docker 컨테이너 이미지 목록을 가져오려면 `docker image ls --all`을 입력 합니다.

3. 다음 명령을 사용 하 여 컨테이너 이미지를 Docker 허브에 푸시하고 여기에 대 한 새 리포지토리를 만듭니다. `docker push <your_docker_username>/my-nextjs-app:v1`

4. 이제 다음을 방문 하 여 Docker 허브에서 리포지토리를 보고, 설명을 입력 하 고, GitHub 계정을 연결할 수 있습니다 (원하는 경우). https://cloud.docker.com/repository/list

5. @No__t-0 (또는 `docker ps`)을 사용 하 여 활성 Docker 컨테이너의 목록을 볼 수도 있습니다.

6. 포트 3333-> 3000/tcp에서 "내 nextjs-앱: v1" 컨테이너가 활성화 된 것을 확인할 수 있습니다. 여기에 나열 된 "컨테이너 ID"도 볼 수 있습니다. 컨테이너 실행을 중지 하려면 다음 명령을 입력 합니다. `docker stop <container ID>`

7. 일반적으로 컨테이너를 중지 한 후에도 제거 해야 합니다. 컨테이너를 제거 하면 남아 있는 모든 리소스를 정리 합니다. 컨테이너를 제거 하면 해당 이미지 파일 시스템 내에서 변경한 내용이 영구적으로 손실 됩니다. 변경 내용을 나타내려면 새 이미지를 작성 해야 합니다. 컨테이너를 제거 하려면 다음 명령을 사용 합니다. `docker rm <container ID>`

[Docker를 사용 하 여 컨테이너 화 된 웹 응용 프로그램을 빌드하는](https://docs.microsoft.com/learn/modules/intro-to-containers/)방법에 대해 자세히 알아보세요.

## <a name="deploy-to-azure-container-registry"></a>Azure Container Registry에 배포

ACR ( [**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) )를 사용 하면 컨테이너 이미지를 개인, 인증 된 리포지토리에서 안전 하 게 저장, 관리 및 유지할 수 있습니다. 표준 Docker 명령과 호환 되는 ACR는 컨테이너 상태 모니터링 및 유지 관리와 같은 중요 한 작업을 처리 하 고, [Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes) 와 페어링 하 여 확장 가능한 오케스트레이션 시스템을 만들 수 있습니다. 주문형으로 빌드하거나 소스 코드 커밋 및 기본 이미지 업데이트와 같은 트리거로 빌드를 완전히 자동화 합니다. 또한 ACR은 방대한 Azure 클라우드 네트워크를 활용 하 여 네트워크 대기 시간, 글로벌 배포를 관리 하 고 [Azure App Service](https://docs.microsoft.com/azure/app-service/) (웹 호스팅, 모바일 백 엔드, REST api) 또는 [기타 Azure cloud services를 사용 하는 모든 사용자에 게 원활한 기본 환경을 만듭니다. ](https://azure.microsoft.com/product-categories/containers/).

> [!IMPORTANT]
> Azure에 컨테이너를 배포 하기 위해 고유한 Azure 구독이 필요 하며, 요금을 받을 수 있습니다. Azure 구독이 아직 없는 경우 시작 하기 전에 [무료 계정을 만듭니다](https://azure.microsoft.com/free/) .

Azure Container Registry를 만들고 앱 컨테이너 이미지를 배포 하는 방법에 대 한 도움말은 다음 연습을 참조 하세요. [Docker 이미지를 Azure Container Instance에 배포](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance)합니다.

## <a name="additional-resources"></a>추가 자료

- [Azure의 node.js](https://azure.microsoft.com/en-us/develop/nodejs/)
- 빠른 시작: [Azure에서 node.js 웹 앱 만들기](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- 온라인 과정: [Azure에서 컨테이너 관리](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- VS Code 사용: [Docker 작업](https://code.visualstudio.com/docs/azure/docker)
- Docker Docs: [Docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
