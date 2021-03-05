# 패키지 import

- 다른 패키지를 프로그램에서 사용하기 위해서는 import 를 사용하여 패키지를 포함시킨다. 예를 들어, Go의 표준 라이브러리인 fmt 패키지를 사용하기 위하여, `import "fmt"` 와 같이 해당 패키지를 포함시킬 것을 선언해 준다. `Import 후에는 아래 예제처럼 fmt 패키지의 Println() 함수를 호출하여 사용할 수 있다.`
```
package main
 
import "fmt"
 
func main(){
 fmt.Println("Hello")
}
```

- `패키지를 import 할 때, Go 컴파일러는 GOROOT 혹은 GOPATH 환경변수를 검색`하는데, 표준 패키지는 GOROOT/pkg 에서 그리고 사용자 패키지나 3rd Party 패키지의 경우 GOPATH/pkg 에서 패키지를 찾게 된다.

- GOROOT 환경변수는 Go 설치시 자동으로 시스템에 설정되지만, `GOPATH는 사용자가 지정`해 주어야 한다.