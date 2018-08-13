---
layout: post
title: do-try-catch 유닛테스트 하는 방법
tags:
- Swift
- UnitTest
- do-try-catch
---

#### do-try-catch 유닛테스트 하기 위한 코드
    - error enum
    - try
    - do-try-catch
    - 테스트코드 : 정상 확인 코드
    - 테스트코드 : 오류 확인 코드

#### error enum
> Error Protocol 을 상속받아 에러 케이스를 정의합니다.
```
    enum JsonError : Error {
        case unSupportedArrayPattern
        case unSupportedObjectPattern
    }
```

#### try 코드
> 일반 함수와 동일하지만 -> Bool or -> Int 같은 형식이 아닌 throws 만 정의해줍니다.
> 또한, guard를 이용하여 위에서 정의한 enum 케이스 중 하나를 throw 에 담아줍니다.
```
    public static func isValidate(to inputValue:String) throws {
        if inputValue.isArray() {
            guard inputValue.supportedArrayTypes() else {
                throw JsonError.unSupportedArrayPattern
            }
        }
        
        if inputValue.isObject() {
            guard inputValue.supportedObjectTypes() else {
                throw JsonError.unSupportedObjectPattern
            }
        }
    }
```

#### do-try-catch 코드
> do-try-catch 구문을 이용해 위에서 정의한 func를 try 에 정의
> 해당 코드 중에 에러가 발생할 경우를 catch 에 정의
> 마지막 catch 에는 default 로 enum 코드에 정의하지 않은 에러가 발생할 경우를 대비해 정의
```
    func someFunc() -> Bool {
        do {
            try GrammarChecker.isValidate(to: input)
        } catch JsonError.unSupportedArrayPattern {
            return JsonError.unSupportedArrayPattern.isError()
        } catch JsonError.unSupportedObjectPattern {
            return JsonError.unSupportedObjectPattern.isError()
        } catch {
            // 해당 부분은 Default 형식으로 추가해주도록 체크되고 있습니다.
            print("판단되지 않은 에러발생")
            return true
        }
    }
```

#### 테스트코드 : 정상 확인 코드
> 정상적인 결과를 원하는 코드의 경우에는 `XCTAssertNoThrow` 함수를 이용해 테스트를 실행합니다.
```
    func testPass(){
        let input = "{\"name\":\"[oingbong{}}{{}}\"}"
        XCTAssertNoThrow(try GrammarChecker.isValidate(to: input))
    }
```

#### 테스트코드 : 오류 확인 코드
> 에러가 나는 경우의 결과를 원하는 경우에는 `XCTAssertThrowsError` & `XCTAssertEqual` 함수 사용
> XCTAssertThrowsError : 실행 할 코드와 값을 입력합니다.
> XCTAssertEqual : enum 에서 정의한 에러와 입력한 코드와 값이 동일한 에러인지 확인합니다.

```
    func testFail(){
        let input = "[{{,}}]"
        XCTAssertThrowsError(try GrammarChecker.isValidate(to: input)) { error in
            XCTAssertEqual(error as? JsonError, JsonError.unSupportedArrayPattern)
        }
    }
```

#### 참고
1. [XCTAssertThrowsError - Apple](https://developer.apple.com/documentation/xctest/1500795-xctassertthrowserror)
2. [XCTAssertNoThrow - Apple](https://developer.apple.com/documentation/xctest/xctassertnothrow)
3. [How to unit test throwing functions in Swift? - stackoverflow](https://stackoverflow.com/questions/32860338/how-to-unit-test-throwing-functions-in-swift)