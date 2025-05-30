# MISSION 01

## MISSION: 더러운 코드를 고쳐라!

아래 가이드를 보고 총 3개의 더러운 코드를 깨끗하게 고쳐 보세요.

### QUIZ 01.

- Hint❕ : 검색하기 쉬운 이름을 사용하세요.
- blastOFF는 로켓 발사를 의미. 86400000은 하루의 밀리초 (milliseconds) 의미.

```jsx
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);

// GOOD 😎
// 위 코드를 깨끗하게 다시 작성해 주세요.
const dayMillisecond = 86400000;

/**
 * 하루 뒤에 로켓을 발사하는 함수
*/
function blassOffAfterOneDay () {
	setTimeout(blassOff, dayMillisecond);
}

blassOffAfterOneDay();

// 어떻게 고쳤는지, 사례에서 무엇을 배워야 하는지 설명해주세요.
// 1. 검색하기 쉬운 코드를 만들기 위해 직관적인 함수 명칭과 함께 함수로 만들었습니다.
// 2. 86400000 이란 숫자를 검색하기엔 외우기 힘든 텍스트이므로 이를 dayMillisecond라는 직관적인 명칭으로 바꿨습니다.
```

### QUIZ 02.

- Hint❕ : 의미있는 이름을 사용해 주세요.

```jsx
const yyyymmdstr = moment().format("YYYY/MM/DD");

// GOOD 😎
// 위 코드를 깨끗하게 다시 작성해 주세요.

const dateFormattedAsMoment = moment().format("YYYY/MM/DD");

// 어떻게 고쳤는지, 사례에서 무엇을 배워야 하는지 설명해주세요.
// 1. 변수의 명칭을 할당되는 값을 이해하기 명확한 명칭으로 수정하였습니다.
// 이를 통해 다른 작성자들이 명칭만 봐도 어떤 변수인지 파악하기 쉽도록 개선하였습니다.
```

### QUIZ 03.

- Hint❕ : 불필요하게 반복하지 마세요.

```jsx
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}

// GOOD 😎
// 위 코드를 깨끗하게 다시 작성해 주세요.

const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};
/**
 * 차의 옵션을 설정하는 함수
 * @params {string} option - 설정할 옵션
 * @params {string} changeValue - 설정할 값
*/
function setCarOption(option, changeValue) {
	car[option] = changeValue;
}

// 어떻게 고쳤는지, 사례에서 무엇을 배워야 하는지 설명해주세요.
// 1. 각 옵션별로 함수를 만드는게 아니라 매개변수를 통해 여러 옵션을 수정할 수 있는 공통 함수를 만들었습니다.
// 2. 가능하면 중복된 코드는 줄이는게 좋으므로 공통된 로직은 하나의 함수로 만드는게 더 좋습니다.
```
