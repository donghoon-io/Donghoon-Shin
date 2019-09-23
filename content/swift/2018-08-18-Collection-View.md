---
title: "Swift UICollectionView"
description: "Swift에서 Collection View와 Collection View Cell 사용하기"
slug: "swift_uicollectionview"
date: 2018-08-18 19:53:00 +0900
category: Swift
thumbnail: images/posts/collectionview7.png
excerpt: "콜렉션뷰와 셀 초기 설정법"
tags:
    - Swift
    - iOS
    - Xcode
    - Collection View
---

## Collection View란?

iOS를 사용하는 사람은 많이 봤겠지만, 한 화면에 분할된 몇개의 부분(Cell)을 두고, 이를 이용하여 정보를 보여주는(또는 실행하는) 형태를 Collection View라고 한다. 다음과 같이, 우리가 평소에 많이 사용하는 앱에 Collection View가 적용된 경우가 많다.

|Netflix|Instagram[^insta]|App Store|
|---|---|---|
|![Netflix](/images/posts/netflix.png)| ![Instagram](/images/posts/instagram.png)| ![App Store](/images/posts/appstore.png) |

수평/수직 스크롤링이 적용되며, Data Source에 따라 자동으로 행/열 개수를 조정해주기 때문에 아주 편리한 도구이다.
하지만, 몇가지 참고해야 할 부분이 있기 때문에, 오늘 어떻게 Collection View를 사용하는지 알아보도록 한다.

## Collection View 사용법

먼저, Storyboard에 Collection View를 추가해준다.

![collectionview1](/images/posts/collectionview1.png)

Collection View를 추가하고 나서, 이에 해당하는 셀이 하나 있을 것이다. 여기에 맞는 View Controller을 추가해줘야 한다. 일단, `File - New - File...`로 들어가서 새 Cocoa Touch Class를 추가해준다. 이때, 이름은 임의로 설정해준다. 단, `subclass of` 란에는 반드시 `UICollectionViewCell`를 선택해준다.

![collectionview2](/images/posts/collectionview2.png)

그 다음, 파일이 추가되었으면 다시 Storyboard를 연다. 여기서 아까 처음 만들 때 저절로 생긴 Collection View Cell를 선택해주고, 세번째 탭의 `Custom Class` 란에 아까 임의로 지정해줬던 클래스의 이름을 입력해준다.

![collectionview3](/images/posts/collectionview3.png)

그리고, 네번째 탭의 `Identifier`은 셀을 불러올 때 쓸 수 있도록 임의의 이름을 지정해준다.

여기까지 하면 반이 끝났다. 그 다음, 메인 View Controller에 들어가서, 다음과 같이 `UICollectionViewDelegate`, `UICollectionViewDataSource`를 추가해준다. 어, 근데 다음과 같은 에러가 뜬다.

![collectionview4](/images/posts/collectionview4.png)

해석하면, 추가해준 Protocol이 메인 View Controller에 순응하지 않는다는 뜻이다. 이 경우, 이에 순응할 수 있도록 `Protocol Stubs`를 추가해줘야 한다. Swift는 친절하게도 버튼 하나만 누르면 추가할 수 있도록 했다. 에러 창의 Fix를 누르면

```swift

import UIKit

class ViewController: UIViewController, UICollectionViewDelegate, UICollectionViewDataSource {
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        <#code#>
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        <#code#>
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
}

```

이렇게 collectionView에 순응하도록 만들어주는 stubs가 두개 생긴다. 위 <#code#> 자리에 이제 코드를 추가해줘야 한다.

먼저, 첫번째 stub에서는 우리가 만들어준 Collection View에 몇 개의 Cell이 들어갈 것인지 지정해줘야 한다. 우리가 어떤 Data Source를 이용할 것인지에 따라 개수가 달라지므로, 이에 해당하는 Int 값을 return 해야 한다.

간단하게 Data Source를 만들어보자. 다음과 같은 행렬에 따른 Collection View가 만들어진다고 가정하자.

```swift

var dataSourceArray: [String] = ["pig1", "pig2", "pig3", "pig4"]

```

