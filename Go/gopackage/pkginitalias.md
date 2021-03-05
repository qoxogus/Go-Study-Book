# 패키지 init 함수와 alias

- 개발자가 패키지를 작성할 때, 패키지 실행시 처음으로 호출되는 `init()` 함수를 작성할 수 있다. 즉, **init 함수는 패키지가 로드되면서 실행되는 함수로 별도의 호출 없이 자동으로 호출된다.**

```
package testlib
 
var pop map[string]string
 
func init() {   // 패키지 로드시 map 초기화
    pop = make(map[string]string)
}
```

- 경우에 따라 패키지를 import 하면서 단지 그 패키지 안의 `init()` 함수만을 호출하고자 하는 케이스가 있다. 이런 경우는 패키지 import 시 _ 라는 alias 를 지정한다. 아래는 `other/xlib` 패키지를 호출하면서 `_ alias`를 지정한 예이다.

```
package main

import _ "other/xlib"
```

- 만약 패키지 이름이 동일하지만, 서로 다른 버젼 혹은 서로 다른 위치에서 로딩하고자 할 때는, 패키지 alias를 사용해서 구분할 수 있다.

```
import (
    mongo "other/mongo/db"
    mysql "other/mysql/db"
)

func main() {
    mondb := mongo.Get()
    mydb := mysql.Get()
    //...
}
```