# Map 이란

- Map은 키(Key)에 대응하는 값(Value)을 신속히 찾는 해시테이블(Hash table)을 구현한 자료구조이다. Go 언어는 Map 타입을 내장하고 있는데, `map[Key타입]Value타입` 과 같이 선언할 수 있다. 예를 들어 정수를 키로하고 문자열을 값으로 하는 맵 변수 idMap을 선언하기 위해서는 다음과 같이 할 수 있다.

```
var idMap map[int]string
```

- 이때 선언된 변수 idMap은 `(map은 reference 타입이므로) nil 값을 갖으며`, 이를 `Nil Map`이라 부른다. Nil map에는 어떤 데이터를 쓸 수 없는데, map을 초기화하기 위해 make()함수를 사용할 수 있다 (map 리터럴을 사용할 수도 있는 이는 아래 참조).

```
idMap = make(map[int]string)
```

- make() 함수의 첫번째 파라미터로 map 키워드와 [키타입]값타입 을 지정하는데, 이때의 `make()`함수는 해시테이블 자료구조를 메모리에 생성하고 그 메모리를 가리키는 map value를 리턴한다 _(map value는 내부적으로 runtime.hmap 구조체를 가리키는 포인터이다)._ 따라서 idMap 변수는 이 해시테이블을 가리키는 map을 가리키게 된다.

- map은 `make()` 함수를 써서 초기화할 수도 있지만, 리터럴(literal)을 사용해 초기화할 수도 있다. 리터럴 초기화는 `map[Key타입]Value타입 { key:value }` 와 같이 `Map 타입 뒤 { } 괄호 안에 "키: 값" 들을 열거`하면 된다.

```
//리터럴을 사용한 초기화
tickers := map[string]string{
    "GOOG": "Google Inc",
    "MSFT": "Microsoft",
    "FB":   "FaceBook",
}
```

# Map을 사용해보자!
- 처음 map이 `make()` 함수에 의해 초기화 되었을 때는, 아무 데이터가 없는 상태이다. 이때 새로운 데이타를 추가하기 위해서는 `map변수[키] = 값` 과 같이 해당 키에 그 값을 할당하면 된다. 예를 들면, 아래 예제에서 `키 901 에 Apple을 할당하면 새 해시 키-값 쌍이 추가된다. 만약 키 901의 값이 이미 존재했다면, 추가대신 값만 갱신한다.`

```
package main
 
func main() {
    var m map[int]string
 
    m = make(map[int]string)

    //추가 혹은 갱신
    m[901] = "Apple"
    m[134] = "Grape"
    m[777] = "Tomato"
 
    // 키에 대한 값 읽기
    str := m[134]
    println(str)
 
    noData := m[999] // 값이 없으면 nil 혹은 zero 리턴
    println(noData)
 
    // 삭제
    delete(m, 777)
}
```
- map에서 특정 키에 대해 값을 읽을 때는 `map변수[키]`를 읽으면 된다. 즉, 위 예제에서 m[134]는 Grape라는 값을 str 변수에 할당하게 된다. 만약 map안에 찾는 키가 존재하지 않는다면 reference 타입인 경우 nil을 value 타입인 경우 zero를 리턴한다.

- map에서 `특정 키와 그 값을 삭제`하기 위해서는 `delete()`함수를 사용한다.

# Map 키 체크

- map을 사용하는 경우 종종 map안에 특정 키가 존재하는지를 체크할 필요가 있다. 이를 위해 Go에선 `map변수[키]` 읽기를 수행할 때 2개의 리턴값을 리턴한다. `첫번째는 키에 상응하는 값이고, 두번째는 그 키가 존재하는지 아닌지를 나타내는 bool 값`이다. 즉, 아래 예제에서 `val, exists := tickers["MSFT"]` 의 `val 에는 Microsoft`라는 값이 리턴되고, 변수 `exists에는 키가 존재하므로 true`가 리턴된다.

```
package main
 
func main() {
    tickers := map[string]string{
        "GOOG": "Google Inc",
        "MSFT": "Microsoft",
        "FB":   "FaceBook",
        "AMZN": "Amazon",
    }
 
    // map 키 체크
    val, exists := tickers["MSFT"]
    if !exists {
        println("No MSFT ticker")
    }
}
```

# for루프를 이용한 Map 열거
- Map이 갖고 있는 모든 요소들을 출력하기 위해, for range 루프를 사용할 수 있다. `Map 컬렉션에 for range를 사용하면, Map 키와 Map 값 2개의 데이터를 리턴`한다. 아래 예제는 `for 루프를 사용하여 맵으로부터 Key와 Value를 하나씩 가져와서 출력하는 코드`이다.
```
package main
 
import "fmt"
 
func main() {
    myMap := map[string]string{
        "A": "Apple",
        "B": "Banana",
        "C": "Charlie",
    }
 
    // for range 문을 사용하여 모든 맵 요소 출력
    // Map은 unordered 이므로 순서는 무작위
    for key, val := range myMap {
        fmt.Println(key, val)
    }
}
```