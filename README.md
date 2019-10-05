# Best-Practice
Best practices for iOS App Development

#### [iOS App Performance](#ios-app-performance)
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

