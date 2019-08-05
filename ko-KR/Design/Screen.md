# 화면

클라이어언트의 디스플레이 장치를 통해 화면에 다음과 같은 UI 항목을 제공해야 합니다.

* [부팅 화면](#BootingScreen)
* [로고 표시](#DisplayingLogo)
* [Green Dot VUI](#GreenDotVUI)

## 부팅 화면 {#BootingScreen}

기기를 켠 후 부팅이 완료될 때까지 표시되는 화면입니다. 부팅 화면은 주로 로고가 표시되며 Clova의 로고를 다른 로고와 조합하지 않고 단독으로 화면에 표시합니다. 또한, 같은 크기와 비율로 로고를 표시해야 합니다.

![](/Design/Assets/Images/Clova-Client-Partner_Logo_on_Loading_Screen.png) ![](/Design/Assets/Images/Clova-Client-Clova_Logo_on_Loading_Screen.png)

## 로고 표시 {#DisplayingLogo}

화면 레이아웃에 따라 Clova 로고를 배치하는 방법에 대해서 설명합니다.

* [레이아웃 A](#LayoutA)
* [레이아웃 B](#LayoutB)
* [레이아웃 C](#LayoutC)

### 레이아웃 A {#LayoutA}

화면 하단의 일부나 전체를 덮는 UI 화면으로 Clova 로고가 좌측 상단에 배치되는 레이아웃입니다.

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_A-Bottom_Overlay.png) ![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_A-Full_Screen_Overlay.png)

* Clova 로고는 좌측 상단에 배치되어야 합니다.
* Clova 로고를 투명하게 만들지 않아야 합니다.

### 레이아웃 B {#LayoutB}

[레이아웃 A](#LayoutA)와 비슷하게 화면 하단의 일부나 전체를 덮는 UI 화면으로 Clova 로고가 우측 상단에 배치되는 레이아웃입니다.

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_B-Bottom_Overlay.png) ![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_B-Full_Screen_Overlay.png)

* Clova 로고는 우측 상단에 배치되어야 합니다.
* Clova 로고를 투명하게 만들지 않아야 합니다.

### 레이아웃 C {#LayoutC}

단순 텍스트 형태의 결과를 표현하는 화면에서 Clova 로고가 상단에 배치되는 레이아웃입니다.

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_C.png)

* Clova 로고는 상단에 배치되어야 합니다.
* 콘텐츠의 출처는 콘텐츠 하단에 표시해야 합니다.


## Green Dot VUI {#GreenDotVUI}

Green Dot VUI는 Clova의 음성 검색 도구이자 정체성을 나타내는 UI 디자인입니다. Green Dot VUI는 사용자의 음성 입력 수신, Clova 음성 출력 등 Clova 음성 동작과 관련된 상태를 표현합니다.

화면을 가진 클라이언트 기기는 Green Dot VUI를 표현해야 합니다. Green Dot VUI에서 사용하는 색상이 무엇이고 클라이언트의 상태에 따라 어떻게 표현되어야 하는지 설명합니다.

* [Green Dot VUI 색상](#GreenDotVUIColor)
* [Green Dot VUI 표현](#GreenDotVUIMotions)

### Green Dot VUI 색상 {#GreenDotVUIColor}

Green Dot VUI는 다음과 같이 도넛 형태의 고리 모양에 3 가지 색상 변화(gradient)를 주어 표시해야 합니다.

![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Color.png)

위 그림에서 사용된 3 가지 색의 정보는 다음과 같습니다.

| 색상 이름        | RGB 값       | CMYK 값     | Pantone 값   |
|----------------|-------------|-------------|-------------|
| Greendot Green | <span style="color:#03EB64; font-size:150%; vertical-align:middle;">&#9724;</span> 3, 235, 100(#03EB64) | 60, 0, 75, 0   | 2270C |
| Greendot Mint  | <span style="color:#14E6BE; font-size:150%; vertical-align:middle;">&#9724;</span>20, 230, 190(#14E6BE) | 50, 0, 25, 0   | 3255C |
| Greendot Blue  | <span style="color:#1EC8EB; font-size:150%; vertical-align:middle;">&#9724;</span>30, 200, 235(#1EC8EB) | 70, 5,  0, 0   |  298C |


### Green Dot VUI 동작 {#GreenDotVUIMotions}

Green Dot VUI의 UI 표현은 구분된 동작에 따라 달라집니다. 다음은 Green Dot VUI의 동작을 설명한 표입니다.

| 동작 이름        | 설명                           | UI 동작(Motion) |
|----------------|-------------------------------|---------------|
| 준비(Intro)          | 사용자가 호출어를 부르거나 버튼을 눌렀을 때 표시되는 동작         | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Intro.gif)      |
| 대기(Waiting)        | 사용자의 음성 입력이 실제로 입력되기 전까지 표시되는 동작         | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Waiting.gif)    |
| 입력(Speaking)      | 사용자의 음석 입력이 실제로 인지되어 입력되고 있을 때 표시되는 동작 | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Speaking.gif)    |
| 분석/처리(Processing) | 사용자의 요청을 분석하고 이를 처리하고 있을 때 표시되는 동작       | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Processing.gif) |
| 대답(Answering)      | 사용자의 요청에 대한 응답을 TTS로 출력하고 있을 때 표시되는 동작. 이 동작은 다음과 같이 세 구간으로 구분됩니다.<ul><li>진입 구간: 응답(TTS)이 시작됨을 표현(1~13 번 프레임)</li><li>반복 구간: TTS 출력을 표현(14~29 번 프레임). 응답이 완료될 때까지 이 구간을 반복 재생해야 합니다.</li><li>종료 구간: 응답이 종료됨을 표현(30~41 번 프레임)</li></ul>    | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Answering.gif)  |
| 완료(Complete)       | 사용자의 요청에 대한 응답을 마무리할 때 표시되는 동작            | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Complete.gif)  |
| 오류(Error)          | 오류가 발생한 상태                                       | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Error.gif)     |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Green Dot VUI 동작을 보여주는 속도는 30fps여야 합니다.</p>
</div>

