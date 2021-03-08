# recover

- Go 내장함수인 `recover()`함수는 **panic 함수에 의한 패닉상태를 다시 정상상태로 되돌리는 함수**이다. 위의 panic 예제에서는 main 함수에서 println() 이 호출되지 못하고 프로그램이 crash 하지만, 아래와 예제와 같이 recover 함수를 사용하면 panic 상태를 제거하고 openFile()의 다음 문장인 println() 을 호출하게 된다.
```
package main
 
import (
    "fmt"
    "os"
)
 
func main() {
    // 잘못된 파일명을 넣음
    openFile("false File adress")
 
    // recover에 의해
    // 이 문장 실행됨
    println("Done") 
}
 
func openFile(fn string) {
    // defer 함수. panic 호출시 실행됨
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("OPEN ERROR", r)
        }
    }()
 
    f, err := os.Open(fn)
    if err != nil {
        panic(err)
    }
 
    defer f.Close()
}
```