# Anonymous Function (익명함수)

- 함수명을 갖지 않는 함수를 익명함수(Anonymous Function)이라 부른다. 일반적으로 익명함수는 그 함수 전체를 변수에 할당하거나 다른 함수의 파라미터에 직접 정의되어 사용되곤 한다. 아래 예제는 main() 함수 안에서 익명함수가 선언되어 sum이라는 변수에 할당되고 있음을 보여준다. sum에 할당된 익명함수를 보면 func 바로 뒤에 함수명이 생략되었음을 알 수 있다. 이점을 제외하고 함수의 정의 내용을 동일한다. 일단 익명함수가 변수에 할당된 이후에는 변수명이 함수명과 같이 취급되며 "변수명(파라미터들)" 형식으로 함수를 호출할 수 있다.
```
package main
 
func main() {
    sum := func(n ...int) int { //익명함수 정의
        s := 0
        for _, i := range n {
            s += i
        }
        return s
    }
 
    result := sum(1, 2, 3, 4, 5) //익명함수 호출
    println(result)
}
```