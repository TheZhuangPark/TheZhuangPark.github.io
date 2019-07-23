---
title: swift动画
layout: post
date: 2019-07-13 23:45:00
comments: true
tags: 
    - swift
categories: 
    - swift
keywords: swift
description: 
thumbnail: /gallery/swift/swiftAnimation.JPG
---

swift动画讲解
<!--more-->  
### 1 动画的种类（Animation catagories)
swift的动画种类有很多种  
第一种是uiview属性改变动画：就是设定一个时间，然后改变Uiview的一些属性，比如3秒内alph值为0，于是它就呈现一个逐渐消失的动画。  

第二种是Controller transition：也就是在跳转界面的时候产生的一些让跳转更流畅的动画。

第三种是Core Animation。

第四种是OpenGl，Metal(3D)

第五种是spiritKit（2.5D)

第六种是Dynamic Animation：动态动画，也就是给各种对象设置初始化速度，然后摩擦力，重力，然后不管它们。于是它们就按照你设置的物理规则进行运动。

### 2 UIView Properties
这是常用的一种swift演绎动画的模式，一个uiview有很多属性：  
1 frame/center  
2 bounds  
3 transform  
4 alpha  
5 backgroudColor     
然后通过设定时间，改变这些属性，从而演绎出动画效果！   
   
基本流程是：  
1 定义一些动画参数并传递闭包。  
2 闭包里包含对UIView进行更改的代码。  
3 闭包将仅对上述UIView属性的更改进行动画处理。  
4 闭包里的更改立即生效（只是没有呈现给用户）。  
5 动画完成时要执行的另一个“完成块”。  
这是一个基本的动画闭包代码：   
 
class func runningPropertyAnimator(  
withDuration: TimeInterval,  
delay: TimeInterval,  
options: UIViewAnimationOptions,  
animations: () -> Void,  
completion: ((position: UIViewAnimatingPosition) ->   Void)? = nil  
)    

举个例子：
if myView.alpha == 1.0 {  
UIViewPropertyAnimator.runningPropertyAnimator(  
withDuration: 3.0,  
delay: 2.0,  
options: [.allowUserInteraction],  
animations: { myView.alpha = 0.0 },  
completion: { if $0 == .end   { myView.removeFromSuperview() } }  
)  
print(“alpha = \(myView.alpha)”)  
}    

但是在option那里就有不同的打法比如：    
beginFromCurrentState  
allowUserInteraction   
layoutSubviews   
repeat   
autoreverse  
overrideInheritedDuration  
overrideInheritedCurve   
allowAnimatedContent  
curveEaseInEaseOut   
curveEaseIn   
curveLinear   
用来控制这个动画的模式，可以看到repeat就是这个动画重复播放的意思，curveEaseInEaseOut是动画进入放缓出去也放缓。      

### 3 Transition Animation
当然你也不局限，可以    UIViewAnimationOptions.transitionFlipFrom{Left,Right,Top,Bottom}，这个可以令view翻转。然后比如你的有个卡牌view想翻转的时候就使用这个方式。    
UIView.transition(with: myPlayingCardView,   
duration: 0.75,   
options: [.transitionFlipFromLeft],   
animations: { cardIsFaceUp = !cardIsFaceUp }    
completion: nil)     

### 4 Dynamic Animation 
这个就是个物理世界的模型，当你设置好了重力，和速度，然后物体就会按照你设立的物理规则运动，从而形成动画。创造这样的世界有几步。
#### 1 创建一个UIDynamicAnimator
var animator = UIDynamicAnimator(referenceView: UIView)

#### 2 创建和添加 UIDynamicBehavior 实例

let gravity = UIGravityBehavior()
animator.addBehavior(gravity)

collider = UICollisionBehavior()
animator.addBehavior(collider) 

#### 3 添加 UIDynamicItems 到 UIDynamicBehavior

let item1: UIDynamicItem = ... // usually a UIView
let item2: UIDynamicItem = ... // usually a UIView
gravity.addItem(item1)
collider.addItem(item1)
gravity.addItem(item2)

任何要运动的物体都得实现这个UIDynamicItem协议。  
这是这个协议：  
protocol UIDynamicItem {  
var bounds: CGRect { get } // essentially the size  
var center: CGPoint { get set } // and the position  
var transform: CGAffineTransform { get set } // rotation usually  
var collisionBoundsType:   UIDynamicItemCollisionBoundsType { get set }     
var collisionBoundingPath: UIBezierPath { get set }    
}    
如果在动画进行过程中，某个item,物体改变了中心点和transform则需要呼叫更新函数：    
func updateItemUsingCurrentState(item: UIDynamicItem)    
来更新。  

#### Behavior 
接下来会具体讲有哪些behavior，从上面可以知道这些behavior都是你创造的物理规则，然后添加到物体上的。   

##### UIGravityBehavior
显而易见这是一个重力的物理规则。  

var angle: CGFloat // in radians; 0 is to the right; positive numbers are clockwise
var magnitude: CGFloat // 1.0 is 1000 points/s/s   

这是它两个变量，顺带一提1000像素点每秒的平方近似于重力加速度。
##### UIAttachmentBehavior
这是一个叫附着的行为，想象一根棒子，联系两个端点，一个端点受重力加速度往下掉另一个会因为附着这个端点，于是先旋转再往下掉。 
   
init(item: UIDynamicItem, attachedToAnchor: CGPoint)  
init(item: UIDynamicItem, attachedTo: UIDynamicItem)  
init(item: UIDynamicItem, offsetFromCenter: CGPoint, attachedTo[Anchor]…)  
var length: CGFloat // distance between attached things (this is settable while animating!)    

