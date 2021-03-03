# Array (배열)

- 배열은 연속적인 메모리 공간에 동일한 타입의 데이타를 순서적으로 저장하는 자료구조이다. Go에서 배열의 첫번째 요소는 0번, 그 다음은 1번, 2번 등으로 인덱스를 매긴다 (Zero based array).

- 배열의 선언은 `var 변수명 [배열크기] 데이타타입` 과 같이 하는데, 배열크기를 데이타타입 앞에 써 주는 것이 C, Java 같은 다른 언어들과 다르다. `Go에서 배열의 배열크기는 Type을 구성하는 한 요소이다.` 즉, `[3]int와 [5]int는 서로 다른 타입으로 인식된다.` 배열이 선언된 후에 각 배열의 요소를 인덱스를 사용하여 읽거나 쓸 수 있다.

```
package main
 
func main() {
    var a [3]int    //정수형 3개 요소를 갖는 배열 a 선언
    a[0] = 1
    a[1] = 2
    a[2] = 3
    println(a[1])   // 2 출력
}
```

# 배열의 초기화

- 배열을 정의할 때, 초기값을 설정할 수도 있다. 초기값은 `[배열크기] 데이타타입 뒤에 { } 괄호를 두고 초기값을 순서대로 적으면 된다.` 즉, 위 예제의 배열 초기화를 다음과 같이 할 수 있다. 만약 `초기화 과정에서 [...] 를 사용하여 배열크기를 생략하면 자동으로 초기화 요소 숫자만큼 배열크기가 정해진다.`

```
var a1 = [3]int{1, 2, 3}
var a3 = [...]int{1, 2, 3}    //배열크기 자동으로
```

# 다배열 배열

- Go 언어는 다차원 배열을 지원한다. 다차원 배열은 배열크기 부분을 복수 개로 설정하여 선언한다. 예를 들어, 3 x 4 x 5 차원 정수 배열을 만들려면, 다음과 같이 배열을 정의한다. 다차원 배열의 사용은 일차원 배열과 유사하게 각 차원별 배열인덱스를 지정하여 사용한다.

```
var multiArray [3][4][5]int  // 정의
multiArray[0][1][2] = 10     // 사용
```

# 다차원 배열의 초기화

- 다차원 배열의 초기화는 단차원 배열의 초기화와 비슷하다. 다만, 다차원이므로 배열 초기값 안에 다시 배열값을 넣는 형태를 띤다. 즉, 아래 예제에서 보듯이, 2개의 행과 3개의 열을 이루는 배열이 초기화되었는데, 1행이 {1,2,3} 처럼 묶여서 표현되고 있다.

```
func main() {
    var a = [2][3]int{
        {1, 2, 3},
        {4, 5, 6},      //끝에 콤마 추가
    }
    println(a[1][2])
}
```