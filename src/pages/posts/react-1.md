리액트의 역사

리액트는 이전에는 jQuery, backbone.js, angular.js(앵귤러 버전 1을 이젠 angular.js라고 함)등을 사용하여 프론트엔드 개발을 진행함

jQuery

jQuery는 과거 크로스브라우징이슈로 인해 표준화 되어 있지 않은 웹에서 DOM조작을 간단하게 하고 추상화 하여 웹 구축을 할때 혁명을 일으킴! 아래와 같이 사용 가능

$( "button.continue" ).html( "Next Step..." )

그럼 비동기처리는?

$.ajax({
  url: "/api/getWeather",
  data: {
    zipcode: 97201
  },
  success: function( result ) {
    $( "#weather-temp" ).html( "<strong>" + result + "</strong> degrees" );
  }
});

하지만 아쉽게도 가장 큰 단점이 있었음 바로 mutable state를 통해 state가 변경될 여지가 있었던 것

jQuery에 필요한것은 좀더 구조가 잘 잡혀 서로 변경 가능한(mutable)상태(state)를 최소화 시키는것

그래서 backbone.js가 나옴

Backbone

Backbone 같은경우 MVC패턴을 통해 전통적인 소프트웨어구축을 하는 방법을 javascript에서도 할 수 있다는 방법론을 제시함

이를통해 으마으마한 인기가 생김

Backbone 같은 경우 DOM에 존재하는 대신 MVC에 M 인즉 모델 내부에 존재함

거기서부터 모델이 변경될 때마다 해당 모델의 state를 갖는 모든 view가 다시 랜더링 됨

그럼 Backbone이 MVC라는 형태의 방법론이 javascript에서 가능하다는걸 알려줬으면 그 이후는 뭔데?

Angular.js

가능한것부터 나열해보자

양방향 데이터 바인딩

데이터 필터

라우팅

컨트롤러

종속성 주입

템플릿

Angular.js같은 경우 프레임워크인데, npm install 한번으로 이 모든것이 가능해지고, 구조화가 가능해지다보니 인기는 미친듯이 치솟았음

뭔가 가능한것들이 Java의 그것과 비슷함(코프링? 스프링? 쨋든 안써봐서 잘 모름 비슷하긴함)

그럼 문제가 뭐냐? 양방향 바인딩을 너무나도 좋아함

그게 뭐가문제냐면 모델 ↔︎ 뷰 와 같은 생김새때문에 한쪽이 업데이트 되면 다른 한쪽도 업데이트가 발생함

이론적으로는 수동으로 DOM을 조작하지 않아도 되기 때문에 너무 좋았음

실제로는 모델만 디버깅하고있다가 view쪽에서 업데이트가 발생한다? 진짜 헬이었음

디버깅 일단 빡세고, 개발자 툴 따로 없고, 가장 큰 문제는 Angular.js가 모델이나 뷰쪽에서 state변경이 되는지 찾기위해 앱을 지속적으로 watch하고 있음(성능 개똥)

이건 작성자가 사용해봐서 진짜 헬임 ㅠㅠ 라이트 하우스라도 켜보는 날엔 진짜…

jQuery 기반 프레임워크 특)

model을 기반으로 함

model을 view와 동기화 됨

어떤 경우 Angular.js에서 본것처럼 양방향 바인딩

이 패턴들이 mutation이 늘어나게끔 되어있어 어쩔 수 없이 소프트웨어가 복잡해짐



그럼 이 문제를 해결하는 방법이 뭘까?

mutation 최소화시키면 됨

그리고 이걸 잘 추상화 시키면?

그래서 React

React는 view = function(state) 와 같은 형식으로 View를 어플리케이션의 state의 기능으로 만들었다는것임

그럼 우리는 state가 변경되면 알아서 리액트가 view를 그리기 때문에 state만 걱정 하면 됨

이 아이디어와 component API가 함께하였을때, 혁신이 일어남

함수를 만들고, 구성하는것과 같은 논리로 component개발이 가능해짐

인즉 함수의 매개변수와 함수의 구성요소가 달라지면 view도 달라짐

component의 props와 내부 state가 달라지면 view도 달라짐

이는 위  view = function(state)이라는 공식의 기반되어 아이디어가 발생함



관심사 분리 및 JSX의 역사

리액트는 관심사도 분리하는 기준이 달랐음

관심사 분리는 보통 컴퓨터가 프로그램을 별개의 섹션으로 분리 하는 기준으로 나누어짐

역사적으로도 웹에서는 이를통해 관심사 분리를 javascript, html, css 요 3가지로 분리함

하지만 리액트는 달랐음

아니 state가 변경되면 view도 변경될태고, 이를 기반으로 css도 변경되어야 하는거아님?

예시를 들어보면 토글을 클릭(state 변경)되면 토글이 옆으로 이동됨(css도 변경)

그래서 React는 state, ui, style, view render 요것들을 하나로 합침

그게 현대 프로그래밍의 가장 큰 영향을 준것준 하나인 JSX되시겠다.

function Buntton({ onClick, children }) {
  return <button onClick={onClick}>{children}</button>
}

이를 통해 Component 기반 API와 JSX를 수용할 수 있게 되었고, 이를통해 위에서 계속 말했던 state가 변경되면 view가 변경된다는 방법이 완성됨

또한 이를통해 우리는 view가 어떻게 변경될것 인지 고민하지 않고 왜 변경될 것인지의 집중 할 수 있게됨

즉 명령형 프로그래밍에서 선언형프로그래밍이 가능하게 됨

이를통해 서드파티에 대한 생태계가 좀더 활발하게 됨

그래서 리액트를 사용하는듯 ㅠ



위에서 말한것들이 2013년도의 일이었음

그럼 뭐가 달라졌냐면… 달라진건 별로 없어보임

여전히 사람들은 리액트를 사용하고 그 이후에 나오는 프레임워크, 라이브러리들도 비슷한 방법론을 사용하고있음

하지만 변경된건 사용하는 방식이 달라짐.

최근에는 React가 UI를 만드는 기본 요소라는점이 방식이 달라졌다고 할 수 있다.

 대표적으로 Astro, Remix, Next.js

React를 기반으로 한 SSR, intelligent bundling, route pre-fetching 등등