# Introduction

<canvas id="custom" class="canvas" data-fragment-url="cmyk-halftone.frag" data-textures="vangogh.jpg" width="700px" height="320px"></canvas>

위 두 이미지는 다른 방법으로 만들어졌다. 첫 번째는 반 고흐가 손수 물감 위에 물감을 덧그리는 방식으로 만들어졌다. 작업에 제법 시간이 걸렸다. 두 번째는 사이언 행렬, 마젠타 행렬, 노란색 행렬, 검은색 행렬의 픽셀을 조합하는 방식으로 몇 초만에 제작되었다. 중요한 차이는 두 번째 이미지가 순차적이지 않은 (즉, 차례차례가 아니라 모든 것을 동시에 처리하는) 방식으로 만들어졌다는 점이다.

이 책은 디지털 이미지 생성을 새로운 단계로 이끄는 혁명적인 연산 기법인 *프래그먼트 셰이더*(fragment shader)를 다룬다. 그래픽스에 있어 구텐베르크 인쇄술과 같다고 생각할 수 있다.

![구텐베르크의 인쇄기](gutenpress.jpg)

프래그먼트 셰이더를 사용하면 굉장히 빠른 속도로 화면에 표시되는 픽셀들을 완전히 통제할 수 있다. 휴대전화의 비디오 필터부터 굉장한 3D 비디오 게임들까지 온갖 사례에 사용되는 것도 그런 이유다.

![댓게임컴퍼니의 저니](journey.jpg)

이어지는 챕터들에서 여러분은 이 기법의 굉장한 속도와 능력을 확인하고 자신의 업무나 개인 프로젝트에 응용할 수 있는 방법을 알게 된다.

## 누구를 위한 책인가?

