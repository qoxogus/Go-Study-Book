# 사용자정의 패키지 생성

- 개발자는 `사용자 정의 패키지를 만들어 재사용 가능한 컴포넌트를 만들어 사용할 수 있다.` 사용자 정의 라이브러리 패키지는 일반적으로 폴더를 하나 만들고 그 폴더 안에 .go 파일들을 만들어 구성한다. 하나의 서브 폴더안에 있는 .go 파일들은 동일한 패키지명을 가지며, **패키지명은 해당 폴더의 이름과 같게 한다.** 즉, 해당 폴더에 있는 여러 *.go 파일들은 하나의 패키지로 묶인다.

- 예제로 간단한 패키지를 만들기 위해 /src 폴더 안에 (임의의 폴더명으로) `{foldername}/{testlib}` 폴더를 생성한 후에 다음 코드를 music.go 라는 파일에 저장한다. ***여기서 패키지명은 폴더명과 동일하게 testlib로 정해주어야 한다.*** 패키지 폴더 안에 여러 파일들이 있을 경우에도, 동일하게 testlib 패키지명을 사용한다.

```
package testlib
 
import "fmt"
 
var pop map[string]string
 
func init() {
    pop = make(map[string]string)
    pop["Adele"] = "Hello"
    pop["Alicia Keys"] = "Fallin'"
    pop["John Legend"] = "All of Me"
}
 
// GetMusic : Popular music by singer (외부에서 호출 가능)
func GetMusic(singer string) string {
    return pop[singer]
}
 
func getKeys() {  // 내부에서만 호출 가능
    for _, kv := range pop {
        fmt.Println(kv)
    }
}
```

- `Optional` : 사이즈가 큰 복잡한 라이브러리 같은 경우, `go install` 명령을 사용하여 라이브러리를 컴파일하여 Cache할 수 있는데, 이렇게 하면 `다음 빌드시 빌드타임을 크게 줄일 수 있다.` Go 패키지를 빌드하고 /pkg 폴더에 인스톨하기 위해서 `go install` 명령어를 testlib 폴더안에서 실행할 수 있다. 이 명령어가 실행되면, testlib.a 라는 파일이 `/pkg/{folder}/{testlib}` 안에 생성된다.
- main pkg에 `go install` 명령어를 수행하면 /bin 폴더에 실행파일을 생성한다.

- 이제 사용자 정의 패키지를 사용하기 위해서 /src 안에 다음과 같은 코드를 작성해 보자. (main 패키지가 반드시 /src 밑에 있을 필요는 없다).
```
package main
 
import "{foldername}/testlib"
 
func main() {
    song := testlib.GetMusic("taehyeon song")
    println(song)
}
```
- 여기 코드에선 `{foldername}/testlib` 패키지를 import 하고 해당 패키지의 Export 함수인 `GetMusic()`을 호출하고 있다. `{foldername}/testlib` 패키지가 있는 위치를 찾기 위해 GOROOT와 GOPATH의 경로를 사용하는데, *GOROOT와 GOPATH에 있는 각 루트폴더의 src 밑에 {foldername}/testlib 폴더를 순서대로 찾게 된다. 즉, GOPATH가 \GoApp; \GoSrc 인경우, 지정된 라이브러리를 찾기 위해 다음과 같은 폴더를 순차적으로 검색하게 된다.*
```
\Go\src\{foldername}\testlib (from $GOROOT)  
\GoApp\src\{foldername}\testlib (from $GOPATH)  
\GoSrc\src\{foldername}\testlib  
```