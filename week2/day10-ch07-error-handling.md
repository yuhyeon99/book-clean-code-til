# 7장. 오류 처리

> **뭔가 잘못되면 바로 잡을 책임은 바로 우리 프로그래머에게 있다.**
> 

## **오늘 TIL 3줄 요약**

- 깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다.
- 오류 코드보다 예외를 사용하라
- Try-Catch-Finally 문부터  작성하라

## **TIL (Today I Learned) 날짜**

- 2025.06.03~2025.06.04

## **오늘 읽은 범위**

- 7장. 오류 처리

## **책에서 기억하고 싶은 내용을 써보세요.**

### 오류 코드보다 예외를 사용하라

- 오류가 발생하면 예외를 던지는 편이 낫다.
    - 그러면 호출자 코드가 더 깔끔해진다.
    - 논리가 오류 처리 코드와 뒤섞이지 않으니까.
- 단순히 보기만 좋아지는게 아니다.
    - 코드 품질도 나아진다.
    - 뒤섞였던 개념, 즉 디바이스를 종료하는 알고리즘과 오류를 처리하는 알고리즘을 분리했기 때문
    - 각 개념을 독립적으로 살펴보고 이해할 수 있다.

### Try-Catch-Finally 문부터 작성하라

- try 블록에서 무슨 일이 생기든지 catch 블록은 프로그램 상태를 일관성 있게 유지해야 한다.
- 예외가 발생할 코드를 짤 때는 `try-catch-finally` 문으로 시작하는 편이 낫다.
    - 그러면 try 블록에서 **무슨 일이 생기든지** 호출자가 기대하는 상태를 정의하기 쉬워진다.

### 미확인 예외를 사용하라

- 확인된 예외는 `OCP(Open Closed Principle)`를 위반한다.
    - 메서드에 확인된 예외를 던졌는데 catch 블록이 세 단계 위에 있다면 그 사이 메서드 모두가 선언부에 해당 예외를 정의해야 한다.
    
    ```jsx
    // 잘못된 설계: 가상의 checked exception처럼 예외를 propagate하며 
    // 중간 계층이 예외를 알아야 하는 경우
    class CustomCheckedError extends Error {
      constructor(message) {
        super(message);
        this.name = "CustomCheckedError";
      }
    }
    
    function lowLevelFunction() {
      throw new CustomCheckedError("Database connection failed");
    }
    
    function midLevelFunction() {
      // 중간 계층이 이 예외를 알고 있어야 함 (OCP 위반)
      try {
        lowLevelFunction();
      } catch (e) {
        if (e instanceof CustomCheckedError) {
          throw e; // 다시 던짐
        }
      }
    }
    
    function highLevelFunction() {
      try {
        midLevelFunction();
      } catch (e) {
        console.error("Handled at high level:", e.message);
      }
    }
    
    highLevelFunction();
    ```
    
    - 문제점: `midLevelFunction`이 `CustomCheckedError`에 의존하고 있으므로, 예외가 바뀌면 해당 메서드도 수정해야 함 → OCP 위반
    
    ```jsx
    // 개선된 설계 (미확인 예외처럼 동작: 중간 계층이 예외를 알 필요 없음)
    class AppError extends Error {
      constructor(message) {
        super(message);
        this.name = "AppError";
      }
    }
    
    function lowLevelFunction() {
      // 예외를 던지지만 중간 계층이 의존하지 않음
      throw new AppError("Database connection failed");
    }
    
    function midLevelFunction() {
      // 예외를 처리하지 않고 위로 전달 (중립적 역할)
      lowLevelFunction();
    }
    
    function highLevelFunction() {
      try {
        midLevelFunction();
      } catch (e) {
        if (e instanceof AppError) {
          console.error("Handled at top level:", e.message);
        } else {
          throw e; // 예외 재전파
        }
      }
    }
    
    highLevelFunction();
    ```
    
    - `midLevelFunction`은 `AppError`를 몰라도 됨 → OCP 준수
    - 예외 처리를 오직 필요 위치(최상단)에서만 수행

