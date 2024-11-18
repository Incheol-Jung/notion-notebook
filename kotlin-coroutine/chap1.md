# JVM 프로세스와 스레드

- 대부분 JVM 애플리케이션은 실행되면 프로세스를 시작하고 메인 스레드를 생성하며 main 함수 내부의 코드들을 수행한다
- 메인 스레드는 일반적으로 프로세스의 시작과 끝을 함께하는 매우 중요한 역할을 한다

```jsx
fun main() {
	println("Hello world!")
}
```

- 만약 예외로 인해 메인 스레드가 강제로 종료되면 프로세스도 강제 종료된다

```jsx
fun mainn() {
	println("메인 스레드 시작")
	throw Exception("Dummy Exception")
	println("메인 스레드 종료")
}
```

## 단일 스레드 애플리케이션의 한계

- 스레드는 하나의 작업을 수행할 때 다른 작업을 동시에 수행하지 못한다
- 서버사이드 작업에서도 마찬가지다. 클라이언트로부터 오래 걸리는 작업 요청이 들어왔을 때 단일 스레드만 사용해 처리한다면 요청을 처리하는 솟도가 늦어져 응답속도가 늦어진다.

## 멀티 스레드 프로그래밍을 통한 단일 스레드의 한계 극복

![1](https://github.com/user-attachments/assets/371c7073-786a-4553-a93f-488658fb5cb3)


- 앞서 살펴본 안드로이드와 서버사이드 작업 시 단일 스레드를 사용했을 때 생긴 문제를 멀티 스레드를 사용해 해결할 수 있다
- 멀티 스레드는 여러 개의 스레드로 작업을 실행한다
- 오래 걸리는 작업이 요청됐을 때 작업을 스레드 간에 독립적인 작은 작업으로 나눈 후 각 작업이 서로 다른 스레드에서 실행되도록 만드면 응답 속도를 높일 수 있다.
- 이런 방식으로 여러 스레드가 동시에 작업을 처리하면 단일 스레드만 사용하는 것에 비해 처리 속도가 빨라지는데 이를 병렬 처리라고 한다.

```
💡

모든 작업을 작은 단위로 나눠서 병렬로 실행할 수 있는것은 아니다. 작은 작업 간에 독립성이 있을 때만 병렬로 실행할 수 있다

```

## Thread 클래스를 사용하는 방법과 한계

- 코루틴 또한 기존의 멀티 스레드 프로그래밍 문제를 해결한 토대 위에서 만들어졌다
- 오래 걸리는 작업이 별로 스레드에서 실행되도록 Thread 클래스를 상속하는 클래스를 만들 것이다.

```jsx
class ExampleThread : Thread() {
    override fun run() {
        println("[${Thread.currentThread().name}] 새로운 스레드 시작")
        Thread.sleep(2000L) // 2초 동안 대기
        println("[${Thread.currentThread().name}] 새로운 스레드 종료")
    }
}
```

- 이제 ExampleThread 클래스를 인스턴스화해 실행해보자

```jsx
fun main() {
    println("[${Thread.currentThread().name}] 메인 스레드 시작")
    ExampleThread().start()
    Thread.sleep(100L)
    println("[${Thread.currentThread().name}] 메인 스레드 종료")
}

// [main] 메인 스레드 시작
// [Thread-0] 새로운 스레드 시작
// [main] 메인 스레드 종료
// [Thread-0] 새로운 스레드 종료
```

- 스레드는 각각 하나의 작업을 진행할 수 있으므로 메인 스레드의 작업과 Thread-0 스레드가 요청받은 작업은 동시에 실행된다

```
💡

사용자 스레드와 데몬 스레드

JVM 프로세스는 일반적으로 메인 스레드의 작업이 종료되면 종료된다고 했다. 하지만 방금 예시에서는 메인 스레드가 종료될 때 프로세스가 종료되지 않고 새로운 스레드의 작업이 종료될 때 프로세스가 종료된다. 

그 이유는 바로 메인 스레드와 ExampleThread 객체를 실행하면서 만들어진 스레드 모두 사용자 스레이기 때문이다

사용자 스레드 - 우선도가 높은 스레드
데몬 스레드 - 우선도가 낮은 스레드

JVM 프로세스가 종료되는 시점은 우선도가 높은 스레드가 종료되는 시점이다

만약 기존에 생성한 스레드를 데몬 스레드로 바꾸고 싶다면 다음과 같이 isDaemon = true 속성을 적용해야 한다. 

```

- 데몬 스레드 정의하기

```jsx
fun main() {
    println("[${Thread.currentThread().name}] 메인 스레드 시작")
    ExampleThread().apply { isDaemon = true }.start()
    Thread.sleep(100L)
    println("[${Thread.currentThread().name}] 메인 스레드 종료")
}
```

## Thread 클래스를 직접 다루는 방법의 한계

- Thread 클래스를 직접 생성하는 것은 다음과 같이 문제가 두 가지 있다
    1. 스레드 생성 비용은 비싸기 때문에 매번 새로운 스레드를 생성하는 것은 성능적으로 좋지 않다
    2. 스레드 생성과 관리의 책임이 모두 개발자에게 있어 리스크가 있다(ex. 메모리 누수, 오류)

## Executor 프레임워크를 통해 스레드풀 사용하기

- Executor 프레임워크는 개발자가 스레드를 직접 관리하는 문제를 해결하고 생성된 스레드의 재사용성을 높이려고 등장했다
- 개발자가 해야 할 일은 스레드풀에 속한 스레드의 개수를 설정하고, 해당 스레드풀을 관리하는 서비스에 작업을 제출하는 것 뿐이다.

### Executor 프레임워크 사용해 보기

- Executor 프레임워크에서 사용자가 사용할 수 있는 함수는 크게 두 가지 이다
    1. 스레드 풀을 생성하고 생성된 스레드풀을 관리하는 객체를 반환하는 함수
    2. 스레드풀을 관리하는 객체에 작업을 제출하는 함수
- Executors 클래스에서 제공하는 newFixedThreadPool 함수를 사용해 예제를 구현해보자

```jsx
fun main() {
    val startTime = System.currentTimeMillis()
    // ExecutorService 생성
    val executorService = Executors.newFixedThreadPool(2)

    // 작업 1 제출
    executorService.submit{
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}]")
        Thread.sleep(1000L)
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}]")
    }

    // 작업 2 제출
    executorService.submit{
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}]")
        Thread.sleep(1000L)
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}]")
    }

    executorService.shutdown()
}

// [pool-1-thread-1][지난 시간: 5ms]
// [pool-1-thread-2][지난 시간: 5ms]
// [pool-1-thread-1][지난 시간: 1010ms]
// [pool-1-thread-2][지난 시간: 1010ms]
```

- 만약 작업3이 추가로 실행되면 어떻게 될까?

```jsx
fun main() {
    val startTime = System.currentTimeMillis()
    // ExecutorService 생성
    val executorService = Executors.newFixedThreadPool(2)

    // 작업 1 제출
    executorService.submit{
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}] 작업 1 시작")
        Thread.sleep(1000L)
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}] 작업 1 완료")
    }

    // 작업 2 제출
    executorService.submit{
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}] 작업 2 시작")
        Thread.sleep(1000L)
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}] 작업 2 완료")
    }

    // 작업 3 제출
    executorService.submit{
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}] 작업 3 시작")
        Thread.sleep(1000L)
        println("[${Thread.currentThread().name}][${getElapsedTime(startTime)}] 작업 3 완료")
    }

    executorService.shutdown()
}
```

## Executor 프레임워의 의의와 한계

- Executor 프레임웍은 개발자가 더 이상 스레드를 직접 관리하지 않는 장점도 있지만 스레드 불로킹이 발생할수 있다는 점이다
- 스레드 블로킹은 스레드가 아무것도 하지 못하고 사용될 수 없는 상태에 있는 것을 뜻한다
- 스레드 블로킹을 발생시키는 원인은 다양하다
- 그중 하나는 여러 스레드가 동기화 블록에 동시에 접근하는 경우 하나의 스레드만 동기화 블록에 접근이 허용되 대기하는 경우에도 발생할 수 있다

```jsx
fun main() {
    val executorService = Executors.newFixedThreadPool(2)
    val future = executorService.submit<String> {
        Thread.sleep(2000)
        "작업1완료"
    }

    val result = future.get() // 메인 스레드가 블로킹 됨
    println(result)
    executorService.shutdown()
}
```

## 이후의 멀티 스레드 프로그래밍과 한계

- Future 객체의 단점을 보완해 스레드 블로킹을 줄이고 작업을 체이닝하는 기능을 제공하는 CompletableFuture 객체가 나오기도 했고, 리액티브 프로그래밍 패러다임을 지원하는 RxJava가 등장하기도 했다
- 스레드는 생성 비용과 작업을 전환하는 비용이 비싸다
- 만약 스레드가 아무 작업을 하지 못하고 기다려야 한다면 컴퓨터의 자원이 낭비된다

## 코루틴은 스레드 불로킹 문제를 어떻게 극복하는가?

- 코루틴은 작업이 일시 중단되면 더 이상 스레드 사용이 필요하지 않으므로 스레드의 사용 권한을 양보하며, 양보된 스레드는 다른 작업을 실행하는 데 사용할 수 있다
- 프로그래머가 코루틴을 만들어 코루틴 스케줄러에 넘기면 코루틴 스케줄러는 자신이 사용할 수 있는 스레드나 스레드풀에 해당 코루틴을 분배해 작업을 수행한다
- 코루틴이 스레드를 사용하던 중에 필요가 없어지면 해당 스레드를 다른 코루틴이 쓸 수 있게 양보할 수 있어서 스레드 블로킹이 일어나지 않게 된다
- 코루틴은 작업 단위로서의 코루틴이 스레드를 사용하지 않을 때 스레드 사용 권한을 양보하는 방식으로 스레드 사용을 최적화하고 스레드가 블로킹되는 상황을 방지한다
- 코루틴은 스레드에 비해 생성과 전환 비용이 적게 들고 스레드에 자유롭세 뗐다 붙였다 할 수 있어 작업을 생성하고 전환하는 데 필요한 리소스와 시간이 매우 줄어든다
- 또한 코루틴은 구조화된 동시성을 통해 비동기 작업을 안전하게 만들고, 예외 처리를 효과적으로 처리할 수 있도록 하며, 코루틴이 실행 중인 스레드를 손쉽게 전환할 수도 있도록 하는 등 기존의 멀티 스레드 프로그래밍에 비해 많은 장점이 있다

![image](https://github.com/user-attachments/assets/a7310058-de61-4ca0-bd7d-a5bfa0daa0a4)

