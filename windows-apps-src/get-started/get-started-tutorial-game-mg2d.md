---
title: MonoGame 2D로 UWP 게임 만들기
description: C# 및 MonoGame으로 작성된 간단한 Microsoft Store용 UWP 게임
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5d5f7af2-41a9-4749-ad16-4503c64bb80c
ms.localizationpriority: medium
ms.openlocfilehash: a3fa5833d126ea41a6efbf714d2f9dae87eba933
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318877"
---
# <a name="create-a-uwp-game-in-monogame-2d"></a>MonoGame 2D로 UWP 게임 만들기

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-c-and-monogame"></a>C# 및 MonoGame으로 작성된 간단한 Microsoft Store용 2D UWP 게임


![Walking Dino 스프라이트 시트](images/JS2D_0.png)

## <a name="introduction"></a>소개

MonoGame은 경량의 게임 개발 프레임워크입니다. 이 자습서에서는 콘텐츠를 로드하는 방법, 스프라이트를 그리고 애니메이션하는 방법, 사용자 입력을 처리하는 방법을 포함하여 MonoGame으로 게임을 개발하기 위한 기본 사항을 안내합니다. 충돌 감지나 고 DPI 화면의 크기 조정 같은 일부 고급 개념도 다룹니다. 이 자습서는 30-60분 정도 걸립니다.

