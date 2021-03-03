# break, continue, goto

- 경우에 따라 `for 루프내에서 즉시 빠져나올 필요가 있는데, 이때 break 문을 사용한다.` 만약 `for 루프의 중간에서 나머지 문장들을 실행하지 않고 for 루프 시작부분으로 바로 가려면 continue문을 사용한다.` 그리고 `기타 임의의 문장으로 이동하기 위해 goto 문을 사용할 수 있다.`

- 📌goto문은 for 루프와 관련없이 사용될 수 있다.

- break문은 for 루프 이외에 switch문이나 select문에서도 사용할 수 있다. 하지만, continue문은 for 루프와 연관되어 사용된다.
```
package main

func main() {
    var a = 1
    for a < 15 {
        if a == 5 {
            a += a
            continue    // for루프 시작으로
        }
        a++
        if a > 10 {
            break       // 루프 빠져나옴
        }
    }
    if a == 11 {
        goto END        //goto 사용예시
    }
    println(a)
 
END:
    println("End")
}
```

- break문은 보통 단독으로 사용되지만, 경우에 따라 "break 레이블"과 같이 사용하여 지정된 레이블로 이동할 수도 있다. break의 "레이블"은 보통 현재의 for 루프를 바로 위에 적게 되는데, 이러한 "break 레이블"은 현재의 루프를 빠져나와 지정된 레이블로 이동하고, break문의 직속 for 루프 전체의 다음 문장을 실행하게 한다. `아래 예제는 언뜻 보기에 무한루프를 돌 것 같지만, 실제로는 OK를 출력하고 프로그램을 정상 종료한다. 이는 "break L1" 문이 for 루프를 빠져나와 L1 레이블로 이동한 후, break가 있는 현재 for 루프를 건너뛰고 다음 문장인 println() 으로 이동하기 때문이다.`
```
package main
 
func main() {
    i := 0
 
L1:
    for {
     
        if i == 0 {
            break L1
        }
    }
 
    println("OK")
}
```