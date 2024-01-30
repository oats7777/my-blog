React는 선언적이라고 말함


그럼 우리는 react가 말하고자하는 선언적인 의미를 알고있어야하지 않을까?

한번 알아보자

일단 정의부터 보자면

정의

명령형 프로그래밍은 작업을 수행하는 방법 이고 선언형 프로그래밍은 수행하는 작업에 더 가깝습니다.



라고 하는데 그럼 명령형 프로그래밍과 선언형 프로그래밍의 차이는 무엇일까?

이 주제가 참 느낌적인 느낌은 있는데 명확히 구분히 힘든듯하다.

느낌적인 느낌 바이브주도 개발은 어떨까?



실생활에서 한번 찾아보자면,

우리가 점심을 먹으러 갈 시간이고, 마침 점심시간에 맞춰 식당에 도착하였다.

직원에게 두가지 방법으로 우린 말을 할 수 있는데,

명령(어떻게)

창가쪽 근처 테이블이 비어있는걸 확인하였다. 우리는 저쪽 자리에 가서 앉을 예정이다.

선언적 (무엇을)

6인 테이블 주세요.



명령형 접근 방식은 실제로 어떻게 자리를 잡는지와 관련이 있는것을 볼 수 있다.

테이블을 얻는 방법을 직원에게 설명하여야 하고, 선언적 접근 방식은 원하는 것, 즉 2인용 테이블에 관심이 있다는걸 알 수 있다.

이해가 안 갈 수 있다.

배가 고파 주문을 시켰을때, “스푼라디오는 어떻게 가나요?” 라는 질문을 받게 된다면,

명령

…(이하생략) 역에서 내려가지고 앞으로 20m정도 직진하다가 좌회전한뒤에, 에스컬레이터를 타고 올라가서 다시 우회전,

이후에는 걸어서 좀 가다 업비트 문구가 있으면 건물 문을 열고 엘레베이터를 타고 13층을 누른뒤에 13층에 내려서 도착하면 된다. 

선언적

강남구 테헤란로4길 14 (역삼동) 13층이요



우리가 원하는것은 음식을 받기위함이지 그 과정을 설명하기 위함이 아니다.

위와같은 예시를 볼때 우리는 선언적 접근 방식이 일종의 명령적 추상화 계층이 있다는걸 알 수 있다.



우리는 엘레베이터가 무엇인지 알고 어떤식으로 동작하는지 알고있다. 이를통해 일종의 엘레베이터 추상화 계층이 있음을 확인할 수 있다.

주소를 알고 있다는것은 주소를 통해 지도의 검색하는 행위가 포함되어 있음이 있다.



이제 일상생활에서의 예시는 충분하니 코드베이스로 들어가 보겠다.

명령형

C
C++
JAVA 근데 자바가 명령형이라 하기에도 애매하네

선언형

HTML
SQL

둘다

JAVSCRIPT
C#
Python

일반적인 SQL 또는 HTML 코드에 대해 생각해보자

SELECT * FROM Users WHERE Contry='kr';



<article>
  <header>
    <h1>선언형 프로그래밍</h1>
    <p>오우 매우 스마트</p>
  </header>
</article>

두 예를 확인했을때, 우리가 무엇을 할지에 대한 방식보단 무엇을 하고 싶은지에 대해 관심을 가지고 있다.



즉 달성하고자 하는 무언가에 대해 방법을 지시하지 않고 달성할려는 목표를 설명하고 있다는 뜻이다.

SQL 같은경우 한국에 사는 유저에 대해 선택하는 구현자체가 추상화되어있고, 

HTML같은경우에는 웹 브라우저가 article 요소를 구문 분석하고 이를 화면에 나타내는 방법에는 관심이 없다.



이제부터 javascript로 예제를 알아보도록 하자

기술테스트처럼 약 3가지 정도 예시를 알아보고, 어떻게 하느냐에 집중하는 것처럼 명령적으로 코드를 작성하도록 하겠다.



숫자 배열을 받아 해당 해발의 모든 항목을 두배로 늘린후 새 배열을 반환하는 double 함수를 작성하세요double([1,2,3]) → [2,4,6]

function double(arr) {
  let results = [];
  for(let i = 0; i < arr.length; i++) {
    results.push(arr[i] * 2);
  }
  return results;
}

배열을 받아서 배열의 모든 항목을 더한 결과를 반환하는 add 함수를 작성하세요 add([1,2,3]) → 6

function add(arr) {
  let result = 0;
  for (let i = 0; i< arr.length; i++) {
    result += arr[i];
  }
  return result;
}

