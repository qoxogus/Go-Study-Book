# Go의 특별한 Switch문법

1. switch 뒤에 expression이 없을 수 있음 
    -  다른 언어는 switch 키워드 뒤에 변수나    expression 반드시 두지만, Go는 이를 쓰지 않아도 된다. 이 경우 Go는 switch expression을 true로 생각하고 첫번째 case문으로 이동하여 검사한다
        - Go의 switch 문에서 한가지 특징적인 용법은 switch 뒤에 조건변수 혹은 Expression을 적지 않는 용법이다. 이 경우 각 case 조건문들을 순서대로 검사하여 조건에 맞는 경우 해당 case 블럭을 실행하고 switch문을 빠져나온다. 이 용법은 복잡한 if...else if...else if... 문장을 단순화하는데 유용하다.
2. case문에 expression을 쓸 수 있음
    - 다른 언어의 case문은 일반적으로 리터럴 값만을 갖지만, Go는 case문에 복잡한 expression을 가질 수 있다
```
func grade(score int) {
    switch {
    case score >= 90:
        println("A")
    case score >= 80:
        println("B")
    case score >= 70:
        println("C")
    case score >= 60:
        println("D")
    default:
        println("No Hope")
    }
}
```
<hr>


3. Type switch
    - 다른 언어의 switch는 일반적으로 변수의 값을 기준으로 case로 분기하지만, Go는 그 변수의 Type에 따라 case로 분기할 수 있다.
        - Go의 또 다른 용법은 switch 변수의 타입을 검사하는 타입 switch가 있다. 아래 예제는 변수 v의 타입이 int 인지, bool, string 인지를 체크한 후 해당 case 블럭을 실행하는 예이다.
```
switch v.(type) {
case int:
    println("int")
case bool:
    println("bool")
case string:
    println("string")
default:
    println("unknown")
}
```
<hr>

4. No default fall through
    - 다른 언어의 case문은 break를 쓰지 않는 한 다음 case로 이동하지만, Go는 다음 case로 가지 않는다
        - `C 혹은 C# 과 같은 언어에서 case 문은 case 블럭 마지막에 break 문을 명시하여 switch 문을 빠져나온다.` 만약 break 문이 없으면, case 문 밑의 모든 문장들을 실행해 버린다. `Go는 case문 마지막에 break 문을 적든 break 문을 생략하든, 항상 break 하여 switch 문을 빠져나온다.` 이는 Go 컴파일러가 자동으로 break 문을 각 case문 블럭 마지막에 추가하기 때문이다. `Go에서 만약 이러한 디폴트 break 문을 사용하지 않고, C나 C#처럼 계속 밑의 문장들(다음 case문 코드 블럭들)을 실행하게 하려면, fallthrough 문을 명시해 주면 된다.` fallthrough 문을 사용한 아래 예제를 실행하면, "2 이하/3 이하/default 도달"을 모두 출력하게 된다. 즉, 일단 case 2 에 도착한 후 fallthrough 가 있으므로, (val 값이 3이 아님에도) case 3의 코드 블럭을 계속 실행하고, case 3에도 fallthrough 가 있으므로 default 블럭을 계속 실행한다.
```
package main
 
import "fmt"
 
func main() {
    check(2)
}
 
func check(val int) {
    switch val {
    case 1:
        fmt.Println("1 이하")
        fallthrough
    case 2:
        fmt.Println("2 이하")
        fallthrough
    case 3:
        fmt.Println("3 이하")
        fallthrough
    default:
        fmt.Println("default 도달")
    }
}
```