# Best-Practice
Best practices for iOS App Development

#### [iOS App Performance](#ios-app-performance)
#### [iOS Coding Guidelines ](#ios-coding-guidelines)
#### [iOS Standard Guidelines](#ios-standard-guidelines)
#### [iOS UI Best Practices](#ios-ui-best-practices)

#### iOS App Performance
- **Use ARC to Manage Memory:**
Automatic Reference Counting (ARC) is Swift’s system of tracking memory we’re using. When we create an object from a class, Swift remembers that instance is being referenced precisely once. If we then point another variable at that object, Swift will increment the reference count to 2, because two variables are pointing at the same object. If we now destroy the first variable (perhaps it was inside a function and that function ended), Swift takes the reference count back down to 1.

- **Use a reuseIdentifier Where Appropriate:**
For maximum performance, a table view’s data source should generally reuse UITableViewCell objects when it assigns cells to rows in tableView:cellForRowAtIndexPath:. A table view maintains a queue or list of UITableViewCell objects that the data source has marked for reuse.

- **Set Views as Opaque When Possible:**
This property provides a hint to the drawing system as to how it should treat the view. If set to YES, the drawing system treats the view as fully opaque, which allows the drawing system to optimize some drawing operations and improve performance. If set to NO ,the drawing system composites the view normally with other content. The default value of this property is YES.

- **Avoid Fat XIBs:**
Note that when we load a XIB into memory, all of its contents are loaded into memory, including any images. If we have a view we’re not using immediately, then we’re wasting precious memory. It’s worth noting that this won’t happen with storyboards, since a storyboard will only instantiate a view controller when it’s needed.


- **Don’t Block the Main Thread:**
We should never do any heavy lifting on the main thread. This is because UIKit does all of its own work on the main thread, such as drawing, managing touches, and responding to input.
The risk of doing all of our app’s work on the main thread is that if our code does block this thread, our app will appear unresponsive. That’s a quick route to one-star reviews on the App Store!.

- **Size Images to Image Views:**
If we’re displaying an image from the app’s bundle in a UIImageView, make sure that the image and the UIImageView are same size. Scaling images on the fly can be expensive, especially if our UIImageView is embedded in a UIScrollView.
If the image is downloaded from a remote service, sometimes we don’t have control over the size, or we might not be able to scale it on the server prior to downloading. In cases like these, we can scale the image manually once we’ve finish downloading it — preferably on a background thread! — and then use the resized image in our UIImageView.

- **Choose the Correct Collection:**
Learning to use the most appropriate class or object for the task at hand is fundamental to writing efficient code. This is especially true when dealing with collections(Arrays, Dictionaries, Sets).

- **Reuse and Lazy Load Views:**
More views means more drawing; which ultimately means more CPU and memory overhead. This is especially true if our app embeds many views inside of a UIScrollView.
The trick to managing this is to mimic the behavior of UITableView and UICollectionView: don’t create all subviews at once. Instead, create our views as we need them, adding them to a reuse queue when you’re finished with them.

- **Cache:**
A great rule of thumb when developing our app is to “cache what matters” — that is, things that are unlikely to change, but are accessed frequently.
What can you cache? Some candidates for caching are remote server responses, images, or even calculated values, such as UITableView row heights.

- **Handle Memory Warnings:**
It’s very important to release all memory possible once you receive a memory warning. Otherwise, we run the risk of having our app killed by the system.

- **Reuse Expensive Objects:**
Some objects are very slow to initialize- NSDateFormatter and NSCalendar are two examples. However, you can’t always avoid using them, such as when parsing dates from a JSON/XML response.
To avoid performance bottlenecks when working with these objects, try to reuse these objects if at all possible. You can do this by either adding a property to our class, or by creating a static variable.
Note that if you choose the second approach, the object will remain in memory while our app is running, much like a singleton.

- **Avoid Re-Processing Data**
Many apps make calls for data from remote servers to get information the app requires to function. This data usually comes across in JSON or XML format. It’s important to try and use the same data structure at both ends when requesting and receiving the data. Why? Manipulating data in memory to fit our data structures can be expensive.