id가 btn 요소에 click 이벤트 핸들러를 추가합니다. 클릭하면 highlight 클래스를 토글(추가 또는 제거) 하고 요소의 현재 상태에 따라 텍스트를 변경합니다. 클래스가 추가되어 있으면 Add Highlight 제거되었으면 Remove Highlight로 변경합니다.

$("#btn").click(function () {
  $(this).toggleClass("highlight");
  $(this).text() === "Add Highlight"
    ? $(this).text("Remove Highlight")
    : $(this).text("Add Highlight");
});



이 세가지 명령형으로 작성된 코드의 대해 한번 이야기를 해보자

가장 분명한 공통점은 무언가를 수행하는 방법을 설명하는 코드라는 점이다. 각 예시에서는 명시적으로 배열을 반복하거나, 원하는 기능을 구현하는 방법에 대한 단계를 명시적으로 배치하고 있다.

선언적방식이나 더욱 구체적인 기능적방식으로 생각하는데 익숙하지 않은 경우 이는 명확하지 않을 있다. 각 예에서는 우리는 상태의 일부를 변경하고 있다.

개인적으로 읽기 쉽지 않음… 코드만 보고 무슨일이 일어날지 단계별로 해석해야 하기때문에 코드에 대한 컨텍스트도 이해 해야함(변경 가능한 데이터의 또 다른 단점)



위 문제들을 기반으로 선언적으로 코드가 작성되면 어떤 모습인지 살표보고 이를 다시 파악해보도록 하자.



숫자 배열을 받아 해당 해발의 모든 항목을 두배로 늘린후 새 배열을 반환하는 double 함수를 작성하세요double([1,2,3]) → [2,4,6]

function double(arr) {
  return arr.map((item) => item * 2);
}

배열을 받아서 배열의 모든 항목을 더한 결과를 반환하는 add 함수를 작성하세요 add([1,2,3]) → 6

function add(arr) {
  return arr.reduce((prev, current) => prev + current, 0);
}

id가 btn 요소에 click 이벤트 핸들러를 추가합니다. 클릭하면 highlight 클래스를 토글(추가 또는 제거) 하고 요소의 현재 상태에 따라 텍스트를 변경합니다. 클래스가 추가되어 있으면 Add Highlight 제거되었으면 Remove Highlight로 변경합니다.

<Btn
  onToggleHighlight={handleToggle}
  highlight={highlight}>
    {buttonText}
</Btn>



처음 두가지 예시는 javascript의 내장 메소드인 map reduce를 활용하고 있다.

대부분에 선언적 솔루션은 일부 명령형 구현에 대한 추상화라고 불 수 있다.



위 3가지 예시 전부 어떻게보다는 무엇을 원하는지를 바탕으로 코드를 작성하고 있다.(map과 reduce의 사전적 의미만 알고 있고 내부 구현은 알지 못함)

또한 모든 mutation은 내부적으로 추상화되어있다

마지막 예시는 어떠한가?

적어도 기존 javascript로 작성되어있는 코드보단 선언적이라고 생각한다.

즉 React의 진정한 장점은 선언적 사용자 인터페이스를 개발 할 수 있다는 것이다.

선언적 코드에 대해 또 다른 이점은 프로그램은 상황에 독립적일 수 있다는것이다. 즉 코드를 목표에 달성하기 위한 단계보다는 최종 목표가 무엇인지에 대해 관심이 있기 때문에, 동일한 코드를 다른 프로그램(코드베이스)에서도 사용할 수 있으며 제대로 작동이 될 수 있다는 말이다.



위 3가지모두 특정 상황에 대해 구애받지 않고 사용이 가능하다.



이 글을 마치기 전에 다른 선언적 프로그래밍의 대한 정의를 알아보록 하자.



Declarative programming is “the act of programming in languages that conform to the mental model of the developer rather than the operational model of the machine.”

Declarative Programming is programming with declarations, i.e., declarative sentences.

The declarative property is where there can exist only one possible set of statements that can express each specific modular semantic. The imperative property is the dual, where semantics are inconsistent under composition and/or can be expressed with variations of sets of statements.

Declarative languages contrast with imperative languages which specify explicit manipulation of the computer’s internal state; or procedural languages which specify an explicit sequence of steps to follow.

In computer science, declarative programming is a programming paradigm that expresses the logic of a computation without describing its control flow.

I draw the line between declarative and non-declarative at whether you can trace the code as it runs. Regex is 100% declarative, as it’s untraceable while the pattern is being executed.