# Pass By Reference /  Pass By Value
1. Pass By Value  
- 이전 [[function]](./function.md)의 예제에서는 msg의 값 "Hello" 문자열이 복사되어 함수 say()에 전달된다. 즉, 만약 파라미터 msg의 값이 say() 함수 내에서 변경된다하더라도 호출함수 main()에서의 msg 변수는 변함이 없다.

2. Pass By Reference
- 아래의 예제에서처럼 msg 변수앞에 & 부호를 붙이면 msg 변수의 주소를 표시하게 된다. 흔히 포인터라 불리우는 이 용법을 사용하면 함수에 msg 변수의 값을 복사하지 않고 msg 변수의 주소를 전달하게 된다. 피호출 함수 say()에서는 *string 과 같이 파라미터가 포인터임을 표시하고 이때 say 함수의 msg는 문자열이 아니라 문자열을 갖는 메모리 영역의 주소를 갖게 된다. msg 주소에 데이타를 쓰기 위해서는 *msg = "" 과 같이 앞에 *를 붙이는데 이를 흔히 Dereferencing 이라 한다.  

- 아래 예제의 경우 main 함수의 msg 변수의 값이 say 함수에서 Changed 로 변경되었으므로 main 함수의 println() 에서 변경된 값이 출력된다.
```
package main

func main() {
    msg := "Hello"
    say(&msg)
    println(msg) //변경된 메시지 출력
}
 
func say(msg *string) {
    println(*msg)
    *msg = "Changed" //메시지 변경
}
```