### 예외에 의미를 제공하라

- 예외를 던질 때는 전후 상황을 충분히 덧붙인다.
    - 오류 메세지에 정보를 담아 예외와 함께 던진다
    - 애플리케이션이 로깅 기능을 사용한다면 catch 블록에서 오류를 기록하도록 충분한 정보를 넘겨준다.

### 호출자를 고려해 예외 클래스를 정의하라

- 애플리케이션에서 오류를 정의할 때 **프로그래머에게 가장 중요한 관심사는 오류를 잡아내는 방법**이 되어야 한다.
- 다음은 오류를 형편없이 분류한 사례다:
    - 외부 라이브러리가 던질 예외를 모두 잡아낸다.
    
    ```jsx
    try {
    	// 오류 가능성이 있는 로직
    } catch (DeviceResponseException e) {
    
    } catch (또 다른 exception e) {
    
    } catch (또 다른 exception e) { 
    
    }
    .
    .
    .
    ```
    
    - 위 코드는 중복이 심하지만 그리 놀랍지 않다.
    - 예외에 대응하는 방식이 예외 유형과 무관하게 거의 동일하다.
- 호출하는 라이브러리 API를 감싸면서 예외 유형을 하나로 반환하면 된다.
    
    ```jsx
    LocalPort port = new LocalPort(12)
    try {
    	// 오류 가능성이 있는 로직
    	port.open();
    } catch (PortDevice**Failure** e) {
    	reportError(e);
    	logger.log(e.getMessage(), e);
    } finally {
     // ...
    }
    ```
    
    - 여기서 LocalPort 클래스는 단순히 던지는 예외를 잡아 변환하는 감싸기 클래스 일 뿐이다.
    
    ```jsx
    public class LocalPort {
    	private ACMEPort innerPort;
    	
    	public LocalPort(int portNumber) {
    		innerPort = new ACMEPort(portNumber);
    	}
    	
    	public void open() {
    		try {
    			// 오류 가능성이 있는 로직
    		} catch (DeviceResponseException e) {
    			throw new PortDeviceFailure(e);
    		} catch (또 다른 exception e) {
    			throw new PortDeviceFailure(e);
    		} catch (또 다른 exception e) { 
    			throw new PortDeviceFailure(e);
    		}
    	}
    }
    ```
    
    - 외부 API를 감싸면 외부 라이브러리와 프로그램 사이에서 의존성이 크게 줄어든다.
    - 나중에 다른 라이브러리로 갈아타도 비용이 적다.

### null을 반환하지 마라

- 오류를 유발하는 행위도 언급해야한다
    - 그 중 첫째가 null을 반환하는 습관이다.
    - 한 줄 건너 하나씩 null을 확인하는 코드로 가득한 애플리케이션을 지금까지 수도 없이 봤다.
    - null을 반환하는 코드는 일거리를 늘릴 뿐만 아니라 호출자에게 문제를 떠넘긴다.
    - 누구 하나라도 null 확인을 빼먹는다면 애플리케이션이 통제 불능에 빠질지도 모른다.

### null을 전달하지 마라

- 메서드에서 null을 반환하는 방식도 나쁘지만 메서드로 null을 전달하는 방식은 더 나쁘다.
- 애초에 null을 넘기지 못하도록 금지하는 정책이 합리적이다.

### 결론

- 깨끗한 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다
- 이 둘은 상충하는 목표가 아니다.
- 오류 처리를 프로그램 논리와 분리해 독자적인 사안으로 고려하면 튼튼하고 깨끗한 코드를 작성할 수 있다.
- 오류 처리를 프로그램 논리와 분리하면 독립적인 추론이 가능해지며 코드 유지보수성도 크게 높아진다.

## **궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요.**

## **오늘 읽은 다른사람의 TIL**

- gogoj님의 TIL: https://nomadcoders.co/community/thread/10696

## 나만의 공부법
![image](https://github.com/user-attachments/assets/d7073414-3b2f-45c7-95d4-a577a4e2bbfb)
