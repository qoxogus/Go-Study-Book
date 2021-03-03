# Closure (클로저)

- Go 언어에서 함수는 Closure로서 사용될 수도 있다. `Closure는 함수 바깥에 있는 변수를 참조하는 함수값(function value)를 일컫는데`, 이때의 함수는 `바깥의 변수를 마치 함수 안으로 끌어들인 듯이 그 변수를 읽거나 쓸 수 있게 된다.`

- 아래 예제에서 nextValue() 함수는 int를 리턴하는 익명함수(func() int)를 리턴하는 함수이다. Go 언어에서 함수는 일급함수로서 다른 함수로부터 리턴되는 리턴값으로 사용될 수 있다. 그런데 여기서 이 익명함수가 그 함수 바깥에 있는 변수 i 를 참조하고 있다. 익명함수 자체가 로컬 변수로 i 를 갖는 것이 아니기 때문에 (만약 그렇게 되면 함수 호출시 i는 항상 0으로 설정된다) 외부 변수 i 가 상태를 계속 유지하는 즉 값을 계속 하나씩 증가시키는 기능을 하게 된다.
예제에서 next := nextValue() 에서 Closure 함수를 next라는 변수에 할당한 후에, `계속 next()를 3번 호출하는데 이때마다 Clouse 함수내의 변수 i는 계속 증가된 값을 가지고 있게 된다. 이것은 마치 next 라는 함수값이 변수 i 를 내부에 유지하고 있는 모양새이다.` 그러나 만약 `anotherNext := nextValue()`와 같이 새로운 Closure 함수값을 생성한다면, _변수 i는 초기 0을 갖게 되므로 다시 1부터 카운팅_ 을 하게 된다.

```
package main
 
func nextValue() func() int {
    i := 0
    return func() int {
        i++
        return i
    }
}
 
func main() {
    next := nextValue()
 
    println(next())  // 1
    println(next())  // 2
    println(next())  // 3
 
    anotherNext := nextValue()
    println(anotherNext()) // 1 다시 시작
    println(anotherNext()) // 2
}
```