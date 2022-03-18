# Access Control(접근 제어)

[Access Control - The Swift Programming Language (Swift 5.6)](https://docs.swift.org/swift-book/LanguageGuide/AccessControl.html#//apple_ref/doc/uid/TP40014097-CH41-ID3)

다른 소스 파일 및 모듈의 코드에서 코드에 대한 접근을 제한한다. 코드 구현의 세부 정보를 숨기고, 해당 코드를 접근하고 사용할 수 있는 인터페이스를 지정한다.

개별 유형(클래스, 구조체, 열거형)과 이 유형에 해당하는 프로퍼티, 메소드, 이니셜라이저 그리고 서브스크립트들의 특정 접근 수준을 할당할 수 있다. 프로토콜은 전역 상수, 변수 혹은 함수와 마찬가지로 확실한 context로 제한될 수 있다.

거기다 다양한 수준의 접근 제어를 제공하는 것 외에도, Swift는 전형적인 시나리오에 대한 기본 접근 수준을 제공함으로써 접근 제어 수준을 구체적으로 명시할 필요성을 줄인다. 실제로 만약 단일 대상 앱을 작성한다면, 접근 제어 수준을 전혀 명시할 필요가 없을 수 있다.

> **참고**<br>
접근 제어를 적용할 수 있는 코드의 다양한 측면(프로퍼티, 유형, 함수 등)은 간결함을 위해 아래 섹션에서 엔티티라고 언급된다.


# 모듈과 소스 파일

Swift의 접근 제어 모델은 모듈과 소스 파일의 개념에 기초한다.

모듈은 코드를 배포하는 하나의 단위다. 프레임 워크 혹은 애플리케이션으로, 하나의 단위로 빌드되고 사용되며 다른 모듈에서는 Swift의 import 키워드를 사용하여 import될 수 있다.

Xcode에서 앱 번들과 프레임워크와 같은 각 빌드 타겟은 Swift에서 하나의 분리된 모듈처럼 다뤄진다. 여러 앱에서 그 코드를 캡슐화하고 재사용하기 위해 만약 앱의 여러 측면을 독립실행형 프레임워크처럼 그룹화 하면, 해당 프레임워크 내에서 정의한 모든 것들은 앱 내에서 가져와서 사용되거나 다른 프레임워크 내에서 사용될 때 별도의 모듈의 일부가 된다.

소스 파일은 모듈(앱 혹은 프레임워크) 내에서 단일 Swift 소스 코드 파일이다. 비록 별도의 소스 파일에 개별 유형을 정의하는 것이 일반적이지만, 하나의 단일 소스에 여러 유형, 함수 등을 선언할 수 있다.

# 접근 수준

Swift는 코드 내에서 엔티티들을 위해 다섯개의 다른 Access Level들을 제공한다. 이러한 Access Level은 엔티티가 선언된 소스 파일과 소스 파일이 속하는 모듈과 관련되어 있다.

- Open, Public
    - 모듈이 정의된 모든 소스 파일에서 엔티티들에 접근 가능
    - 정의된 모듈을 import 하는 다른 모듈의 소스 파일에서도 엔티티들에 접근 가능
    - 프레임워크에 대한 공개 인터페이스를 지정할 때 open 혹은 public 접근을 사용
    - Open과 Public의 차이
        - Open
            - class들과 class의 멤버들에 적용
            - 모듈의 외부 코드를 서브클래스화하고 오버라이드할 수 있음
            - 가장 높은(가장 덜 제한적인) 접근 수준
            - 해당 클래스를 superclass로 사용하는 것을 고려한 경우
        - Public
            - 모듈의 외부 코드를 서브클래스화하고 오버라이드할 수 없음
- Internal
    - 정의된 모듈의 모든 소스파일 내에서 사용 가능
    - 모듈 밖의 소스파일에서는 사용 불가
    - 앱 혹은 프레임워크의 내부 구조체를 선언할 때 일반적으로 사용
- File-private
    - 정의된 소스파일 내에서만 사용 가능
    - 전체 파일 내에서 세부 사항들이 사용될 때 특정 기능의 세부 구현을 숨기고 싶은 경우 사용
- Private
    - 같은 파일의 선언이 닫히는 부분까지 사용 제한
    - 같은 파일의 선언의 확장(extension)까지 사용 제한
    - 하나의 선언 내에서만 사용되는 세부 사항일 때 기능의 특정한 조각의 세부 구현을 숨기기 위해 사용
    - 가장 낮은(제한적인) 접근 수준

## 접근 수준에 대한 기본 원칙

Swift에서 접근 수준은 전반적인 기본 원칙을 따른다.

어떠한 엔티티도 더 낮은(제한적인) 접근 수준을 가지는 다른 엔티티를 정의할 수 없다.

예시

- public 변수는 internal, file-private 혹은 private 유형을 가지도록 정의될 수 없다.
    
    그 유형은 public 변수가 사용되는 모든 곳에서 사용이 불가능하기 때문이다.
    
- 함수는 매개변수 타입이나 반환 타입보다 더 높은 접근 수준을 가질 수 없다.
    
    함수는 주변 코드에서 구성 유형을 사용할 수 없는 상황에서 사용될 수 있기 때문
    

언어의 다양한 측면에 대한 기본 원칙의 구체적인 구현은 아래에서 자세히 설명된다.

## 기본 접근 수준

(이 장의 뒷부분에서 설명하는 일부 특정 예외들을 제외한) 코드 내의 모든 엔티티들은 명확한 접근 수준을 직접 지정하지 않는 한 기본 internal 접근 수준을 가진다. 결과적으로 많은 경우 코드에서 명확한 접근 수준을 지정할 필요가 없다.

## 단일 대상 앱용 접근 수준

만약 하나의 간단한 단일 대상 앱을 작성할 경우, 앱의 코드는 일반적으로 앱 내에 자체에 포함되고 앱 모듈 외부에서 사용 가능하게 만들 필요가 없다. internal 기본 접근 수준은 이미 이 요구사항을 충족한다. 그러므로 다른 사용자 정의 접근 수준을 명시할 필요가 없다. 하지만 앱 모듈 내에서 다른 코드로부터 세부 구현을 숨기기 위해서 코드의 특정한 부분을 file-private 혹은 private로 표시할 수 있다.

## 프레임워크용 접근 수준

프레임워크를 개발할 때, 프레임워크를 import하는 앱 같이 다른 모듈에서 보고 접근할 수 있도록 해당 프레임워크에 대한 공개 인터페이스를 public 혹은 open으로 표시한다. 이 공개 인터페이스는 프레임워크용 API이다.

> **참고**<br>
프레임워크 내부 세부 구현은 여전히 internal 기본 접근 수준을 사용할 수 있고, 프레임워크 internal 코드의 파트들로부터 세부 구현들을 숨기고 싶을 때 private 혹은 file-private으로 표시할 수 있다. 프레잌워크 API의 일부가 되길 원하는 경우에만 open 혹은 public으로 표시해야 한다.


## 단위 테스트용 접근 수준

앱의 단위 테스트 타겟을 작성할 때, 앱 내의 코드를 테스트하기 위해 그 모듈에 접근하도록 만들어야 한다. 기본적으로 open 혹은 public으로 표시된 엔티티들만 다른 모듈에 접근할 수 있다. 하지만 단위 테스트의 경우 만약 `@testable` 속성으로 모듈에 대한 import 선언을 표시하고, 테스트 가능한 제품 모듈을 컴파일하면, 단위 테스트 대상은 모든 내부 엔티티에 접근할 수 있다.

# 접근 제어 구문

엔티티 선언의 시작 부분에 open, public, internal, fileprivate, 혹은 private modifier 중 하나를 배치하여 엔티티에 대한 접근 수준을 정의한다.

```swift
public class SomePublicClass {}
internal class SomeInternalClass {}
fileprivate class SomeFilePrivateClass {}
private class SomePrivateClass {}

public var somePublicVariable = 0
internal let someInternalConstant = 0
fileprivate func someFilePrivateFunction() {}
private func somePrivateFunction() {}
```

달리 지정하지 않는 한 기본 접근 수준은 internal이다. 이것은 `SomeInternalClass`와 `someInternalConstant`는 접근 수준 한정자 명시 없이 적을 수 있고 그것은 동일하게 internal 접근 수준을 가진다.

```swift
class SomeInternalClass {}   // implicitly internal
let someInternalConstant = 0 // implicitly internal
```

# 사용자 정의 유형

만약 사용자 정의 유형용 접근 수준을 명시하고 싶다면, 타입을 정의하는 지점에서 지정하면 된다. 그러면 새 유형은 접근 수준이 허용하는 모든 곳에서 사용될 수 있다. 예를 들어 file-private 클래스 하나를 정의했다면, file-private 클래스가 정의된 소스파일 내의 프로퍼티 타입, 함수 매개변수 혹은 반환 타입으로 사용될 수 있다.

한 유형의 접근 수준은 유형의 멤버(프로퍼티, 메소드, 이니셜라이저, 서브스크립트)의 기본 접근 수준에 영향을 미친다. 만약 private 혹은 file-private으로 유형의 접근 수준을 선언했다면, 그 유형의 멤버의 기본 접근 수준 또한 private 혹은 file-private가 될 것이다. 만약 한 유형의 접근 수준을 internal 혹은 public(혹은 특정한 접근 수준을 명시하지 않고 기본 접근 수준인 internal)으로 했다면, 그 유형 멤버들의 기본 접근 수준은 internal이 된다.

> **중요**<br>
public 유형은 기본적으로 public 멤버가 아닌 internal 멤버들을 가진다. 만약 멤버를 public으로 만들고 싶다면, 마찬가지로 public을 명시해야 한다. 이 요구사항은 유형에 대한 공개 API가 게시를 선택하는 것임을 확인하고, 실수로 유형의 내부 작업을 공개 API로 표시하는 것을 방지한다.


```swift
// explicitly public class
public class SomePublicClass {   

	// explicitly public class member
	public var somePublicProperty = 0 

	// implicitly internal class member    
	var someInternalProperty = 0       

	// explicitly file-private class member
	fileprivate func someFilePrivateMethod() {}   

	// explicitly pirvate class member
	private func somePrivateMethod() {}           

}

// implicitly internal class
class SomeInternalClass {
		
	// implicitly internal class member
	var someInternalProperty = 0
		
	// explicitly file-private class member
	fileprivate func someFilePrivateMethod() {}

	// explicitly private class member
	private func somePrivateMethod() {}

}

// explicitly file-private class
fileprivate class SomeFilePrivateClass {

	// implicitly file-private class member
	func someFilePrivateMethod() {}
		
	// explicitly private class member
	private func somePrivateMethod() {}

}

// explicitly private class
private class SomePrivateClass {
		
	// implicitly private class member
	func somePrivateMethod() {}

}
```

## 튜플 유형

튜플 유형용 접근 수준은 튜플에서 사용되는 모든 유형 중 가장 제한적인 접근 수준이다. 예를 들어 만약 튜플이 internal 유형과 private 유형으로 구성되어 있다면, 그 복합 튜플 유형의 접근 수준은 private이 될 것이다.

> **참고**<br>
튜플 유형은 클래스, 구조체, 열거형 그리고 함수와 같은 방식으로 독립형 선언이 아니다. 튜플 유형의 접근 수준은 그 튜플 유형을 이루고 있는 유형들로부터 자동적으로 결정되고 명시적으로 지정할 수 없다.


## 함수 유형

함수용 접근 수준은 함수의 매개변수 유형과 반환 유형 중 가장 제한적인 접근 수준로 계산된다. 만약 함수의 계산된 접근 수준이 문맥상 기본값과 같지 않다면 함수 정의에서 명시적으로 접근 수준을 지정해야 한다. 

아래 예시에서 `someFunction()` 전역 함수가 함수용 특정 접근 수준 한정자를 지정하지 않은 채 정의되어 있다. 해당 함수가 internal 기본 접근 수준을 가질 것이라 예상할 수도 있지만, 이 경우에는 그렇지 않다. 사실 `someFunction()`은 아래와 같이 컴파일 되지 않는다.

```swift
func someFunction() -> (SomeInternalClass, SomePrivateClass) {
	// function implementation goes here
}
```

이 함수의 반환 값은 위 사용자 정의 유형에서 정의된 두 사용자 정의 클래스로 구성되어 있는 튜플 유형이다. 이 클래스들 중 하나는 internal으로 나머지 하나는 private으로 선언되어 있다. 그러므로 복합 튜플 유형의 전체 접근 수준은 private(튜플의 구성 유형 중 가장 낮은 접근 수준)이다.

함수의 반환 값이 private이기 때문에, 유효한 함수 정의를 위해서는 함수의 전체 접근 수준을 private으로 지정해야 한다.

```swift
private func someFunction() -> (SomeInternalClass, SomePrivateClass) {
	// function implementation goes here
}
```

함수의 public 혹은 internal 사용자가 함수의 반환 유형에 사용된 private 클래스에 대해 적절한 접근을 가지지 않을 수 있기 때문에, public 혹은 internal 한정자를 `someFunction()` 정의에 표시하거나, 기본 설정인 internal을 사용하는 것은 유효하지 않다. 

## 열거형

열거형의 개별 case들은 자동적으로 자신이 속해있는 접근 수준에 따른다. 각 열거형 case들에 다른 접근 수준을 지정할 수 없다.

아래 예시에서 `CompassPoint` 열거는 명시된 public 접근 수준 가진다. 그러므로 열거 case들인 north, south, east 그리고 west 또한 public 접근 수준을 가진다.

```swift
public enum CompassPoint {
	case north
	case south
	case east
	case west
}
```

# 원시값(Raw Values)와 연관값(Associated Values)

열거형 정의에서 원시값 혹은 연관값의 유형은 최소한 열거형의 접근 수준만큼 높은 접근 수준을 가져야 한다. 예를 들어 접근 수준 internal인 열거형의 원시값에 private 접근 수준을 사용할 수 없다.

→ 열거형의 원시값 혹은 연관값의 접근 수준은 열거형의 접근 수준보다 덜 제한적이여야 한다.

## 중첩 유형(Nested Types)

중첩 유형의 접근 수준은 포함하는 유형이 public이 아닌 한 포함한 유형의 접근 수준과 동일하다. public 유형 내에서 정의된 중첩 유형은 자동으로 internal 접근 수준을 가진다. 만약 public 유형 내의 중첩 유형을 공개적으로 사용하고자 한다면, 중첩 유형을 public으로 명확하게 선언해야 한다.

# 서브클래싱(Subclassing)

현재 접근 맥락에서 접근할 수 있고 동일한 모듈에 정의된 모든 클래스의 하위 클래스를 만들 수 있다. 다른 모듈에서 정의된 open 클래스 또한 하위 클래스를 만들 수 있다. 하위 클래스는 상위 클래스보다 더 높은 접근 수준을 가질 수 없다. 예를 들어 internal 상위 클래스의 하위 클래스로 public 클래스를 작성할 수 없다.

게다가, 특정한 접근 상황에서 같은 모듈에 정의된 클래스들의 경우 접근 가능한 클래스 멤버(메소드, 프로퍼티, 이니셜라이저 혹은 서브스크립트)는 오버라이드 할 수 있다. 다른 모듈에서 정의된 클래스들은 open 클래스 멤버를 오버라이드할 수 있다.

오버라이드는 상속된 클래스 멤버에 대한 권한을 상위 클래스 버전보다 높게 설정할 수 있다. 아래 예시에서 클래스 A는 `someMethod()`라는 file-private 메소드를 가진 public 클래스이다. 클래스 B는 A의 하위 클래스로, 더 낮은 접근 수준인 internal을 가진다. 단, 클래스 B는 기존`someMethod()`의 구현보다 더 높은 접근 레벨인 internal로 `someMethod()`를 오버라이드한다.

```swift
public class A {
	fileprivate func someMethod() {}
}

internal class B: A {
	override internal func someMethod() {}
}
```

허용 가능한 범위(file-private 멤버 호출의 상위 클래스와 같은 소스파일 내 혹은 internal 멤버 호출의 상위 클래스와 같은 모듈 내) 내에서 상위 클래스 멤버에 대한 호출이 이뤄지는 경우 하위 클래스 멤버는 하위 클래스 멤버보다 더 낮은 접근 권한을 가지는 상위 클래스 멤버를 호출할 수 있습니다.

```swift
public class A {
	fileprivate func someMethod() {}
}

internal class B: A {
	override internal func someMethod() {
		super.someMethod()
	}
}
```

상위 클래스 A와 하위 클래스 B는 같은 소스 파일 내에 선언되어 있기 때문에, `super.someMethod()`를 호출하는 B의 `someMethod()` 구현이 유효하다.

# 상수, 변수, 프로퍼티 그리고 서브스크립트

상수, 변수, 혹은 프로퍼티는 그것의 유형보다 더 공개될 수 없다. 예를 들어 private 유형의 public 프로퍼티를 작성할 수 없다. 비슷하게 서브스크립트는 인덱스 유형 혹은 반환 유형보다 더 공개될 수 없다.

만약 상수, 변수, 프로퍼티 혹은 서브스크립트가 private 유형을 사용한다면, 상수, 변수, 프로퍼티 혹은 서브스크립트는 private으로 표기해야 한다.

```swift
private var privateInstance = SomePrivateClass()
```

## 접근자(Getters)와 설정자(Setters)

상수, 변수, 프로퍼티, 그리고 서브스크립트의 접근자와 설정자는 자동적으로 상수, 변수, 프로퍼티 혹은 서브스크립트가 속해있는 접근 수준과 동일하게 설정된다.

설정자를 접근자보다 더 낮은 접근 수준으로 설정하면, 변수, 프로퍼티 혹은 서브스크립트의 읽기/쓰기 범위를 더 제한할 수 있다. var 혹은 subscript 앞에 `fileprivate(set)`, `private(set)`, 혹은 `internal(set)`을 적어 더 낮은 접근 수준을 할당한다.

> **참고**<br>
저장 프로퍼티뿐만 아니라 연산 프로퍼티에도 적용되는 규칙이다. 저장 프로퍼티의 명시적인 접근자와 설정자를 작성하지 않더라도, Swift는 여전히 암묵적인 접근자와 설정자를 설정하여 저장 프로퍼티의 백업 저장소에 접근을 제공한다. fileprivate(set), private(set), internal(set)을 사용하여 연산 프로퍼티의 명시적인 설정자와 정확히 동일한 방식으로 합성된 설정자의 접근 수준을 변경한다.


아래 예시는 문자열이 변경된 횟수를 추적하는 `TrackedString` 구조체를 선언한다.

```swift
struct TrackedString {
	private(set) var numberOfEdits = 0
	var value: String = "" {
		didSet {
			numberOfEdits += 1
		}
	}
}
```

`TrackedString` 구조체는 초기값 “”(비어있는 문자열)인 `value`라는 문자열 저장 프로퍼티를 정의한다. 또한 `value`가 수정되는 횟수를 추적하는 `numberOfEdits`라는 정수 저장 프로퍼티를 정의한다. 변경 추적은 `value` 속성의 `didSet` 프로퍼티 관찰자를 사용하여 구현되며 `value` 프로퍼티가 새로운 값으로 설정될 때마다 `numberOfEdits`가 증가된다.

`TrackedString` 구조체와 `value` 프로퍼티는 명확한 접근 수준 제한자를 제공하지 않고, 기본 접근 수준인 internal을 가진다. 하지만, `numberOfEdits` 프로퍼티의 접근 수준은 `private(set)` 제한자로 표기되어 있는데, 이는 접근자가 internal 기본 접근 수준을 가지지만 프로퍼티는 `TrackedString` 구조체 코드 내에서만 설정할 수 있음을 나타냅니다. 이를 통해 내부적으로는 `TrackedString`이 `numberOfEdits`를 수정할 수 있지만, 구조체 선언 밖에서 사용될 때는 읽기 전용 프로퍼티로 표시하도록 할 수 있다.

만약 `TrackedString` 객체를 만들고 `value`의 값을 몇 번 수정하면, `numberOfEdits` 프로퍼티 값은 수정한 횟수와 일치하도록 업데이트 되어있다.

```swift
var stringToEdit = TrackedString()
stringToEdit.value = "This string will be tracked."
stringToEdit.value += " This edit will increment numberOfEdits."
stringToEdit.value += " So will this one."
print("The number of edits is \(stringToEdit.numberOfEdits)")
// Prints "The number of edits is 3"
```

다른 소스 파일 내에서 `numberOfEdits` 프로퍼티의 현재 값을 쿼리할 수 있지만, 다른 소스 파일에서 프로퍼티를 수정할 수는 없다. 이 제한은 기능적인 측면에서 편리한 접근을 제공하는 동시에 `TrackedString` 수정 추적 기능의 세부 구현을 보호한다. 

필요에 따라 접근자와 이니셜라이저 모두 명시적인 접근 수준을 할당할 수 있다. 아래 예시에서 public 접근 수준을 명시하여 선언한 버전의 `TrackedString` 구조체를 볼 수 있다. 그러므로 (`numberOfEdits` 프로퍼티를 포함한) 멤버들은 기본적으로 internal 접근 수준을 가진다. public과 `private(set)` 접근 수준 한정자를 결합하여, 구조체의 `numberOfEdits` 프로퍼티의 접근자를 public으로 만들고 설정자는 private으로 만들 수 있다.

```swift
public struct TrackedString {
	public private(set) var numberOfEdits = 0
	public var value: String = "" {
		didSet {
			numberOfEdits += 1
		}
	}
	public init)() {}
}
```

# 이니셜라이저(Initializers)

사용자 정의 이니셜라이저는 생성하는 유형 이하의 접근 수준으로 할당할 수 있다. 유일한 예외는 [필수 이니셜라이저](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html#ID231)이다. 필수 이니셜라이저는 속한 클래스와 동일한 접근 수준을 가져야 한다.

함수 및 메소드의 매개변수와 마찬가지로 이니셜라이저의 매개변수 유형은 이니셜라이저 자체의 접근 수준보다 더 제한적일 수 없다.

## 기본 이니셜라이저

[기본 이니셜라이저](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html#ID213)에 설명된 것처럼, Swift는 자동으로 모든 속성에 기본값을 제공하고 하나 이상의 이니셜라이저를 선언하지 않은 구조체 혹은 기본 클래스에 인수가 없는 기본 이니셜라이저를 제공한다.

기본 이니셜라이저는 생성하는 유형이 public으로 선언되어 있지 않은 한 생성하는 유형과 동일한 접근 수준을 가진다. public으로 선언되어 있다면, 기본 이니셜라이저는 internal로 간주된다. 만약 다른 모듈에서 사용될 때 인수가 없는 이니셜라이저를 사용하여 public 유형으로 초기화되도록 하려면, 유형의 정의에서 인수가 없는 이니셜라이저를 public으로 지정해야 한다.

## 구조체용 기본 멤버와이즈 이니셜라이저(Memberwise Initializer)

구조체에 저장 프로퍼티 중 하나가 private이면 구조체용 기본 멤버와이즈 이니셜라이저도 private으로 간주된다. 마찬가지로, 만약 구조체의 저장 프로퍼티가 file-private라면 생성자도 file-private이다. 그 외의 이니셜라이저는 internal 접근 수준을 가진다.

위의 기본 이니셜라이저처럼 만약 다른 모듈에서 사용될 때 멤버와이즈 이니셜라이저를 사용하여 public 구조체로 초기화되도록 하려면, 유형의 정의에서 멤버와이즈 이니셜라이저를 public으로 지정해야 한다.

# 프로토콜

프로토콜 유형에 명시적 접근 수준을 지정하려면 프로토콜을 정의할 때 지정하면 된다. 이를 통해 특정한 접근 상황 내에서만 채택 가능한 프로토콜을 만들 수 있다. 

프로토콜 정의에서 요구사항들의 접근 수준은 자동적으로 프로토콜과 동일하게 설정된다. 프로토콜과 다른 접근 수준으로 프로토콜의 요구사항을 설정할 수 없다. 채택하는 어떤 유형이든 모든 프로토콜 요구사항을 볼 수 있음을 보장한다.

> **참고**<br>
public 프로토콜을 선언하면, 프로토콜의 요구사항을 구현할 때 public 접근수준이 필요하다. 이는 public 유형 정의가 유형 멤버의 internal 접근 수준을 의미하는 다른 유형과는 다르다.


## 프로토콜 상속

이미 존재하는 프로토콜을 상속하여 새로운 프로토콜을 정의하려면, 새로운 프로토콜이 상속하는 프로토콜과 거의 동일한 접근 수준을 가져야 한다. 예를 들어, internal 프로토콜을 상속하는 public 프로토콜을 작성할 수는 없다.

## 프로토콜 적합성(Conformance)

유형이 자신보다 더 낮은 접근 수준의 프로토콜을 준수할 수 있다. 에를 들어 다른 모듈에서 사용되지만 프로토콜에 대한 적합성은 internal 프로토콜의 정의 모듈 내에서만 사용할 수 있는 public 유형을 정의할 수 있다.

한 유형이 특정 프로토콜을 준수하는 상황에서의 접근 수준은 유형의 접근 수준과 프로토콜의 접근 수준의 최소이다. 예를 들어 public 유형이지만 internal 프로토콜을 준수한다면, 그 프로토콜 적합성에 대한 접근 수준은 internal이다.

만약 한 프로토콜을 준수하는 유형을 만들거나 확장한다면, 프로토콜 요구사항의 구현은 프로토콜에 대한 유형의 적합성 이상의 접근 수준을 보장해야 한다. 예를 들어 public 유형이 internal 프로토콜을 준수할 때, 프로토콜 각 요구사항에 대한 유형의 구현은 internal 이상이여야 한다.

> **참고**<br>
Objective-C와 같이 Swift에서 프로토콜 적합성은 global이다. 즉, 같은 프로그램 내에서 두 가지 다른 방식으로 프로토콜을 준수하는 것은 불가능하다.


# 확장

클래스, 구조체, 혹은 열거형을 사용할 수 있는 접근 상황에서 클래스, 구조체, 혹은 열거형을 확장할 수 있다. 확장 내에서 추가된 유형 구성원은 기존 유형에서 선언된 유형 구성원과 동일한 기본 접근 수준을 가진다. public 혹은 internal 유형을 확장하면, 새로 추가한 유형의 멤버들은 internal 기본 접근 수준을 가진다. file-private 유형을 확장하면, 새로 추가한 유형의 멤버들은 file-private 기본 접근 수준을 가진다. 만약 private 유형이면, private 기본 접근 수준을 가진다.

또는 확장에 접근 수준 제한자(예를 들어 ‎private)를 명시하여 확장 내에서 선언된 모든 유형에 새로운 기본 접근 수준을 설정할 수 있다. 새로운 기본값이 개별 유형 멤버들을 확장 내에서 오버라이드 할 수 있다.

확장을 프로토콜을 채택을 위해 사용하면, 확장에 접근 수준 제한자를 명시할 수 없다. 대신 그 프로토콜의 접근 수준이 확장 내 각 프로토콜 요구사항 구현에 기본 접근 수준으로 제공된다.

## 확장에서 Private 멤버

확장하는 클래스, 구조체, 혹은 열거형과 동일한 파일에 있는 확장은 확장에 대한 코드가 원래 유형 선언으로 작성된 것처럼 작동한다. 결과적으로 다음을 할 수 있다.

- 원래 선언에서 private 멤버를 선언하고, 같은 파일의 확장에서 그 멤버에 접근한다.
- 하나의 확장에서 private 멤버를 선언하고, 같은 파일의 확장에서 그 멤버에 접근한다.
- 하나의 확장에서 private 멤버를 선언하고, 같은 파일의 원래 선언에서 그 멤버에 접근한다.

이 동작은 private 엔티티를 가지는지와 상관 없이, 원래처럼 코드를 구성하는 데 확장을 사용할 수 있음을 의미한다. 예를 들어, 다음같이 간단한 프로토콜이 있다.

```swift
protocol SomeProtocol {
	func doSomething()
}
```

이렇게 프로토콜을 채택하기 위해 확장을 사용할 수 있다.

```swift
struct SomeStruct {
	private var privateVariable = 12
}

extension SomeStruct: SomeProtocol {
	func doSomething() {
		print(privateVariable)
	}
}
```

# 제네릭(Generics)

제네릭 유형 혹은 제네릭 함수에 대한 접근 수준은 제네릭 유형 혹은 함수 그리고 그 유형의 모든 매개변수 접근 수준의 최소이다.

# 유형 별칭(Type Aliases)

정의한 유형 별칭은 접근 제어를 위해 다른 유형으로 취급된다. 유형 별칭은 별칭된 유형의 접근 수준 이하의 접근 수준을 가질 수 있다. 예를 들어 private 유형 별칭은 private, file-private, internal, public 혹은 open 유형을 별칭할 수 있지만, pulbic 타입 별칭은 internal, file-private, 혹은 private 유형을 멸칭할 수 없다.

> **참고**<br>
이 규칙은 프로토콜 적합성을 충족하기 위해 사용되는 연관 유형의 유형을 별칭하는데도 적용된다.
