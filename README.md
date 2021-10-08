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
function add(...nums: number[]) {				// ...nums 는 매개변수를 배열형태로 나타내는 것을 의미.
	return nums.reduce((result, num) => result + num, 0);	// 이것또한 :number[] 를 붙여주어 정확한 배열타입을 명시해줘야 에러가 안난다.
}

add(1,2,3);		//6
add(1,2,3,4,5,6)	//55
```

#### this 타입
```javascript
interface User {  // 인터페이스는 User
	name: string;
}

const Sam: User = {name: 'Sam'} //Sam은 User interface 형식을 따름

function showName(this:User, age: number, gender:'m' | 'f'){		// this.name을 출력하기 위해선 this에 대한 타입을 명시해줘야 된다.
	console.log(this.name, age, gender)		    		// 여기선 this:User 로 명시, 또한 this의 타입 선언은 매개변수에서 첫번째에서 해줘야 한다.
}

const a = showName.bind(Sam);
a(30, 'm')								// this:User 이후에 age 와 gender 타입에 해당되는 매개변수이다.
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


## 리터럴 타입

예시 1 )
```javascript
const userName1 = "Bob";	// Bob 외의 값은 받을 수 없다. 이런 경우 문자열 리터럴 타입 이라고 한다.
let userName2 = "Tom"		// userName2 는 let이므로 string type 아무거나 가능.
```

예시 2 )
```javascript
type Job = "police" | "developer" | "teacher";	 //Job 의 타입은 3 가지

interface User {
	name: string;
	job: Job;				//Job의 value는 Job의 속성
}

const user: User = {
	name : "Bob",
	job: "teacher" 	// 여기서 job 속성은 위에 정의된 3개의 Job 속성중에서만 선택할 수 있다.
}
```

#### 식별 가능한 유니온 타입 ( Union Type , A or B )

예시 1 )
```javascript

interface Car {
	name: "car";		// 동일한 name속성 이지만 다른 타입을 줌으로써 interface를 식별
	color: string;		// 여기서는 name속성의 타입은 car
	start(): void;
}

interface Mobile {
	name: "mobile";		// 여기서는 name속성의 타입은 mobile
	color: string;
	call(): void;
}

function getGift(gift: Car | Mobile) {
	console.log(gift.color);	// color속성은 둘다 string타입으로 정의 되었으므로 출력가능.

	// ex) gift.start();		// Error) start()는 Car인터페이스 내부에만 있으므로 아래와 같이 조건문으로 구분해줘야 함.

	if(gift.name === "car"){	// gift.name 이 car 이면 gift.start() 실행 , 아니면 gift.call() 실행
		gift.start();
	} else {
		gift.call();
	}
}
```

#### 교차 타입 ( Intersection Type , A and B )

예시 1 )
```javascript
interface Car {
	name: string;
	start(): void;
}

interface Toy {
	name: string;
	color: string;
	price: number;
}

const toyCar: Toy & Car = {		  // Toy 와 Car 타입 둘다 사용한다를 의미
	name: "타요",			// 그러므로 Car 와 Toy 의 속성을 모두 적어줘야만 에러가 안난다.
	start() {},
	color: "blue",
	price: 1000,
};
```

## 클래스 선언

예시)
```javascript
class Car {
	color: string;					// 멤버변수를 미리 선언해야한다. 여기선 color : string (멤버변수 선언 방법)
	constructor(color: string) {      
  //constructor(public color:string {			// 아니면 constructor 내의 color : string 앞에 public 이나 readonly를 적어줘도 됨.
		this.color = color; // ( 접근 제한자 방법 )
	}
	start() {
		console.log("start");
	}
}

const bmw = new Car("red");
```

#### 접근 제한자 ( public, private, protected )

1. public - 자식 클래스 , 클래스 인스턴스 모두 접근이 가능
2. protected - 자식 클래스 내부에서는 참조 가능, 하지만 클래스 인스턴스에선 사용 불가
3. private ( 앞에 # 표기 ) - 해당 클래스 내부에서만 접근 가능 , 자식 클래스에서는 사용 불가
4. readonly - 수정, 변경 불가

#### 정적 선언 ( static )
- 앞에 static 이 붙여진 정적 멤버변수나 메소드는 this. 로 접근하는게 아닌, 클래스명. 으로 접근해야 한다.

```javascript
class Car {
	static wheels = 4 ;
}

console.log(this.wheels);	// X
console.log(Car.wheels);	// O
```

#### 추상 클래스 ( abstract )
- 클래스 앞에 abstract를 붙여주어 추상 클래스로 만들어 줄 수 있다.
- 추상된 클래스는 new를 사용할 수 없고, **상속**에 의해서는 사용 될 수 있다.

예시 )
```javascript
abstract class Car {
	color: string;
	constructor(color: string) {
		this.color = color;
	}
	start() {
		console.log("start");
	}
	abstract doSomething():void;		// 추상 클래스 내부의 추상 메소드는 상속받는 클래스에서 구체적이게 정의해줘야 함.
}						// 여기선 단순히 함수명과 반환타입만 기재하면 된다.

// const car = new Car("red");			// Error; new를 사용할 수 없다.

class Bmw extends car {
	constructor(color: string) {
		super(color);
	}
	doSomething(){			// 추상 메소드의 이름은 같지만 구체적인 기능은 상속 받은 클래스마다 다르게 정의 될 것 이다.
		alert(3);		// Bmw 클래스 내부의 doSomething() 은 alert(3) 을 실행하는 기능이 정의 되어있다.
	}
}
```

## 제네릭 ( generic )
- 제네릭은 클래스, 인터페이스 그리고 함수 등을 다양한 타입으로 재사용 가능하게 한다.

예시 ) 일반적 교차 타입을 사용

