기본적일 수 있지만 소프트웨어를 작성할때 가장 중요한것 중 하나는 프로그램의 동작이 예측 가능하게끔 코드를 작성하는것이다. 보통 대부분의 VOC, 혹은 버그들은 프로그래머가 작성한 코드가 예상을 벗어나는 edge case 가 발생하였기 때문이다. 이러한 경우 프로그램을 보다 예측 가능하게 만들 수 있는 방법에 대해 적어도 시간을 들여 생각을 하는것은 절대 무리가 아니다. 결국 우리의 애플리케이션이 완전히 예측 가능한 세상은 이론적으로는 버그가 더 이상 존재하지 않는다고 봐도 무방하다.



이를  더 잘 파악할려면 범위를 좁히는 것이 도움이 될 것이라고 생각한다. 앱을 더욱 예측 가능하게 만드는것은 실제 사용하기에는 목표가 매우 광범위하지 않은가? 대신, 앱을 구성하는 각각의 부분을 더 예측가능하게 만들어서 앱을 더 예측 가능하게 만들 수 있다고 생각하는 편이 훨씬 편하다.

그럼 그 각각의 부분이란 무엇인가? 보통은 함수라고 생각한다.



함수는 input과 output을 계산하는 일종의 프로세스이다. 정의에 따르면 함수는 매우 간단하다. 이 글을 읽고있는 여러분도 알고있듯이 특정 문제에 대한 복잡성이 방금말한 정의만큼 단순하지는 않다.



간단하고 구성 가능한 함수로 시작하는 경우는 많지만, 정신차리고 주의를 하지 않으면 애플리케이션이 성장함에 따라 예측 가능하지 않는 혼란으로 변할 수 있다.



현실 세계에서 우리는 엄격한 규칙을 설정하고 이를 부지런히 준수함으로써 이러한 종류의 혼란스러운 시나리오를 해결 할 수 있다. 그럼 이 시간의 지남에 따라 혼란스러운 시나리오를 해결 할 수 있는 규칙이란 무엇일까?



이 규칙을 말하기전에  원인을 알아야 규칙을 정할 수 있듯, 원인부터 알아보자 여러분은 원인이 무엇이라고 생각하는가?

나는 두가지라고 생각한다 바로 Side Effect, 일관되지 않은 출력



Side Effect

앞서 우리는 함수의대해 함수는 input과 output을 계산하는 일종의 프로세스이다.  라고 정의하였다.

함수가 받은 input값이 아닌 다른 상태에 의존하거나 프로그램 자체에 관찰 가능한 변경을 생성하는등 이 외 다른 작업을 수행할때마다 우리는 이를 Side Effect라고 말한다

간단한 예시를 알아보자

function addTodo(todo) {
  todos.push(todo);
}

todo를 받아와서 todos의 push를 해줌으로써, 이것은 받은 입력값 이외의 상태에도 의존한다(todos를 변경함으로). 따라서 이는 Side Effect 라고 볼 수 있다.

또 다른 예시를 알아보자

function getGithubProfile(username) {
  return fetch(
    `https://api.github.com/users/${username}`
  ).then((res) => res.json());
}

input으로 받은 상태 외 다른 상태에도 의존하고 있진 않지만, 외부 API로 인해 프로그램 자체에 관찰 가능한 변경을 생성하고 있다. 따라서 이는 Side Effect 라고 볼 수 있다.



아래는 어떠한가?

function updateDocumentTitle(title) {
  document.title = title;
}

이쯤되면 짐작이 가지 않는가? 위 함수는 수신되는 input값에 이외 다른 상태에 의존하고 있지 않지만, 외부에 document를 변경하는 행위를 하고있다. 따라서 Side Effect라고 볼 수 있다.



그러면 Side Effect가 꼭 나쁜것일까? 절대 그렇지 않다. 단지 예측할 수 없을 뿐이다.



한가지 문제는 로컬 환경 외부의 컨텍스트에 의존하기 때문에 애플리케이션의 현재 상태와 관련이 있다는 것이다.

애플리케이션의 상태가 다르면 다르게 동작한다.



아래 예시를 보자

function calculateFinalPrice (price, qty) {
  const total = price * qty
  return total * (1 + TAX_RATE)
}

TAX_RATE는 변수이고 해당 변수가 정수인 프로그램에서만 작동한다. TAX_RATE가 변화가 일어나거나, 삭제되면 더이상 

calculateFinalPrice는 다르게 동작한다. 즉 함수의 정의에 따라 동작을 예측할 수 없게 된다.



그리고 이것으로 우리는 첫번째 규칙을 가지게 되었다.

규칙 #1

Side Effect 없음

일관되지 않은 출력

기능을 예측할 수 없게 만드는 다음 원인으로는 일관성 없는 output(또는 output이 없음)이다.

함수의 반환 값이 없으면 예측할 수 없다.

thisIsUnpredictable()
soIsThis()

이 시나리오에서는 함수가 아무 작업도 수행하지 않거나, SideEffect가 있다고 가정할 수 있다.



