---
title: 시작 자습서 - JavaScript로 작성된 3D UWP 게임
description: JavaScript 및 three.js로 작성된 Microsoft Store용 UWP 게임
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fb4249b2-f93c-4993-9e4d-57a62c04be66
ms.localizationpriority: medium
ms.openlocfilehash: b4ce91e32b14bdf81b40b24e810e0bd86bcaa99b
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321085"
---
# <a name="creating-a-3d-javascript-game-using-threejs"></a>three.js를 사용하여 3D JavaScript 게임 만들기

## <a name="introduction"></a>소개

웹 개발자 또는 JavaScript에 서투른 분들의 경우 JavaScript를 사용하여 UWP 앱을 개발하면 손쉽게 전 세계의 사용자에게 앱을 제공할 수 있습니다. C# 또는 C++ 같은 언어를 배우지 않아도 됩니다!

이 샘플에서는 **three.js** 라이브러리를 활용하겠습니다. 이 라이브러리는 웹 브라우저의 2D 및 3D 그래픽을 렌더링하는 데 사용되는 API인 WebGL을 기반으로 합니다. **three.js**는 이 복잡한 API를 간소화하여 3D 개발을 훨씬 간편하게 만들어 줍니다. 


다음 과정으로 넘어가기 전에 현재 만들고 있는 앱을 먼저 살펴보고 싶은 분들은 CodePen에서 확인해보세요.

<iframe height='300' scrolling='no' title='Dino game final' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpKejy/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)의 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpKejy/'>Dino game final</a>을 참조하세요.
</iframe>

> [!NOTE] 
> 이는 완전한 게임이 아니며, JavaScript 및 타사 라이브러리를 사용하여 Microsoft Store에 바로 게시할 수 있는 앱을 만드는 방법을 설명하기 위해 설계되었습니다.


## <a name="requirements"></a>요구 사항

