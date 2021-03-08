# Go channel

- Go 채널은 그 채널을 통하여 데이터를 주고 받는 통로라 볼 수 있는데, 채널은 `make() 함수를 통해 미리 생성되어야 하며, 채널 연산자 `<-` 을 통해 데이터를 보내고 받는다. 채널은 흔히 goroutine들 사이 데이터를 주고 받는데 사용되는데, 상대편이 준비될 때까지 채널에서 대기함으로써 별도의 lock을 걸지 않고 데이터를 동기화하는데 사용된다.

- 아래 예제는 정수형 채널을 생성하고, 한 goroutine 에서 그 채널에 123이란 정수 데이터를 보낸 후, 이를 다시 메인 루틴에서 채널로부터 123 데이터를 받는 코드이다. 채널을 생성할 때는 `make()` 함수에 어떤 타입의 데이터를 채널에서 주고 받을지를 미리 지정해 주어야 한다. 채널로 데이터를 보낼 때는 `채널명 <- 데이터` 와 같이 사용하고, 채널로부터 데이터틀 받을 경우는 `<- 채널명` 와 같이 사용한다.
아래 예제에서 메인 루틴은 마지막에서 채널로부터 데이터를 받고 있는데, 상대편 goroutine에서 데이터를 전송할 때까지는 계속 대기하게 된다. 따라서, 이 예제에서는 `time.Sleep()` 이나 `fmt.Scanf() `같이 goroutine 이 끝날 때까지 기다리는 코드를 적지 않았다.
```
package main
 
func main() {
  // 정수형 채널을 생성한다 
  ch := make(chan int)
 
  go func() {
    ch <- 123   //채널에 123을 보낸다
  }()
 
  var i int
  i = <- ch  // 채널로부터 123을 받는다
  println(i)
}
```
- Go 채널은 수신자와 송신자가 서로를 기다리는 속성때문에, 이를 이용하여 (다음 예제와 같이) Go루틴이 끝날 때까지 기다리는 기능을 구현할 수 있다. 즉, 익명함수를 사용한 한 Go 루틴에서 어떤 작업이 실행되고 있을 때, 메인루틴은 `<-done` 에서 계속 수신하며 대기하고 있게 된다. `익명함수 Go 루틴에서 작업이 끝난 후, done채널에 true를 보내면, 수신자 메인루틴은 이를 받고 프로그램을 끝내게 된다.`
```
package main
 
import "fmt"
 
func main() {
    done := make(chan bool)
    go func() {
        for i := 0; i < 10; i++ {
            fmt.Println(i)
        }
        done <- true
    }()
 
    // 위의 Go루틴이 끝날 때까지 대기
    <-done
}
```
# Go 채널 버퍼링
- Go 채널은 2가지의 채널이 있는데, `Unbuffered Channel`과 `Buffered Channel`이 있다. 위의 예제에서의 Go 채널은 Unbuffered Channel로서 이 채널에서는 `하나의 수신자가 데이터를 받을 때까지 송신자가 데이터를 보내는 채널에 묶여 있게 된다.` 하지만, Buffered Channel을 사용하면 비록 `수신자가 받을 준비가 되어 있지 않을 지라도 지정된 버퍼만큼 데이터를 보내고 계속 다른 일을 수행할 수 있다.` 버퍼 채널은 make(chan type, N) 함수를 통해 생성되는데, 두번째 파라미터 N에 사용할 버퍼 갯수를 넣는다. 예를 들어, make(chan int, 10)은 10개의 정수형을 갖는 버퍼 채널을 만든다.

- 버퍼 채널을 이용하지 않는 경우, 아래와 같은 코드는 에러 `(fatal error: all goroutines are asleep - deadlock!)` 를 발생시킨다. 왜냐하면 메인루틴에서 채널에 1을 보내면서 상대편 수신자를 기다리고 있는데, 이 채널을 받는 수신자 Go루틴이 없기 때문이다.
```
package main
 
import "fmt"
 
func main() {
  c := make(chan int)
  c <- 1   //수신루틴이 없으므로 데드락 
  fmt.Println(<-c) //코멘트해도 데드락 (별도의 Go루틴없기 때문)
}
```
- 하지만 아래와 같이 버퍼채널을 사용하면, **수신자가 당장 없더라도 최대버퍼 수까지 데이터를 보낼 수 있으므로, 에러가 발생하지 않는다.**
```
package main
 
import "fmt"
 
func main() {
    ch := make(chan int, 1)
 
    //수신자가 없더라도 보낼 수 있다.
    ch <- 101
 
    fmt.Println(<-ch)
}
```

# 채널 파라미터
- 채널을 함수의 파라미터도 전달할 때, 일반적으로 송수신을 모두 하는 채널을 전달하지만, 특별히 해당 채널로 송신만 할 것인지 혹은 수신만할 것인지를 지정할 수도 있다. `송신 파라미터는 (p chan<- int)와 같이 chan<- 을 사용`하고, `수신 파라미터는 (p <-chan int)와 같이 <-chan 을 사용`한다. _만약 송신 채널 파라미터에서 수신을 한다거나, 수신 채널에 송신을 하게되면, 에러가 발생한다._

- 아래 예제에서 만약 `sendChan()` 함수 안에서 `x := <- ch` 를 실행하면 _송신전용 채널에 수신을 시도_하므로 에러가 발생한다.
```
package main
 
