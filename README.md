# TypeScript 란?
- TypeScript는 MS에 의해 개발/관리되고 있는 오픈소스 프로그래밍 언어.
- 대규모 애플리케이션을 개발하는 데 자바스크립트가 어렵고 불편하다는 불만에 대응하기 위해 개발됨.
- TypeScript는 ES5의 Superset이므로 기존의 자바스크립트(ES5) 문법을 그대로 사용할 수 있다.
- ES6의 새로운 기능들을 사용하기 위해 Babel과 같은 별도 트랜스파일러(Transpiler)를 사용하지 않아도 ES6의 새로운 기능을 기존의 자바스크립트 엔진(현재의 브라우저 또는 Node.js)에서 실행할 수 있다.

## 특징
- Javascript (동적언어) 같은 경우, 런타임에 타입이 결정되므로 오류 발견에 어려움이 있다.
- 반면, Java와 TypeScript (정적언어) 같은 경우, 컴파일 타임에 타입이 결정되어 동적언어 보다 명확한 오류를 출력해준다.
그리고 초기 코드 작성은 길어지지만 안정적이고 빠르게 작업이 가능하다는 장점이 있다.

## 기본 사용법
- 변수나 파라미터 뒤에 **콜론(:) + type** 을 써주면 됨. 배열같은 경우 **콜론(:) + type + []** 표기

예시)
```javascript
let age:number = 30;		// number type
let isAdult:boolen = true;		// boolean type
let a:number[] = [1,2,3];		// number type of array -1 
let a2:Array<number> = [1,2,3];	// number type of array -2

let week1:string[] = ['mon', 'tue', 'wed'];		// string type of array 
let week2:Array<string> = ['mon','tue','wed'];	// string type of array 
```

#### 튜플(Tuple)

```javascript
let b:[string, number];	// 배열의 첫번째 인자는 문자열, 두번째는 숫자 타입으로 지정
```

#### void, never 
- 함수에서 아무것도 반환하지 않을때는 :void 사용
```javascript
function sayHello():void{
	console.log('hello');	// 반환이 없이 콘솔로그만 출력하는 함수 일때 :void 사용
}
```
- 함수가 끝나지 않고 무한 루프일 경우 :never
```javascript
// case 1
function showError() {	// 항상 에러만 반환할때 예시
	throw new Error();
}

// case 2
function infLoop();never{	// 무한 루프일때 예시
	while (true) {
	}
}
```

#### enum
```javascript
enum Os {	// Os를 enum 으로 정의
	Window,	// 자동으로 번호가 매겨짐 -> Window = 1 , Ios = 2, Android = 3
	Ios,
	Android
}
```

#### null, undefined
```javascript
let a:null = null;
let b:undefined = undefined;
// 변수 a와 b를 null과 undefined로 정의 한 예시.
```

## 인터페이스(Interface)
- 인터페이스는 일반적으로 타입체크를 위해 사용되며 변수, 함수, 클래스에 사용할 수 있다.
인터페이스는 여러가지 타입의 프로퍼티로 새로운 타입을 정의하는 것과 유사하며, 정의된 인터페이스는 일관성을 유지하기 위해 내부에 선언된 프로퍼티 또는 메소드의 구현을 강제하는 특징이 있다.

- 인터페이스는 클래스와 유사하지만 인스턴스 생성이 불가능하고 모든 메소드는 추상 메소드로 이루어져 있습니다. 또한 인터페이스의 추상 메소드는 abstract 키워드를 사용하지 않는다는 특징이 있습니다. 또한 ES6에서 지원하지 않고 TypeScript에서만 지원합니다.  

예시)
```javascript
interface User {
	name: string;
	age: number;
}

let user : User = {
	 // interface에 정의된 name과 age를 안써줄 경우 에러가 나타남 
	name: 'xx', 
	age:30
}
```

#### 수정 및 추가 할 경우

예시)
```javascript
user.age = 10;
user.gender = "male"
```
> User Interface 내부에 gender 속성이 없으므로 추가 해줘야 한다. ( gender: string  추가 )

예시) Optional property
```javascript
interface User {
	name: string;
	age: number;
	gender? : string;
}

let user : User = {
	name: 'xx',
	age: 30
```
> 원래는 User Interface의 gender 속성을 user 내부에서 무조건 써줘야 하지만, ? (Optional property) 를 gender에 부여 함으로써 꼭 user에서 gender를 사용 안해줘도 된다.

#### read-only
- interface 내에서 readonly 로 앞에 적어주면 수정 불가. 읽기만 가능하게 만듬
- 즉, 최초에 생성할때만 값을 정하고 그 이후에 수정 및 변경 할 수 없다 

#### 문자열 인덱스
- Interface 내에서 [ grade:number] : string, 이면 grade는 number를 key로 하고, string을 value로 지정한다는 의미. 

#### 문자열 리터럴 타입
- 단순히 [ grade: number] : string 이면 grade에 대한 범위가 모든 string type 이므로 광범위하다. 이것을 문자열 리터럴 타입을 선언 해줌으로써 해결이 가능하다.
```javascript
type Score = 'A' | 'B' | 'C' | 'D' // Score type은 A,B,C,D 만을 받겠다는 의미
```

최종 예시)
```javascript
type Score = 'A' | 'B' | 'C' | 'F'; // Score type (리터럴 타입)

ineterface User {
	name: string;
	age: number;
	gender? : string;
	readonly birthYear : number;
	[grade:number] : Score; // 문자열 인덱스 + 문자열 리터럴 타입
}							// grade 의 Key값은 number type, Value값은 Score Type. 

let user : User = {
	name: 'xx',
	age: 30,
	birthYear : 2000,
	// gender : 'male'		// gender은 optional property 이므로 굳이 안써줘도 에러가 안난다.
	1 : 'A',
	2 : 'B'
} 
```

