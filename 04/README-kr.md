## 셰이더 실행하기

이 책을 쓰면서 그리고 스스로 연습을 위해 나는 셰이더를 만들고 표시하고 공유하고 전시할 수 있는 툴 생태계를 만들었다. 이 툴들은 리눅스 데스크탑, 맥OS, [라즈베리 파이](https://www.raspberrypi.org/), 브라우저들에서 일관되게 작동하는 만큼 환경에 따라 코드를 수정할 필요가 없다.

**표시하기**: 이 책에 실린 모든 예시는  [glslCanvas](https://github.com/patriciogonzalezvivo/glslCanvas)를 사용해 표시된다. glslCanvas는 독립된 셰이더 제작 과정을 아주 손쉽게 해준다.

```html
<canvas class="glslCanvas" data-fragment-url=“yourShader.frag" data-textures=“yourInputImage.png” width="500" height="500"></canvas>
```

보다시피 `canvas` 요소에 `class="glslCanvas"` 속성을 지정하고 `data-fragment-url`에 셰이더 URL을 넣어주기만 하면 된다. [여기](https://github.com/patriciogonzalezvivo/glslCanvas)에서 자세한 사용법을 알 수 있다.

나처럼 콘솔 창에서 직접 셰이더를 실행하고 싶은 사람이라면 [glslViewer](https://github.com/patriciogonzalezvivo/glslViewer)를 확인해보자. 이 애플리케이션은 [ImageMagick](http://www.imagemagick.org/script/index.php)이 이미지를 다루는 것과 비슷하게 셰이더를 배시 (`bash`) 스크립트나 유닉스 파이프라인으로 셰이더를 쓸 수 있게 해준다. 또 [glslViewer](https://github.com/patriciogonzalezvivo/glslViewer)은 [라즈베리 파이](https://www.raspberrypi.org/)에서 세이더를 컴파일하는 데 편리해서 [openFrame.io](http://openframe.io/)가 셰이더 아트워크를 전시하는 데 사용한다. [여기](https://github.com/patriciogonzalezvivo/glslViewer)에서 이 애플리케이션에 대해 더 알아볼 수 있다.

```bash
glslViewer yourShader.frag yourInputImage.png —w 500 -h 500 -s 1 -o yourOutputImage.png
```

**제작하기**: 셰이더 코딩 경험을 즐겁게 하고 싶은 생각에 나는 [glslEditor](https://github.com/patriciogonzalezvivo/glslEditor)라는 온라인 에디터를 만들었다. GLSL 코드를 다루는 추상적인 작업을 더 와닿게 해주는 장치들로 이루어진 이 에디터는 이 책의 상호작용 예시에서 사용하고 있다. [editor.thebookofshaders.com/](http://editor.thebookofshaders.com/)에서 별도의 웹 애플리케이션으로 실행할 수도 있다. [여기](https://github.com/patriciogonzalezvivo/glslEditor)에서 이 애플리케이션에 대해 더 알아볼 수 있다.

![](glslEditor-01.gif)

[SublimeText](https://www.sublimetext.com/)를 사용한 오프라인 작업을 선호한다면 여기 [glslViewer 패키지](https://packagecontrol.io/packages/glslViewer)를 설치할 수 있다. [여기](https://github.com/patriciogonzalezvivo/sublime-glslViewer)에서 패키지에 대해 더 알아볼 수 있다. 

![](glslViewer.gif)

**공유하기**: 앞서 소개한 온라인 에디터([editor.thebookofshaders.com/](http://editor.thebookofshaders.com/))로 셰이더를 공유할 수 있다! 책에 삽입되 에디터와 별도 온라인 에디터 모두 내보내기 (export) 버튼으로 자기 셰이더 고유의 URL을 확인할 수 있다. 그리고 [openFrame.io](http://openframe.io/)로 직접 내보낼 수도 있다.

![](glslEditor-00.gif)

**전시하기**: 코드 공유는 셰이더 아트워크 공유의 시작일 뿐이다. [openFrame.io](http://openframe.io/)으로 내보내는 옵션 외에도 어떤 사이트에도 갤러리를 삽입할 수 있는 [glslGallery](https://github.com/patriciogonzalezvivo/glslGallery)를 만들었다. [여기](https://github.com/patriciogonzalezvivo/glslGallery)에서 자세히 알아볼 수 있다.

![](glslGallery.gif)

## 자주 쓰는 프레임워크에서 셰이더 실행하기

이미 [프로세싱](https://processing.org/)이나 [Three.js](http://threejs.org/), [OpenFrameworks](http://openframeworks.cc/) 같은 프레임워크로 프로그래밍해본 경험이 있다면 사용해온 플랫폼에서 셰이더를 시험해보고 싶어 손이 근질거릴 것이다. 몇 가지 인기있는 프레임워크에서 이 책에서 사용하는 것과 동일한 유니폼들을 가지고 세이더를 사용하는 방법을 알아보자. ([이 챕터의 GitHub 저장소](https://github.com/patriciogonzalezvivo/thebookofshaders/tree/master/04)에서 세 프레임워크의 소스 코드를 찾을 수 있다.)

### **Three.js**에서

똑똑하고 겸손한 리카르도 카벨로(또는 [MrDoob](https://twitter.com/mrdoob))가 다른 [기여자들](https://github.com/mrdoob/three.js/graphs/contributors)과 함께 개발한 [three.js](http://threejs.org/)는 아마 가장 유명한 WebGL 프레임워크 중 하나일 것이다. 이 자바스크립트 라이브러리로 멋진 3D 그래픽스를 만드는 방법을 가르쳐주는 예시와 튜토리얼, 책을 많이 찾아볼 수 있을 것이다.

아래는 three.js에서 셰이더를 사용하는 데 필요한 HTML와 자바스크립트의 예시다. `id="fragmentShader"` 부분을 주목하자. 이 책에 있는 셰이더 코드를 이 부분에 넣으면 된다.

```html
<body>
    <div id="container"></div>
    <script src="js/three.min.js"></script>
    <script id="vertexShader" type="x-shader/x-vertex">
        void main() {
            gl_Position = vec4( position, 1.0 );
        }
    </script>
    <script id="fragmentShader" type="x-shader/x-fragment">
        uniform vec2 u_resolution;
        uniform float u_time;

        void main() {
            vec2 st = gl_FragCoord.xy/u_resolution.xy;
            gl_FragColor=vec4(st.x,st.y,0.0,1.0);
        }
    </script>
    <script>
        var container;
        var camera, scene, renderer;
        var uniforms;

        init();
        animate();

        function init() {
            container = document.getElementById( 'container' );

            camera = new THREE.Camera();
            camera.position.z = 1;

            scene = new THREE.Scene();

            var geometry = new THREE.PlaneBufferGeometry( 2, 2 );

            uniforms = {
                u_time: { type: "f", value: 1.0 },
                u_resolution: { type: "v2", value: new THREE.Vector2() },
                u_mouse: { type: "v2", value: new THREE.Vector2() }
            };

            var material = new THREE.ShaderMaterial( {
                uniforms: uniforms,
                vertexShader: document.getElementById( 'vertexShader' ).textContent,
                fragmentShader: document.getElementById( 'fragmentShader' ).textContent
            } );

            var mesh = new THREE.Mesh( geometry, material );
            scene.add( mesh );

            renderer = new THREE.WebGLRenderer();
            renderer.setPixelRatio( window.devicePixelRatio );

            container.appendChild( renderer.domElement );

            onWindowResize();
            window.addEventListener( 'resize', onWindowResize, false );

            document.onmousemove = function(e){
              uniforms.u_mouse.value.x = e.pageX
              uniforms.u_mouse.value.y = e.pageY
            }
        }

        function onWindowResize( event ) {
            renderer.setSize( window.innerWidth, window.innerHeight );
            uniforms.u_resolution.value.x = renderer.domElement.width;
            uniforms.u_resolution.value.y = renderer.domElement.height;
        }

        function animate() {
            requestAnimationFrame( animate );
            render();
        }

        function render() {
            uniforms.u_time.value += 0.05;
            renderer.render( scene, camera );
        }
    </script>
</body>
```

### **프로세싱**에서

[벤 프라이](http://benfry.com/)와 [케이시 리스](http://reas.com/)가 2001년에 시작한 [Processing](https://processing.org/)은 처음 코딩을 시작하는 사람에게 있어 굉장히 간단하면서 강력한 환경을 제공한다. (적어도 내 경험은 그랬다.) [안드레스 콜루브리](https://codeanticode.wordpress.com/)가 프로세싱에 기여한 OpenGL과 비디오 업데이트 덕분에 이 환경에서 GLSL 셰이더를 가지고 놀고 사용하기 훨씬 편해졌다. 프로세싱은 스케치의 `data` 폴더에서 `"shader.frag"`라고 이름 붙여진 셰이더를 찾는다. 이 책의 예시를 그 폴더에 붙여넣어 파일 이름을 바꾸도록 하자.

```cpp
PShader shader;

void setup() {
  size(640, 360, P2D);
  noStroke();

  shader = loadShader("shader.frag");
}

void draw() {
  shader.set("u_resolution", float(width), float(height));
  shader.set("u_mouse", float(mouseX), float(mouseY));
  shader.set("u_time", millis() / 1000.0);
  shader(shader);
  rect(0,0,width,height);
}
```

2.1 이전 버전에서 셰이더를 작동시키려면 셰이더 시작 부분에 `#define PROCESSING_COLOR_SHADER`를 넣어야 한다. 다음과 같이:

```glsl
#ifdef GL_ES
precision mediump float;
#endif

#define PROCESSING_COLOR_SHADER

uniform vec2 u_resolution;
uniform vec3 u_mouse;
uniform float u_time;

void main() {
    vec2 st = gl_FragCoord.st/u_resolution;
    gl_FragColor = vec4(st.x,st.y,0.0,1.0);
}
```

프로세싱의 셰이더 구동을 더 자세히 알고 싶다면 이 [튜토리얼](https://processing.org/tutorials/pshader/)을 참고하자.

### **openFrameworks**에서

각자 편하게 느끼는 환경이 있겠지만 내 경우는 아직 [openFrameworks 커뮤니티](http://openframeworks.cc/)다. 이 C++ 프레임워크는 OpenGL과 다른 오픈 소스 C++ 라이브러리를 사용하는데, 여러 부분에서 프로세싱과 유사하지만 C++ 컴파일러에서 기인하는 명백한 복잡성도 있다. 프로세싱과 마찬가지로 openFrameworks도 `data` 폴더에서 셰이더 파일을 찾는다. 예시를 `shader.frag`로 복사하자.

```cpp
void ofApp::draw(){
    ofShader shader;
    shader.load("","shader.frag");

    shader.begin();
    shader.setUniform1f("u_time", ofGetElapsedTimef());
    shader.setUniform2f("u_resolution", ofGetWidth(), ofGetHeight());
    ofRect(0,0,ofGetWidth(), ofGetHeight());
    shader.end();
}
```

openFrameworks의 셰이더 사용에 대한 더 자세히 알고 싶다면 [조슈아 노블](http://thefactoryfactory.com/)이 만든 이 훌륭한 튜토리얼을 보자.