## <a name="prerequisites"></a>필수 구성 요소
+   Windows 10 및 Microsoft Visual Studio 2019.  [Visual Studio를 사용하여 설정하는 방법을 알아보려면 여기를 클릭하세요](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ .NET 데스크톱 개발 프레임워크. Visual Studio가 아직 설치되지 않은 경우, Visual Studio 설치 프로그램을 다시 실행하여 Visual Studio 2019 설치를 수정할 수 있습니다.
+   C# 또는 유사한 개체 중심 프로그래밍 언어에 대한 기본 지식. [C#을 시작하는 방법을 알아보려면 여기를 클릭하세요](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).
+   클래스, 메서드, 변수 등 기본적인 컴퓨터 공학 개념에 대한 지식.

## <a name="why-monogame"></a>MonoGame을 사용하는 이유
게임 개발 환경에 필요한 옵션은 차고 넘치게 많습니다. Unity 같은 완전한 기능을 갖춘 엔진부터 DirectX 같은 포괄적이고 복잡한 멀티미디어 API까지 너무나도 많은 옵션이 있기 때문에 어디서 시작해야 할지 판단하기가 쉽지 않습니다. MonoGame은 복잡성 수준으로 볼 때 게임 엔진과 DirectX 같은 grittier API 사이에 속하는 도구 세트입니다. 간편한 콘텐츠 파이프라인을 제공하며, 다양한 플랫폼에서 실행되는 가벼운 게임을 만드는 데 필요한 모든 기능을 갖추고 있습니다. 무엇보다도 MonoGame 앱은 순수한 C#로 작성되기 때문에 Microsoft Store 또는 기타 비슷한 배포 플랫폼을 통해 신속하게 배포할 수 있습니다.

## <a name="get-the-code"></a>코드 다운로드
자습서를 단계별로 연습하지 않고 MonoGame이 실제로 작동하는 것만 보려면 [여기를 클릭하여 완성된 앱을 다운로드하세요](https://github.com/Microsoft/Windows-appsample-get-started-mg2d).

Visual Studio 2019에서 프로젝트를 열고 **F5** 키를 눌러 샘플을 실행합니다. 처음 실행하는 경우 Visual Studio가 현재 설치된 파일에서 누락된 NuGet 패키지를 가져와야 하므로 다소 시간이 걸릴 수 있습니다.

이 작업을 마쳤으면 MonoGame 설정에 대한 다음 섹션으로 넘어가서 단계별 코드 연습을 살펴봅니다.

**참고:** 이 샘플에서 만드는 게임은 완전한(또는 재미를 위한) 게임이 아닙니다 이 샘플의 유일한 목적은 MonoGame으로 2D 게임을 개발하는 핵심 개념을 보여주는 것입니다. 이 코드를 자유롭게 사용하고 필요에 따라 개선하셔도 되고, 기본 사항을 익힌 후 처음부터 새로 시작하셔도 됩니다!

## <a name="set-up-monogame-project"></a>MonoGame 프로젝트 설정
1. [MonoGame.net](https://www.monogame.net/)에서 Visual Studio용 **MonoGame 3.6**을 다운로드합니다.

2. Visual Studio 2019를 시작합니다.

3. **파일 -> 새로 만들기 -> 프로젝트**로 이동합니다.

4. Visual C# 프로젝트 템플릿에서 **MonoGame**, **MonoGame Windows 10 유니버설 프로젝트**를 선택합니다.

5. 프로젝트 이름을 “MonoGame2D"로 지정하고 확인을 선택합니다. 생성된 프로젝트에 오류가 잔뜩 있는 것처럼 보일 것입니다. 프로젝트를 처음으로 실행하면 이러한 오류가 사라지고, 누락된 NuGet 패키지가 설치됩니다.

6. **x86** 및 **로컬 컴퓨터**를 대상 플랫폼으로 설정하고 **F5** 키를 눌러 빈 프로젝트를 빌드하여 실행합니다. 위의 단계를 잘 따라 하셨다면 프로젝트 빌드를 마친 후 비어 있는 파란색 창이 보일 것입니다.

## <a name="method-overview"></a>메서드 개요
프로젝트를 만들었으니, 이제 **솔루션 탐색기**에서 **Game1.cs** 파일을 엽니다. 대부분의 게임 논리가 이 파일로 이동합니다. 개발자가 새 MonoGame 프로젝트를 만들 때 여러 중요한 메서드가 여기에 자동으로 생성됩니다. 메서드를 간략하게 살펴보도록 하겠습니다.

**public Game1()** 생성자입니다. 이 자습서에서는 이 메서드를 변경할 일이 없습니다.

**protected override void Initialize()** 사용되는 모든 클래스 변수를 여기서 초기화합니다. 이 메서드는 게임을 시작할 때 한 번 호출됩니다.

**protected override void LoadContent()** 이 메서드는 게임이 시작되기 전에 콘텐츠(예: 텍스처, 오디오, 글꼴)를 메모리에 로드합니다. Initialize와 마찬가지로, 앱이 시작될 때 한 번 호출됩니다.

**protected override void UnloadContent()** 이 메서드는 비 콘텐츠-관리자 콘텐츠를 언로드하는 데 사용됩니다. 여기서는 이 메서드는 사용할 일이 없습니다.

**protected override void Update(GameTime gameTIme)** 이 메서드는 게임 루프 주기마다 한 번 호출됩니다. 게임에 사용되는 개체 또는 변수의 상태를 여기서 업데이트합니다. 개체의 위치, 속도, 색 등이 포함됩니다. 사용자 입력도 여기서 처리됩니다. 즉, 이 메서드는 화면에 개체를 그리는 것을 제외한 게임 논리의 모든 부분을 처리합니다.

**protected override void Draw(GameTime gameTime)** 이 메서드는 Update 메서드가 제공하는 위치를 사용하여 화면에 개체를 그립니다.

## <a name="draw-a-sprite"></a>스프라이트 그리기
새로운 MonoGame 프로젝트를 실행하면 멋진 파란색 하늘을 볼 수 있습니다. 이제 땅을 추가하겠습니다.
MonoGame에서 2D 아트는 "스프라이트" 형태로 앱에 추가됩니다. 스프라이트는 단일 엔터티로 조작되는 컴퓨터 그래픽일 뿐입니다. 스프라이트를 이동하고, 모양을 만들고, 애니메이션하고, 결합하여 상상하는 그 무엇이든 2D 공간에 만들 수 있습니다.

### <a name="1-download-a-texture"></a>1. 텍스처 다운로드
우리 목적에 필요한 이 첫 번째 스프라이트는 매우 지루할 것입니다. [여기를 클릭하여 아무 특징 없는 이 녹색 사각형을 다운로드하세요](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/grass.png).

### <a name="2-add-the-texture-to-the-content-folder"></a>2. Content 폴더에 텍스처 추가
- **솔루션 탐색기**를 엽니다.
- **Content** 폴더에서 **Content.mgcb**를 마우스 오른쪽 단추로 클릭한 다음, **연결 프로그램**를 선택합니다. 팝업 메뉴에서 **Monogame 파이프라인**을 선택하고 **확인**을 선택합니다.
- 새 창에서 **콘텐츠** 항목을 마우스 오른쪽 단추로 클릭하고 **추가 -> 기존 항목**을 선택합니다.
- 파일 브라우저에서 녹색 사각형을 찾아 선택합니다.
- 항목 이름을 "grass.png"로 지정하고 **추가**를 선택합니다.

### <a name="3-add-class-variables"></a>3. 클래스 변수 추가
이 이미지를 스프라이트 텍스처로 추가하려면 **Game1.cs**를 열고 다음 클래스 변수를 추가합니다.

```CSharp
const float SKYRATIO = 2f/3f;
float screenWidth;
float screenHeight;
Texture2D grass;
```

SKYRATIO 변수는 화면에서 하늘과 풀밭의 비율을 알려줍니다. 이 예에서는 2/3입니다. **screenWidth** 및 **screenHeight**는 앱 창 크기를 추적하고, **grass**는 녹색 사각형을 저장할 위치입니다.

### <a name="4-initialize-class-variables-and-set-window-size"></a>4. 클래스 변수를 초기화하고 창 크기 설정
여전히 **screenWidth** 및 **screenHeight** 변수를 초기화해야 하므로 **Initialize** 메서드에 다음 코드를 추가합니다.

```CSharp
ApplicationView.PreferredLaunchWindowingMode = ApplicationViewWindowingMode.FullScreen;

screenHeight = (float)ApplicationView.GetForCurrentView().VisibleBounds.Height;
screenWidth = (float)ApplicationView.GetForCurrentView().VisibleBounds.Width;

this.IsMouseVisible = false;
```

화면의 높이와 너비 외에도 앱의 창 작업 모드를 **전체 화면**으로 설정하고 마우스를 보이지 않게 합니다.

### <a name="5-load-the-texture"></a>5. 텍스처 로드
grass 변수에 텍스처를 로드하기 위해 **LoadContent** 메서드에 다음을 추가합니다.

```CSharp
grass = Content.Load<Texture2D>("grass");
```

### <a name="6-draw-the-sprite"></a>6. 스프라이트 그리기
사각형을 그리기 위해 **Draw** 메서드에 다음 줄을 추가합니다.

```CSharp
GraphicsDevice.Clear(Color.CornflowerBlue);
spriteBatch.Begin();
spriteBatch.Draw(grass, new Rectangle(0, (int)(screenHeight * SKYRATIO),
  (int)screenWidth, (int)screenHeight), Color.White);
spriteBatch.End();
```

여기서는 **spriteBatch.Draw** 메서드를 사용하여 사각형 개체의 경계 내부에 지정된 텍스처를 배치하겠습니다. **Rectangle**은 왼쪽 위와 오른쪽 위 모서리의 x 및 y 좌표를 통해 정의됩니다. 앞에서 정의한 **screenWidth**, **screenHeight** 및 **SKYRATIO** 변수를 사용하여 화면의 하단 1/3에 녹색 사각형 텍스처를 그립니다. 이제 프로그램을 실행하면 앞에서 본 파란색 배경에 부분적으로 녹색 사각형이 덮인 것을 볼 수 있습니다.

![녹색 사각형](images/monogame-tutorial-1.png)

## <a name="scale-to-high-dpi-screens"></a>높은 DPI로 화면 조정
Surface Pro나 Surface Studio처럼 픽셀 밀도가 높은 모니터에서 Visual Studio를 실행하면 위에서 만든 녹색 사각형이 화면의 하단 1/3을 완전히 덮고 있지 않은 것을 발견할 수도 있습니다. 아마도 화면의 왼쪽 아래 모서리 위에 떠 있는 상태일 것입니다. 이 문제를 해결하고 모든 디바이스에서 동일한 게임 환경을 제공하려면 화면의 픽셀 밀도에 비례하여 특정 값을 조정하는 메서드를 만들어야 합니다.

```CSharp
public float ScaleToHighDPI(float f)
{
  DisplayInformation d = DisplayInformation.GetForCurrentView();
  f *= (float)d.RawPixelsPerViewPixel;
  return f;
}
```

다음으로 **Initialize** 메서드의 **screenHeight** 및 **screenWidth** 초기화를 다음 항목으로 바꿉니다.

```CSharp
screenHeight = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Height);
screenWidth = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Width);
```

DPI가 높은 화면에서 앱을 다시 실행하면 녹색 사각형이 화면의 하단 1/3을 우리가 의도한 대로 덮고 있는 것을 볼 수 있습니다.

## <a name="build-the-spriteclass"></a>SpriteClass 빌드
스프라이트를 애니메이션하기 전에 "SpriteClass"라고 하는 새 클래스를 만들겠습니다. 이 클래스는 스프라이트 조작의 표면 수준 복잡성을 줄여 줍니다.

### <a name="1-create-a-new-class"></a>1. 새 클래스 만들기
**솔루션 탐색기**에서 **MonoGame2D(유니버설 Windows)** 를 마우스 오른쪽 단추로 클릭하고 **추가 -> 클래스**를 선택합니다. 클래스 이름을 "SpriteClass.cs"로 지정하고 **추가**를 선택합니다.

### <a name="2-add-class-variables"></a>2. 클래스 변수 추가
방금 만든 클래스에 다음 코드를 추가합니다.

```CSharp
public Texture2D texture
{
  get;
}

public float x
{
  get;
  set;
}

public float y
{
  get;
  set;
}

public float angle
{
  get;
  set;
}

public float dX
{
  get;
  set;
}

public float dY
{
  get;
  set;
}

public float dA
{
  get;
  set;
}

public float scale
{
  get;
  set;
}
```

여기서 스프라이트를 그리고 애니메이트하는 데 필요한 클래스 변수를 설정합니다. **x** 및 **y** 변수는 평면에서 스프라이트의 현재 위치를 나타내고 **angle** 변수는 스프라이트의 현재 각을 도 단위로 나타냅니다(0은 수직, 90은 시계 방향으로 90도 기울어진 상태). 이 클래스의 **x** 및 **y**는 스프라이트의 **center** 좌표를 나타냅니다(기본 원점은 왼쪽 위 모서리). 따라서 스프라이트가 지정된 원점을 기준으로 회전하기 때문에 스프라이트를 보다 쉽게 회전할 수 있으며, 중심을 기준으로 회전하면 일관적인 회전 동작을 얻을 수 있습니다.

그 다음으로 각각 **x**, **y**, **angle** 변수의 초당 변화 속도를 나타내는 **dX**, **dY**, **dA**가 있습니다.

### <a name="3-create-a-constructor"></a>3. 생성자 만들기
**SpriteClass** 인스턴스를 만들 때 생성자에 **Game1.cs**의 그래픽 디바이스, 프로젝트 폴더에 대한 텍스처 경로, 원래 크기를 기준으로 원하는 텍스처 배율을 제공합니다. 나머지 클래스 변수는 게임을 시작한 후 업데이트 메서드에서 설정하겠습니다.

```CSharp
public SpriteClass (GraphicsDevice graphicsDevice, string textureName, float scale)
{
  this.scale = scale;
  if (texture == null)
  {
    using (var stream = TitleContainer.OpenStream(textureName))
    {
      texture = Texture2D.FromStream(graphicsDevice, stream);
    }
  }
}
```

### <a name="4-update-and-draw"></a>4. 업데이트 및 그리기
SpriteClass 선언에 추가해야 하는 메서드가 몇 개 더 남아 있습니다.

```CSharp
public void Update (float elapsedTime)
{
  this.x += this.dX * elapsedTime;
  this.y += this.dY * elapsedTime;
  this.angle += this.dA * elapsedTime;
}

public void Draw (SpriteBatch spriteBatch)
{
  Vector2 spritePosition = new Vector2(this.x, this.y);
  spriteBatch.Draw(texture, spritePosition, null, Color.White, this.angle, new Vector2(texture.Width/2, texture.Height/2), new Vector2(scale, scale), SpriteEffects.None, 0f);
}
```

**Update** SpriteClass 메서드는 Game1.cs의 **Update** 메서드에서 호출되며 각 스프라이트의 변화 속도를 기반으로 스프라이트 **x**, **y** 및 **angle** 값을 업데이트하는 데 사용됩니다.

**Draw** 메서드는 Game1.cs의 **Draw** 메서드에서 호출되며 게임 창에 스프라이트를 그리는 데 사용됩니다.

## <a name="user-input-and-animation"></a>사용자 입력 및 애니메이션
앞에서 만든 SpriteClass를 사용하여 새로운 게임 개체를 두 개 만들겠습니다. 첫 번째 개체는 플레이어가 화살표 키와 스페이스바로 제어할 수 있는 아바타입니다. 두 번째는 플레이어가 피해야 하는 개체입니다.

### <a name="1-get-the-textures"></a>1. 텍스처 가져오기
플레이어 아바타로는 믿음직한 티라노사우루스를 타고 있는 Microsoft 고유의 닌자 고양이를 사용하겠습니다. [이미지를 다운로드하려면 여기를 클릭하세요](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/ninja-cat-dino.png).

이번에는 플레이어가 피해야 할 장애물을 만들 차례입니다. 닌자 고양이와 육식 공룡이 가장 싫어하는 것은 무엇일까요? 바로 야채입니다! [이미지를 다운로드하려면 여기를 클릭하세요](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/broccoli.png).

앞에서 본 녹색 사각형과 마찬가지로, **MonoGame 파이프라인**를 통해 이러한 이미지를 **Content.mgcb**에 추가하고, 각각 이름을 “ninja-cat-dino.png”와 “broccoli.png”로 지정합니다.

### <a name="2-add-class-variables"></a>2. 클래스 변수 추가
다음 코드를 **Game1.cs**의 클래스 변수 목록에 추가합니다.

```CSharp
SpriteClass dino;
SpriteClass broccoli;

bool spaceDown;
bool gameStarted;

float broccoliSpeedMultiplier;
float gravitySpeed;
float dinoSpeedX;
float dinoJumpY;
float score;

Random random;
```

**dino** 및 **broccoli**는 SpriteClass 변수입니다. **dino**는 플레이어 아바타를 붙들고 있고 **broccoli**는 브로콜리 장애물을 붙들고 있습니다.

**spaceDown**은 스페이스바를 눌렀다 떼는 것이 아니라 계속 누르고 있는지 여부를 추적합니다.

**gameStarted**는 사용자가 게임을 처음으로 시작했는지 여부를 알려줍니다.

**broccoliSpeedMultiplier**는 화면에서 브로콜리 장애물이 움직이는 속도를 결정합니다.

**gravitySpeed**는 플레이어 아바타가 점프한 후 아래로 떨어지는 가속도를 결정합니다.

**dinoSpeedX** 및 **dinoJumpY**는 플레이어 아바타가 이동하고 점프하는 속도를 결정합니다.
score는 플레이어가 피한 장애물 수를 추적합니다.

마지막으로 **random**은 브로콜리 장애물의 동작에 임의성을 추가하는 데 사용됩니다.

### <a name="3-initialize-variables"></a>3. 변수 초기화
다음으로 이러한 변수를 초기화해야 합니다. Initialize 메서드에 다음 코드를 추가합니다.

```CSharp
broccoliSpeedMultiplier = 0.5f;
spaceDown = false;
gameStarted = false;
score = 0;
random = new Random();
dinoSpeedX = ScaleToHighDPI(1000f);
dinoJumpY = ScaleToHighDPI(-1200f);
gravitySpeed = ScaleToHighDPI(30f);
```

마지막 변수 세 개는 픽셀 변화 속도를 지정하므로 고 DPI 디바이스에 맞게 조정해야 합니다.

### <a name="4-construct-spriteclasses"></a>4. SpriteClasses 생성
**LoadContent** 메서드에서 SpriteClass 개체를 만들겠습니다. 현재 갖고 있는 항목에 다음 코드를 추가합니다.

```CSharp
dino = new SpriteClass(GraphicsDevice, "Content/ninja-cat-dino.png", ScaleToHighDPI(1f));
broccoli = new SpriteClass(GraphicsDevice, "Content/broccoli.png", ScaleToHighDPI(0.2f));
```

브로콜리 이미지는 우리가 화면에 표시하려는 크기보다 훨씬 더 큽니다. 따라서 원래 크기의 0.2배로 줄이겠습니다.

### <a name="5-program-obstacle-behaviour"></a>5. 장애물 동작 프로그래밍
화면 밖 어딘가에서 브로콜리가 생성되어 플레이어 아바타가 있는 방향으로 이동하면 플레이어가 브로콜리를 피하도록 해야 합니다. 이 동작을 구현하기 위해 **Game1.cs** 클래스에 다음 메서드를 추가합니다.

```CSharp
public void SpawnBroccoli()
{
  int direction = random.Next(1, 5);
  switch (direction)
  {
    case 1:
      broccoli.x = -100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 2:
      broccoli.y = -100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
    case 3:
      broccoli.x = screenWidth + 100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 4:
      broccoli.y = screenHeight + 100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
  }

  if (score % 5 == 0) broccoliSpeedMultiplier += 0.2f;

  broccoli.dX = (dino.x - broccoli.x) * broccoliSpeedMultiplier;
  broccoli.dY = (dino.y - broccoli.y) * broccoliSpeedMultiplier;
  broccoli.dA = 7f;
}
```

이 메서드의 첫 번째 부분은 임의의 숫자 2개를 사용하여 화면 밖에서 브로콜리 개체가 생성되는 지점을 결정합니다.

두 번째 부분은 브로콜리의 이동 속도를 결정하며, 이동 속도는 현재 점수에 따라 결정됩니다. 플레이어가 브로콜리 5개를 피할 때마다 속도가 빨라집니다.

세 번째 부분은 브로콜리 스프라이트의 이동 방향을 설정합니다. 생성된 브로콜리는 플레이어 아바타(dino)가 있는 방향으로 이동합니다. 또한 브로콜리가 플레이어를 추격할 때 브로콜리가 공중에서 회전하도록 **dA** 값을 7f로 지정합니다.

### <a name="6-program-game-starting-state"></a>6. 게임 시작 상태 프로그래밍
키보드 입력을 처리하려면 앞에서 만든 두 개체의 초기 게임 상태를 설정하는 메서드가 필요합니다. 우리가 원하는 것은 앱이 실행되는 즉시 게임이 시작되는 것이 아니라 사용자가 스페이스바를 눌러 수동으로 게임을 시작하게 만드는 것입니다. 애니메이션된 개체의 초기 상태를 설정하고 점수를 다시 설정하는 다음 코드를 추가합니다.

```CSharp
public void StartGame()
{
  dino.x = screenWidth / 2;
  dino.y = screenHeight * SKYRATIO;
  broccoliSpeedMultiplier = 0.5f;
  SpawnBroccoli();  
  score = 0;
}
```

### <a name="7-handle-keyboard-input"></a>7. 키보드 입력 처리
다음으로 키보드를 통해 사용자 입력을 처리하는 새 메서드가 필요합니다. 다음 메서드를 **Game1.cs**에 추가합니다.

```CSharp
void KeyboardHandler()
{
  KeyboardState state = Keyboard.GetState();

  // Quit the game if Escape is pressed.
  if (state.IsKeyDown(Keys.Escape))
  {
    Exit();
  }

  // Start the game if Space is pressed.
  if (!gameStarted)
  {
    if (state.IsKeyDown(Keys.Space))
    {
      StartGame();
      gameStarted = true;
      spaceDown = true;
      gameOver = false;
    }
    return;
  }            
  // Jump if Space is pressed
  if (state.IsKeyDown(Keys.Space) || state.IsKeyDown(Keys.Up))
  {
    // Jump if the Space is pressed but not held and the dino is on the floor
    if (!spaceDown && dino.y >= screenHeight * SKYRATIO - 1) dino.dY = dinoJumpY;

    spaceDown = true;
  }
  else spaceDown = false;

  // Handle left and right
  if (state.IsKeyDown(Keys.Left)) dino.dX = dinoSpeedX * -1;

  else if (state.IsKeyDown(Keys.Right)) dino.dX = dinoSpeedX;
  else dino.dX = 0;
}
```

위에서 4개의 if 문이 사용되었습니다.

첫 번째는 **Escape** 키를 누르면 게임을 종료하는 문입니다.

두 번째는 게임이 시작되지 않은 상태에서 **스페이스바**를 누르면 게임을 시작하는 문입니다.

세 번째는 **스페이스바**를 누르면 **dY** 속성을 변경하여 공룡 아바타가 점프하게 만드는 문입니다. 플레이어는 발이 “땅”(dino.y = screenHeight * SKYRATIO)에 붙어 있지 않으면 점프할 수 없으며, 스페이스바를 한 번 누르는 대신 계속 누르고 있는 경우에도 점프하지 않습니다. 이렇게 하면 게임이 시작되자마자 공룡이 점프하는 것을 방지하고, 게임을 시작하는 동일한 키 동작에 공룡 등에 올라탑니다.

마지막으로 if/else 절은 왼쪽 또는 오른쪽 화살표 키가 눌렸는지 검사하고, 눌렸으면 그에 따라 공룡의 **dX** 속성을 변경합니다.

**과제:** 위에 나온 키보드 처리 메서드가 화살표 키 뿐만 아니라 WASD 입력 스키마도 처리하게 만들 수 있을까요?

### <a name="8-add-logic-to-the-update-method"></a>8. Update 메서드에 논리 추가
다음으로 이 모든 부분에 대한 논리를 **Game1.cs**의 **Update** 메서드에 추가해야 합니다.

```CSharp
float elapsedTime = (float)gameTime.ElapsedGameTime.TotalSeconds;
KeyboardHandler(); // Handle keyboard input
// Update animated SpriteClass objects based on their current rates of change
dino.Update(elapsedTime);
broccoli.Update(elapsedTime);

// Accelerate the dino downward each frame to simulate gravity.
dino.dY += gravitySpeed;

// Set game floor so the player does not fall through it
if (dino.y > screenHeight * SKYRATIO)
{
  dino.dY = 0;
  dino.y = screenHeight * SKYRATIO;
}

// Set game edges to prevent the player from moving offscreen
if (dino.x > screenWidth - dino.texture.Width/2)
{
  dino.x = screenWidth - dino.texture.Width/2;
  dino.dX = 0;
}
if (dino.x < 0 + dino.texture.Width/2)
{
  dino.x = 0 + dino.texture.Width/2;
  dino.dX = 0;
}

// If the broccoli goes offscreen, spawn a new one and iterate the score
if (broccoli.y > screenHeight+100 || broccoli.y < -100 || broccoli.x > screenWidth+100 || broccoli.x < -100)
{
  SpawnBroccoli();
  score++;
}
```

### <a name="9-draw-spriteclass-objects"></a>9. SpriteClass 개체 그리기
마지막으로 **Game1.cs**의 **Draw** 메서드에서 마지막 **spriteBatch.Draw** 호출의 바로 뒤에 다음 코드를 추가합니다.

```CSharp
broccoli.Draw(spriteBatch);
dino.Draw(spriteBatch);
```

MonoGame에서 새 **spriteBatch.Draw** 호출은 이전 호출 위에 그립니다. 다시 말해서 브로콜리 및 공룡 스프라이트는 기존의 풀밭 스프라이트 위에 그려지기 때문에 위치에 관계없이 풀밭 뒤에 숨길 수 없습니다.

이제 게임을 실행하고 화살표 키와 스페이스바로 공룡을 움직여 보세요. 위의 단계를 잘 따라 하셨다면 게임 창 내에서 아바타를 움직일 수 있을 것이며 브로콜리의 속도가 점점 빨라져야 합니다.

![플레이어 아바타 및 장애물](images/monogame-tutorial-2.png)

## <a name="render-text-with-spritefont"></a>SpriteFont로 텍스트 렌더링
위의 코드를 사용하여 플레이어의 점수를 백그라운드에서 추적하지만 플레이어에게 그 점수가 표시되지 않습니다. 또한 앱이 시작될 때 표시되는 화면이 전혀 직관적이지 않습니다. 파란색과 녹색으로 된 창 하나가 표시되기는 하지만 게임을 시작하려면 스페이스바를 눌러야 한다는 사실을 플레이어가 알 수 있는 방법이 전혀 없습니다.

이러한 문제를 해결하기 위해 **SpriteFonts**라고 하는 새로운 종류의 MonoGame 개체를 사용하겠습니다.

### <a name="1-create-spritefont-description-files"></a>1. SpriteFont 설명 파일 만들기
**솔루션 탐색기**에서 **Content** 폴더를 찾습니다. 이 폴더에서 **Content.mgcb** 파일을 마우스 오른쪽 단추로 클릭하고 **연결 프로그램**을 선택합니다. 팝업 메뉴에서 **MonoGame 파이프라인**을 선택한 다음, **확인**을 누릅니다. 새 창에서 **콘텐츠** 항목을 마우스 오른쪽 단추로 클릭하고 **추가 -> 새 항목**을 선택합니다. **SpriteFont 설명**을 선택하고 이름을 “점수”로 지정한 다음, **확인**을 누릅니다. 그런 다음 동일한 방법으로 “게임상태”라고 하는 또 다른 SpriteFont 설명을 추가합니다.

### <a name="2-edit-descriptions"></a>2. 설명 편집
**MonoGame 파이프라인**에서 **Content** 폴더를 마우스 오른쪽 단추로 클릭하고 **파일 위치 열기**를 선택합니다. 지금까지 Content 폴더에 추가한 이미지 외에도 방금 만든 SpriteFont 설명 파일이 포함된 폴더가 보일 것입니다. 이제 MonoGame 파이프라인 창을 닫고 저장할 수 있습니다. **파일 탐색기**에서 두 설명 파일을 텍스트 편집기(Visual Studio, NotePad++, Atom 등)로 엽니다.

각 설명 파일에는 SpriteFont를 설명하는 여러 값이 포함되어 있습니다. 그 중에서 몇 가지를 변경하겠습니다.

**Score.spritefont**에서 **<Size>** 값을 12에서 36으로 변경합니다.

**GameState.spritefont**에서 **<Size>** 값을 12에서 72로 변경하고 **<FontName>** 값을 굴림에서 Agency로 변경합니다. Agency는 Windows 10 컴퓨터에 기본적으로 제공되는 또 다른 글꼴이며 시작 화면을 좀 더 멋지게 꾸미는 데 도움이 될 것입니다.

### <a name="3-load-spritefonts"></a>3. SpriteFonts 로드
Visual Studio로 돌아가서, 먼저 시작 화면의 새 텍스처를 추가하겠습니다. [이미지를 다운로드하려면 여기를 클릭하세요](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/start-splash.png).

이전과 마찬가지로 콘텐츠를 마우스 오른쪽 단추로 클릭하고 **추가 -> 기존 항목**을 선택하여 프로젝트에 텍스처를 추가합니다. 새 항목의 이름을 “start-splash.png”로 지정합니다.

그리고 **Game1.cs**에 다음 클래스 변수를 추가합니다.

```CSharp
Texture2D startGameSplash;
SpriteFont scoreFont;
SpriteFont stateFont;
```

그런 후 **LoadContent** 메서드에 다음 줄을 추가합니다.

```CSharp
startGameSplash = Content.Load<Texture2D>("start-splash");
scoreFont = Content.Load<SpriteFont>("Score");
stateFont = Content.Load<SpriteFont>("GameState");
```

### <a name="4-draw-the-score"></a>4. 점수 그리기
**Game1.cs**의 **Draw** 메서드로 이동하여 **spriteBatch.End();** 바로 앞에 다음 코드를 추가합니다.

```CSharp
spriteBatch.DrawString(scoreFont, score.ToString(),
new Vector2(screenWidth - 100, 50), Color.Black);
```

위의 코드는 우리가 앞에서 만든 스프라이트 설명(Arial Size 36)을 사용하여 화면 오른쪽 위에 플레이어의 현재 점수를 그립니다.

### <a name="5-draw-horizontally-centered-text"></a>5. 가로 방향으로 가운데 맞춤된 텍스트 그리기
게임을 만들 때 가로 또는 세로 방향으로 가운데 맞춤된 텍스트를 그려야 하는 경우가 많이 있을 것입니다. 소개 텍스트를 가로 방향으로 가운데에 맞추려면 **Draw** 메서드에서 **spriteBatch.End();** 바로 앞에 다음 코드를 추가합니다.

```CSharp
if (!gameStarted)
{
  // Fill the screen with black before the game starts
  spriteBatch.Draw(startGameSplash, new Rectangle(0, 0,
  (int)screenWidth, (int)screenHeight), Color.White);

  String title = "VEGGIE JUMP";
  String pressSpace = "Press Space to start";

  // Measure the size of text in the given font
  Vector2 titleSize = stateFont.MeasureString(title);
  Vector2 pressSpaceSize = stateFont.MeasureString(pressSpace);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, title,
  new Vector2(screenWidth / 2 - titleSize.X / 2, screenHeight / 3),
  Color.ForestGreen);
  spriteBatch.DrawString(stateFont, pressSpace,
  new Vector2(screenWidth / 2 - pressSpaceSize.X / 2,
  screenHeight / 2), Color.White);
  }
```

먼저 문자열을 두 개 만듭니다. 그 중 하나는 우리가 그리려고 하는 각 텍스트 줄에 대한 것입니다. 다음으로 **SpriteFont.MeasureString(String)** 메서드를 사용하여 각 줄을 인쇄했을 때의 너비와 높이를 측정합니다. 그러면 **Vector2** 개체의 크기, 너비가 포함된 **X** 속성, 높이가 포함된 **Y** 속성을 구할 수 있습니다.

마지막으로 각 줄을 그립니다. 텍스트를 가로 방향으로 가운데에 맞추기 위해 위치 벡터의 **X** 값을 **screenWidth / 2 - textSize.X / 2**로 맞춥니다.

**과제:** 텍스트를 가로 방향뿐 아니라 세로 방향으로도 가운데에 맞추려면 위의 절차를 어떻게 변경해야 할까요?

게임을 실행해 보세요. 시작 화면이 보이나요? 브로콜리가 생성될 때마다 점수가 올라가나요?

![시작 화면](images/monogame-tutorial-3.png)

## <a name="collision-detection"></a>충돌 감지
플레이어 주변을 따라다니는 브로콜리와 브로콜리가 새로 생성될 때마다 올라가는 점수를 만들었습니다. 하지만 현재는 이 게임이 끝나는 조건이 없습니다. 공룡 스프라이트와 브로콜리 스프라이트의 충돌 여부를 확인하는 방법 그리고 두 스프라이트가 충돌할 경우 게임 종료를 선언하는 방법을 알아야 합니다.

### <a name="1-get-the-textures"></a>1. 텍스처 가져오기
필요한 마지막 이미지는 "게임 종료"에 대한 것입니다. [이미지를 다운로드하려면 여기를 클릭하세요](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/game-over.png).

앞에서 본 녹색 사각형, 닌자 고양이 및 브로콜리 이미지와 마찬가지로, **MonoGame 파이프라인**을 통해 이러한 이미지를 **Content.mgcb**에 추가하고, 이름을 “game-over.png”로 지정합니다.

### <a name="2-rectangular-collision"></a>2. 사각형 충돌
게임에서 충돌을 감지할 때 종종 관련 수식의 복잡성을 줄이기 위해 개체를 간소화하곤 합니다. 충돌을 감지하기 위해 플레이어 아바타와 브로콜리 장애물을 사각형으로 취급하겠습니다.

**SpriteClass.cs**를 열고 새 클래스 변수를 추가합니다.

```CSharp
const float HITBOXSCALE = .5f;
```

이 값은 플레이어에 대한 충돌 감지의 "엄격성"을 나타냅니다. 이 값이 .5f이면 공룡이 브로콜리와 충돌해도 아무 문제 없는 사각형 가장자리(“히트박스”라고도 함)는 텍스처 전체 크기의 절반입니다. 이렇게 하면 두 텍스처의 모서리가 충돌하는 인스턴스는 거의 없으며, 이미지의 어떤 부분도 맞닿은 것처럼 보이지 않습니다. 각자 취향에 따라 이 값을 자유롭게 조정하시면 됩니다.

다음으로 **SpriteClass.cs**에 새 메서드를 추가합니다.

```CSharp
public bool RectangleCollision(SpriteClass otherSprite)
{
  if (this.x + this.texture.Width * this.scale * HITBOXSCALE / 2 < otherSprite.x - otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y + this.texture.Height * this.scale * HITBOXSCALE / 2 < otherSprite.y - otherSprite.texture.Height * otherSprite.scale / 2) return false;
  if (this.x - this.texture.Width * this.scale * HITBOXSCALE / 2 > otherSprite.x + otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y - this.texture.Height * this.scale * HITBOXSCALE / 2 > otherSprite.y + otherSprite.texture.Height * otherSprite.scale / 2) return false;
  return true;
}
```

이 메서드는 두 사각형 개체의 충돌을 감지합니다. 이 알고리즘은 사각형의 측면 사이에 틈이 있는지 테스트하는 방식으로 작동합니다. 틈이 있으면 충돌하지 않은 것이고 틈이 없으면 충돌한 것입니다.

### <a name="3-load-new-textures"></a>3. 새 텍스처 로드

그런 다음 **Game1.cs**를 열고 새로운 클래스 변수 두 개를 추가합니다. 하나는 게임 종료 스프라이트 텍스처를 저장하는 변수이고, 다른 하나는 게임 상태를 추적하는 부울입니다.

```CSharp
Texture2D gameOverTexture;
bool gameOver;
```

그런 다음, **Initialize** 메서드에서 **gameOver**를 초기화합니다.

```CSharp
gameOver = false;
```

마지막으로 텍스처를 **LoadContent** 메서드의 **gameOverTexture**에 로드합니다.

```CSharp
gameOverTexture = Content.Load<Texture2D>("game-over");
```

### <a name="4-implement-game-over-logic"></a>4. “게임 종료” 논리 구현
**Update** 메서드에서 **KeyboardHandler** 메서드가 호출되는 위치 바로 뒤에 다음 코드를 추가합니다.

```CSharp
if (gameOver)
{
  dino.dX = 0;
  dino.dY = 0;
  broccoli.dX = 0;
  broccoli.dY = 0;
  broccoli.dA = 0;
}
```

이렇게 하면 게임 종료 시 모든 동작이 멈추고 공룡과 브로콜리 스프라이트는 현재 위치에 고정됩니다.

다음으로 **Update** 메서드 끝부분에서 **base.Update(gameTime)** 바로 앞에 다음 줄을 추가합니다.

```CSharp
if (dino.RectangleCollision(broccoli)) gameOver = true;
```

이렇게 하면 우리가 **SpriteClass**에 만든 **RectangleCollision** 메서드가 호출되고, 만약 true 값이 반환되면 게임이 끝난 것으로 플래그가 지정됩니다.

### <a name="5-add-user-input-for-resetting-the-game"></a>5. 게임 초기화를 위한 사용자 입력 추가
사용자가 Enter 키를 눌러 게임을 초기화할 수 있도록 다음 코드를 **KeyboardHandler** 메서드에 추가합니다.

```CSharp
if (gameOver && state.IsKeyDown(Keys.Enter))
{
  StartGame();
  gameOver = false;
}
```

### <a name="6-draw-game-over-splash-and-text"></a>6. 게임 종료 화면 및 텍스트 그리기
마지막으로 Draw 메서드에서 **spriteBatch.Draw**가 처음으로 호출되는 위치 바로 뒤에 다음 코드를 추가합니다(풀밭 텍스처를 그리는 호출이어야 함).

```CSharp
if (gameOver)
{
  // Draw game over texture
  spriteBatch.Draw(gameOverTexture, new Vector2(screenWidth / 2 - gameOverTexture.Width / 2, screenHeight / 4 - gameOverTexture.Width / 2), Color.White);

  String pressEnter = "Press Enter to restart!";

  // Measure the size of text in the given font
  Vector2 pressEnterSize = stateFont.MeasureString(pressEnter);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, pressEnter, new Vector2(screenWidth / 2 - pressEnterSize.X / 2, screenHeight - 200), Color.White);
}
```

이전과 똑같은 메서드를 사용하여 가로 방향으로 가운데 맞춤된 텍스트를 그리고(시작 화면에 사용한 글꼴을 재사용), **gameOverTexture**를 창의 위쪽 절반 가운데에 맞춥니다.

모든 작업이 끝났습니다! 게임을 다시 실행해 보세요. 위의 단계를 잘 따라 하셨다면 이제 공룡이 브로콜리와 충돌하면 게임이 종료되고 게임을 다시 시작하려면 Enter 키를 누르라는 메시지가 표시될 것입니다.

![게임 종료](images/monogame-tutorial-4.png)

## <a name="publish-to-the-microsoft-store"></a>Microsoft Store에 게시
이 게임은 UWP 앱입니다. 따라서 이 프로젝트를 Microsoft Store에 게시할 수 있습니다. 몇 가지 단계를 처리해야 합니다.

Windows 개발자로 [등록](https://developer.microsoft.com/en-us/store/register)해야 합니다.

[앱 제출 검사 목록](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions)을 사용해야 합니다.

앱을 제출하여 [인증](https://docs.microsoft.com/en-us/windows/uwp/publish/the-app-certification-process)을 받아야 합니다.

자세한 내용은 [UWP 앱 게시](https://docs.microsoft.com/windows/uwp/publish/)를 참조하세요.
