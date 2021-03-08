# 지연실행 defer
- Go 언어의 defer 키워드는 **특정 문장 혹은 함수를 나중에 (defer를 호출하는 함수가 리턴하기 직전에) 실행**하게 한다. 일반적으로 defer는 C#, Java 같은 언어에서의 finally 블럭처럼 마지막에 Clean-up 작업을 위해 사용된다. 아래 예제는 파일을 Open 한 후 바로 파일을 Close하는 작업을 defer로 쓰고 있다. 이는 차후 문장에서 어떤 에러가 발생하더라도 항상 파일을 Close할 수 있도록 한다.
```
package main
 
import "os"
 
func main() {
    f, err := os.Open("File adress")
    if err != nil {
        panic(err)
    }
 
    // main 마지막에 파일 close 실행
    defer f.Close()
 
    // 파일 읽기
    bytes := make([]byte, 1024)
    f.Read(bytes)
    println(len(bytes))
}
```