사용자의 동작이나 [클라이언트의 상태](/Design/Client_State_And_Event.md) 변화에 따라 Green Dot VUI 동작을 표현해야 합니다. 다음은 각 상태에 따라 어떻게 Green Dot VUI를 표현해야 하는지 그 규칙을 설명합니다.

* 사용자가 호출어를 부르거나 버튼을 눌러 클라이언트가 **Attending** 모드로 진입하면 **준비** 동작을 재생합니다.
* **준비** 동작을 한 번 표시한 후 마이크를 통해 사용자의 실제 음성 입력이 시작되기 전까지 **대기** 동작을 반복 재생합니다.
* 사용자의 실제 음성이 입력되기 시작하여 클라이언트가 **Listening** 모드로 진입하면 사용자의 음성 입력이 끝날 때까지 **입력** 동작을 반복 재생합니다.
* 사용자의 입력이 끝나고 클라이언트가 **Processing & reporting** 상태로 진입하면 응답을 출력하거나 결과 화면을 보여주기 전까지 **분석/처리** 동작을 반복 재생합니다.
* 응답(TTS)를 출력할 때는 **대답**의 진입 구간을 한 번 재생한 후 반복 구간을 응답이 끝날 때까지 반복 재생합니다. 응답이 종료되면 종료 구간을 한 번 재생합니다.
* 만약, 응답을 출력할 때 **대답** 동작을 구분하여 표현하기 어렵다면 반복 구간만 반복 재생합니다.
* 사용자 요청에 대해 필요한 작업을 모두 수행하여 클라이언트가 **Idle** 상태로 진입하게 되면 **완료** 동작을 한 번 재생합니다.

Green Dot VUI가 동작하는 예시는 다음과 같습니다.

![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Example.gif)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Green Dot VUI 동작에 대한 이미지 자료를 구하려면 제휴 담당자에게 연락바랍니다.</p>
</div>
