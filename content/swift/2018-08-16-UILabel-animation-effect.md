---
title: "Swift UILabel effect"
description: "Swift에서, UILabel에 애니메이션 효과 적용하기 (노래방 가사 효과를 예시로)"
slug: "swift_uilabel_effect"
date: 2018-08-16 22:53:00 +0900
category: Swift
thumbnail: images/posts/card1.gif
excerpt: "UILabel의 Text를 글자별로 색이 바뀌도록 하기(노래방의 가사처럼)"
tags:
    - Swift
    - iOS
    - Xcode
---

## 오늘의 목표

- TTS(Text-to-Speech)로 UILabel의 Text를 읽어주기
- Image View 내 이미지의 layer에서 가장자리를 Animate하기
- UILabel의 Text를 글자별로 색이 바뀌도록 하기(노래방의 가사처럼)


최근 AAC 앱 개발을 하면서, 인지에 어려움이 따르는 친구들을 위해 사진을 누르면 위와 같이 되도록 하려고 했다. 첫번째, 두번째 기능은 쉽게 구현했지만, 세번째는 Swift 개발자가 UILabel.borderColor에 Animate 할 수 있도록 설정해놓지 않아서(ㅠㅠ) 직접 함수를 구현해보았다.

## Code

`Swift 4`

```swift

var timer = Timer() // 타이머 설정
var charCount = 0 // String -> Char 변환했을 때, Character 개수를 세기 위해 변수 설정

timer = Timer.scheduledTimer(withTimeInterval: 0.25, repeats: true, block: { timer in
	if charCount < Array(item1.titleLabel.text!).count {
		let range = (item1.titleLabel.text! as NSString).rangeOfComposedCharacterSequence(at: charCount)
       		let attributedText = NSMutableAttributedString.init(string: item1.titleLabel.text!)
        	attributedText.addAttribute(NSAttributedStringKey.foregroundColor, value: UIColor.green , range: range)
        	item1.titleLabel.attributedText = attributedText
        	charCount += 1
	} else {
		timer.invalidate()
        	item1.titleLabel.textColor = .black
	}
})
```

## 결과
![card1](/images/posts/card1.gif)
