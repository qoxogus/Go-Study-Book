# 함수 리턴값

- Go 프로그래밍 언어에서 함수는 리턴값이 없을 수도, 리턴값이 하나 일 수도, 또는 리턴값이 복수 개일 수도 있다. C 언어에서 void 혹은 하나의 값만을 리턴하는 것과 대조적으로 Go 언어는 복수개의 값을 리턴할 수 있다.
Go 언어는 또한 `Named Return Parameter` 라는 기능을 제공하는데, 이는 리턴되는 값들을 (함수에 정의된) 리턴 파라미터들에 할당할 수 있는 기능이다.

- 함수에서 리턴값이 있는 경우는 func 문의 (파라미터 괄호 다음) 마지막에 리턴값의 타입을 정의해 준다. 그리고 값을 리턴하기 위해 함수내에서 return 키워드를 사용한다. 예를 들어, 아래 예제는 sum() 함수의 리턴 타입이 int 임을 표시하고 있으며, sum 함수 마지막에 return s 와 같이 정수 s의 값을 리턴하고 있다.
```
package main
 
func main() {
    total := sum(1, 7, 3, 5, 9)
    println(total)
}
 
func sum(nums ...int) int {
    s := 0
    for _, n := range nums {
        s += n
    }
    return s
}
```
- `Go에서 복수 개의 값을 리턴하기 위해서는 해당 리턴 타입들을 괄호 ( ) 안에 적어 준다.` 예를 들어, 처음 리턴값이 int이고 두번째 리턴값이 string 인 경우 `(int, string)` 과 같이 적어 준다.
아래 예제는 sum() 함수에 가변인수로 숫자들이 전달될 때, 그 숫자들의 갯수와 합계를 함께 리턴하는 코드이다.
```
package main
 
func main() {
    count, total := sum(1, 7, 3, 5, 9)
    println(count, total)   
}
 
func sum(nums ...int) (int, int) {
    s := 0              // 합계
    count := 0          // 요소 갯수
    for _, n := range nums {
        s += n
        count++
    }
    return count, s
}
```
- Go에서 `Named Return Parameter`들에 리턴값들을 할당하여 리턴할 수 있는데, 이는 리턴되는 값들이 여러 개일 때, 코드 가독성을 높이는 장점이 있다. 예를 들어, `위의 sum() 함수를 Named Return Parameter를 이용하여 고쳐쓰면 다음과 같다.` 아래 예제에서 func 라인의 마지막 리턴타입 정의 부분을 보면, `(int, int)` 가 아니라 `(count int, total int)` 처럼 정의되어 있음을 볼 수 있다. 즉, 리턴 파라미터명과 그 타입을 함께 정의한 것이다. 그리고 함수 내에서는 이 count, total에 결과값을 직접 할당하고 있음을 볼 수 있다. 또한 마지막에 return 문이 있는 것을 볼 수 있는데, `실제 return 문에는 아무 값들을 리턴하지 않지만, 그래도 리턴되는 값이 있을 경우에는 빈 return 문을 반드시 써 주어야 한다 (이를 생략하면 에러 발생).`
```
func sum(nums ...int) (count int, total int) {
    for _, n := range nums {
        total += n
    }
    count = len(nums)
    return
}
```