#### 인터페이스로 함수 정의 
```javascript
interface Add {
	(num1:number, num2:number): number; // num1, num2 의 인자값은 number type 그리고 반환값도 number type
}

const add: Add = function(x,y) { // add 함수는 :Add Interface를 붙여줌으로써 Add Interface의 형식을 따라야 에러가 안난다.
	return x + y;
}

add(10, 20);	// result 30
```

```javascript
interface IsAdult {
	(age:number):boolean; // age는 number type 그리고 반환값은 boolean type (t or f)
}

const a:IsAdult = (age) => {
	return age > 19;
}

a(33); // True
```

#### 인터페이스로  클래스 정의  및 상속 ( implements )
```javascript
interface Car {
	color: string;
	wheels: number;
	start():void;
}

class Bmw implements Car {	//Bmw 클래스는 Car interface를 상속(implements) 받아 사용
	color = "red";
	wheels = 4;
	start() {
		console.log('go..');
	}
}
```

#### 인터페이스 확장 ( extends )
- 기존 정의된 인터페이스에서 인터페이스의 추가적인 확장이 가능하다 ( 다수 인터페이스 확장 가능)
```javascript
interface Car {  //기본 인터페이스
	color:string;
	wheels:number;
	start():void;
}

interface Benz extends Car { // 기존 Car interface의 확장 , 확장은 여러개가 가능하다
	door: number;
	stop(): void
}

const benz : Benz = { 

} // benz는 Benz와 Car의 속성 값을 모두 사용해야 에러가 안난다.
```

## 함수 타입 정의
- 기본 정의
```javascript
function add(num1: number, num2: number) : number { //num 1, num2 그리고 반환값은 number 타입
	return num1 + num2;
}
```

#### 매개변수에 optional type 지정
예시)
```javascript
function hello(name?: string) {
	return `Hello, ${name || "world"}`;	// 명시적으로 정의 되어있지만, name 매개변수에 optional property를 부여해줘서
}										// 보다 명확한 명시적 정의를 해줘야 result가 hello함수를 실행 할 수 있게 해준다.

const result = hello();
// 즉, name 뒤에 optional type인 ?를 명시해주면, 위와 같이 빈 매개변수의 함수를 호출했을때 에러가 안난다.
// 또한 만약 name 을 쓸 경우에는 name은 string 타입을 지켜줘야 한다.
```

예시) 자바스크립트일 경우?
```javascript
function hello(name="world") {
	return `Hello, ${name}`;
}
```
> 자바스크립트에선 매개변수의 default 값 설정이 가능하고 자동으로 default값에 따라 type이 지정된다.

#### Optional Type 의  위치 ?
- 원래는 명시적 선언 뒤에 무조건 optional type의 매개변수가 오는게 맞다
```javascript
( name: string, age?:number)  // name은 string, age는 있어도 되고 없어도 되지만 string type을 받는 매개변수 형태.
```

-  하지만 먼저 optional 매개변수가 오고, 그 뒤에 명시적 타입의 매개변수가 오는 경우는, 단순히 위치만 바꾸는게 아닌
```javascript
( age?:number , name: string)
```
보다 명확하게 underfined 적어주어 명시해줘야 에러가 안난다.
```javascript
( age: number | undefined, name: string) 

console.log(hello(undefined, "Sam")
// number or undefined 으로 명시해주고 호출할때도 undefined을 써줘야 에러가 안난다.
```

#### 나머지 타입 매개변수 작성법
```javascript
function add(...nums: number[]) {							// ...nums 는 매개변수를 배열형태로 나타내는 것을 의미.
	return nums.reduce((result, num) => result + num, 0);	// 이것또한 :number[] 를 붙여주어 정확한 배열타입을 명시해줘야 에러가 안난다.
}

add(1,2,3);			//6
add(1,2,3,4,5,6)	//55
```

#### this 타입
```javascript
interface User {  // 인터페이스는 User
	name: string;
}

const Sam: User = {name: 'Sam'} //Sam은 User interface 형식을 따름

function showName(this:User, age: number, gender:'m' | 'f'){		// this.name을 출력하기 위해선 this에 대한 타입을 명시해줘야 된다.
	console.log(this.name, age, gender)		    					// 여기선 this:User 로 명시, 또한 this의 타입 선언은 매개변수에서 첫번째에서 해줘야 한다.
}

const a = showName.bind(Sam);
a(30, 'm')		// this:User 이후에 age 와 gender 타입에 해당되는 매개변수이다.
```

#### 함수 오버로드 
```javascript
function join(name: string, age: number | string): User | string {
	if (typeof age === 'number') {
		return {
			name,
			age,
		};
	} else {
		return "나이는 숫자로 입력해주세요. ";
	}
}

const sam: User = join("Sam", 30);
const jane: string = join("Jane", "30");
```
- 위 예시는 에러가 난다.
- 이럴 땐, ' age가 number이거나 string 일 때 User 나 string을 반환 해준다. ' 를 오버로드 작성해줘야 한다.

```javascript
function join(name: string, age: string): string;	//추가
function join(name: string, age: number): User; 	//추가
function join(name: string, age: number | string): User | string {
...
}
// 즉, 매개변수의 타입이나 갯수에 따라 다른방식으로 동작할때 오버로드 해줘야 함  
```
> 위 함수를 기존 join함수 위에 작성해주면 된다.
