# if문

- if 문은 해당 조건이 맞으면 `{ }` 블럭안의 내용을 실행한다. Go의 if 조건문은 아래 예제에서 보듯이 조건식을 괄호`( )`로 둘러 싸지 않아도 된다. 그리고 반드시 조건 블럭 시작 브레이스`({)`를 if문과 같은 라인에 두어야 한다. 이를 다음 라인에 두게 되면 에러를 발생시킨다.

그리고 한가지 주목할 점은 if 문의 조건식은 반드시 Boolean 식으로 표현되어야 한다는 것이다. 이점은 C/C++ 같은 다른 언어들이 조건식에 1, 0 과 같은 숫자를 쓸 수 있는 것과 대조적이다.
```
if k == 1 {  //같은 라인
    println("One")
}
```
<hr>

# else if, else
- if 문은 else if, 혹은 else 문을 함께 가질 수 있다. else if 문은 if 조건문이 거짓일 때 다시 다른 if 조건식을 검사하는 데 사용되며, else 문은 이전의 if 문들이 모두 거짓일 때 실행된다. if 문과 마찬가지로 else if 혹은 else 문의 블럭 시작 브레이스는 같은 라인에 있어야 한다.
```
if k == 1 {

    println("One")

} else if k == 2 {

    println("Two")

} else {           

    println("Other")

}
```
- if 문에서 조건식을 사용하기 이전에 간단한 문장(Optional Statement)을 함께 실행할 수 있다. 즉, 아래 예제처럼 `val := i*2` 라는 문장을 조건식 이전에 실행할 수 있는데, `여기서 주의할 점은 이때 정의된 변수 val는 if문 블럭 (혹은 if-else 블럭 scope) 안에서만 사용할 수 있다는 것이다.` 이러한 Optional Statement 표현은 아래의 switch문, for문 등 Go의 여러 문법에서 사용할 수 있다.
```
if val := i * 2; val < max {

    println(val)

}
 
val++ // << 처럼 사용하면 Scope 벗어나 에러
```