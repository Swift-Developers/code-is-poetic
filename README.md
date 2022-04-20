# code-is-poetic
Swift 代码规范

在我们自己的项目中使用 Swift 代码时，我们尽量遵循如下的原则：

- 对于命名，在使用时能清晰表意是最重要。因为 API 被使用的次数要远远多于被声明的次数，所以我们应当从使用者的角度来考虑它们的名字。尽快熟悉 [Swift API 设计准则](https://swift.org/documentation/api-design-guidelines/)，并且在你自己的代码中坚持使用这些准则。
- 简洁经常有助于代码清晰，但是简洁本身不应该独自成为我们编码的目标。
- 在设计你的 API 时，尽量按照积极引导用户去做“正确的事情”的方式来进行 ([Xiaodi Wu](https://forums.swift.org/t/please-define-actively-harmful-as-related-to-swift-evolution/36293/2))。不要让程序员有“自掘坟墓”的机会。
- 务必为函数添加文档注释 — 特别是泛型函数。
- 类型使用大写字母开头，函数、变量和枚举成员使用小写字母开头，两者都使用驼峰式命名法。
- 使用类型推断。省略掉显而易见的类型会有助于提高可读性。
- 如果存在歧义或者在进行定义契约 (比如 func 就需要显式地指定返回类型) 的时候不要使用类型推断。
- “优先选择结构体，只在确实需要使用到类特有的特性或者是引用语义时才使用类。
- 除非你的设计就是希望某个类被继承使用，否则都应该将它们标记为 final。如果你允许这个类被模块内部继承，但不允许外部的用户进行子类化，那么标记这个类为 public，而不是 open。
- 除非一个闭包后面立即跟随有左花括号 (比如在 if 条件中)，否则都应该使用尾随闭包 (trailing closure) 的语法。
- 使用 guard 来提早退出方法。
- 避免对可选值进行强制解包和隐式强制解包。它们偶尔有用，但是经常需要使用它们的话往往意味着有其他不妥的地方。
- 不要写重复的代码。如果你发现你写了好几次类似的代码片段的话，试着将它们提取到一个函数里，并且考虑将这个函数转化为协议扩展的可能性。
- 试着去使用 map 和 reduce，但这不是强制的。当合适的时候，使用 for 循环也无可厚非。高阶函数的意义是让代码可读性更高。但是如果使用 reduce 的场景难以理解的话，强行使用往往事与愿违，这种时候简单的 for 循环可能会更清晰。
- 试着去使用不可变值：除非你需要改变某个值，否则都应该使用 let 来声明变量。不过如果能让代码更加清晰高效的话，也可以选择使用可变的版本。同样这也不是强制的，在结构体上用可变方法通常比返回一个全新的结构体更加惯用及高效。
- 结构体的属性通常可以设计为可变的，因为 API 的用户可以通过把结构体变量标记为 let 或 var 的方式来控制可变性。
- 除非你确实需要，否则不要使用 self.。不过在闭包表达式中，self 是被强制使用的，这是一个清晰的信号，表明闭包将会捕获 self。
- 尽可能地对现有的类型和协议进行扩展，而不是写一些自由函数。这有助于提高可读性，让别人更容易发现你的代码。
- 当有意义时，去扩展已有 (标准库中的) 类型，不必犹豫。


## 一. 格式规范
### 1.1 使用4个空格进行缩进
`推荐`

```swift
if value == 1 {
    print("")
}
```

### 1.2 二元运算符(+, ==, 或->)的前后都需要添加空格

`推荐`

```swift
let value = 1 + 2
                    
if value == 1 {
    /* ... */
}
                    
func test(with value: TestClass) -> returnValue {
    /* ... */
}
```

### 1.3 一般情况下，在逗号和冒号后面加一个空格

`推荐`
```swift
let array = [1, 2, 3, 4, 5]

let dic: [String: Any] = [:]
```

`不推荐`
```swift
let array = [1,2,3,4,5]

let dic : [String :Any] = [:]
```

### 1.4 `switch` 每个`case`结尾留一行空行, 最后一行不留. `if`同理.(推荐)
`推荐`
```swift
switch i {
case .a:
    print("a")

case .b:
    print("b")
    
default:
    default
}

if isTrue {
    /* ... 结尾空行 */

} else if isFlase {
    /* ... */

} else {
    /* ... 最后一行不需要空行 */
}
```

`不推荐`
```swift
switch i {
case .a:
    print("a")
case .b:
    print("b")
default:
    default

}

if isTrue {
    /* ... */
} else if isFlase {
    /* ... */
} else {
    /* ... */

}
```

### 1.5 代码结尾不要使用分号;

`推荐`
```swift
print("Hello World")
```

`不推荐`
```swift
print("Hello World");
```

### 1.6 左大括号不要另起一行

`推荐`
```swift
// 1. Class Define
class TestClass {
    /* ... */
}

// 2. if
if value == 1 {
    /* ... */
}

// 3. if else
if value == 1 {
    /* ... */
} else {
    /* ... */
}

// 4. while
while isTrue {
    /* ... */
}

// 5. guard
guard let value = value else  {
    /* ... */
}
```

`不推荐`
```swift
// 1. Class Define
class TestClass 
{
    /* ... */
}

// 2. if
if value == 1
{
    /* ... */
}

// 3. if else
if value == 1
{
    /* ... */
}
else
{
    /* ... */
}

// 4. while
while isTrue 
{
    /* ... */
}

// 5. guard
guard let value = value else 
{
    /* ... */
}
```

### 1.7 判断语句不用括号

`推荐`
```swift
if value == 1 {
    /* ... */
}

if value == 1, string == "test" {
    /* ... */
}
```

`不推荐`
```swift
if (value == 1) {
    /* ... */
}

if ((value == 1) && (string == "test")) {
    /* ... */
}
```

### 1.8 不建议使用`self.` , 除非方法参数与属性同名或其他必要情况

`推荐`
```swift
func setup() {
    label = UILabel()
}

```

`不推荐`
```swift
func setup() {
    self.label = UILabel()
}
```

### 1.9 善用类型推导, 不写多余代码

`推荐`
```swift
func set(color: UIColor) {
    /* ... */
}

set(color: .black)
```

`不推荐`
```swift
func set(color: UIColor) {
    /* ... */
}

set(color: UIColor.black)
```

### 1.10 添加有必要的注释，尽可能使用Xcode注释快捷键（⌘⌥/）

`推荐`
```swift
/// <#Description#>
///
/// - Parameter test: <#test description#>
/// - Returns: <#return value description#>
func test(string: String?) -> String? {
    /* ... */
}
```

`不推荐`
```swift
/// <#Description#>
func test(string: String?) -> String? {
    /* ... */
}
```

### 1.11 注释符`//`后加空格，如果`//`跟在代码后面，前面也加一个空格

`推荐`

```swift
// 注释
let aString = "xxx" // 注释
```

### 1.12 使用`// MARK: -`，按功能和协议 / 代理分组

```swift
/// MARK顺序没有强制要求，但System API & Public API一般分别放在第一块和第二块。

// MARK: - Public

// MARK: - Action

// MARK: - Private

// MARK: - Data

// MARK: - xxxDelegate
```

### 1.13 对外接口不兼容时，使用`@available(iOS x.0, *)`标明接口适配起始系统版本号

```swift
@available(iOS x.0, *)
class MyClass {
    /* ... */
}

@available(iOS x.0, *)
func myFunction() {
    /* ... */
}
```

### 1.14 方法间合理换行

`推荐`
```swift
extension XXX { 
    // 扩展开始 增加一个换行
    func xxxx1() {
        /* ... */
    }
    // 方法之间 增加一个换行
    func xxxx2() {
        /* ... */
    }
}
```

### 1.15 代码块合理换行

`推荐`
```swift
func xxxx() {
    /* ... */
    
    /* ... */
    
    /* ... */
}
```

## 二. 命名规范

### 2.1 建议不要使用前缀, 善用命名空间.

`推荐`

```swift
HomeViewController
Bundle
```

`不推荐`
```swift
NEHomeViewController
NSBundle
```

### 2.2 不要缩写、简写、单个字母来命名

`推荐`
```swift
let frame = view.frame
let image = imageView.image
let backgroundColor = view.backgroundColor 
```

`不推荐`
```swift
let f = view.frame
let img = imageView.image
let bgColor = view.backgroundColor 
```

### 2.3 如非必要, 不要声明全局常量/变量/函数, 应该根据类型适当归类包装, 合理利用命名空间.

`推荐`
```swift
enum Main {
    static let color = UIColor(red: 0.1, green: 0.2, blue: 0.3, alpha: 1.0)
    static let count = 13
    static func font(_ size: CGFloat) -> UIFont {
        return UIFont(name: "xxx", size: size) ?? .systemFont(ofSize: size)
    }
}
```

`不推荐`
```swift
let mainColor = UIColor(red: 0.1, green: 0.2, blue: 0.3, alpha: 1.0)
let mainCount = 13
func mainFont(_ size: CGFloat) -> UIFont {
    return UIFont(name: "xxx", size: size) ?? UIFont.systemFont(ofSize: size)
}

```

### 2.4 变量命名

- 使用小驼峰，首字母小写

- 变量命名应该能推断出该变量类型，如果不能推断，则需要以变量类型结尾

`推荐`
```swift
class TestClass: class {
    // UIKit的子类，后缀最好加上类型信息
    let coverImageView: UIImageView
    @IBOutlet weak var usernameTextField: UITextField!

    // 作为属性名的firstName，明显是字符串类型，所以不用在命名里不用包含String
    let firstName: String

    // UIViewContrller以ViewController结尾
    let fromViewController: UIViewController

    // 集合类型以复数形式命名
    var datas: [Data] = []
    var items: [Item] = []
}
```

`不推荐`
```swift
class TestClass: class {
    // image不是UIImageView类型
    let coverImage: UIImageView
    // or cover不能表明其是UIImageView类型
    var cover: UIImageView

    // String后缀多余
    let firstNameString: String

    // UIViewContrller不要缩写
    let fromVC: UIViewController
    
    // 集合类型多余后缀和描述
    var dataList: [Data] = []
    var itemArray: [Item] = []
}
```

### 2.5 类型命名：使用大驼峰表示法，首字母大写

### 2.6 方法命名：使用参数标签让方法语义更清楚, 参数标签和参数需要表达正确的语义(Public接口及基础组件必须遵循) [(from Swift API Design Guidelines)](https://swift.org/documentation/api-design-guidelines/)

#### 2.6.1 省略所有的冗余的外部参数标签(推荐)

`推荐`

```swift
func min(_ number1: Int, _ number2: Int) {
    /* ... */
}

min(1, 2)
```

`不推荐`
```swift
func min(number1: Int, number2: Int) {
    /* ... */
}

min(number1: 1, number2: 2)
```

#### 2.6.2 进行安全值类型转换的构造方法可以省略参数标签，非安全类型转换则需要添加参数标签以表示类型转换方法

`推荐`
```swift
extension UInt32 {
  /// 安全值类型转换，16位转32位，可省略参数标签
  init(_ value: Int16)
  
  /// 非安全类型转换，64位转32位，不可省略参数标签
  /// 截断显示
  init(truncating source: UInt64)
  
  /// 非安全类型转换，64位转32位，不可省略参数标签
  /// 显示最接近的近似值
  init(saturating valueToApproximate: UInt64)
}
```

#### 2.6.3 当第一个参数构成整个语句的介词时（如，at, by, for, in, to, with 等），为第一个参数添加介词参数标签(推荐)

`推荐`
```swift
// 添加介词标签having
func removeBoxes(having length: Int) {
    /* ... */
}

let length = 23
x.removeBoxes(having: length)
```

例外情况是，当后面所有参数构成独立短语时，则介词提前。

`推荐`

```swift
// 介词To提前
a.moveTo(x: b, y: c)

// 介词From提前
a.fadeFrom(red: b, green: c, blue: d)
```

`不推荐`
```swift
a.move(toX: b, y: c)

a.fade(fromRed: b, green: c, blue: d)
```

#### 2.6.4 当第一个参数构成整个语句一部分时，省略第一个参数标签，否则需要添加第一个参数标签.

`推荐`

```swift
// 参数构成语句一部分，省略第一个参数标签
view.addSubview(tempView)

// 参数不构成语句一部分，不省略第一个参数标签
dismiss(animated: false)
```

#### 2.6.5 其余情况下，给除第一个参数外的参数都添加标签

### 2.7 方法命名：不要使用冗余的单词，特别是与参数及参数标签重复

`推荐`

```swift
func remove(_ member: Element) -> Element?
```

`不推荐`
```swift
func removeElement(_ member: Element) -> Element?
```

### 2.8 命名出现缩写词，缩写词要么全部大写，要么全部小写，以首字母大小写为准

`推荐`
```swift
let urlRouterString = "https://xxxxx"

let htmlString = "xxxx"

class HTMLModel {
    /* ... */
}

struct URLRouter {
    /* ... */
}
```

`不推荐`
```swift
let uRLRouterString = "https://xxxxx"

let hTMLString = "xxxx"

class HtmlModel {
    /* ... */
}

struct UrlRouter {
    /* ... */
}
```

### 2.9 Bool类型命名：用`is`为前缀
`推荐`
```swift
var isString: Bool = true
```

### 2.10 枚举定义尽量简写，不要包括类型前缀

`推荐`
```swift
public enum UITableViewRowAnimation: Int {
    case fade

    case right // slide in from right (or out to right)

    case left

    case top

    case bottom

    case none // available in iOS 3.0

    case middle // available in iOS 3.2.  attempts to keep cell centered in the space it will/did occupy

    case automatic // available in iOS 5.0.  chooses an appropriate animation style for you
}
```

### 2.11 协议命名 [(from Swift API Design Guidelines)](https://swift.org/documentation/api-design-guidelines/)

#### 2.11.1 如果协议描述的是协议做的事应该命名为名词(eg. `Collection`)(推荐)

`推荐`

```swift
protocol TableViewSectionProvider {
    func rowHeight(at row: Int) -> CGFloat
    var numberOfRows: Int { get }
    /* ... */
}
```

#### 2.11.2 如果协议描述的是能力，需添加后缀able或 ing (eg. Equatable、 ProgressReporting)

`推荐`

```swift
protocol Loggable {
    func logCurrentState()
    /* ... */
}

protocol Equatable {
    func ==(lhs: Self, rhs: Self) -> Bool
         /* ... */
}
```

#### 2.11.3 如果已经定义类，需要给类定义相关协议，则添加`Protocol`后缀

`推荐`

```swift
protocol InputTextViewProtocol {
    func sendTrackingEvent()
    func inputText() -> String
    /* ... */
}
```

#### 2.11.4 如果已经定义类，需要给类定义相关委托协议，则添加`Delegate`后缀

`推荐`
```swift
public protocol UITabBarControllerDelegate: NSObjectProtocol {
    @available(iOS 3.0, *)
    func tabBarController(_ tabBarController: UITabBarController, shouldSelect viewController: UIViewController) -> Bool
}
```

## 三. 语法规范

### 3.1 多使用`let`，少使用`var`

### 3.2 少用`!`去强制解包

### 3.3 可选类型拆包取值时，使用`if let`判断

`推荐`
```swift
if let optionalValue = optionalValue {
    /* ... */
}
```

`杜绝`
```swift
if  optionalValue != nil {
    let value = optionalValue!
    /* ... */
}
```

### 3.4 多个可选类型拆包取值时，将多个if let合并, 除非特殊逻辑需要.

`推荐`

```swift
var subview: UIView?
var volume: Double?

if let subview = subview, let volume = volume {
    /* ... */
}
```

`不推荐`
```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
    if let realVolume = volume {
        /* ... */
    }
}
```

### 3.5 不要使用 as! 或 try!

`推荐`

```swift
// 使用if let as？判断
if let text = text as? String {
    /* ... */
}

// 使用if let try 或者 try?
if let test = try aTryFuncton() {
    /* ... */
}
```

### 3.6 数组和字典变量定义.

定义时需要标明泛型类型，并使用更简洁的语法

`推荐`
```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

`不推荐`
```swift
var names = [String]()
var names: Array<String> = [String]() / 不够简洁

var lookup = [String: Int]()
var lookup: Dictionary<String, Int> =Dictionary<String, Int>() // 不够简洁
```

### 3.7 数组访问

尽可能使用 .first 或 .last, 推荐使用 `for item in items` 或 `items.forEach { }` 而不是 `for i in 0...X`

`推荐`
```swift
items.first
items.last

for item in items {
    /* ... */
}

items.forEach { 
    /* ... */
}
```

`不推荐`
```swift
items[0]
items[items.count - 1]

for i in 0 ..< items.count {
    let item = items[i]
    /* ... */
}
```

### 3.8 如果变量能够推断出类型，则不建议声明变量时指明类型

`推荐`

```swift
let value = 1 

let text = "xxxx"
```

`不推荐`
```swift
let value: Int = 1 

let text: String = "xxxx"
```

### 3.9 判断相等

#### 3.9.1 使用`==`和`!=`判断内容上是否一致(推荐)
`推荐`
```swift
// String类型没有-isEqualToString方法，用==判断是否相等
let str1 = "netease"
let str2 = "netease"
if str1 == str2 {
    // is true
    /* ... */
}

// 对于自定义类型，判断内容是否一致，需要实现Equatable接口
class BookItem {
    let bookId: String
    let title: String
    
    init (bookId: String, title: String) {
        self.bookId = bookId
        self.title = title
    }
}

extension BookItem: Equatable {

    static func ==(lhs: BookItem, rhs: BookItem) -> Bool {
        // 具体判断规则根据实际需要进行
        return lhs.bookId == rhs.bookId
    }
}
```

#### 3.9.2 使用`===`和`!==`判断`class`类型对象是否同一个引用，而不是用`==`和`!=`

`推荐`
```swift
if tenEighty === alsoTenEighty {
    /* ... */
}

if tenEighty !== notTenEighty {
    /* ... */
}
```

### 3.10 协议

3.10.1 当实现protocol时，如果确定protocol的实现不会被重写，建议用extension将protocol实现分离

`推荐`
```swift
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

`不推荐`
```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

### 3.12 尾随闭包

当方法最后一个参数是Closure类型，调用时建议使用尾随闭包语法, 但只在只存在一个闭包参数时才使用尾闭包。

`推荐`

```swift
UIView.animateWithDuration(1.0) {
    self.myView.alpha = 0
}

UIView.animateWithDuration(1.0,
    animations: {
        self.myView.alpha = 0
    },
    completion: { finished in
        self.myView.removeFromSuperview()
    }
)
```

`不推荐`
```swift
UIView.animateWithDuration(1.0, animations: {
    self.myView.alpha = 0
})

UIView.animateWithDuration(1.0,
    animations: {
        self.myView.alpha = 0
    }) { finished in
        self.myView.removeFromSuperview()
}
```

### 3.13 高阶函数推荐最简化语法

`推荐`
```swift
array.sort(by: <)

array.sort { $0.age < $1.age }
```

`不推荐`
```swift
array.sort { (l, r) -> Bool in
    l < r
}

array.sort { (l, r) -> Bool in
    return l < r
}
```

### 3.14 访问控制 (优先考虑最低级)

- `private`

- `fileprivate`

- `internal` (默认忽略不写)

- `public`

- `open`

访问控制权限关键字应该写在最前面，除了`@IBOutlet`、`@IBAction`、`@discardableResult`、`@objc`等关键字.

`推荐`
```swift
// 类似注解修饰词单独占一行
@objc
func print(message: String) -> String {
    /* ... */
    return xxx
}
```

### 3.15 如调用者可以不使用方法的返回值，则需要使用`@discardableResult`标明

`推荐`
```swift
@discardableResult
func print(message: String) -> String {
    let output = "Output : \(message)"
    print(output)
    return output
}
```

### 3.16 Golden Path，最短路径原则

`推荐`
```swift
func test(_ number1: Int?, _ number2: Int?, _ number3: Int?) {
    guard
        let number1 = number1, 
        number2 = number2, 
        number3 = number3 else { 
        fatalError("impossible") 
    }
    /* ... */
}

func login(with username: String?, password: String?) throws -> LoginError {
    guard let username = username else { 
        throw .noUsername 
    }
    guard let password = password else { 
        throw .noPassword
    }

    /* login code */
}
```

`不推荐`

```swift
func test(_ number1: Int?, _ number2: Int?, _ number3: Int?) {
    if let number1 = number1 {
        if let number2 = number2 {
            if let number3 = number3 {
                /* ... */
            } else {
                fatalError("impossible")
            }
        } else {
            fatalError("impossible")
        }
    } else {
        fatalError("impossible")
    }
}

func login(with username: String?, password: String?) throws -> LoginError {
    if let username = username {
      if let password = password {
          /* login code */
      } else {
          throw .noPassword
      }
    } else {
        throw .noUsername
    }
}
```

### 3.17 循环引用

#### 3.17.1 使用委托和协议时，避免循环引用，定义属性的时候使用weak修饰

`推荐`
```swift
public weak var dataSource: UITableViewDataSource?

public weak var delegate: UITableViewDelegate?
```

#### 3.17.2 在逃逸Closures中使用self时避免循环引用

`推荐`

```swift
request(.list) { [weak self] (result: Result<[Model]>) in
    guard let self = self else { return }

    self.items = self.result.value
    self.tableView.reloadData()
}
```

`不推荐`
```swift
request(.list) { [unowned self] (result: Result<[Model]>) in
    self.items = self.result.value
    self.tableView.reloadData()
}
```

`不推荐`
```swift
request(.list) { [weak self] (result: Result<[Model]>) in
    self?.items = self?.result.value
    self?.tableView.reloadData()
}
```

### 3.18 空判断

`推荐`

```swift
if array.isEmpty {
    /* ... */
}

if string.isEmpty {
    /* ... */
}
```

`不推荐`
```swift
if array.count == 0 {
    /* ... */
}

if string.count == 0 {
    /* ... */
}
```



### 3.19 非空判断

`推荐`

```swift
var value: String?
if value.isNone {
    /* ... */
}

if value.isSome {
    /* ... */
}
```



### 3.20 单例

`推荐`
```swift
class TestManager {
    static let shared = TestManager()

    /* ... */
}
```