- **Choose the Right Data Format:**
There are multiple ways we can transfer data to our app from a web service, but the most common two are JSON and XML. You want to make sure you choose the right one for our app.

- **Set Background Images Appropriately:**
Choose Correct Data Storage Option
There’s at least two different ways to place a background image on our view:
You can set our view’s background color to a color created with UIColor’s colorWithPatternImage.
You can add a UIImageView subview to the view.

- **Choose Correct Data Storage Option:**
Speed up Launch Time
What are your options when it comes to storing and reading large data sets?
We have several options, including:
Store them using NSUserDefaults
Save to a structured file in XML, JSON, or Plist format.
Archive using NSCoding.
Save to a local SQL database such as SQLite.
Use Core Data and Realm.

- **Speed up Launch Time:**
Launching our app quickly is very important, especially when the user launches for the first time. First impressions mean a lot for an app!.
The biggest thing that you can do to ensure our app starts as quickly as possible is to perform as many asynchronous tasks as possible, such as network requests, database access, or parsing data.

### iOS Coding Guidelines

- **Correctness :**
 	Strive to make your code compile without warnings.

- **Naming:**
using camelcase (not snake case).
using uppercase for 1st letter of (protocols, class, structure, enum), lowercase for everything else.
using names based on roles, not types
sometimes compensating for weak type information
beginning factory methods with make/fetch/get
naming methods for their side effects :
protocols that describe what something is should read as nouns
protocols that describe a capability should end in -able or -ible
verb methods follow the -ed, -ing rule for the non-mutating version
noun methods follow the formX rule for the mutating version
using terms that don't surprise experts or confuse beginners(let vc / let viewController)
generally avoiding abbreviations
preferring methods and properties to free functions
giving the same base name to methods that share the same meaning
choosing good parameter names that serve as documentation
labeling closure and tuple parameters
taking advantage of default parameters
- **Class Prefixes :**
If two names from different modules collide you can disambiguate by prefixing the type name with the module name. 
- **Delegates :**
When creating custom delegate methods, an unnamed first parameter should be the delegate source. (UIKit contains numerous examples of this.)

Preferred:
~~~
func datePickerView(_ date: String, getSelected time:  Date)
~~~
Not Preferred:
~~~
func datePickerView(date: String, getSelectedTime:  Date)
~~~
- **Use Type Inferred Context :**
Prefer compact code and let the compiler infer the type for constants or variables of single instances. Type inference is also appropriate for small (non-empty) arrays and dictionaries. When required, specify the specific type such as CGFloat or Int16.

Preferred:
~~~
let message = "Click the button"
let calculateBounds = calculateViewBounds()
var names = ["Meera", "Naveen", "bandish"]
let maximumWidth: CGFloat = 100.9
var names: [String] = []
var lookupDictionary: [String: Int] = [:]
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
 ~~~
Not Preferred:
~~~
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
var names = [String]()
var lookup = [String: Int]()
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
 ~~~
- **Generics:**
Generic type parameters should be descriptive, upper camel case names. When a type name doesn't have a meaningful relationship or role, use a traditional single uppercase letter such as T, U, or V.

Preferred:
~~~
struct Link<Element, Address> { ... }
func write<Target: OutputStream>(to target: inout Target)
~~~
Not preferred:
~~~
struct Link<E, A> { ... }
func write<T: OutputStream>(to target: inout target)
 ~~~
- **Languages:**
Use US English spelling to match Apple's API.
Preferred:
~~~
let color = "white"
~~~ 
Not preferred:
~~~
let colour = "white"
 ~~~
- **Code Organisation:**
Use extensions to organize your code into logical blocks of functionality. Each extension should be set off with a // MARK: - comment to keep things well-organized.
Protocol Conformance:
 Preferred:
