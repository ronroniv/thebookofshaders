## Uniform

여기까지 우리는 GPU가 대량의 병렬 쓰레드를 관리하는 법, 각각의 쓰레드가 전체 이미지의 일부분에 색 배정을 담당한다는 점을 알아보았다. 각 병렬 쓰레드는 다른 쓰레드들이 하는 작업을 알 수 없다해도 CPU에서 온 입력들을 쓰레드들에 보낼 수는 있다. 그래픽 카드의 아키텍처 때문에 이 입력들은 모든 쓰레드에 대해 동일('uniform', 유니폼)하고 '읽기 전용'이어야 한다. 즉, 각 쓰레드는 읽을 수 있지만 변경할 수는 없는  동일한 데이터를 받는다.

이런 입력을 `uniform`이라고 하고, 대부분 데이터 타입(`float`, `vec2`, `vec3`, `vec4`, `mat2`, `mat3`, `mat4`, `sampler2D`, `samplerCube`)을 지원한다. 유니폼 변수들은 대응하는 타입과 함께 셰이더 상단, 기본 실수 정밀도 설정 직후에 정의한다.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution; // 캔버스 크기 (너비, 높이)
uniform vec2 u_mouse;      // 마우스 위치 (화면 픽셀)
uniform float u_time;	  // 로드 이후 지난 시간 (초)
```

유니폼을 CPU와 GPU 사이에 놓인 작은 다리라고 생각해보자. 구현에 따라 이름은 다를 수 있지만 이 책에서는 항상 `u_time` (셰이더 시작된 이후 지난 초 단위 시간), `u_resolution` (셰이더 그려지는 영역 크기), `u_mouse` (영역 속의 픽셀 단위 마우스 위치)라고 쓰기로 한다. 유니폼 변수명 앞에는 `u_`를 붙여 이 변수가 유니폼임을 명시하는 관습을 따랐다. 하지만 유니폼에 온갖 이름을 쓰는 모습을 볼 수 있을 것인데, 가령 [ShaderToy.com](https://www.shadertoy.com/)에서는 동일한 유니폼들에 다음과 같은 이름을 쓰고 있다:

```glsl
uniform vec3 iResolution;   // 뷰포트 해상도 (픽셀)
uniform vec4 iMouse;        // 마우스 위치 좌표. xy: 현재, zw: 클릭
uniform float iTime;        // 셰이더 실행 시간 (초)
```

설명은 이 정도로 하고, 유니폼이 실제로 쓰이는 모습을 보자. 다음 코드에서는 `u_time`(셰이더가 시작된 이후 초 단위로 지난 시간)을 사인 함수와 함께 사용해 영역 안에서 빨간색의 양이 변화하는 모습을 보여준다.

<div class="codeAndCanvas" data="time.frag"></div>

코드에서 GLSL의 재미있는 점을 하나 더 볼 수 있다. GPU에서는 각, 삼각, 지수함수가 하드웨어로 고속처리된다. 이와 같은 함수로는 [`sin()`](../glossary/?search=sin), [`cos()`](../glossary/?search=cos), [`tan()`](../glossary/?search=tan), [`asin()`](../glossary/?search=asin), [`acos()`](../glossary/?search=acos), [`atan()`](../glossary/?search=atan), [`pow()`](../glossary/?search=pow), [`exp()`](../glossary/?search=exp), [`log()`](../glossary/?search=log), [`sqrt()`](../glossary/?search=sqrt), [`abs()`](../glossary/?search=abs), [`sign()`](../glossary/?search=sign), [`floor()`](../glossary/?search=floor), [`ceil()`](../glossary/?search=ceil), [`fract()`](../glossary/?search=fract), [`mod()`](../glossary/?search=mod), [`min()`](../glossary/?search=min), [`max()`](../glossary/?search=max), [`clamp()`](../glossary/?search=clamp)가 있다.

다시 한 번 위 코드를 가지고 놀아보자.

* 인지하기 어려운 수준으로 색 변화 빈도를 줄여보자.

* 깜빡임 없이 한 가지 색상만 보일 정도로 속도를 높여보자.

* 세 개 채널(RGB)을 서로 다른 빈도로 바꾸어서 흥미로운 패턴을 만들어보자.

## gl_FragCoord

GLSL이 기본 출력으로 `vec4 gl_FragColor`를 주는 것처럼, 현재 활성화된 쓰레드가 담당하는 '픽셀', 즉 '프래그먼트'의 화면 좌표를 담은 기본 입력으로 `vec4 gl_FragCoord`가 존재한다. `vec4 gl_FragCoord`를 통해 어떤 쓰레드가 영역의 어디를 담당하고 있는지 알 수 있다. `gl_FragCoord`는 값이 쓰레드마다 다르기 때문에 `uniform` 변수라고 부르지 않고 가변('varying') 변수라고 부른다.

<div class="codeAndCanvas" data="space.frag"></div>

위 코드에서는 프래그먼트의 좌표를 영역의 해상도로 나누어 '정규화'했다. 이렇게 값은 `0.0`과 `1.0` 사이로 변환되어 X와 Y 값을 빨강과 녹색 채널에 매핑하기 쉬워진다.

셰이더 세상에서는 변수들에 강렬한 색상을 대입해 파악해보는 법 외에 별달리 디버깅 수단이 많지 않다. 때로는 GLSL 코딩이 병 속에 배를 넣는 일과 비슷하게 느껴질 것이다. 아름답고 만족스러운 만큼 어렵다.

![](08.png)

이제 여러가지 시도해보면서 지금까지 배운 것을 시험해보자.

* 위 캔버스에서 `(0.0, 0.0)`이 어디인지 말할 수 있는가?

* `(1.0, 0.0)`, `(0.0, 1.0)`, `(0.5, 0.5)`, `(1.0, 1.0)`은 어디인가?

* `u_mouse`는 그 값들이 정규화된 값이 아닌 픽셀인데 어떻게 사용해야 할지 알 수 있겠는가? 이 변수를 사용해 색들을 움직이는 코드를 만들 수 있는가?

* `u_time`과 `u_mouse`를 이용해 흥미로운 색상 패턴을 만들어볼 수 있는가?

이런 연습을 하고 나면 이런 셰이딩 기술을 시험해볼 수 있는 곳들이 또 있는지 궁금할 것이다. 다음 챕터에서는 three.js와 Processing, openFrameworks 같은 툴로 자기만의 셰이더를 만드는 방법을 알아본다.
