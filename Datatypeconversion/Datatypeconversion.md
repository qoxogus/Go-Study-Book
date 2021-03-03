# Datatype conversion(데이터 타입 변환)

- 하나의 데이타 타입에서 다른 데이타 타입으로 변환하기 위해서는 T(v) 와 같이 표현하고 이를 Type Conversion이라 부르는데, 여기서 T는 변환하고자 하는 타입을 표시하고, v는 변환될 값(value)을 지정한 것이다. 예를 들어, 정수 100을 float로 변경하기 위하여 float32(100) 처럼 표현하고, 문자열을 바이트배열로 변경하기 위하여 []byte("ABC") 처럼 표현할 수 있다.
<hr>

- 아래 예제는 여러 Type Conversion의 예를 보인 것으로, int와 uint/float간 변환과 문자열과 바이트배열간의 변환을 예시하고 있다. 한가지 주의할 점은 Go에서 타입간 변환은 명시적으로 지정해 주어야 한다는 점인데, 예를 들어 정수형 int에서 uint로 변환할 때, 암묵적(implicit) 변환이 일어나지 않으므로 uint(i) 처럼 반드시 변환을 지정해 주어야 한다. 만약 명시적 지정이 없이 변환이 일어나면 런타임 에러가 발생한다.
```
func main() {
    var i int = 100
    var u uint = uint(i)
    var f float32 = float32(i)  
    println(f, u)
 
    str := "ABC"
    bytes := []byte(str)
    str2 := string(bytes)
    println(bytes, str2)
}
```