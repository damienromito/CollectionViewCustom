

#Custom UICollectionView 

*Create custom UICollectionViewLayoutWith with a custom paging size of UICollectionViewCell by overriding scrollViewWillEndDragging*

![alt text](https://github.com/damienromito/CollectionViewCustom/blob/master/image.jpg "custom UICollectionViewCell size")

```swift
let collectionMargin = CGFloat(16)
let itemSpacing = CGFloat(10)
let itemHeight = CGFloat(322)
var itemWidth = CGFloat(0)
var currentItem = 0
```

## 1 Custom CollectionViewLayout

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    let layout: UICollectionViewFlowLayout = UICollectionViewFlowLayout()

    itemWidth =  UIScreen.main.bounds.width - collectionMargin * 2.0
    
    layout.sectionInset = UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
    layout.itemSize = CGSize(width: itemWidth, height: itemHeight)
    layout.headerReferenceSize = CGSize(width: collectionMargin, height: 0)
    layout.footerReferenceSize = CGSize(width: collectionMargin, height: 0)
    layout.minimumLineSpacing = itemSpacing
    layout.scrollDirection = .horizontal

    collectionView!.collectionViewLayout = layout
    collectionView?.decelerationRate = UIScrollViewDecelerationRateFast

}
```


## 2 Custom Paging size

```swift 
func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>) {
    
    let pageWidth = Float(itemWidth + itemSpacing)
    let targetXContentOffset = Float(targetContentOffset.pointee.x)
    let contentWidth = Float(collectionView!.contentSize.width  )
    var newPage = Float(self.pageControl.currentPage)
    
    if velocity.x == 0 {
        newPage = floor( (targetXContentOffset - Float(pageWidth) / 2) / Float(pageWidth)) + 1.0
    } else {
        newPage = Float(velocity.x > 0 ? self.pageControl.currentPage + 1 : self.pageControl.currentPage - 1)
        if newPage < 0 {
            newPage = 0
        }
        if (newPage > contentWidth / pageWidth) {
            newPage = ceil(contentWidth / pageWidth) - 1.0
        }
    }
    self.pageControl.currentPage = Int(newPage)
    let point = CGPoint (x: CGFloat(newPage * pageWidth), y: targetContentOffset.pointee.y)
    targetContentOffset.pointee = point
}
```
    