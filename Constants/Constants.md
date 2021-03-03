# Constants(상수)

- 상수는 Go 키워드 const 를 사용하여 선언한다. const 키워드 뒤에 상수명을 적고, 그 뒤에 상수타입, 그리고 상수값을 할당한다.
```
const c int = 10
const s string = "Hi"
```
<hr>

- Go 에서는 할당되는 값을 보고 그 타입을 추론하는 기능이 자주 사용된다. 즉, 위의 경우 타입 int, string 을 생략하면 Go에서 자동으로 그 타입을 추론하게 된다.
```
const c = 10
const s = "Hi"
```
<hr>

- 여러 개의 상수들 묶어서 지정할 수 있는데, 아래와 같이 괄호 안에 상수들을 나열하여 사용할 수 있다.
```
const (
    Golang = "Golang"
    Master = "MasterCard"
    Amex = "American Express"
)
```
<hr>

- 한가지 유용한 팁으로 상수값을 0부터 순차적으로 부여하기 위해 iota 라는 identifier를 사용할 수 있다. 이 경우 iota가 지정된 Apple에는 0이 할당되고, 나머지 상수들을 순서대로 1씩 증가된 값을 부여받는다.
```
const (
    Apple = iota // 0
    Grape        // 1
    Orange       // 2
)
```
