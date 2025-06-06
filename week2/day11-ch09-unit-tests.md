# 9장. 단위 테스트
## **오늘 TIL 3줄 요약**

- **테스트 코드는 실제 코드 못지 않게 중요하다.**
- **테스트 케이스가 없다면 모든 변경이 잠정적인 버그다.**
- **사실상 깨끗한 테스트 코드라는 주제는 책 한 권을 할애해도 모자랄 주제다.**

## **TIL (Today I Learned) 날짜**

- 2025-06-05

## **오늘 읽은 범위**

- 9장. 단위테스트

## **책에서 기억하고 싶은 내용을 써보세요.**

### TDD 법칙 세 가지

- 지금 즈음이면 TDD가 실제 코드를 짜기 전에 단위 테스트부터 짜라고 요구한다.
- 하지만 이 규칙은 빙산의 일각에 불과하다. 아래 세 가지 법칙을 살펴보자
1. **첫째 법칙:** 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
2. **둘째 법칙:** 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
3. **셋째 법칙:** 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.
- 위 세 가지 규칙을 따르면 개발과 테스트가 대략 30초 주기로 묶인다.
- 하지만 실제 코드와 맞먹을 정도로 방대한 테스트 코드는 심각한 관리 문제를 유발하기도 한다.

### 깨끗한 테스트 코드 유지하기

- 지저분한 테스트 코드를 내놓으나 테스트를 안 하나 오십보 백보라는, 아니 오히려 못하다
- 문제는 실제 코드가 진화하면 테스트 코드도 변해야 한다는 데 있다.
    - 테스트 코드가 지저분할수록 변경하기 어려워진다.
- 하지만, 테스트 슈트가 없으면 개발자는 자신이 수정한 코드가 제대로 도는지 확인할 방법이 없다.
    - 테스트 슈트가 없으면 시스템 이쪽을 수정해도 저쪽이 안전하다는 사실을 검증하지 못한다.(사이드 이팩트)
    - 그래서 결함률이 높아진다.
    - 의도하지 않는 결함 수가 많아지면 개발자는 변경을 주저한다.
    - 변경하면 특보다 해가 크다 생각해 도 이상 코드를 정리하지 않는다 → 코드가 망가지기 시작
- **테스트 코드는 실제 코드 못지 않게 중요하다.**

### 깨끗한 테스트 코드

- 깨끗한 테스트 코드를 만들려면 **가독성**이 중요하다.
- **테스트당 개념 하나:**
    - 테스트 함수마다 한 개념만 테스트하라
    - 이것저것 잡다한 개념을 연속으로 테스트하는 긴 함수는 피해야한다.
    - 새 개념을 한 함수로 몰아넣으면 독자가 각 절이 거기에 존재하는 이유와 각 절이 테스트하는 개념을 모두 이해해야 한다.

### F.I.R.S.T

- 깨끗한 테스트는 다음 다섯 가지 규칙을 따르는데, 각 규칙에서 첫 글자를 따오면 FIRST가 된다.
    - **빠르게(Fast):** 테스트는 빨라야 한다. 테스트는 빨리 돌아야 한다는 말이다.
    - **독립적으로(Independent):** 각 테스트는 서로 의존하면 안 된다.
    - **반복가능하게(Repeatable):** 테스트는 어떤 환경에서도 반복 가능해야 한다.
    - **자가검증하는(Self-Validating):** 테스트는 부울 값으로 결과를 내야 한다.
    - **적시에(Timely):** 테스트는 적시에 작성해야 한다. 단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현한다.

### 결론

- 사실상 깨끗한 테스트 코드라는 주제는 책 한 권을 할애해도 모자랄 주제다.
- 테스트 코드는 실제 코드 만큼이나 프로젝트 건강에 중요하다.
- 테스트 코드가 방치되어 망가지면 실제 코드도 망가진다.

## **궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요.**

## **오늘 읽은 다른사람의 TIL**

- **jaeelee님의 TIL:** [https://velog.io/@jaeelee/클린코드-과제-8](https://velog.io/@jaeelee/%ED%81%B4%EB%A6%B0%EC%BD%94%EB%93%9C-%EA%B3%BC%EC%A0%9C-8)
    - 비슷한 관점으로 TIL을 작성해주셔서 공감되었습니다.
- **broccoli님의 TIL:** [https://velog.io/@broccoliindb/객체와-자료구조](https://velog.io/@broccoliindb/%EA%B0%9D%EC%B2%B4%EC%99%80-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0)
    - 기차충돌 파트를 체이닝과 비교해서 설명해주신게 인상 깊었습니다.
- **Ki In Han님의 TIL**: https://cpusuite.blogspot.com/2025/06/clean-code-til-6-6.html
    - 절차 지향과 객체 지향의 장단점에 대해서 잘 정리해주셨습니다.
