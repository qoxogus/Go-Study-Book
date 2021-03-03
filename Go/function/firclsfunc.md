# 일급함수

- Go 프로그래밍 언어에서 함수는 일급함수로서 Go의 기본 타입과 동일하게 취급되며, 따라서 다른 함수의 파라미터로 전달하거나 다른 함수의 리턴값으로도 사용될 수 있다. 즉, 함수의 입력 파라미터나 리턴 파라미터로서 함수 자체가 사용될 수 있다. 함수를 다른 함수의 파라미터로 전달하기 위해서는 익명함수를 변수에 할당한 후 이 변수를 전달하는 방법과 직접 다른 함수 호출 파라미터에 함수를 적는 방법이 있다.
```
package main
 
func main() {
    //변수 add 에 익명함수 할당
    add := func(i int, j int) int {
        return i + j
    }
 
    // add 함수 전달
    r1 := calc(add, 10, 20)
    println(r1)
 
    // 직접 첫번째 파라미터에 익명함수를 정의함
    r2 := calc(func(x int, y int) int { return x - y }, 10, 20)
    println(r2)
 
}
 
func calc(f func(int, int) int, a int, b int) int {
    result := f(a, b)
    return result
```