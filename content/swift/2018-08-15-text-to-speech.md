---
title: "Swift TTS"
description: "Swift에서, UILabel의 텍스트를 TTS(Text-to-Speech) 기능을 이용해 음성으로 듣기"
slug: "swift_uilabel_tts"
date: 2018-08-15 21:51:00 +0900
categories: Swift
excerpt: "TTS(Text-to-Speech)로 UILabel의 Text를 읽어주기"
image: 
tags:
    - Swift
    - iOS
    - Xcode
---

고달프다... 음성으로 또 텍스트를 읽어줘야 한다.

## 오늘의 목표

- TTS(Text-to-Speech)로 UILabel의 Text를 읽어주기

이럴 경우에, Swift 라이브러리에 내장된 AVFoundation을 import한다.
이를 구현하기 위해서, 다음과 같이 한다.


## Code

`Swift 4`

```swift
import AVFoundation

func speak() -> Void {
	let synthesizer = AVSpeechSynthesizer() // AVSpeechSynthesizer 변수 설정
	let item = "TEXT_TO_BE_SPOKEN" // String to be spoken
	let utterance = AVSpeechUtterance(string: item)
	utterance.voice = AVSpeechSynthesisVoice(language: "ko-KR") // language에는 언어에 맞는 단어른 locale를 적용해준다
	utterance.rate = 0.4 // 말하기 속도 설정
	synthesizer.speak(utterance)
}

```
