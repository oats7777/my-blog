많은 사람들이 React를 좋아하는 이유는 HTML이 오늘날 다시 발명된다면 어떤 모습일까에 대한 답이 아닐까라고 생각한다.(비슷한 느낌에 HTMX도 있음)



HTML의 주요 이점은 선언적이라고 이전에 말한적이 있다. 또한 구조화된 마크업을 생성또한 가능하다.

이와 대조적으로 JS의 이점은 모듈 시스템, scope, 클로저 및 실제 프로그래밍 언어에서 기대할 수 있는 다양한 기능을 제공함에 따라 애플리케이션 구축의 복잡성을 관리하기 위한 여러 방면을 제공한다는 것이다.



물론 React는 JS의 여러 기능들과 함께 HTML의 선언성을 제공하는 제공하여 개발이 가능하다. 그럼 실제로는 어떤식으로 동작할까? 간단하게 알아보자



<aside>
  <h2>Authors</h2>
  <ul>
    <li>Kyu</li>
    <li>Grek</li>
    <li>Sol</li>
  </ul>
</aside>

이제 이 HTML을 여러페이지에서 재사용하고 싶다 여러분은 어떤식으로 하겠는가?

가장 간단한 방법은 복붙이다… ㅠ



이와같이 사용하면 필요한 부분을 다시 수정하여 사용할 순 있지만 이상적이지는 않다.

JS에서는 위에서 말한 모듈 시스템을 통해 이를 해결하였다. 해당 코드를 함수에 넣어 작성한뒤 해당 모듈을 불러오는 식으로 사용이 가능하다.



이러한 JS의 기능을 HTML에 적용할 수 있다면 아래와 같이 코드를 작성할 수 있을 것이다.



export default function authors () {
  return <aside>
  <h2>Authors</h2>
  <ul>
    <li>Kyu</li>
    <li>Grek</li>
    <li>Sol</li>
  </ul>
</aside>
}

이것이 React의 본질중 하나라고 생각한다.

가장 먼저 눈에 띄는 부분은 React의 컴포넌트는 단지 함수라는 것이다. 함수에 대해 가지고 있는 동일한 mental mode을 React 컴포넌트에 직접 적용이 가능하다.



이를 실제 컴포넌트로 사용하는 방법은 함수의 이름 첫글자를 대문자로 바꾸는 것이다.

export default function Authors () {
  return <aside>
  <h2>Authors</h2>
  <ul>
    <li>Kyu</li>
    <li>Grek</li>
    <li>Sol</li>
  </ul>
</aside>
}



실제로 javascript에는 어떤식으로 적용되 잇을까?

import { jsx as _jsx } from "react/jsx-runtime";
import { jsxs as _jsxs } from "react/jsx-runtime";
export default function Authors() {
  return /*#__PURE__*/_jsxs("aside", {
    children: [/*#__PURE__*/_jsx("h2", {
      children: "Authors"
    }), /*#__PURE__*/_jsxs("ul", {
      children: [/*#__PURE__*/_jsx("li", {
        children: "Kyu"
      }), /*#__PURE__*/_jsx("li", {
        children: "Grek"
      }), /*#__PURE__*/_jsx("li", {
        children: "Sol"
      })]
    })]
  });
}

위와같이 컴파일된다.

코드를 보면 Authors 함수의 h2와 같이 엘리먼트의 이름을 정의하고, 이에대해 자식요소의 대해서는 children으로 나타내고 있다는 것을 알 수 있다.

여담으로 리액트가 왜 컴포넌트의 첫글자를 대문자로 바꾸냐면, 해당 컴포넌트가 일반 DOM인지 리액트의 컴포넌트인지 알 수 있도록 하기 위해 지었다고 한다.



어찌되었던 우리는 해당 컴포넌트를 자체 모듈로써 캡슐화했기 때문에 재사용할려는 곳 어디서나 사용할 수 있게 되었다.

import Authros from './Authros';

export default function About () {
  return <main>
    <h1>About Us</h1>
    <Authros />
  </main>
};

JS가 컴포넌트를 읽는 시점과 브라우저가 UI를 화면에 그리는 시점 사이에는 매우 많은일이 발생한다.

그러나 최종출력은 여러분이 기대하는 바와 같을 것이다. 이를통해 우리는 리액트가 선언적으로 작성하고 있다는 것을 알 수 있다.

또한 컴포넌트는 단지 함수일 뿐이므로 함수를 구성하고 논리를 함수로 추상화하는 시점에 대해 갖는 것과 동일한 직관을 컴포넌트에 직접 적용할 수 있다.



아직 여러분이 위에서 말하는 직관이라는것이 없다면 SOLID의 단일 책임 원칙을 따르는 것이 언제 어디서 컴포넌트를 생성할지에 대해 적절한 지표라고 생각한다. 즉 컴포넌트는 한가지 작업만 수행하여야 한다.



단일 책임 원칙을 따르면 작은 컴포넌트가 많이 생길 가능성이 높다.하지만 이는 우리가 작성하는 함수와 마찬가지로 나쁜것이 절대 아니다. 그러나 주의하지 않으면 컴포넌트로 디렉토리가 압도될 수 있다.



아래예시를 살펴보자

import Sidebar from './Sidebar'
import Timeline from './Timeline'
import Search from './Search'
import Trending from './Trending'
import Follow from './Follow'
import Footer from './Footer'

export default function Home () {
  return <main>
    <Sidebar />
    <Timeline />
    <Search />
    <Trending />
    <Follow />
    <Footer />
  </main>
}


단일 책임을 따르지만 각 컴포넌트 대해 새 파일을 생성하므로 약간 지저분하다.

대신에 따라야할 좋은 법칙을 하나 소개해보겠다. 다른 곳에서 재사용되는  컴포넌트가 있는 경우 자체 파일로 만드는 것이다. 그렇지 않은 경우 해당 컴포넌트를 필요하 하는 파일 내부에 만들어라.



예를들어,  Timeline, Trending,Follow가 Home 컴포넌트에서만 사용된다고 가정할시 아래와 같이 리팩토링이 가능하다.

import Sidebar from './Sidebar'
import Search from './Search'
import Footer from './Footer'

function Timeline () {
  ...
}

function Trending () {
  ...
}

function Follow () {
  ...
}

export default function Home () {
  return <main>
    <Sidebar />
    <Timeline />
    <Search />
    <Trending />
    <Follow />
    <Footer />
  </main>
}



리액트와 순수함수

SIde Effect가 없고 동일한 입력이 주어지면 항상 동일한 출력을 반환하는 함수는 우리는 순수함수라고 말한다.

함수를 순수하게 유지하기 위해 노력하고 포용하면 프로그램에 있는 버그의 양을 최소화하면서 자연스럽게 프로그램이 예측 가능해진다고 이전에 배웠다.

이러한 이유로 React는 순수 함수를 수용하며, 원칙적으로 컴포넌트는 항상 순수하도록 작성해야 한다.

이와 관해서는 추후에 더 서술하겠지만 일단 여러분은 모든 SideEffect는 이벤트 핸들러 내부에 래핑되어야 한다는 점만 알아주었으면 한다.