그렇다면, 첫번째 stub에서 return 해야 하는 값은 `dataSourceArray.count`가 될 것이다.

```swift

import UIKit

class ViewController: UIViewController, UICollectionViewDelegate, UICollectionViewDataSource {
	var dataSourceArray: [String] = ["pig1", "pig2", "pig3", "pig4"]
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return dataSourceArray.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        <#code#>
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
}

```

그 다음, 두번째 stub에서는 내부에 있는 cell의 property를 정의해줘야 한다. cell은 다음과 같은 코드로 불러올 수 있다.

```swift

let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "cellID", for: indexPath) as! CollectionViewCell

```

여기서, `withReuseIdentifier` 항목에는 아까 지정해준 `Identifier`을 넣고, `as!`는 그 다음 올 항목이 지금 정의하는 cell의 종류가 될 것이라는 걸 강제로 지정해주는 역할을 한다.

(물론, `as!`에서 느낌표가 force하는 역할이기 때문에 만약 뒤에 나올 `CollectionViewCell`이 없거나 cell이 실제로 이를 따르지 않는다면 에러가 뜰 것이다.)

그 다음, 다시 Storyboard로 돌아가보자. 여기서 cell 안에 UILabel을 하나 넣어보도록 하자.

![collectionview5](/images/posts/collectionview5.png)

그 다음, 이 라벨을 cell의 View Controller로 끌어와야 한다. 라벨을 선택한 상태로 control 버튼을 누르는 동시에, 이를 CollectionViewCell 클래스 내부로 끌어당긴다.

![collectionview6](/images/posts/collectionview6.png)

위와 같은 화면이 생기면, 라벨에 지정해줄 아무 이름이나 넣고 connect를 클릭한다. 일단 여기선 `cellLabel`으로 지정하도록 하겠다.

다시 메인 View Controller으로 돌아간다. 아래와 같은 코드로 두번째 stub를 수정해준다.

```swift
func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
	let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "cellID", for: indexPath) as! CollectionViewCell
    cell.cellLabel.text = dataSourceArray[indexPath.item]

	return cell
}
```

여기서, `cell.cellLabel.text`는 셀 내부에 있는 라벨의 텍스트를 말하고, `dataSourceArray[indexPath.item]`는 dataSourceArray의 indexPath.item번째 원소를 말한다. 여기서, `indexPath`는 셀의 순서를 말하고(`int` 아님, 개발자가 설정한 특수한 순서), 뒤에 붙은 item은 이 순서를 `int`로 반환하는 역할을 한다.

자, 거의 다 됐다. 마지막으로, 아까 UILabel을 끌어당긴 방식과 동일하게, Collection View를 메인 View Controller에 끌어당긴다. 그 다음, 여기에 알맞는 이름을 설정하고, `viewDidLoad()` 함수에 다음과 같이 추가한다. 여기선 Collection View에 해당하는 이름을 `testCollectionView`로 한다.

```swift

override func viewDidLoad() {
	super.viewDidLoad()
    
    testCollectionView.delegate = self
    testCollectionView.dataSource = self
}

```

그리고 실행해보면 다음과 같은 화면이 정상적으로 나온다.

## 결과

![collectionview7](/images/posts/collectionview7.png)

## 최종 코드

`메인 View Controller`

```swift
import UIKit

class ViewController: UIViewController, UICollectionViewDelegate, UICollectionViewDataSource {
    
    @IBOutlet weak var testCollectionView: UICollectionView!
    
    var dataSourceArray: [String] = ["pig1", "pig2", "pig3", "pig4"]
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return dataSourceArray.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "cellID", for: indexPath) as! CollectionViewCell
        cell.cellLabel.text = dataSourceArray[indexPath.item]
        
        return cell
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        testCollectionView.delegate = self
        testCollectionView.dataSource = self
        
        // Do any additional setup after loading the view, typically from a nib.
    }
}

```

`Cell의 View Controller`

```swift

import UIKit

class CollectionViewCell: UICollectionViewCell {
    @IBOutlet weak var cellLabel: UILabel!
}

```

##### 각주
[^insta]: Instagram은 Collection View를 수정한 IGListKit을 사용한다.