import "fmt"
 
func main() {
    ch := make(chan string, 1)
    sendChan(ch)
    receiveChan(ch)
}
 
func sendChan(ch chan<- string) {
    ch <- "Data"
    // x := <-ch // 에러발생
}
 
func receiveChan(ch <-chan string) {
    data := <-ch
    fmt.Println(data)
}
```
# 채널 닫기
- 채널을 오픈한 후 데이터를 송신한 후, `close()`함수를 사용하여 채널을 닫을 수 있다. **채널을 닫게 되면, 해당 채널로는 더이상 송신을 할 수 없지만, 채널이 닫힌 이후에도 계속 수신은 가능하다.** 채널 수신에 사용되는 `<- ch` 은 두개의 리턴값을 갖는데, `첫째는 채널 메시지이고`, `두번째는 수신이 제대로 되었는가를 나타낸다.` _만약 채널이 닫혔다면, 두번째 리턴값은 false를 리턴_ 한다.
```
package main
 
func main() {
    ch := make(chan int, 2)
     
    // 채널에 송신
    ch <- 1
    ch <- 2
     
    // 채널을 닫는다
    close(ch)
 
    // 채널 수신
    println(<-ch)
    println(<-ch)
     
    if _, success := <-ch; !success {
        println("더이상 데이타 없음.")
    }
}
```

# 채널 range문
- `채널에서 송신자가 송신을 한 후, 채널을 닫을 수 있다`. `그리고 수신자는 임의의 갯수의 데이터를 채널이 닫힐 때까지 계속 수신할 수 있다`. 아래 예제는 이러한 송수신 방법을 표현한 것으로, **수신자는 채널이 닫히는 것을 체크하면서 계속 루프를 돌게 된다.**   
방법1. 은 `무한 for 루프 안에서 if 문으로 수신 채널의 두번째 파라미터를 체크`하는 방식이고,   
방법2. 는 방법1과 동일한 표현이지만, `for range문으로 보다 간결하게 표현`한 것이다.     
`채널 range문은 range 키워드 다음의 채널로부터 계속 수신하다가 채널이 닫힌 것을 감지하면 for 루프를 종료한다.`
```
package main
 
func main() {
    ch := make(chan int, 2)
 
    // 채널에 송신
    ch <- 1
    ch <- 2
 
    // 채널을 닫는다
    close(ch)
 
    // 방법1
    // 채널이 닫힌 것을 감지할 때까지 계속 수신
    /*
    for {
        if i, success := <-ch; success {
            println(i)
        } else {
            break
        }
    }
    */
 
    // 방법2
    // 위 표현과 동일한 채널 range 문
    for i := range ch {
        println(i)
    }
}
```

# 채널 select문
-  Go의 select문은 복수 채널들을 기다리면서 준비된 (데이터를 보내온) 채널을 실행하는 기능을 제공한다. 즉, `select문은 여러 개의 case문에서 각각 다른 채널을 기다리다가 준비가 된 채널 case를 실행하는 것`이다. **select문은 case 채널들이 준비되지 않으면 계속 대기**하게 되고, *가장 먼저 도착한 채널의 case를 실행*한다. `만약 복수 채널에 신호가 오면, Go 런타임이 랜덤하게 그 중 한 개를 선택`한다. 하지만, **select문에 default 문이 있으면, case문 채널이 준비되지 않더라도 계속 대기하지 않고 바로 default문을 실행**한다.

- 아래 예제는 for 루프 안에 select 문을 쓰면서 두개의 goroutine이 모두 실행되기를 기다리고 있다.  
첫번째 `run1()`이 1초간 실행되고 done1 채널로부터 수신하여 해당 case를 실행하고, 다시 for 루프를 돈다. for루프를 다시 돌면서 다시 select문이 실행되는데, 다음 `run2()`가 2초후에 실행되고 done2 채널로부터 수신하여 해당 case를 실행하게 된다. done2 채널 case문에 break EXIT 이 있는데, 이 문장으로 인해 for 루프를 빠져나와 EXIT 레이블로 이동하게 된다. **Go의 "break 레이블" 문은 C/C# 등의 언어에서의 goto 문과 다른데, Go에서는 해당 레이블로 이동한 후 자신이 빠져나온 루프 다음 문장을 실행하게 된다.** 따라서, 여기서는 for 루프 다음 즉 `main()` 함수의 끝에 다다르게 된다.
```
package main
 
import "time"
 
func main() {
    done1 := make(chan bool)
    done2 := make(chan bool)
 
    go run1(done1)
    go run2(done2)
 
EXIT:
    for {
        select {
        case <-done1:
            println("run1 완료")
 
        case <-done2:
            println("run2 완료")
            break EXIT
        }
    }
}
 
func run1(done chan bool) {
    time.Sleep(1 * time.Second)
    done <- true
}
 
func run2(done chan bool) {
    time.Sleep(2 * time.Second)
    done <- true
}
```