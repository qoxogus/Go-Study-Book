# type문을 이용한 함수 원형 정의
- type 문은 구조체(struct), 인터페이스 등 Custom Type(혹은 User Defined Type)을 정의하기 위해 사용된다. type 문은 또한 함수 원형을 정의하는데 사용될 수 있다. 즉, [[일급함수]](./firclsfunc.md)에서 func(x int, y int) int 함수 원형이 코드 상에 계속 반복됨을 볼 수 있는데, 이 경우 type 문을 정의함으로써 해당 함수의 원형을 간단히 표현할 수 있다.
```
// 원형 정의
type calculator func(int, int) int
 
// calculator 원형 사용
func calc(f calculator, a int, b int) int {
    result := f(a, b)
    return result
}
```