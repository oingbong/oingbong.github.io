---
layout: post
title: 여러 타입을 저장하기 위한 enum 만들기
tags:
- Swift
- Enum
- Associated Value
---

* Array Or Dictionary 에 Bool,Int,String 등 다양한 값을 추가하기 위해서 enum을 활용합니다.
* Any 타입을 사용하지 않고 다른 방법을 이용하기 위해서 enum 의 연관값을 이용합니다.
* 따라서, enum을 활용하는 Array Or Dictionary 에서는 다양한 타입의 값을 저장 & 관리할수 있습니다.

#### enum 정의

```
enum JsonType {
    case string(String)
    case int(Int)
    case bool(Bool)
    case object(String)
}
```

#### enum 타입을 배열에 저장할 때

* Array를 선언할 때는 <> 타입형태에 `JsonType`을 선언해줍니다.
* 저장할 때 enum case에 정의된 형태로 값을 추가해줍니다.

```
struct JsonArray {
    private var array:Array<JsonType>
    
    public mutating func addArray(element:String) {
        // 객체 {} 의 경우에는 String 으로 저장합니다.
        if element.isObject() {
            self.array.append(JsonType.object(element))
        }else if element.isBool(){
            self.array.append(JsonType.bool(Bool(element)!))
        }else if element.isNumber() {
            self.array.append(JsonType.int(Int(element)!))
        }else {
            self.array.append(JsonType.string(element))
        }
    }
}
```

#### enum 타입을 배열에서 꺼낼 때

* 이미 `JsonType` 으로 저장되어 있기 때문에 `.string` or `.int` 등을 이용해 switch 문에서 관리해줍니다.

```
struct JsonArray {
    public func count() -> (Int,Int,Int,Int) {
        var string = 0
        var int = 0
        var bool = 0
        var object = 0

        for element in self.array {
            switch element {
            case .string:
                string = string + 1
            case .int:
                int = int + 1
            case .bool:
                bool = bool + 1
            case .object:
                object = object + 1
            }
        }
        return (string, int, bool, object)
    }
}
```

#### 참고
[Enumerations - Apple](https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html)