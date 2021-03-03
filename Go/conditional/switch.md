# Switch문
- 여러 값을 비교해야 하는 경우 혹은 다수의 조건식을 체크해야 하는 경우 switch 문을 사용한다. 다른 언어들과 비슷하게 switch 문 뒤에 하나의 변수(혹은 Expression)를 지정하고, case 문에 해당 변수가 가질 수 있는 값들을 지정하여, 각 경우에 다른 문장 블럭들을 실행할 수 있다. 복수개의 case 값들이 있을 경우는 아래 예제에서 보듯이 case 3,4 처럼 콤마를 써서 나열할 수 있다.
```
package main
 
func main() {
    var name string
    var category = 1
 
    switch category {
    case 1:
        name = "Paper Book"
    case 2:
        name = "eBook"
    case 3, 4:
        name = "Blog"
    default:
        name = "Other"
    }
    println(name)
     
    // Expression을 사용한 경우
    switch x := category << 2; x - 1 {
        //...
    }   
}
```