이 프로젝트를 사용하여 플레이하려면 다음이 필요합니다.
-   Windows 10 최신 버전을 실행하는 Windows 컴퓨터(또는 가상 머신)
-   Visual Studio 복사본. 체험판 Visual Studio Community Edition은 [Visual Studio 홈 페이지](https://visualstudio.com/)에서 다운로드할 수 있습니다.
이 프로젝트에서는 **three.js** JavaScript 라이브러리를 사용합니다. **three.js**는 MIT 라이선스를 기반으로 릴리스됩니다. 이 라이브러리는 이미 프로젝트에 있습니다(솔루션 탐색기 보기에서 `js/libs` 검색). 이 라이브러리에 대한 자세한 내용은 [**three.js**](https://threejs.org/) 홈 페이지에서 확인할 수 있습니다.

## <a name="getting-started"></a>시작

앱의 전체 소스 코드는 [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js3d)에 저장되어 있습니다.

시작하는 가장 간단한 방법은 GitHub를 방문하여 녹색의 복제 또는 다운로드 단추를 클릭하고 Visual Studio에서 열기를 선택하는 것입니다. 

![복제 또는 다운로드 단추](images/3dclone.png)

프로젝트를 복제하는 대신 zip 파일로 다운로드할 수도 있습니다.
솔루션이 Visual Studio에 로드되면 다음이 포함된 여러 파일이 표시됩니다.
-   Images/ - UWP 앱에 필요한 다양한 아이콘이 들어 있는 폴더.
- css/ - 사용할 CSS가 포함된 폴더.
-   js/ - JavaScript 파일이 포함된 폴더. main.js 파일은 게임이고, 다른 파일은 타사 라이브러리입니다.
-   models/ - 3D 모델이 포함된 폴더. 이 게임에는 공룡 모델 하나만 있습니다.
-   index.html - 게임의 렌더러를 호스트하는 웹 페이지.

이제 게임을 실행할 수 있습니다!

F5 키를 눌러 앱을 시작합니다. 창이 열리고 화면을 클릭하라는 메시지가 표시됩니다. 배경을 돌아다니는 공룡도 보일 것입니다. 게임을 닫고 앱과 핵심 구성 요소 검사를 시작하겠습니다.

> [!NOTE] 
> 문제가 있나요? 웹 기능이 지원되는 Visual Studio가 설치되어 있어야 합니다. 새 프로젝트를 만들어서 확인할 수 있습니다. JavaScript가 지원되지 않는 경우 Visual Studio를 다시 설치하고 Microsoft Web 개발자 도구 확인란을 선택해야 합니다.

## <a name="walkthrough"></a>연습

이 게임을 시작하면 화면을 클릭하라는 메시지가 표시됩니다. [포인터 잠금 API](https://developer.mozilla.org/docs/Web/API/Pointer_Lock_API)는 마우스로 주변을 둘러보는 데 사용됩니다. W, A, S, D/화살표 키를 눌러 이동할 수 있습니다.
이 게임의 목표는 공룡과 거리를 유지하는 것입니다. 공룡이 일정 거리 이내로 접근하면 플레이어가 범위를 벗어날 때까지 또는 공룡과의 거리가 너무 가까워져서 게임이 끝날 때까지 공룡이 플레이어를 추적합니다.

### <a name="1-setting-up-your-initial-html-file"></a>1. 초기 HTML 파일 설정

시작하려면 **index.html** 내에서 약간의 HTML을 추가해야 합니다. 이 파일은 앱이 포함된 기본 웹 페이지입니다.

이제 사용할 라이브러리와 그래픽 렌더링에 사용할 `div`(이름은 `container`)를 통해 파일을 설정하겠습니다. 또한 **main.js**(게임 코드)를 가리키도록 파일을 설정하겠습니다.


```html
<!DOCTYPE html>
<html lang='en'>

<head>
    <link rel="stylesheet" type="text/css" href="css/stylesheet.css" />
</head>

    <body>
        <div id='container'></div>
        <script src='js/libs/three.js'></script>
        <script src="js/controls/PointerLockControls.js"></script>
        <script src="js/main.js"></script>
    </body>

</html>
```


스타터 HTML이 준비되었으니, **main.js**로 넘어가서 그래픽을 만들겠습니다.

### <a name="2-creating-your-scene"></a>2. 장면 만들기

연습 섹션에서는 게임의 기반을 추가하겠습니다.

먼저 구체적인 `scene`을 만들겠습니다. **three.js**의 `scene`은 카메라, 개체 및 조명이 추가되는 장소입니다. 카메라에 잡히는 장면을 포착하여 표시하는 렌더러도 필요합니다.

**main.js**에서는 이 모든 작업을 수행하는 `init()` 함수를 만들겠습니다.이 함수에는 약간의 추가 기능이 필요합니다.

```javascript
var UNITWIDTH = 90; // Width of a cubes in the maze
var UNITHEIGHT = 45; // Height of the cubes in the maze

var camera, scene, renderer;

init();
animate();

function init() {
    // Create the scene where everything will go
    scene = new THREE.Scene();

    // Add some fog for effects
    scene.fog = new THREE.FogExp2(0xcccccc, 0.0015);

    // Set render settings
    renderer = new THREE.WebGLRenderer();
    renderer.setClearColor(scene.fog.color);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);

    // Get the HTML container and connect renderer to it
    var container = document.getElementById('container');
    container.appendChild(renderer.domElement);

    // Set camera position and view details
    camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 2000);
    camera.position.y = 20; // Height the camera will be looking from
    camera.position.x = 0;
    camera.position.z = 0;

    // Add the camera
    scene.add(camera);

    // Add the walls(cubes) of the maze
    createMazeCubes();

    // Add lights to the scene
    addLights();

    // Listen for if the window changes sizes and adjust
    window.addEventListener('resize', onWindowResize, false);
}

```

그 외에도 다음과 같은 함수를 만들어야 합니다.
- `createMazeCubes()`
- `addLights()`
- `onWindowResize()`
- `animate()` / `render()`
- 단위 변환 함수

#### <a name="createmazecubes"></a>createMazeCubes()

`createMazeCubes()` 함수는 장면에 간단한 큐브를 추가합니다. 이 자습서의 후반부에서는 여러 큐브를 추가하는 함수를 작성하여 미로를 만들 것입니다.

```javascript
function createMazeCubes() {

  // Make the shape of the cube that is UNITWIDTH wide/deep, and UNITHEIGHT tall
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  // Make the material of the cube and set it to blue
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });
  
  // Combine the geometry and material to make the cube
  var cube = new THREE.Mesh(cubeGeo, cubeMat);

  // Add the cube to the scene
  scene.add(cube);

  // Update the cube's position
  cube.position.y = UNITHEIGHT / 2;
  cube.position.x = 0;
  cube.position.z = -100;
  // rotate the cube by 30 degrees
  cube.rotation.y = degreesToRadians(30);
}

```

#### <a name="addlights"></a>addLights()

`addLights()` 함수는 조명 생성을 그룹화하여 장면에 추가하는 간단한 함수입니다.

```javascript
function addLights() {
  var lightOne = new THREE.DirectionalLight(0xffffff);
  lightOne.position.set(1, 1, 1);
  scene.add(lightOne);

  // Add a second light with half the intensity
  var lightTwo = new THREE.DirectionalLight(0xffffff, .5);
  lightTwo.position.set(1, -1, -1);
  scene.add(lightTwo);
}
```

#### <a name="onwindowresize"></a>onWindowResize()

`onWindowResize` 함수는 이벤트 수신기가 `resize` 이벤트 발생 소식을 수신할 때마다 호출됩니다. 이 동작은 사용자가 창 크기를 조정할 때마다 발생합니다. 이 동작이 발생하면 이미지 비율이 유지되고 이미지가 전체 창에 표시되는지 확인해야 합니다.

```javascript
function onWindowResize() {

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);
}
```

#### <a name="animate"></a>animate()

마지막으로, `render()` 함수를 호출하는 `animate()` 함수가 필요합니다. [`requestAnimationFrame()`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame) 함수는 렌더러를 지속적으로 업데이트하는 데 사용됩니다. 이 자습서의 후반부에 이러한 함수를 사용하여 미로 주변을 움직이는 것처럼 멋진 애니메이션을 사용하여 렌더러를 업데이트하겠습니다.

```javascript
function animate() {
    render();
    // Keep updating the renderer
    requestAnimationFrame(animate);
}

function render() {
    renderer.render(scene, camera);
}
```

#### <a name="unit-conversion-functions"></a>단위 변환 함수

**three.js**에서 회전은 라디안 단위로 측정됩니다. 사용 편의를 위해 각도와 라디안 간에 쉽게 변환할 수 있도록 함수 몇 개를 추가하겠습니다. 


```javascript
function degreesToRadians(degrees) {
  return degrees * Math.PI / 180;
}

function radiansToDegrees(radians) {
  return radians * 180 / Math.PI;
}
```

30도가 .523라디안이라는 것을 기억하는 분이 몇 명이나 될까요? 그 대신 `degreesToRadians(30)`을 사용하여 `createMazeCubes()` 함수에 사용되는 라디안 단위로 변환하는 것이 훨씬 간단합니다.

___

그것도 제법 괜찮은 코드이긴 하지만, 이제 `container`에 렌더링되는 멋진 큐브가 있습니다! CodePen에서 결과를 확인하세요.

문제가 발생할 경우 모든 JavaScript를 복사하여 CodePen에 붙여넣어서 문제를 해결할 수도 있고, 코드를 편집하여 일부 조명을 조정하고 일부 색상을 변경할 수도 있습니다. 

<iframe height='300' scrolling='no' title='Lights, camera, cube!' src='//codepen.io/MicrosoftEdgeDocumentation/embed/YZWygZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/YZWygZ/'>Lights, camera, cube!</a>를 참조하세요. <a href='https://codepen.io'>CodePen</a>의 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)에 있습니다.
</iframe>


### <a name="3-making-the-maze"></a>3. 미로 만들기

큐브는 그 자체로도 아름답지만 큐브로 이루어진 전체 미로는 그보다 훨씬 멋집니다! 레벨을 만드는 가장 빠른 방법 중 하나는 2D 배열을 사용하여 큐브를 배치하는 것이라는 사실은 게임 커뮤니티에서 널리 알려진 비밀입니다.
 
![2D 배열로 이루어진 미로](images/dinomap.png)

큐브가 있는 곳에 1’s를 배치하고 빈 공간에 0’s를 배치하면 수동으로 간단하게 미로를 만들고 수정할 수 있습니다.

이를 위해 기존의 `createMazeCubes()` 함수를 중첩 루프를 사용하여 여러 큐브를 만들고 배치하는 함수로 대체했습니다. 또한 이 자습서의 후반부에서 `collidableObjects`라는 배열을 만들고 충돌 감지를 위한 큐브를 추가할 것입니다.

```javascript
var totalCubesWide; // How many cubes wide the maze will be
var collidableObjects = []; // An array of collidable objects used later

function createMazeCubes() {
  // Maze wall mapping, assuming even square
  // 1's are cubes, 0's are empty space
  var map = [
    [0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ]
  ];

  // wall details
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });

  // Keep cubes within boundry walls
  var widthOffset = UNITWIDTH / 2;
  // Put the bottom of the cube at y = 0
  var heightOffset = UNITHEIGHT / 2;
  
  // See how wide the map is by seeing how long the first array is
  totalCubesWide = map[0].length;

  // Place walls where 1`s are
  for (var i = 0; i < totalCubesWide; i++) {
    for (var j = 0; j < map[i].length; j++) {
      // If a 1 is found, add a cube at the corresponding position
      if (map[i][j]) {
        // Make the cube
        var cube = new THREE.Mesh(cubeGeo, cubeMat);
        // Set the cube position
        cube.position.z = (i - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        cube.position.y = heightOffset;
        cube.position.x = (j - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        // Add the cube
        scene.add(cube);
        // Used later for collision detection
        collidableObjects.push(cube);
      }
    }
  }
    // The size of the maze will be how many cubes wide the array is * the width of a cube
    mapSize = totalCubesWide * UNITWIDTH;
}

```

이제 큐브가 몇 개나 사용되는지(그리고 크기가 얼마나 되는지) 알고 있으므로 계산된 `mapSize` 변수를 사용하여 수평면의 치수를 설정할 수 있습니다.

```javascript
var mapSize;    // The width/depth of the maze

function createGround() {
    // Create ground geometry and material
    var groundGeo = new THREE.PlaneGeometry(mapSize, mapSize);
    var groundMat = new THREE.MeshPhongMaterial({ color: 0xA0522D, side: THREE.DoubleSide});

    var ground = new THREE.Mesh(groundGeo, groundMat);
    ground.position.set(0, 1, 0);
    // Rotate the place to ground level
    ground.rotation.x = degreesToRadians(90);
    scene.add(ground);
}
```

우리가 추가할 미로의 마지막 조각은 모든 것을 둘러쌀 외벽입니다. 루프를 사용하여 한 번에 두 개의 평면(벽)을 만들겠습니다. `createGround()`에서 계산한 `mapSize` 변수를 사용하여 평면의 너비를 결정합니다. 새로 만든 벽도 이후에 충돌을 감지할 수 있도록 `collidableObjects` 배열에 추가합니다.

```javascript
function createPerimWalls() {
    var halfMap = mapSize / 2;  // Half the size of the map
    var sign = 1;               // Used to make an amount positive or negative

    // Loop through twice, making two perimeter walls at a time
    for (var i = 0; i < 2; i++) {
        var perimGeo = new THREE.PlaneGeometry(mapSize, UNITHEIGHT);
        // Make the material double sided
        var perimMat = new THREE.MeshPhongMaterial({ color: 0x464646, side: THREE.DoubleSide });
        // Make two walls
        var perimWallLR = new THREE.Mesh(perimGeo, perimMat);
        var perimWallFB = new THREE.Mesh(perimGeo, perimMat);

        // Create left/right wall
        perimWallLR.position.set(halfMap * sign, UNITHEIGHT / 2, 0);
        perimWallLR.rotation.y = degreesToRadians(90);
        scene.add(perimWallLR);
        // Used later for collision detection
        collidableObjects.push(perimWallLR);
        // Create front/back wall
        perimWallFB.position.set(0, UNITHEIGHT / 2, halfMap * sign);
        scene.add(perimWallFB);

        // Used later for collision detection
        collidableObjects.push(perimWallFB);

        sign = -1; // Swap to negative value
    }
}
```


`init()` 함수의 `createMazeCubes()` 뒤에서 `createGround()` 및 `createPerimWalls`에 호출을 추가해야만 컴파일된다는 사실을 꼭 기억하세요!
___

이제 멋진 미로가 생겼습니다. 하지만 카메라가 한 지점에 고정되어 있기 때문에 얼마나 멋진 미로인지 직접 살펴볼 수가 없습니다. 이 게임의 수준을 한 단계 높이고 일부 카메라 컨트롤을 추가할 시간입니다.

CodePen에서 `init()` 함수의 `createGround()`를 주석 처리하여 자유롭게 큐브 색상을 변경하거나 땅을 제거해 보세요.


<iframe height='300' scrolling='no' title='Maze building' src='//codepen.io/MicrosoftEdgeDocumentation/embed/JWKYzG/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)의 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/JWKYzG/'>Maze building</a>을 참조하세요.
</iframe>

### <a name="4-allowing-the-player-to-look-around"></a>4. 플레이어가 주변을 둘러볼 수 있게 만들기

이제 미로에 들어가서 주변을 둘러볼 시간입니다. 이렇게 하기 위해 **PointerLockControls.js** 라이브러리와 카메라를 사용하겠습니다.

**PoinerLockControls.js** 라이브러리는 마우스를 사용하여 마우스가 움직이는 방향으로 카메라를 회전함으로써 플레이어가 주변을 둘러볼 수 있게 합니다. 

먼저 **index.html** 파일에 몇 가지 새로운 요소를 추가하겠습니다.

```html
<div id="blocker">
    <div id="instructions">
    <strong>Click to look!</strong>
    </div>
</div>

<script src="main.js"></script>
```

이 섹션의 맨 아래에서 CodePen에 있는 CSS도 전부 필요합니다. **stylesheet.css** 파일에 붙여넣어야 합니다.

다시 **main.js**로 전환하여 새로운 글로벌 변수를 몇 개 추가합니다. `controls`는 컨트롤러를 저장하는 데 사용되고, `controlsEnabled`는 컨트롤러 상태를 추적하는 데, `blocker`는 **index.html**에서 `blocker` 요소를 잡는 데 사용됩니다.

```javascript
var controls;
var controlsEnabled = false;

// HTML elements to be changed
var blocker = document.getElementById('blocker');
```


이제 `init()` 함수에서 새 `PoinerLockControls` 개체를 만들어서 `camera`에 전달하고 `camera`(`controls.getObject()`를 통해 액세스)를 추가할 수 있습니다.

```javascript
controls = new THREE.PointerLockControls(camera);
scene.add(controls.getObject());
```

이제 카메라가 연결되었지만 주변을 둘러보려면 마우스와 컨트롤러가 상호 작용할 수 있어야 합니다. 

이와 같은 상황에서는 [포인터 잠금 API](https://docs.microsoft.com/microsoft-edge/dev-guide/dom/pointer-lock)를 사용하여 마우스 동작을 카메라와 연결하면 됩니다. 또한 포인터 잠금 API는 좀 더 몰입도 높은 환경을 제공하기 위해 마우스를 숨깁니다. ESC 키를 눌러 마우스-카메라 연결을 종료하면 마우스가 다시 나타납니다. `getPointerLock()` 및 `lockChange()` 함수를 추가하면 이 동작을 구현하는 데 도움이 됩니다.

`getPointerLock()` 함수는 마우스 클릭이 발생하는 시기를 수신합니다. 클릭이 발생하면 렌더링된 게임(`container` 요소의)이 마우스 제어권을 확보하려고 시도합니다. 또한 플레이어가 잠금을 활성화 또는 비활성화하는 시기를 감지하여 `lockChange()` 함수를 호출하는 이벤트 수신기도 추가합니다. 

```javascript
function getPointerLock() {
  document.onclick = function () {
    container.requestPointerLock();
  }
  document.addEventListener('pointerlockchange', lockChange, false); 
}

```

`lockChange()` 함수는 컨트롤과 `blocker` 요소를 활성화 또는 비활성화해야 합니다. [`pointerLockElement`](https://developer.mozilla.org/docs/Web/API/Document/pointerLockElement) 속성에서 마우스 이벤트의 대상이 `container`로 설정되었는지 확인하면 포인터 잠금 상태를 확인할 수 있습니다.

```javascript
function lockChange() {
    // Turn on controls
    if (document.pointerLockElement === container) {
        // Hide blocker and instructions
        blocker.style.display = "none";
        controls.enabled = true;
    // Turn off the controls
    } else {
      // Display the blocker and instruction
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

이제 `init()` 함수 바로 앞에 `getPointerLock()` 호출을 추가할 수 있습니다.
```javascript
// Get the pointer lock state
getPointerLock();
init();
animate();
```

---

드디어 주변을 **둘러보는** 기능이 생기기는 했지만 가장 중요한 요소는 **이동** 기능입니다. 이제부터 약간의 수학이 나오고 벡터가 나옵니다. 사실 수학이 전혀 들어가지 않은 3D 그래픽이란 불가능하죠.

<iframe height='300' scrolling='no' title='Look around' src='//codepen.io/MicrosoftEdgeDocumentation/embed/gmwbMo/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)의 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/gmwbMo/'>Look around</a>를 참조하세요.
</iframe>


### <a name="5-adding-player-movement"></a>5. 플레이어 이동 추가

플레이어의 이동 방법을 구현하기 위해 학창 시절에 배운 미적분을 떠올려 봅시다. 우리는 특정 벡터(방향)를 따라 `camera`에 속도(이동)를 적용하려 합니다.

플레이어가 움직이는 방향을 추적하는 글로벌 변수를 몇 개 추가하고 초기 속도 벡터를 설정해 보겠습니다.

```javascript
// Flags to determine which direction the player is moving
var moveForward = false;
var moveBackward = false;
var moveLeft = false;
var moveRight = false;

// Velocity vector for the player
var playerVelocity = new THREE.Vector3();

// How fast the player will move
var PLAYERSPEED = 800.0;

var clock;
```

`init()` 함수 시작 부분에서 `clock`을 새 `Clock` 개체로 설정합니다. 이 개체는 새 프레임을 렌더링하는 데 걸리는 시간(델타)을 추적하기 위해 사용됩니다. 사용자 입력을 수집하는 `listenForPlayerMovement()` 호출도 추가해야 합니다. 

```
clock = new THREE.Clock();
listenForPlayerMovement();
```

`listenForPlayerMovement()` 함수는 방향 상태를 전환합니다. 이 함수 맨 아래에는 키를 누르고 떼는 동작을 기다리는 두 개의 이벤트 수신기가 있습니다. 이러한 이벤트 중 하나가 발생하면 그 키가 동작을 트리거하려는 키인지 아니면 동작을 중지하려는 키인지 확인해야 합니다.

이 게임에서는 플레이어가 W, A, S, D 키 또는 화살표 키로 이동할 수 있도록 설정해 두었습니다.

```javascript
function listenForPlayerMovement() {
    
    // A key has been pressed
    var onKeyDown = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = true;
        break;

      case 37: // left
      case 65: // a
        moveLeft = true;
        break;

      case 40: // down
      case 83: // s
        moveBackward = true;
        break;

      case 39: // right
      case 68: // d
        moveRight = true;
        break;
    }
  };

  // A key has been released
    var onKeyUp = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = false;
        break;

      case 37: // left
      case 65: // a
        moveLeft = false;
        break;

      case 40: // down
      case 83: // s
        moveBackward = false;
        break;

      case 39: // right
      case 68: // d
        moveRight = false;
        break;
    }
  };

  // Add event listeners for when movement keys are pressed and released
  document.addEventListener('keydown', onKeyDown, false);
  document.addEventListener('keyup', onKeyUp, false);
}
```

이제 사용자가 어떤 방향으로 이동하려 하는지(이제 글로벌 방향 플래그 중 하나에 `true`로 저장됨) 확인할 수 있게 되었으며, 다음은 행동입니다. 이 행동은 `animatePlayer()` 함수의 형태로 발생합니다.

프레임 속도를 변경하는 동안 움직임이 동기화되지 않은 것처럼 보이지 않게 이 함수는 `animate()` 내에서 호출된 후 `delta`로 전달되어 프레임 간 변화를 시간 단위로 가져옵니다.

```javascript
function animate() {
  render();
  requestAnimationFrame(animate);
  // Get the change in time between frames
  var delta = clock.getDelta();
  animatePlayer(delta);
}
```

지금부터 재미있는 부분이 나옵니다! 운동량 벡터(`playerVeloctiy`)에는 세 개의 매개 변수(`(x, y, z)`)가 있는데, 그 중에서 `y`는 수직 운동량입니다. 이 게임에는 점프 동작이 없으므로 `x` 및 `z` 매개 변수만 작업하면 됩니다. 처음에는 이 벡터가 (0, 0, 0)으로 설정됩니다.

아래 코드와 같이, 어떤 방향 플래그가 `true`로 전환되는지 확인하기 위해 일련의 검사가 수행됩니다. 방향이 확인되면 `x` 및 `y`에 더하거나 빼서 해당 방향으로 운동량을 적용합니다. 이동 키를 누르지 않으면 벡터는 다시 `(0, 0, 0)`으로 설정됩니다.


```javascript

function animatePlayer(delta) {
  // Gradual slowdown
  playerVelocity.x -= playerVelocity.x * 10.0 * delta;
  playerVelocity.z -= playerVelocity.z * 10.0 * delta;

  if (moveForward) {
    playerVelocity.z -= PLAYERSPEED * delta;
  } 
  if (moveBackward) {
    playerVelocity.z += PLAYERSPEED * delta;
  } 
  if (moveLeft) {
    playerVelocity.x -= PLAYERSPEED * delta;
  } 
  if (moveRight) {
    playerVelocity.x += PLAYERSPEED * delta;
  }
  if( !( moveForward || moveBackward || moveLeft ||moveRight)) {
    // No movement key being pressed. Stop movememnt
    playerVelocity.x = 0;
    playerVelocity.z = 0;
  }
  controls.getObject().translateX(playerVelocity.x * delta);
  controls.getObject().translateZ(playerVelocity.z * delta);
}
```

결국, 업데이트된 `x` 및 `y` 값을 카메라에 화면 전환으로 적용하여 플레이어를 실제로 이동하는 것입니다.

---

축하합니다. 주변을 이동하고 둘러볼 수 있는 플레이어 제어 카메라를 만들었습니다. 여전히 벽을 바로 통과하기는 하지만 나중에 해결해도 됩니다. 다음으로 공룡을 추가하겠습니다.

<iframe height='300' scrolling='no' title='Move around' src='//codepen.io/MicrosoftEdgeDocumentation/embed/qrbKZg/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)의 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qrbKZg/'>Move around</a>를 참조하세요.
</iframe>

> [!NOTE]
> UWP 앱에서 이러한 컨트롤을 사용하면 동작 지연 및 등록되지 않은 `keyUp` 이벤트가 발생할 수 있습니다. 현재 이 부분을 조사 중이며 샘플에서 이 부분이 곧 해결될 것입니다!

### <a name="6-load-that-dino"></a>6. 공룡을 로드합니다!

이 프로젝트 리포지토리를 복제 또는 다운로드한 경우 `dino.json` 안에 `models` 폴더가 있을 것입니다. 이 JSON 파일은 Blender에서 만들어서 내보낸 3D 공룡 모델입니다.


이 공룡을 완전히 로드하려면 글로벌 변수를 더 추가해야 합니다.

```javascript
var DINOSCALE = 20;  // How big our dino is scaled to

var clock;
var dino;
var loader = new THREE.JSONLoader();

var instructions = document.getElementById('instructions');
```

만들어 둔 `JSONLoader`가 있으니, **dino.json** 경로와 파일에서 수집한 기하 도형 및 재료가 포함된 콜백을 전달하겠습니다.
공룡을 로드하는 작업은 비동기 작업입니다. 즉, 공룡이 완전히 로드될 때까지 아무 것도 렌더링되지 않는다는 의미입니다. 현재 로딩 중이라는 것을 플레이어가 알 수 있도록 **index.html**에서 `instructions` 요소의 문자열을 `"Loading..."`으로 변경했습니다.

공룡이 로드되면 `instructions` 요소를 게임의 실제 지침으로 업데이트하고, `animate()` 함수를 `init()` 끝에서 아래에 보이는 함수 콜백의 끝으로 이동합니다.

```javascript
   // load the dino JSON model and start animating once complete
    loader.load('./models/dino.json', function (geometry, materials) {


        // Get the geometry and materials from the JSON
        var dinoObject = new THREE.Mesh(geometry, new THREE.MultiMaterial(materials));

        // Scale the size of the dino
        dinoObject.scale.set(DINOSCALE, DINOSCALE, DINOSCALE);
        dinoObject.rotation.y = degreesToRadians(-90);
        dinoObject.position.set(30, 0, -400);
        dinoObject.name = "dino";
        scene.add(dinoObject);
        
        // Store the dino
        dino = scene.getObjectByName("dino"); 

        // Model is loaded, switch from "Loading..." to instruction text
        instructions.innerHTML = "<strong>Click to Play!</strong> </br></br> W,A,S,D or arrow keys = move </br>Mouse = look around";

        // Call the animate function so that animation begins after the model is loaded
        animate();
    });
```

---

드디어 공룡 모델이 로드되었습니다. 확인해보세요!

<iframe height='300' scrolling='no' title='Adding the dino' src='//codepen.io/MicrosoftEdgeDocumentation/embed/xqOwBw/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)의 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/xqOwBw/'>Adding the dino</a>를 참조하세요.
</iframe>

### <a name="7-move-that-dino"></a>7. 공룡을 이동합니다!

게임용 AI를 직접 만들기에는 너무 복잡할 수 있으므로 이 예제에서는 이 공룡의 간단한 이동 동작만 구현하겠습니다. 이 공룡은 일직선으로 이동하여 벽을 통과해서 저 멀리 있는 안개 속으로 사라집니다.

이렇게 하려면 먼저 `dinoVelocity` 글로벌 변수를 추가합니다.

```javascript
var DINOSPEED = 400.0;

var dinoVelocity = new THREE.Vector3();
```
 다음으로 `animation()` 함수에서 `animateDino()` 함수를 호출하여 아래 코드에 추가합니다.

```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;

    dinoVelocity.z += DINOSPEED * delta;
    // Move the dino
    dino.translateZ(dinoVelocity.z * delta);
}
```
---

공룡이 그냥 돌아다니는 광경은 별로 재미가 없습니다. 하지만 충돌 감지를 추가하면 훨씬 흥미로운 상황이 발생합니다.

<iframe height='300' scrolling='no' title='Moving the dino - no collision' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/jBMbbL/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)의 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/jBMbbL/'>Moving the dino - no collision</a>을 참조하세요.
</iframe>

### <a name="8-collision-detection-for-the-player"></a>8. 플레이어에 대한 충돌 감지

플레이어와 공룡이 돌아다니는 동작을 구현했지만 벽을 그대로 통과한다는 문제가 여전히 남아 있습니다. 이 자습서의 앞부분에서 처음에 큐브와 벽을 추가할 때 `collidableObjects` 배열에 큐브와 벽을 추가했습니다. 이 배열을 사용하여 플레이어가 통과할 수 없는 물체와 너무 가까이에 있는지 여부를 확인할 것입니다.

raycaster를 사용하여 교차가 발생할 시점을 확인하겠습니다. raycaster를 특정 방향으로 놓인 카메라에서 나오는 레이저 광선으로 생각하면 됩니다. 개체에 닿으면 개체에 닿았다는 사실과 함께 개체와의 정확한 거리를 보고하는 것입니다.

```javascript
var PLAYERCOLLISIONDISTANCE = 20;
```

플레이어가 충돌 가능한 개체와 너무 가까이 있으면 `true`를 반환하는 `detectPlayerCollision()`이라고 하는 새 함수를 만들겠습니다.
플레이어의 경우 플레이어가 이동하는 방향에 따라 플레이어가 가리키는 방향을 변경하는 raycaster를 적용하겠습니다.

이를 위해, 정의되지 않은 매트릭스인 `rotationMatrix`를 만듭니다. 이동하는 방향을 확인할 때 `rotationMatrix`는 결국 정의된 매트릭스가 되거나 앞으로 이동하는 경우에는 정의되지 않은 매트릭스가 됩니다.
정의된 매트릭스인 경우 `rotationMatrix`는 컨트롤 방향에 적용됩니다. 

그런 다음, 카메라에서 시작하여 `cameraDirection` 방향으로 연결되는 raycaster가 생성됩니다.


```javascript
function detectPlayerCollision() {
    // The rotation matrix to apply to our direction vector
    // Undefined by default to indicate ray should coming from front
    var rotationMatrix;
    // Get direction of camera
    var cameraDirection = controls.getDirection(new THREE.Vector3(0, 0, 0)).clone();

    // Check which direction we're moving (not looking)
    // Flip matrix to that direction so that we can reposition the ray
    if (moveBackward) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(180));
    }
    else if (moveLeft) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(90));
    }
    else if (moveRight) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(270));
    }

    // Player is not moving forward, apply rotation matrix needed
    if (rotationMatrix !== undefined) {
        cameraDirection.applyMatrix4(rotationMatrix);
    }

    // Apply ray to player camera
    var rayCaster = new THREE.Raycaster(controls.getObject().position, cameraDirection);

    // If our ray hit a collidable object, return true
    if (rayIntersect(rayCaster, PLAYERCOLLISIONDISTANCE)) {
        return true;
    } else {
        return false;
    }
}
```

`detectPlayerCollision()` 함수는 `rayIntersect()` 도우미 함수에 의존합니다.
여기에는 충돌이 발생한 것으로 판정되기 전까지 `collidableObjects` 배열의 개체에 얼마나 가까이 접근할 수 있는지를 나타내는 값과 raycaster가 필요합니다.

```javascript
function rayIntersect(ray, distance) {
    var intersects = ray.intersectObjects(collidableObjects);
    for (var i = 0; i < intersects.length; i++) {
        // Check if there's a collision
        if (intersects[i].distance < distance) {
            return true;
        }
    }
    return false;
}
```

이제 충돌이 발생하기 직전 시점을 확인할 수 있으므로 `animatePlayer()` 함수를 좀 더 개선할 수 있습니다.

```javascript
function animatePlayer(delta) {
    // Gradual slowdown
    playerVelocity.x -= playerVelocity.x * 10.0 * delta;
    playerVelocity.z -= playerVelocity.z * 10.0 * delta;

    // If no collision and a movement key is being pressed, apply movement velocity
    if (detectPlayerCollision() == false) {
        if (moveForward) {
            playerVelocity.z -= PLAYERSPEED * delta;
        }
        if (moveBackward) {
            playerVelocity.z += PLAYERSPEED * delta;
        } 
        if (moveLeft) {
            playerVelocity.x -= PLAYERSPEED * delta;
        }
        if (moveRight) {
            playerVelocity.x += PLAYERSPEED * delta;
        }

        controls.getObject().translateX(playerVelocity.x * delta);
        controls.getObject().translateZ(playerVelocity.z * delta);
    } else {
        // Collision or no movement key being pressed. Stop movememnt
        playerVelocity.x = 0;
        playerVelocity.z = 0;
    }
}
```

---

플레이어 충돌 감지를 구현했으니 벽을 뚫고 지나갈 수 있는지 직접 테스트해 보세요!

<iframe height='300' scrolling='no' title='Moving the player - collision' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/qraOeO/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)의 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qraOeO/'>Moving the player - collision</a>을 참조하세요.
</iframe>


### <a name="9-collision-detection-and-animation-for-dino"></a>9. 공룡에 대한 충돌 감지 및 애니메이션

공룡이 더 이상 벽을 통과하지 않고, 충돌 가능한 개체에 너무 가까이 접근하면 임의의 방향으로 이동하도록 만들겠습니다.

먼저 공룡이 충돌하는 시기를 알아보겠습니다. 

충돌 거리에 대한 또 다른 글로벌 변수를 설정해야 합니다.

```javascript
var DINOCOLLISIONDISTANCE = 55;     
```

공룡이 충돌하는 거리는 앞에서 지정했으니, `detectPlayerCollision()` 함수와 비슷하지만 좀 더 간단한 함수를 추가해 봅시다.
`detectDinoCollision` 함수는 항상 공룡의 정면에서 raycaster가 나온다는 점에서 간단한 함수라고 할 수 있습니다. 플레이어 충돌처럼 회전할 필요가 없습니다.

```javascript
function detectDinoCollision() {
    // Get the rotation matrix from dino
    var matrix = new THREE.Matrix4();
    matrix.extractRotation(dino.matrix);
    // Create direction vector
    var directionFront = new THREE.Vector3(0, 0, 1);

    // Get the vectors coming from the front of the dino
    directionFront.applyMatrix4(matrix);

    // Create raycaster
    var rayCasterF = new THREE.Raycaster(dino.position, directionFront);
    // If we have a front collision, we have to adjust our direction so return true
    if (rayIntersect(rayCasterF, DINOCOLLISIONDISTANCE))
        return true;
    else
        return false;
}
```

충돌이 감지되면 마지막 `animateDino()` 함수가 어떤 형태로 표시되는지 살펴보겠습니다.


```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;


    // If no collision, apply movement velocity
    if (detectDinoCollision() == false) {
        dinoVelocity.z += DINOSPEED * delta;
        // Move the dino
        dino.translateZ(dinoVelocity.z * delta);

    } else {
        // Collision. Adjust direction
        var directionMultiples = [-1, 1, 2];
        var randomIndex = getRandomInt(0, 2);
        var randomDirection = degreesToRadians(90 * directionMultiples[randomIndex]);

        dinoVelocity.z += DINOSPEED * delta;
        dino.rotation.y += randomDirection;
    }
}
```

우리가 원하는 것은 공룡을 항상 -90, 90 또는 180도 회전하는 것입니다. 이 작업을 간단하게 처리하기 위해, 앞에서 우리는 90을 곱하면 이러한 숫자가 나오는 `directionMultiples` 배열을 만들었습니다.
임의의 회전 각도를 선택하기 위해, 임의의 배열 인덱스를 나타내는 값인 0, 1 또는 2를 선택하는 `getRandomInt()` 도우미 함수를 추가했습니다.

```javascript
function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min)) + min;
}
```

이 작업을 모두 완료한 후에는 임의의 배열 인덱스에 90을 곱해서 회전 각도(라디안 단위로 변환)를 구합니다.
`dino.rotation.y += randomDirection;`을 사용하여 이 값을 공룡의 `y` 회전에 추가하면 공룡이 충돌 시 임의의 방향으로 회전합니다.


---

드디어 끝났습니다! 미로를 돌아다닐 수 있는 AI를 탑재한 공룡이 완성되었습니다!

<iframe height='300' scrolling='no' title='Moving the dino - collision' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/bqwMXZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)의 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/bqwMXZ/'>Moving the dino - collision</a>을 참조하세요.
</iframe>

### <a name="10-starting-the-chase"></a>10. 추적 시작

공룡이 플레이어와 일정 거리 이내에 있으면 플레이어를 추적하도록 만들겠습니다. 이는 단지 예시에 불과하기 때문에 공룡이 플레이어를 추적하는 고급 알고리즘은 적용되지 않았습니다. 그 대신 공룡이 플레이어를 확인하면 플레이어에게 접근합니다. 미로의 개방된 지역에서는 이 동작이 정상적으로 작동하지만 중간에 벽이 있으면 공룡이 움직이지 못합니다.

`animate()` 함수에서 `triggerChase()` 함수가 반환하는 값에 따라 결정되는 부울 변수를 추가하겠습니다.

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();

    animateDino(delta);
    animatePlayer(delta);
}
```

`triggerChase` 함수는 플레이어가 공룡의 추적 범위에 있는지 확인한 다음, 공룡이 항상 플레이어가 있는 방향을 향하게 합니다. 따라서 공룡이 플레이어가 있는 방향으로 이동할 수 있게 됩니다. 

```javascript
function triggerChase() {
    // Check if in dino detection range of the player
    if (dino.position.distanceTo(controls.getObject().position) < 300) {
        // Set the dino target's y value to the current y value. We only care about x and z for movement.
        var lookTarget = new THREE.Vector3();
        lookTarget.copy(controls.getObject().position);
        lookTarget.y = dino.position.y;

        // Make dino face camera
        dino.lookAt(lookTarget);

        // Get distance between dino and camera with a unit offset
        // Game over when dino is the value of CATCHOFFSET units away from camera
        var distanceFrom = Math.round(dino.position.distanceTo(controls.getObject().position)) - CATCHOFFSET;
        // Alert and display distance between camera and dino
        dinoAlert.innerHTML = "Dino has spotted you! Distance from you: " + distanceFrom;
        dinoAlert.style.display = '';
        return true;
        // Not in agro range, don't start distance countdown
    } else {
        dinoAlert.style.display = 'none';
        return false;
    }
}
```

`triggerChase` 함수의 절반은 플레이어에게 공룡과의 거리를 알려주는 텍스트 표시 작업을 처리합니다. 또한 `CATCHOFFSET`을 사용하여 `0`이 얼마나 떨어져 있어야 하는지를 지정합니다. 오프셋이 없으면 `0`이 플레이어 바로 위에 있어서 결말이 매우 시시하게 됩니다.



```javascript
var dinoAlert = document.getElementById('dino-alert');
dinoAlert.style.display = 'none';
```

---

이제 플레이어가 일정 범위 내에 들어오면 플레이어의 위에 위치할 때까지 플레이어를 계속 추적하는 사나운 공룡이 완성되었습니다.
마지막으로 공룡이 `CATCHOFFSET` 단위만큼 떨어져 있으면 게임이 종료되는 조건을 추가합니다.

<iframe height='300' scrolling='no' title='The chase' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpRBqR/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Microsoft Edge 문서(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)의 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpRBqR/'>The chase</a>를 참조하세요.
</iframe>


### <a name="11-ending-the-game"></a>11. 게임 종료


큐브 하나로 시작해서 게임을 만들었으며, 이제 게임을 종료할 시간입니다.

먼저 게임이 끝났는지 여부를 추적하는 변수를 설정합니다.

```javascript
var gameOver = false;
```

공룡이 플레이어와 너무 가까이 있는지 확인하도록 `animate()` 함수를 마지막으로 업데이트해야 합니다.
공룡이 너무 가까이 있으면 `caught()`라는 새 함수를 시작하여 플레이어와 공룡의 이동을 멈추고, 그렇지 않다면 게임을 계속 진행하고 플레이어와 공룡이 움직일 수 있게 합니다.

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();
    // Update our frames per second monitor

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();
    // If the player is too close, trigger the end of the game
    if (dino.position.distanceTo(controls.getObject().position) < CATCHOFFSET) {
        caught();
    // Player is at an undetected distance
    // Keep the dino moving and let the player keep moving too
    } else {
        animateDino(delta);
        animatePlayer(delta);
    }
}
```

공룡이 플레이어를 잡으면 `caught()`는 `blocker` 요소를 표시하고 게임 종료를 알리도록 텍스트를 업데이트합니다.
또한 `gameOver` 변수가 `true`로 설정되어 게임이 끝났음을 알 수 있습니다.  


```javascript
function caught() {
    blocker.style.display = '';
    instructions.innerHTML = "GAME OVER </br></br></br> Press ESC to restart";
    gameOver = true;
    instructions.style.display = '';
    dinoAlert.style.display = 'none';
}
```


게임이 끝났는지 여부를 알게 되었으니, `lockChange()` 함수에 게임 종료 검사를 추가할 수 있습니다.
이제 게임이 종료되면 사용자가 ESC 키를 눌렀을 때 `location.reload`를 추가하여 게임을 다시 시작할 수 있습니다.

```javascript
function lockChange() {
    if (document.pointerLockElement === container) {
        blocker.style.display = "none";
        controls.enabled = true;
    } else {
        if (gameOver) {
            location.reload();
        }
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

---

간단하죠. 긴 여정 끝에 드디어 **three.js**로 작성된 게임이 완성되었습니다.

페이지 맨 위로 돌아가서 [최종 CodePen](#introduction)을 살펴보세요!


## <a name="publishing-to-the-microsoft-store"></a>Microsoft Store에 게시
UWP 앱을 만들었으니 이제 Microsoft Store에 게시할 수 있습니다. 물론 그 전에 앱을 좀 더 다듬으면 좋을 것입니다. 몇 가지 단계를 처리해야 합니다.

1.  Windows 개발자로 [등록](https://developer.microsoft.com/store/register)해야 합니다.
2.  앱 제출 [검사 목록](https://docs.microsoft.com/windows/uwp/publish/app-submissions)을 사용해야 합니다.
3.  앱을 제출하여 [인증](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process)을 받아야 합니다.
자세한 내용은 [UWP 앱 게시](https://docs.microsoft.com/windows/uwp/publish/)를 참조하세요.