~~~
	class HomeViewController: UIViewController {
  // class stuff here
}
// MARK: - UITableViewDataSource
extension HomeViewController: UITableViewDataSource {
  // table view data source methods
}
// MARK: - UIScrollViewDelegate
extension HomeViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
~~~
Not preferred:
~~~
class HomeViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
~~~
- Unused Code: 
Unused (dead) code, including Xcode template code and placeholder comments should be removed. An exception is when your tutorial or book instructs the user to use the commented code.
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}
- Minimal Imports:
For example, don't import UIKit when importing Foundation will suffice.
- Spacing:
Indent using 2 spaces rather than tabs to conserve space and help prevent line wrapping. Be sure to set this preference in Xcode 
Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu). Some of the Xcode template code will have 4-space tabs hard coded, so this is a good way to fix that.
Method braces and other braces (if/else/switch/while etc.) always open on the same line as the statement but close on a new line
Preferred:
~~~
if user.isHappy {
  // Do something
} else {
  // Do something else
}
~~~
Not Preferred:
~~~
if user.isHappy 
{
  // Do something
} else 
{
  // Do something else
}
~~~
- **Comments:**
When they are needed, use comments to explain why a particular piece of code does something. Comments must be kept up-to-date or deleted.

- **Classes and Structures:**
Which One to use?:

Remember, structs have value semantics. Use structs for things that do not have an identity. An array that contains [a, b, c] is really the same as another array that contains [a, b, c] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have reference semantics. Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity

- **Use of Self:**

For conciseness, avoid using self since Swift does not require it to access an object's properties or invoke its methods.
Use self only when required by the compiler (in @escaping closures, or in initializers to disambiguate properties from arguments). In other words, if it compiles without self then omit it.
12. Computed Properties:
For conciseness, if a computed property is read-only, omit the get clause. The get clause is required only when a set clause is provided.
Preferred:
~~~
var diameter: Double {
  return radius * 2
}
 ~~~
Not Preferred:
~~~
var diameter: Double {
  get {
    return radius * 2
  }
}
~~~
- **Final:** 
Marking classes or members as final in tutorials can distract from the main topic and is not required. Nevertheless, use of final can sometimes clarify your intent and is worth the cost. In the below example, Box has a particular purpose and customization in a derived class is not intended. Marking it final makes that clear.
 - **Function Declaration: **
Keep short function declarations on one line including the opening brace.
- **Closures Expressions: **
Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.
	attendeeList.sort { a, b in
	  a > b
	}
- **Types:**
Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

Preferred:
~~~
let width = 120.0                                    // Double
	let widthString = (width as NSNumber).stringValue //String
~~~
Not Preferred:
~~~
	let width: NSNumber = 120.0                          // NSNumber
	let widthString: NSString = width.stringValue        // NSString
~~~
- **Constants:**
Constants are defined using the let keyword, and variables with the var keyword. Always use let instead of var if the value of the variable will not change.
Tip: A good technique is to define everything using let and only change it to var if the compiler complains!
You can define constants on a type rather than on an instance of that type using type properties. To declare a type property as a constant simply use static let. Type properties declared in this way are generally preferred over global constants because they are easier to distinguish from instance properties.

Preferred:
~~~
enum Math {
  static let e = 2.718281828459045235360287
  static let root2 = 1.41421356237309504880168872
}
let hypotenuse = side * Math.root2
~~~
Not Preferred:
~~~
let e = 2.718281828459045235360287  // pollutes global namespace
let root2 = 1.41421356237309504880168872
let hypotenuse = side * root2 // what is root2?
 ~~~
- **Optionals :**
Declare variables and function return types as optional with ? where a nil value is acceptable.

Preferred:
~~~
var subview: UIView?
	var volume: Double?
// later on...
	if let subview = subview, let volume = volume {
 	 // do something with unwrapped subview and volume
	}
~~~	
Not Preferred:
~~~
	var optionalSubview: UIView?
	var volume: Double?
	if let unwrappedSubview = optionalSubview {
 	 if let realVolume = volume {
   	 // do something with unwrappedSubview and realVolume
  	}
	}