마찬가지로, 동일한 입력이 주어지면 함수가 동일한 출력을 일관되게 반환하지 않는 경우에도 예측할 수 없다.

const friends = ["Kyu", "Grek", "Sol"]

friends.splice(0, 1) // ["Kyu"]
friends.splice(0, 1) // ["Grek"]
friends.splice(0, 1) // ["Sol"]

동일한 (0및 1)이 주어지면 splice는 매번 다른 출력을 반환하므로 예측할 수 없다.



또 다른 예시를 보도록 하자.

Math.random() // 0.9511513628376609
Math.random() // 0.6547771399705873

Date.now() // 1665072516384
Date.now() // 1665072525093

const friends = ["Kyu", "Grek", "Sol"]
friends.push("Zero") // 4
friends.push("Hazel") // 5

그리고 이 문제가 있는것은 내장된 메소드 뿐만이 아니다. 예를들어보자

function getGithubProfile(username) {
  return fetch(
    `https://api.github.com/users/${username}`
  ).then((res) => res.json());
}

우리는 위에서 배운것처럼 규칙 #1을 위반하고있기때문에 예측할 수 없다는 것을 알 수 있다.

그러나 그이상으로 동일한 입력이 주어지더라도 일관되지 않은 출력이 반환되기 때문에 훨씬 예측이 힘들다.

const p1 = getGithubProfile("oats7777")
const p2 = getGithubProfile("oats7777")

p1 === p2 // false

게다가 문제가 발생하면 사용자 프로필이 아닌 다양한 다른 항목이 반환될 수 있다.

이제 두번째 규칙을 설정하자.

규칙 #2

일관된 출력



장점

위 두가지 규칙을 따르는 함수를 작성하면 함수를 더 쉽게 예측이 가능하므로 버그에 덜 취약하다는것은 분명하다.

그러나 이 외 장점에 대해 설명해보겠다.



컴포저블

이러한 함수는 애플리케이션의 컨텍스트에 의존하지 않고 대신 명시적인 인수를 통해 input을 받기 때문에 완전히 프로그램에 구애받지 않으므로 구성 및 재사용이 매우 쉬워진다.(node에서 실행하던, 브라우저에서 실행하단 같은 언어 기반에선 똑같은 결과를 반환함)



캐시가능

이러한 함수는 특정 입력에 대해 항상 동일한 출력을 반환하므로 결과를 캐싱할 수 있다.

동일한 입력이 주어지면 이러한 함수를 두번 호출해도 의미가 없다. 출력은 항상 동일하기 때문이다.



주어진 숫자가 소수인지 계산하는 매우매우 비싼 함수가 있다고 가정해보겠다.

const isPrime = (n) => {
  if (n === 1) {
    return false
  }

  for (let i = 2; i <= Math.sqrt(n); i++) {
    if (n % i === 0) {
      return false
    }
  }

  return true
}



이 기능은 완전히 예측이 가능하다. Side Effect가 없고 동일한 입력이 주어지면 항상 동일한 출력을 반환한다.

isPrime함수에 대해서는 특정 숫자에 대해 두 번 이상 호출하는 것은 의미가 없다.

isPrime(997)을 몇번을 실행하더라도 항상 같은 결과가 나올 것이기 때문이다.

이는 반환값이 true라는것이 예측 가능하기 때문에 결과를 캐시하고 재사용이 가능하다.



let primeCache = {
  1: false
}

const isPrime = (n) => {
  if (typeof primeCache[n] === 'boolean') {
    return primeCache[n]
  }

  for (let i = 2; i <= Math.sqrt(n); i++) {
    if (n % i === 0) {
      primeCache[n] = false
      return false
    }
  }

  primeCache[n] = true
  return true
}

isPrime(997) // true
isPrime(997) // true (from cache)
isPrime(997) // true (from cache)



테스트 가능

이러한 기능은 Side Effect가 없고 애플리케이션의 컨텍스트에 의존하지 않기 때문에 테스트에 최적화되어있다.

특정 입력이 주어졌을 때 출력이 예상한 것과 일치하는지 간단히 테스트가 가능하다(unit test)



읽을 수 있음

이러한 함수는 애플리케이션의 컨텍스트에 의존하지 않고 Side Effect가 없기 때문에 가독성을 위해 최적화되어있다.

input은 명시적이고 output도 명시적이며 그 사이에는 black box가 없다.



이 이후에는?

위에서 설명한 예시는 함수형 프로그래밍의 원리를 간단하게 설명해준것이다.

즉 순수함수에 대해 설명한 것이다. SIde Effect가 없고 동일한 입력이 주어지면 항상 동일한 출력을 반환하는 함수는 순수하다.



함수를 순수하게 유지하기 위해 노력하면 프로그램에 있는 버그의 양을 최소화하면서 자연스럽게 프로그램의 예측 가능성을 높일 수 있다.



그렇다면 이것이 React와 어떤 관련이 있을까? 이는 다음장에서 설명하도록 하겠다.