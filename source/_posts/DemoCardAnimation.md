---
title: swift动画Demo
layout: post
date: 2019-07-12 01:13:00
comments: true
tags: 
    - swift
categories: 
    - swift
keywords: swift
description: 
thumbnail: /gallery/swift/BTarpyRyc.JPG
---

这是一个使用MVC架构的翻卡牌动画Demo
<!--more-->  
## Demo的架构
![](/gallery/swift/DemoOverview.png)
这个Demo的架构是这样的,是一个标准的MVC结构,当然除了MVC结构之外可以用MVVM结构。但这里我先不进一步说。    
demo在git的地址如下:  
[https://github.com/TheZhuangPark/SwiftTutorial]()

## Viewcontroller
Viewcontroller有7个变量，两个函数
变量分别是：  
1 CardDeck 卡牌桌子  
2 CardView数组 绑定了storyboard的很多控件的控件数组    
3 animator 用来制作动态动画的  
4 cardBehavior 从动态动画衍生出来的卡牌行为   
5 faceUpCardView 桌子上目前牌面朝上的卡牌   
6 faceUpCardViewMatch 决定桌子上朝上卡牌是否匹配  
7 lastChosenCardView 最新的被选中的卡牌
两个函数：   
1 viewDidLoad() 初始化函数   
2 flipCard() 当你点击卡牌时触发的函数

### viewDidLoad()
首先是super.viewDidload()，然后开始给一个循环，就是根据卡牌控件数目的一半，声明一个随机的卡牌对象，并且给填充两个一样，到卡牌模型数组中。  

switf的for循环 for _ in 1...k{} 其中包括k  
构建好卡牌模型数组后，就开始循环初始化卡牌视图，从卡牌模型数组取出卡牌赋值给卡牌视图。最后卡牌视图再添加手势，动态动画效果。
      
	     let card = CardsModel.remove(at: CardsModel.count.arc4random)
	     ardview.rank = card.rank.order
	     cardview.suit = card.suit.rawValue    
	     cardview.addGestureRecognizer(UITapGestureRecognizer(target: self,  action: #selector(flipCard(_ :) )))
	     cardBehavior.addItem(cardview)
	 
所以简单的说，就是先构造好模型和模型数据，然后再让模型数据赋值给相应的视图控件。

## private func flipCard(）
这个函数，控制卡牌被点击之后的行为。被点击后，卡牌需要进行判断从而来翻转。    
1 没有卡牌被选中时：卡牌被点击后，翻转然后静止，并且把该卡牌加入到卡牌向上数组。  
2 有一个卡牌被选中：比较两个卡牌，match则同时变大，再缩小，再消失。不match则同时翻转回来然后被推出去  
3 有两个卡牌被选中：什么都不做  
而flipcard有个形参，就是UITapGestureRecognizer类型的recognizer，也就是手势。一般检验手势的代码都是用swicth    

         switch recognizer.state {
               case .ended: 代码
               default: 代码
               }
 这里也是这种开头。后面有点复杂。 
 
     if let chosenCardView = recognizer.view as? PlayingCardView, faceUpCardViews.count < 2 {代码} 
 
 这里是个二重if判断，满足逗号前后的判断语句才进入代码块，然后进行更新数据，和动画翻转。并且每次翻转完，会进入 completion:{}块，在completion里面判断，是否有两张卡，是否match。 
 这儿注意有两种动画形式：  
 1 UIView.transition 一种是转场动画  
 2 UIViewPropertyAnimator.runningPropertyAnimator 这个是改变uiview的属性的动画  
 顺带一提这是缩放和  
 
	 cardsToAnimate.forEach {
	     $0.transform = CGAffineTransform.identity.scaledBy(x: 3.0, y: 3.0)
                                    
	  }  
	   
注意这种block，代码块的应用非常容易导致循环调用。

## faceUpCardViews & faceUpCardViewMatch
在这段代码中，当用户不断点击的时候，faceUpCardViews数组是在不断变化的，faceUpCardViewMatch也是在不断变化的，所以这里有个didset的机制，也就是它是不断更新的。


    private var faceUpCardViews: [PlayingCardView]{
        return CardView.filter{ $0.isFaceUp && !$0.isHidden && $0.transform !=
          CGAffineTransform.identity.scaledBy(x: 2.0, y: 2.0) && $0.alpha == 1}
        }  
   

## PlayingCard
这是个结构体，它有suit, rank, description这个三个变量。  
当一个结构体实现了这个CustomStringConvertible协议的时候，就需要实现它的一个变量description。而当你print()这个结构体的时候，就会把description也打印出来。
这里涉及到枚举值，你需要把枚举的类型和范围都定义出来。 

    enum Suit : String,     
    CustomStringConvertible{
        case spades = "♠️"
        case hearts = "❤️"
        case clubs = "♣️"
        case diamonds = "♦️"
        static var all = [Suit.spades,.hearts,.diamonds,.clubs]
        var description: String{ return rawValue }
    }
    

## PlayingCardView

这个比较复杂，这个就得把整个扑克牌的牌面画出来。它是一个继承UIVIEW的类。在类的前面添加@IBDesignable可以让它在storyboard里显示。

    @IBInspectable
    var rank : Int = 12 {
        didSet{ setNeedsDisplay();  setNeedsLayout()
        }}
    @IBInspectable
    var suit : String = "❤️" {
        didSet{ setNeedsDisplay();  setNeedsLayout()
        }}
    @IBInspectable
    var isFaceUp: Bool = true {
        didSet{ setNeedsDisplay();  setNeedsLayout()
        }}
    @IBInspectable
    var faceCardScale: CGFloat = SizeRatio.faceCardImageSizeToBoundsSize {
        didSet{ setNeedsDisplay()
        }}
        
    override var collisionBoundsType: UIDynamicItemCollisionBoundsType{
        return .ellipse
    }
       private var cornerString: NSAttributedString{
        return centeredAttributedString(rankString+"\n"+suit, fontSize: cornerFontSize)
    }
    private lazy var upperLeftCornerLabel = createCornerLabel()
    private lazy var lowerRightCornerLabel = createCornerLabel() 
    
    
卡牌的 1 suit, 2 rank, 3 是否向上, 4 碰撞类型, 5 边角字符, 6 左上角label, 7 右下角label。总共7个属性。  
还有函数func(),具体的实现方法可以去demo地址看  
 
	1 func adjustFaceCardScale(byHandlingGestureRecognizedBy recognizer: UIPinchGestureRecognizer)  
   
	2 private func centeredAttributedString(_ string: String, fontSize: CGFloat)->NSAttributedString    

	3 private func createCornerLabel() -> UILabel  
	
	4 private func configureCornerLabel(_ label: UILabel )
	
	5 override func traitCollectionDidChange(_ previousTraitCollection: UITraitCollection?)
	
	6 override func layoutSubviews() 
	
	7 private func drawPips()
	
	8 override func draw(_ rect: CGRect)

还有extension,给PlayingCardView，CGRect做了一些方便的拓展，和给CGPoint做了些拓展，和CGFloat做了产生随机数的拓展。 

	extension CGRect {
        var leftHalf: CGRect {
            return CGRect(x: minX, y: minY, width: width/2, height: height)
        }
        var rightHalf: CGRect {
            return CGRect(x: midX, y: minY, width: width/2, height: height)
        }
        func inset(by size: CGSize) -> CGRect {
            return insetBy(dx: size.width, dy: size.height)
        }
        func sized(to size: CGSize) -> CGRect {
            return CGRect(origin: origin, size: size)
        }
        func zoom(by scale: CGFloat) -> CGRect {
            let newWidth = width * scale
            let newHeight = height * scale
            return insetBy(dx: (width - newWidth) / 2, dy: (height - newHeight) / 2)
        }
     }
    
	extension CGPoint {
        func offsetBy(dx: CGFloat, dy: CGFloat) -> CGPoint {
            return CGPoint(x: x+dx, y: y+dy)
        }
     }
    
	extension CGFloat {
        var arc4random: CGFloat {
            return self * (CGFloat(arc4random_uniform(UInt32.max))/CGFloat(UInt32.max))
        }
      }

## CardBehavior 
这是一个继承自UIDynamicBehavior的类CardBehavior，专门用来定义卡牌的物理动态运动的。这里有三个Behavior。

	  lazy var collisonBehavior : UICollisionBehavior = {
        let behavior = UICollisionBehavior()
        behavior.translatesReferenceBoundsIntoBoundary = true
        return behavior
    }()
    
    lazy var itemBehavior : UIDynamicItemBehavior = {
        let behavior = UIDynamicItemBehavior()
        behavior.allowsRotation=false
        behavior.elasticity=1.0
        behavior.resistance=0
        return behavior
    }()
    
      private func push(_ item: UIDynamicItem)     {}
    
就三个collisonBehavior，itemBehavior，UIPushBehavior。 具体的实现也可以去demo看代码。

## PlayCardDeck 
init(){}这里，就是根据卡牌的suit,rank的all属性来了解到他们的范围，接着声明一个卡牌数组，把这所有的卡牌都囊括进去。

draw(){}这里，则是随机抽一张牌出来返回。这里的随机数可以很方便的定义，利用extension Int 这是非常强大的特点

	extension Int{
    var arc4random: Int{
        if self>0{
              return Int(arc4random_uniform(UInt32(self)))
        }else if self < 0{
              return -Int(arc4random_uniform(UInt32(abs(self))))
        }else {
            return 0
        }
    }
}