이 책은 선형대수학과 삼각함수에 대한 기본 이해가 있고 자기 작업의 그래픽 퀄리티를 굉장하고 새로운 단계로 끌어올리고 싶은 창작 코더, 게임 개발자, 엔지니어들을 위해 쓰였다. (만약 코딩을 배우고 싶다면 [Processing](https://processing.org/)으로 시작하고 코딩에 익숙해지면 다시 돌아오길 바란다.)

이 책은 여러분에게 셰이더를 쓰는 방법과 자기 프로젝트에 적용해 퍼포먼스와 퀄리티를 높이는 방법을 알려준다. GLSL (OpenGL Shading Language, OpenGL 셰이딩 언어) 셰이더는 다양한 플랫폼에서 컴파일하고 실행할 수 있기 때문에 여기서 배운 것은 OpenGL이나 OpenGL ES, WeGL을 사용하는 모든 환경에 적용할 수 있다. 즉, 여기서 얻은 지식을 [Processing](https://processing.org/) 스케치, [openFrameworks](http://openframeworks.cc/) 애플리케이션, [Cinder](http://libcinder.org/) 설치미술, [Three.js](http://threejs.org/) 웹사이트, iOS/안드로이드 게임들에 적용할 수 있다.

## 어떤 내용을 다루나?

이 책은 GLSL 픽셀 셰이더 활용을 중심으로 한다. 먼저 셰이더가 무엇인지 알아보고, 그것으로 절차적인 형태와 패턴, 텍스처, 애니메이션을 만드는 방법을 배운다. 셰이딩 언어의 기초를 다지고 이미지 처리(이미지 연산, 행렬 회선, 블러, 색상 필터, 룩업 테이블, 기타 효과)와 시뮬레이션(콘웨이의 라이프 게임, 개리-스콧의 반응확산계, 물결, 보로노이 셀 등)처럼 유용한 사례에 적용해 본다. 책 막바지에는 레이 마칭을 바탕으로 한 고급 기술도 살펴본다.

*각 챕터마다 상호작용할 수 있는 예시들이 마련되어 있다.* 여러분이 예시의 코드를 변경하면 그 변화가 즉시 보이게 된다. 이 책에서 다루는 개념들은 추상적이고 난해할 수 있는 만큼 이렇게 상호작용하는 예시들이 배움에 큰 도움이 된다고 본다. 개념이 적용되는 모습을 더 빨리 확인할 수록 배우는 속도도 빨라진다.

이 책이 다루지 않는 부분:

* 이책은 openGL이나 webGL서적이 아니다. openGL/webGL은 GLSL이나 fragment shader보다 훨씬 더 큰 주제이다. 그런것들에 대해 좀더 싶히 공부하고 싶다면: [OpenGL Introduction](https://open.gl/introduction), [the 8th edition of the OpenGL Programming Guide](http://www.amazon.com/OpenGL-Programming-Guide-Official-Learning/dp/0321773039/ref=sr_1_1?s=books&ie=UTF8&qid=1424007417&sr=1-1&keywords=open+gl+programming+guide) (빨간책이라고도 알려진) 이나 [WebGL: Up and Running](http://www.amazon.com/WebGL-Up-Running-Tony-Parisi/dp/144932357X/ref=sr_1_4?s=books&ie=UTF8&qid=1425147254&sr=1-4&keywords=webgl) 등을 추천한다.

* 이 책은 OpenGL이나 WebGL 책이 아니다. OpenGL과 WebGL은 GLSL이나 프래그먼트 셰이더보다 방대한 주제다. OpenGL과 WebGL에 대해 더 배우고 싶다면 [OpenGL Introduction](https://open.gl/introduction)이나 (빨간 책이라는 별명이 있는) [the 8th edition of the OpenGL Programming Guide](http://www.amazon.com/OpenGL-Programming-Guide-Official-Learning/dp/0321773039/ref=sr_1_1?s=books&ie=UTF8&qid=1424007417&sr=1-1&keywords=open+gl+programming+guide), [WebGL: Up and Running](http://www.amazon.com/WebGL-Up-Running-Tony-Parisi/dp/144932357X/ref=sr_1_4?s=books&ie=UTF8&qid=1425147254&sr=1-4&keywords=webgl)을 추천한다.

* 이 책은 수학 책이 아니다. 대수학과 삼각함수에 대한 이해가 필요한 알고리즘과 기법을 다루긴 하지만 그 수학적 배경을 자세히 설명하지는 않는다. 수학에 관련해 의문이 든다면 [3rd Edition of Mathematics for 3D Game Programming and computer Graphics](http://www.amazon.com/Mathematics-Programming-Computer-Graphics-Third/dp/1435458869/ref=sr_1_1?ie=UTF8&qid=1424007839&sr=8-1&keywords=mathematics+for+games)이나 [2nd Edition of Essential Mathematics for Games and Interactive Applications](http://www.amazon.com/Essential-Mathematics-Games-Interactive-Applications/dp/0123742978/ref=sr_1_1?ie=UTF8&qid=1424007889&sr=8-1&keywords=essentials+mathematics+for+developers) 같은 책을 가까이 두길 권한다.

## 시작하기 전에 필요한 게 있나?

별로 없다! WebGL이 지원되는 현대 웹 브라우저(크롬, 파이어폭스, 사파리 등)와 인터넷 연결만 있다면 이 페이지 최하단의 "Next"를 눌러 시작하면 된다.

혹은 필요에 따라 다음과 같은 방법으로 이 책을 활용할 수도 있다:

- [이 책의 오프라인 버전 만들기](https://thebookofshaders.com/appendix/00/)

- [Raspberry Pi로 브라우저 없이 예시 돌려보기](https://thebookofshaders.com/appendix/01/)

- [이 책의 PDF 버전 만들기](https://thebookofshaders.com/appendix/02/)

- 문제가 있거나 코드를 공유하려면 이 책의 [GitHub 저장소](https://github.com/patriciogonzalezvivo/thebookofshaders)를 확인하자.
