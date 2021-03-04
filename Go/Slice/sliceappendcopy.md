# Slice add(추가), append(병합)과 copy(복사)

- 배열은 고정된 크기로 그 크기 이상의 데이타를 임의로 추가할 수 없지만, `슬라이스는 자유롭게 새로운 요소를 추가할 수 있다.` 슬라이스에 새로운 요소를 추가하기 위해서는 Go 내장함수인 `append()`를 사용한다. `append()의 첫 파라미터는 슬라이스 객체이고`, `두번째는 추가할 요소의 값이다. `또한 `여러 개의 요소 값들을 한꺼번에 추가하기 위해서는 append() 두번째 파라미터 뒤에 계속하여 값을 추가할 수 있다.`

```
func main() {
    s := []int{0, 1}
 
    // 하나 확장
    s = append(s, 2)       // 0, 1, 2
    // 복수 요소들 확장
    s = append(s, 3, 4, 5) // 0,1,2,3,4,5
 
    fmt.Println(s)
}
```
- 내장함수 `append()`가 슬라이스에 데이타를 추가할 때, 내부적으로 다음과 같은 일이 일어난다. `슬라이스 용량(capacity)이 아직 남아 있는 경우는 그 용량 내에서 슬라이스의 길이(length)를 변경하여 데이타를 추가하고, 용량(capacity)을 초과하는 경우 현재 용량의 2배에 해당하는 새로운 Underlying array (아래 내부구조 참조) 을 생성하고 기존 배열 값들을 모두 새 배열에 복제한 후 다시 슬라이스를 할당한다.` 아래 예제는 길이 0/용량 3의 슬라이스에 1부터 15까지의 숫자를 계속 추가하면서 슬라이스의 길이와 용량이 어떻게 변하는 지를 체크하는 코드이다. 이 코드를 실행하면 1~3까지는 기존의 용량 3을 사용하고, 4~6까지는 용량 6을, 7~12는 용량 12, 그리고 13~15는 용량 24의 슬라이스가 사용되고 있음을 알 수 있다.
```
package main
 
import "fmt"
 
func main() {
    // len=0, cap=3 인 슬라이스
    sliceA := make([]int, 0, 3)
 
    // 계속 한 요소씩 추가
    for i := 1; i <= 15; i++ {
        sliceA = append(sliceA, i)
        // 슬라이스 길이와 용량 확인
        fmt.Println(len(sliceA), cap(sliceA))
    }
 
    fmt.Println(sliceA) // 1 부터 15 까지 숫자 출력 
}
```
- 한 슬라이스를 다른 슬라이스 뒤에 병합하기 위해서는 아래 예제와 같이 `append()`를 사용한다. 이 append 함수에서는 2개의 슬라이스를 파라미터로 갖는데, 처음 슬라이스 뒤에 두번째 파라미터의 슬라이스를 추가하게 된다. 여기서 한가지 주의할 것은 두번째 슬라이스 뒤에 ... 을 붙인다는 것인데, 이 ellipsis(...)는 해당 슬라이스의 컬렉션을 표현하는 것으로 두번째 슬라이스의 모든 요소들의 집합을 나타낸다. 즉, 아래 예제에서 sliceB... 는 4, 5, 6 으로 치환된다고 볼 수 있다.
```
package main
 
import "fmt"
 
func main() {
    sliceA := []int{1, 2, 3}
    sliceB := []int{4, 5, 6}
 
    sliceA = append(sliceA, sliceB...)
    //sliceA = append(sliceA, 4, 5, 6)
 
    fmt.Println(sliceA) // [1 2 3 4 5 6] 출력
}
```
- 이러한 추가/확장 기능과 더불어, Go 슬라이스는 내장함수 `copy()`를 사용하여 `한 슬라이스를 다른 슬라이스로 복사할 수도 있다.` 아래 예제는 `3개의 요소를 갖는 소스 슬라이스를 그 2배의 크기 즉 6개를 갖는 타겟슬라이스로 복사하는 예를 보여준다.` (주: 아래에서 설명하듯이 슬라이스는 실제 배열을 가리키는 포인터 정보만을 가지므로, 복사를 좀 더 정확히 표현하면, 소스 슬라이스가 갖는 배열의 데이타를 타겟 슬라이스가 갖는 배열로 복제하는 것임)
```
func main() {
    source := []int{0, 1, 2}
    target := make([]int, len(source), cap(source)*2)
    copy(target, source)
    fmt.Println(target)  // [0 1 2 ] 출력
    println(len(target), cap(target)) // 3, 6 출력
}
```