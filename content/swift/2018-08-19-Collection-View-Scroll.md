---
title: "Swift UICollectionView scroll"
description: "How to programmatically scroll UICollectionView to the last cell"
slug: "swift_uilabel_effect"
date: 2018-08-19 03:00:00 +0900
category: Swift
tags:
    - Swift
    - iOS
    - Xcode
    - Collection View
---

## How to programmatically scroll UICollectionView to the last cell

1. First, refer to the collection view's section number using `numberOfSections` (If there is no section within the scrollview, `return` the function using `guard`.)
2. (Of course) the last section index will be `numberOfSections - 1`.
3. In the same way, count the number of items in the section above, and scroll to the last item.

The following is a code for the progress.

## Code

```swift
extension UICollectionView {
    func scrollToLast() {
        guard numberOfSections > 0 else {
            return
        }
        let lastSection = numberOfSections - 1
        guard numberOfItems(inSection: lastSection) > 0 else {
            return
        }
        let lastItemIndexPath = IndexPath(item: numberOfItems(inSection: lastSection) - 1, section: lastSection)
        scrollToItem(at: lastItemIndexPath, at: .bottom, animated: true)
    }
}
```