var anchorPoint: CGPoint // can also be set at any time, even while animating    
 
这是这个UIAttachmentBehavior所有api接口   

##### UICollisionBehavior 
这个就是很显而易见的碰撞行为。  
var collisionMode: UICollisionBehaviorMode // .items, .boundaries, or .everything
If .items, then any items you add to a UICollisionBehavior will bounce off of each other
If .boundaries, then you add UIBezierPath boundaries for items to bounce off of …  

func addBoundary(withIdentifier: NSCopying, for: UIBezierPath)  
func addBoundary(withIdentifier: NSCopying, from: CGPoint, to: CGPoint)  
func removeBoundary(withIdentifier: NSCopying)  
var translatesReferenceBoundsIntoBoundary: Bool // referenceView’s edges  NSCopying means NSString or NSNumber, but remember you can as to String, Int, etc.
这是这个碰撞行为的所有接口。    

当碰撞发生的时候你如何得知呢？  
var collisionDelegate: UICollisionBehaviorDelegate  

… this delegate will be sent methods like …  
func collisionBehavior(behavior: UICollisionBehavior,
began/endedContactFor: UIDynamicItem,
withBoundaryIdentifier: NSCopying // with:UIDynamicItem too
at: CGPoint)  

##### UISnapBehavior
简单的说，就像入场动画一样，不过入场的时候它是会震动的。  
init(item: UIDynamicItem, snapTo: CGPoint)  
var damping: CGFloat  
这里你可以设置它固定位置。

##### UIPushBehavior
就是第一推力  
var mode: UIPushBehaviorMode // .continuous or .instantaneous  
var pushDirection: CGVector
… or …     
var angle: CGFloat // in radians and positive numbers are clockwise    
var magnitude: CGFloat // magnitude 1.0 moves a 100x100 view at 100 pts/s/s    

##### UIDynamicItemBehavior  
这个是item特有的行为，可以设置用来控制item的行为。  
var allowsRotation: Bool   
var friction: CGFloat  
var elasticity: CGFloat   

然后用下面三个函数来获取这个item信息  
func linearVelocity(for: UIDynamicItem) -> CGPoint  
func addLinearVelocity(CGPoint, for: UIDynamicItem)  
func angularVelocity(for: UIDynamicItem) -> CGFloat  

##### UIDynamicBehavior
You can create your own subclass which is a combination of other behaviors.   
你甚至可以创建一个子类，这子类可以是其他物理规则的集合，通常来讲你要重写 init-method, addItem, removeItem   
func addChildBehavior(UIDynamicBehavior)    
你甚至可以加很多api来帮助构建这个子类   
var dynamicAnimator: UIDynamicAnimator? { get }  
func willMove(to: UIDynamicAnimator?)   
   
UIDynamicBehavior’s action的属性      
每次行为作用在items上，然后调用这个action      
var action: (() -> Void)?    

#### Stasis
当然你需要知道这个动画什么时候停止，什么时候恢复，甚至在停止和恢复的时候做一些你希望的操作。
这个时候就是设定协议：  
var delegate: UIDynamicAnimatorDelegate  

…这两个协议里的函数是停止和恢复时会启用的函数  
func dynamicAnimatorDidPause(UIDynamicAnimator)  
func dynamicAnimatorWillResume(UIDynamicAnimator)  

### 5 memory-cycle
在运行动画的时候经常会出现循环调用，循环调用就是a调用b,b调用了a,导致a,b的引用计数都无法减1，所以无法在heap堆里面释放掉，造成内存的堆积。  
例子代码：  
if let pushBehavior = UIPushBehavior(items: […],   mode: .instantaneous) {  
pushBehavior.magnitude = …  
pushBehavior.angle = …  
pushBehavior.action = {  
pushBehavior.dynamicAnimator!.removeBehavior(pushBehavior)   
}  
animator.addBehavior(pushBehavior) // will push right away  
}  
push的action的属性是个闭包，这闭包又调用了pushBehavior本身，所以形成了一个循环调用，无法从heap，堆里释放出来。  

有几种方式来打破这个循环调用：  
1 定义动态的局部变量，不去调用外部的。  
var foo = { [x = someInstanceOfaClass, y = “hello”] in
// use x and y here
}

2 定义的时候，把这些局部变量，用weak，或者unowned修饰  
var foo = { [weak x = someInstanceOfaClass, y = “hello”] in
// use x and y here, but x is now an Optional because it’s weak
}       

unowned意味着引用计数系统不会考虑它们   
var foo = { [unowned x = someInstanceOfaClass, y = “hello”] in
// use x and y here, x is not an Optional
// if you use x here and it is not in the heap, you will crash
}  

还有一种循环调用的情况    
class Zerg {
private var foo = {
self.bar()
}
private func bar() { . . . }
}  
这个class持有foo这个闭包，foo这个闭包又调用了self，所以形成了循环调用。  
我们可以break这个循环调用了   
class Zerg {  
    private var foo = {  
    [weak weakSelf = self] in  
    weakSelf?.bar() // need Optional chaining now   
    }  
    private func bar() { . . . }  
}  
当然也可以用unowned去破除这个循环    
if let pushBehavior = UIPushBehavior(items: […], mode: .instantaneous) {
pushBehavior.magnitude = …
pushBehavior.angle = …
pushBehavior.action = { [unowned pushBehavior] in
pushBehavior.dynamicAnimator!.removeBehavior(pushBehavior)
}
animator.addBehavior(pushBehavior) // will push right away
}
      
但这里是因为我们知道pushBehavior一定会在heap里，从上面的定义代码可以知道，所以使用unowned没有问题，但如果heap里面没有pushBehavior, 程序马上崩溃。

 