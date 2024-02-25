# [1회차] Error와 Exception

생성일: 2024년 2월 25일 오후 10:11

---

## 오류의 종류

### 컴파일 오류

> 소스코드 → .class파일로 컴파일하는 과정에서 JVM이 던지는 오류
> 
- 대부분 소스코드 자체의 문법적 오류로 인해 발생함
- 오타, 잘못된 구문, 자료형 체크와 같은 기본적인 검사에서 발생
- 프로그램 자체에서 처리할 수 있는 방법은 없음

### 런타임 오류

> 정상적 컴파일 후, 프로그램 실행하는 과정에서 발생하는 오류
> 
- 프로그램 실행시에 동작 멈춘 상태로 오래 지속되거나, 갑자기 프로그램이 실행 멈추는 경우
- 개발자가 직접 오류 확인해서 처리해야 함

## 오류와 예외

### 오류(Error)

> 시스템이 종료되어야 할 수준, 수습할 수 없는 심각한 문제
> 
- 개발자가 미리 예측하여 방지하기 힘듦
- 예
    - `StackOverflowError`
    : 호출의 깊이가 깊어지거나, 재귀가 지속되면서 stack overflow 발생 시 던져지는 오류
        - 이를 피하기 위해서 재귀 사용시 유의하거나, 가시적인 loop를 사용하는 등 간접적인 예방은 가능하다.
    - `OutofMemoryError`
    : JVM이 할당한 메모리의 부족으로, 더 이상 객체 할당할 수 없을 때 던져지는 오류
        - 이는 Garbage Collector에 의해 추가적인 메모리가 확보되지 못하는 상황이기도 함.
        - 이를 피하고자 새는 메모리 차단하거나 heap의 크기를 늘려주는 방법 사용할 수 있다.

### 예외(Exception)

> 개발자가 구현한 로직에서 발생한 실수 or 사용자의 영향으로 발생
> 
- 개발자가 미리 예측하여 방지할 수 있음
- 상황에 따른 예외처리가 필요함
- 예
    - `NullPointerException`
        
        : 객체가 필요한 경우에 null을 사용하려고 시동할 경우 던져질 수 있는 예외
        
    - `IllegalArgumentException`
        
        : 메소드가 허가되지 않거나 부적절한 argument 받았을 시 던져질 수 있는 예외 
        

### Throwable

오류와 예외 모두 Throwable 클래스(Object 클래스를 상속받는)를 상속 받음

![Untitled](%5B1%E1%84%92%E1%85%AC%E1%84%8E%E1%85%A1%5D%20Error%E1%84%8B%E1%85%AA%20Exception%206f6e79998bc04c528a26d79ae0ef7756/Untitled.png)

- Throwable 객체는 오류나, 예외에 대한 메세지를 담음
- 예외가 연결될 때, 연결된 예외의 정보를 기록함
- `getMessage()`와 `printStackTrace()`함수가 구현돼 있음
- `getMessage()`
    
    : 발생한 예외클래스의 인스턴스에 저장된 메세지 얻음.
    
- `printStackTrace()`
    
    : 예외 발생 당시의 호출 스택에 있는 메소드의 정보와 예외 메세지를 화면에 출력
    

## 예외의 종류(Checked/Unchecked)

Exception은 크게 Checked와 Unchecked로 나눌 수 있는데, RuntimeException을 상속하는 클래스를 Unchecked로 분류함

왜❓

런타임 예외는 명시적으로 예외 처리를 하지 않아도 되기 때문에

|  | Checked | Unchecked |
| --- | --- | --- |
| RuntimeException 상속 | X | O |
| 예외 확인 시점 | 컴파일 | 런타임 |
| 처리 여부 | 반드시 해야 함 | 명시적으로 하지 않아도 됨 |
| 트랜잭션 처리 | 롤백 안함 | 롤백 해야함 |
| 예 | IO, ClassNotFound | NullPointer,ClassCast |
| 주로 | JVM외부와 통신할 때 | 실행과정 중, 어떠한 특정 논리에 의해 발견 |

## 예외 처리 방법

### try-catch

> `try`: 예외발생할 수 있는 위험한 로직 들어감
`catch`: 예외 발생하면 수행할 로직 들어감
> 

```java
try {
     예외가 생길 가능성이 있는 코드 작성
} catch(예외발생 클래스명 e){
     예외처리 코드
}

try {
            System.out.println(n1/n2);
        } catch (Exception e) {
            System.out.println("e.getMessage(): "+e.getMessage());
        }
```

- 예외에 대한 최종적인 책임을 지고 처리하는 방식\
- `try`중이라도, 예외가 발생한 다음의 코드들은 실행되지 않고 `catch`문으로 들어감
- `catch` 는 else if 처럼 여러개 쓸 수 있음
- `finally`는 마지막에 실행하고 싶은 로직이 들어감
    - 예외의 발생여부와 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용
    - `close()` 많이 씀.
- **구문 흐름**
    - try 블럭 내에서 예외 발생한 경우
        1. 발생한 예외랑 일치하는 catch블럭이 있는지 확인함
        2. 일치하는 것 있으면 그 것 수행 후, try-catch문 자체를 빠져나와서 그 다음 문장 계속 수행함.
            
            <aside>
            ❓ 일치하는 catch문 있는지 확인 방법
            
            catch블럭 괄호 내 선언된 참조변수의 종류와, 생성된 예외클래스의 인스턴스에 instatnceof 연산자 이용해서 true 결과값 나올 때까지 확인함.
            
            </aside>
            
        3. 일치하는 것 없으면 예외 발생하는 문장부터~ 수행하지 않고, JVM의 예외처리기 화면에 예외 출력함
    - try 블럭 내에서 예외가 발생하지 않은 경우
        1. try문만 실행되고 빠져나와서 다음 문장 계속 수행함

### throws

> 발생한 예외의 책임을 호출하는 쪽이지도록 하는 방식
> 

```java
public static void throwTest(int a, int b) throws ArithmeticException{
        System.out.println("throw a/b: "+ a/b);
    }
```

- throws를 쓰면 그 호출한 메소드에서 try-catch문을 해줘야 함!
    
    ⇒ 반드시, try-catch구문으로 메소드 호출 부분을 감싸줘야 함.
    

### throw

> 개발자가 직접 예외를 발생시키고 싶을 때 쓰는 것
> 

```java
public static void throwTest(int a, int b) throws ArithmeticException{
        throw new ArithmeticException();
    }
```

- 주로 UncheckedException(`RuntimeException`) 처리 위해 쓰는 방식
- Spring 쓸 때, ExcptionHandler와 찰떡궁합~
- 예시 처럼, 사용자가 직접 예외 발생시키고 싶은 곳에 throw new를 넣어서 예외를 발생시켜주고, throws를 통해서 예외처리를 함.
- **단, throw는 Exception을 던질 때 예외 내용도 던져주지 않는다.**
    - 그래서 개발자가 직접 Exception 따로 커스터마이징해서 만들고, 그 안에 메시지를 넣어서 던져주는 방식으로 씀.

### 전략

- CheckedException은
    - try-catch문, throws(의존관계)로 처리하기
- UnchechedException은
    - 기본적으로 복구 불가능한 예외로,
    - CheckedException이어도 더 구체적인 UnChenckedException으로 발생시켜서 throw로 exception을 던지고, ExceptionHandler로 처리하자