~~~	
- **Lazy Initialization :**
Consider using lazy initialization for finer grain control over object lifetime. This is especially true for UIViewControllerthat loads views lazily. You can either use a closure that is immediately called { }() or call a private factory method. 

Example:
~~~
lazy var locationManager: CLLocationManager = self.makeLocationManager()
	private func makeLocationManager() -> CLLocationManager {
	  let manager = CLLocationManager()
	  manager.desiredAccuracy = kCLLocationAccuracyBest
 	 manager.delegate = self
 	 manager.requestAlwaysAuthorization()
 	 return manager
	}
~~~
- **Access Control :**
Using private and fileprivate appropriately, however, adds clarity and promotes encapsulation. Prefer private to fileprivate when possible. Using extensions may require you to use fileprivate.	
Only explicitly use open, public, and internal when you require a full access control specification.
Use access control as the leading property specifier. The only things that should come before access control are the static specifier or attributes such as @IBAction, @IBOutlet and @discardableResult.

Preferred:
~~~
private let message = "Great Scott!"
	class TimeMachine {  
  		fileprivate dynamic lazy var fluxCapacitor = FluxCapacitor()
	}
~~~
Not Preferred:
~~~
fileprivate let message = "Great Scott!"
	class TimeMachine {  
 	 	lazy dynamic fileprivate var fluxCapacitor = FluxCapacitor()
	}
 ~~~
- **Control Flow :**
Prefer the for-in style of for loop over the while-condition-increment style.
- **GoldenPath :**
When coding with conditionals, the left-hand margin of the code should be the "golden" or "happy" path. That is, don't nest if statements. Multiple return statements are OK. The guard statement is built for this.

Preferred:
~~~
guard let number1 = number1,
      let number2 = number2,
      let number3 = number3 else {
  fatalError("impossible")
}
 ~~~
Not Preferred: 
~~~
	if let number1 = number1 {
  if let number2 = number2 {
    if let number3 = number3 {
      // do something with numbers
    } else {
      fatalError("impossible")
    }
  } else {
    fatalError("impossible")
  }
} else {
  fatalError("impossible")
}
~~~
- **Semicolons :**
Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.
- **Parentheses :**
Parentheses around conditionals are not required and should be omitted. Note: In larger expressions, optional parentheses can sometimes make code read more clearly. 

Preferred: 
~~~
if name == "Bandish" {
  print("Hii Bandish")
}
~~~
Not Preferred:
~~~
if (name == "Bandish") {
  print("Hii Bandish")
}
~~~

#### iOS Standard Guidelines

- Cocoapod is used as dependency manager (using cocoapods is very easy, and all third-party frameworks use this manager.)
- The properties of a model class should not be declared as “var”, unless there are such requirements
- Structure the code with PRAGMA MARKS and comments.
- Do not store sensitive data in UserDefaults/plist/DB. Always use Keychain.
- Always write a wrapper for third party libraries so that the library is imported only in the wrapper. This ensures data Abstraction. 
- Structure the files by setting up some folder structure depending on app architecture
- Profile the app with Instruments before sending build.

#### iOS UI Best Practices
- Create a common class for Color theme and use that everywhere
- Create BaseViewController that has common functions(showing alert, network checker, etc) and inherit from it
- Use Autolayout for positioning any UI element(avoid setting frames)
- Do not fix the height for UI element. Instead set the constraints accordingly.
- Set the constraints with multiplier or priority when required(instead of setting constant value)
- Use placeholder images till the actual image is downloaded(Download should happen in background. Do not block main thread)
- Request personal data only when your app clearly needs it
- use standard navigation controls such as page controls, tab bars, segmented controls, table views, collection views, and split views.
- Use a navigation bar to traverse a hierarchy of data.
- Fix the UIImageView Content Mode
- Test Different iPhone Size Classes
- Fix Constraint Conflicts
- Don’t use a single Storyboard file for your entire app. Create one storyboard file for each flow in the app. 
- Use size classes for iPhone & iPad UI(Don’t Create separate UI).
