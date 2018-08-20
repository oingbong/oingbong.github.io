---
layout: post
title: Swift one IBAction for multiple buttons
tags:
- Swift
---

### Swift one IBAction for multiple buttons
* Swift에서 하나의 IBAction에 다수의 Button을 등록하는 방법에 대해서 설명합니다.
* 비슷한 유형의 작업을 하는 버튼에 대해 하나씩 @IBAction을 만드는 것이 비효율적이라고 생각되시면 해당 방법을 사용해보세요.
* 해당 방법보다 더 좋은 방법이 있을수도 있으니 참고만 해주시면 감사하겠습니다.

### 방법
1. @IBAction 정의하기
2. Interface Builder 에서 연결하기
3. 결과값 확인해보기

---

##### 1. @IBAction 정의하기
> View Controller에 아래와 같이 정의하기

```
@IBAction func btnNumber(btnN : UIButton) {
	
}
```

##### 2. Interface Builder 에서 연결하기
1. Main.storyboard 클릭
2. View hierarchy 보기 : 컨트롤러 및 뷰, 버튼의 계층이 보이는 화면 (안보이면 왼쪽 하단의 Show Document Outline 클릭)
3. View Controller 를 오른쪽 버튼 클릭
4. Received Actions 섹션에 보이는 btnNumberWithBtnN 의 오른쪽 동그라미 가져대면 + 로 변경됨
*이름은 다음과 같이 정의됩니다 : 코드에서 정의한 func이름 + With + 받는변수명(?)*
5. +를 클릭한 상태에서 등록하려는 Button에 드래그하기
6. 4,5번 반복


##### 3. 결과값 확인해보기
> btnN.titleLabel.text 를 이용해 print OR Label에 저장하여 확인해보기

#### Before Code
```
@IBAction func btn0(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "0"
}
@IBAction func btn1(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "1"
}
@IBAction func btn2(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "2"
}
@IBAction func btn3(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "3"
}
@IBAction func btn4(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "4"
}
@IBAction func btn5(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "5"
}
@IBAction func btn6(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "6"
}
@IBAction func btn7(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "7"
}
@IBAction func btn8(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "8"
}
@IBAction func btn9(_ sender: UIButton) {
	resultDisplay.text = resultDisplay.text! + "9"
}
```

### After Code
```
@IBAction func btnNumber(btnN : UIButton){
    let button = btnN.titleLabel?.text
    resultDisplay.text = resultDisplay.text! + button!
}
```
