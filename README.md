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