```javascript
function getSize(arr: number[]): number {			//getSize함수는 arr.length를 number type으로 반환
	return arr.length;
}

const arr1 = [1,2,3];						// array는 number[] 타입으로 설정해줘서 에러가 없다.
getSize(arr1);

const arr2 = ["a","b","c"];					// arr2 는 string 배열 타입
getSize(arr2);							// getSize(arr: number[] | string[] ) 으로 표시 해줘야됨 (교차 타입)
```

예시 ) 1. 제네릭 사용 - 함수 예시
```javascript
function getSize<T>(arr : T[] ) : number {		// <T> 를 해주고 (arr : T[]), 즉 arr는 T타입의 배열 
	return arr.length;				// T는 사용하는 쪽에서 타입을 정해줘야 한다.
}

const arr1 = [1,2,3];
getSize<number>(arr1);				// getSize<number>(arr1); 즉 arr1 배열은 number 타입의 배열이다. 를 getSize() 함수에 전달

const arr2 = ["a", "b", "c"];			// Javascript 에선 안적어줘도 전달되는 매개변수의 타입이 무었인지 함수에 자동으로 전달해준다.
getSize(arr2);					// == getSize<string>(arr2) 
```

예시 ) 2. 제네릭 사용 - 인터페이스 예시 
```javascript
interface Mobile<T> {
	name: string;
	price: number;
	option: T;				// option 의 타입이 정해지지 않아 T로 설정 
}

const m1: Mobile<object> = {	// m1 의 option 타입은 object이므로 T 타입은 object로 전달 된다.		
	name: "s21",				// 아니면, object 내부의 타입이 정해져 있다고 하면,
	price: 1000,				// <{color: string; coupon: boolean }> 이렇게 전달 해 줘도 된다.
	option: {
		color: "red",
		coupon: false,
	},
}

const m2: Mobile<string> = {	// m2의 option 타입은 string이므로 T 타입은 string으로 전달 된다.
	name: "s20",
	price: 900,
	option: "good",
};
```

## 유틸리티 타입

#### 1. keyof
```javascript
interface User {
	id: number;
	name: string;
	age: number;
	gender: "m" | "f";
}

type UserKey = keyof User;			// 'id' | 'name' | 'age' | 'gender' 와 같다.
						// 즉 User 인터페이스의 키 값들을 유니온 형태로 받을 수 있다 	
const uk:UserKey = "name"
```

#### 2. Partial< T >
- 속성들을 모두 optional로 바꿔준다.
```javascript
interface User {
	id: number;
	name: string;
	age: number;
	gender: "m" | "f";
}

let admin: Partial<User> = {			// admin에서 User를 Partial로 받게 되면, User내부의 속성들은 모두 optinal property로 받아오게된다.
	id: 1,					// id?, name?, age?, gender? 형태
	name: "Bob",
 // job: ""					// Error) job은 User 인터페이스 속성에 있지 않으므로 에러가 난다.
};
```

#### 3.Required < T >
- optional property 를 필수 property 로 만드는 것.
```javascript
interface User {
	id: number;
	age? : number;
}

let admin: Required<User> = {		// Required 로 User를 감싸주면, optional property인 age? 를 무조건 써줘야 에러가 안난다.
	id: 1,
	age : 30
}
```

#### 4.Readonly < T >
- 처음 인터페이스 속성 할당만 가능, 그 이후에 수정은 불가능

#### 5.Record < K, T >
- K는 Key, T는 Type

예시) 1. 기본 Record 활용
```javascript 
type Grade = "1" | "2" | "3" ;
type Score = "A" | "B" | "C" ;

const score: Record<Grade, Score> = {		// Record를 활용하여 Key 값은 Grade, Value 값은 Score로 지정해 줄 수 있다. 
	1: "A",
	2: "C",
	3: "B",
	4: "D",
};
```

예시) 2. Record 와 keyof 혼용
```javascript
interface User {
	id: number;
	name: string;
	age: number;
}

function isValid(user: User) {
	const result: Record<keyof User, boolean> = {		// Record의 Key값은 keyof로 User의 키값을 유니온 형태로 받아온다.
		id: user.id > 0,				// Value값은 boolean 형태로 설정
		name: user.name !== "",
		age: user.age > 0,
	};
	return result;
}
```

#### 6.Pick< T, K > 와 Omit < T, K >
- Pick은 T 내에서 Key 값을 선택하여 사용할 수 있는 기능
- Omit 은 T 내에서 Key 값을 배제 하여 사용할 수 있는 기능

예시 )
```javascript
interface User {
	id: number;
	name: string;
	age: number;
	gender: "M" | "W";
}

const admin: Pick<User, "id" | "name"> = {		// User 인터페이스에서 id와 name 의 Key값만을 사용하겠다는것을 의미.
	id: 0,
	name: "Bob",
};

const admin: Omit<User, "age" | "gender"> = {} 		// User 인터페이스에서 age와 gender 의 Key값은 배제하고 나머지를 사용하겠다는 것을 의미
```

#### 7.Exclude<T1, T2>
- T1 에서 T2 를 제외하고 사용하는 것을 의미. 즉, T1 의 타입들 중에서 T2 타입과 겹치는 것을 제외 시킨다.

예시 )
```javascript
type T1 = string | number;
type T2 = Exclude<T1, number>
// T1의 number와 겹치므로 제거. 결론적으로 type T2 = string 타입이 된다.
```

#### 8.NonNullable< Type >
- Null 과 Undefined 를 함께 제외 시킨다.

예시)
```javascript
type T1 = string | null | undefined | void;
type T2 = NonNullable<T1>
// T2는 NonNullable로 T1을 받아오므로 null 과 undefined를 제외한 string과 void만 받아옴.
// 즉, type T2 = string | void
```
