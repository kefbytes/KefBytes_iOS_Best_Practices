# iOS Best Practices
### Some best practices for iOS

*"Focus on providing the best user experience at all phases of application development."*
	
https://developer.apple.com/ios/human-interface-guidelines/

## Architecture
1. Frameworks & Libraries
	* Use Apple Code whenever possible
	* When necessary to use third party libraries select well established and supported libraries 
		* Popular and well supported libraries
			* Networking
				* AFNetworking - Obj C
				* Alamofire - Swift
			* JSON Parsing
				* JSONModel
				* ObjectMapper
			* CoreData
				* MagicalRecord
				* SugarRecord
			* Image Downloading and Caching
				* SDWebImage
				* KingFisher
	* Make use of reusable modules
	* Use Dependency Managers when possible
		* CocoaPods
		* Carthage
		* Swift Package Manager
2. Authentication & Security: https://www.apple.com/business/docs/iOS_Security_Guide.pdf
	* centralize your security, don’t apply it piecemeal
	* Never use NSUserDefaults, other plist files on disk or Core Data for sensitive data
	* Store sensitive information in Keychain
	* Do not store sensitive data you don't actually need, or for longer than you need
	* Only keep sensitive data around while you need it.
	* Store application-specific authentication tokens rather than passwords
	* Use HTTPS to verify the server you are contacting. Never accept an invalid or untrusted certificate
	* Use the built-in APIs to store things. As Apple improves security, you get the benefits for free
	* Set Data Protection to NSFileProtectionComplete wherever possible
3. Persistence Layer	
	* Good post describing the apps file system: https://petermcintyre.com/topics/persisting-data-on-ios/
	* The options for persisting data are
		* SQLite - Able to access data with sql queries, can store large amount of data
		* Core Data - grows and scales well with your app, often uses SQLite underneath, create a singleton manager class that handles setting up the Core Data stack
		* plist file -  contain either an NSDictionary or an NSArray
		* NSUserDefaults -  meant for nothing more than settings and preferences
		* Keychain - store sensitive data here
		* Custom Files - It is possible to store data into your own file type that gets saved in the apps file system
	* As a general rule. Save any sensitive data such as passwords in the keychain. Save preferences or current app settings in NSUserDefaults. Save larger amounts of data in either SQLite or Core Data. PList files are useful for storing dictionaries of non sensitive data.
4. Design Patterns
	* MVC: Model-View-Controller
	* MVCS: Model-View-Controller-Store
	* MVVM: Model-View-ViewModel
	* The age old pattern that everyone learns first is MVC. But in iOS this pattern often leads to bloated ViewControllers. As a result much often we will use a ViewModel. using this pattern we are able to strip everything except the basic view components out of the view and put them in the ViewModel. It is also common to see a Store implemented. The role of the store is to //handle all data requests. In the MVC and MVVM patterns the functionality of the store is usually built into the model.
5. Unit Testing
	* unit tests should not be written to test for bugs
	* unit tests are most effective when using TDD (Test Driven Development)
	* unit tests should be used for designing individual robust software components
	* unit tests should be effective regardless of the data, meaning they are data independent
	* unit tests work on small individual pieces of code, changes to other pieces of the code should not break them
	* unit tests should not rely on other unit tests
	* unit tests should be stateless
	* mock out data needed for any unti tests
	* don't rely too much on common setup code for individual unit tests
	* name your unit tests clearly
6. Code Repository
	* Use git for your code repository
	* Branching Strategy
		*  A good branching strategy makes the developers job easier and protects against unnessary code breaks
		*  A universally recognized branching strategy makes it easy for developers to know expectations
		*  GitFlow is a common strategy that is widely used and has stood up well over the course of time
		Here are the basic ideas of GitFlow:
			* There are two infinite branches (these names are common but not mandatory)
				* master
					* master branch always represents the code that is in production
				* develop
					* develop branch is similar to master but is ahead by current development
			* In addition to the infinite branches you have any number of branches that fall into one of three or more types
				* hot fix (bugs found in prod)
					* hot fixes are branched off of master and merged back into master as well as develop when completed and (optionally) code reviewed.
				* feature (story)
					* features are branched off of develop and merged back into develop after code review
				* release
					* release is branched off develop when enough features are complete to comprise a release, or optionally at the end of a sprint
					* release is merged into master and back into develop when released
					* when release is created this is a code freeze for this particular release, develop then continues towards the next release
					* you may branch off release for bugs found in QA, these branches are merged back into release
	* agree on commit message standards
7. Project Structure
	* Your actual structure is not as important as just having one and sticking to It
	* Here is an example of a project structure
		*  Views
		*  ViewModels
		*  Stores
		*  Utilities
		*  Assets - Asset catalogs are the best way to manage all your project's visual assets.
8. Localization
	* Localization helps your app get distributed to more users
	* Use localization from the start
9. Event Patterns
	* Delegation: (one-to-one) Apple uses this a lot (some would say, too much). Use when you want to communicate stuff back e.g. from a modal view.
	* Callback blocks: (one-to-one) Allow for a more loose coupling, while keeping related code sections close to each other. Also scales better than delegation when there are many senders.
	* Notification Center: (one-to-many) Possibly the most common way for objects to emit “events” to multiple observers. Very loose coupling — notifications can even be observed globally without reference to the dispatching object.
		* Don’t use NSNotifications for passing data between different parts of your app.
	* Key-Value Observing (KVO): (one-to-many) Does not require the observed object to explicitly “emit events” as long as it is Key-Value Coding (KVC) compliant for the observed keys (properties). Usually not recommended due to its implicit nature and the cumbersome standard library API.
	* Signals: (one-to-many) The centerpiece of ReactiveCocoa, they allow chaining and combining to your heart's content, thereby offering a way out of callback hell.
	* Target - Action: When a user interacts with a UI element an Action is triggered.
10. Models 
	* Keep your models immutable, and use them to translate the remote API's semantics and types to your app.
	* Always fetch data via a data source protocol rather than exposing setters
11. Views: 
	* You should use size classes and Auto Layout to declare constraints on your views. The system will then calculate the appropriate frames based on these rules, and re-evaluate them when the environment changes.
	* Set up your layout constraints to create and activate them once during initialization. If you need to change your constraints dynamically, hold references to them and then deactivate/activate them as required. 
	* Anything beyond static presentation information in a view should be requested via a data source or delegate
12. Controllers:
	* Use dependency injection, i.e. pass any required objects in as parameters, instead of keeping all state around in singletons. The latter is okay only if the state really is global.
	* Avoid bloating your view controllers, use ViewModels keep your ViewControllers lean and clean
13. Push notifications
	* Don’t ignore push notifications until the very end of the app, if you plan on including them do so from the start.
14. Don't ignore “network reachability”
15. Navigation	
	* Don’t create your own navigation unless you’re an expert problem solver - because it will be problematic. 
	* Use built in classes and customize.
	* Every instance of a UIViewController has a navigationItem property which should be used to specify the left and right navigation buttons and the title view.
	* You should not create a UINavigationItem object because the base UIViewController implementation will automatically create one when you access self.navigationItem.
	* Access self.navigationItem and set its properties accordingly.
16. Performance
	* Use cache whenever feasible
17. Networking:
	* Networking calls should never be on the same thread as UI
	* Create a seperate networking layer
	* Give users some indication that network calls are taking place if they are waiting on something
	* Keep any HTTP traffic to remote servers encrypted with TLS at all times
	* AFNetworking and Alamofire support certificate pinning out of the box
	* Create a central manager (singleton) class to handle API requests
18. Logging:
	* Use proper logging levels
	* NSLog should not be used for detailed logging in production apps, since it slows down your application.
	* Provide the right amount of information; no more, no less. Avoid creating clutter.
	* Use hashtags and log levels to make your log messages easier to search and filter.
	* Categorize the event. For example, use the severity values INFO, WARN, ERROR, and DEBUG.
	* If you log to a local file, it provides a local buffer and you aren't blocked if the network goes down.
	* Come up with a rotation strategy for your logs so they don't get overwhelming and take up too much space
19. Profiling:
	* Use Instruments to profile your app
20. Analytics:
	* Business generally dictates what to track, but here are some common elements which get tracked
		*  Track App Opens
		*  Track Logins
		*  Track Signups
		*  Track App Views
21. Block is a part of Objective-C syntax, which allows to pass a function to another function as a direct argument.

## XCode
* The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. 
* Any Xcode groups created should be reflected by folders in the filesystem. 
* Code should be grouped not only by type, but also by feature for greater clarity.
* When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many additional warnings as possible. 
* If you need to ignore a specific warning, use Clang's pragma feature.
* Xcode short cut tips
	* https://spin.atomicobject.com/2014/03/23/xcode-keyboard-shortcuts/
	* https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_help-command_shortcuts/MenuCommands/MenuCommands014.html
	* http://five.agency/xcode-keyboard-shortcuts-which-will-boost-your-productivity/
* Always provide error handling code to allow for graceful handling of errors

## UI
1. style Guides 
	* A style guide listing standards for UI elements go a long way
	* Items to include in the style guide include but are not limited to
		* Colors
		* Fonts
		* Button sizes and shapes
		* Resuable assets
2. standardized Colors
	* Use a UIColor Category to define all your colors and import the category in the prefix.pch file for your project
	* Don't use Macro's. Example #define Color...
3. designing for different devices and screen sizes
	* Use adaptive layout, size classes and auto layout to design for different devices and screen sizes
4. storyboards, nibs or code 
	* The use of storyboards vs nibs vs coce is a hotly debated topic. We live in a world where apps are often maintained by developers coming and going over the lifetime of the app. Enhancements need to be implemented in a timely manner. New developers come on board and need to get up to speed as quickly as possible. With these things in mind follow this very general guideline.
		* Use storyboards whenever possible
		* When you come across a view that just can't be laid out right in a storyboard use a nibs
		* When a view is so complicated that neither a storyboard or nib will work lay it out in code
	* Use multiple storyboards so individual ones don't get too large
	* for repeatable views used over and over again nibs are a good optional
	* code allows the most flexibility but can take longer to layout and maintain
5. UIStackView
	* Since UIStackViews were introduced Apple seems to be encouraging the use of them more and more

## Code
1. constants
	* Constants are preferred over in-line string literals or numbers
	* Swift - use a struct file
	* ObjC - Constants.h file (include in prefix header)
		*"when you only need it inside a class, it should live in that class. Those constants that need to be truly app-wide should be kept in one place"*
		*"Instead of preprocessor macro definitions (via #define), use actual constants"*
	* Constants should be declared as static constants and not #defines unless explicitly being used as a macro
		* consts are much more compiler and debugger friendly than #defines
2. enums
	* enums can make your code more readable by substituting strings for Int values
	* When using enums, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion.
	* typedef NS_ENUM GlobalConstants not enum GlobalConstants
	* Swift enums are much more powerful than their ObjC counterparts
3. Categories
	* Categories can be effective when used sparingly for things like colors.
	* Don’t go overboard with categories, swizzling and other exotic things.
4. Keep a clean AppDelegate
	* AppDelegate should be used for app life cycle events
	* Core Data setup does not belong in AppDelegate
	* Passing data around by creating instances of the AppDelegate in ViewController is not good practice
5. Coding Standards & Style Guide (syntax consistency)
	* There are more than a few coding styles and opinions on such, the important thing is to agree on a coding style and be consistent with it across the team
	* Here are a couple popular and widely used coding styles
		* NY Times Coding Standards - https://github.com/NYTimes/objective-c-style-guide
		* Ray Wenderlich: Swift and Objective-C - https://github.com/raywenderlich/objective-c-style-guide
	* Use consistent spacing, indents etc...
	* Use a consistent bracing format
	* White space should consistent and should be used to make the code more readable
	* Dot-notation should always be used for accessing and mutating properties, as it makes code more concise. Bracket notation is preferred in all other instances.
	* Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class.
	* Objective-C uses YES and NO. Therefore true and false should only be used for CoreFoundation, C or C++ code
	* Since nil resolves to NO it is unnecessary to compare it in conditions
	* Never compare something directly to YES, because YES is defined to 1 and a BOOL can be up to 8 bits
	* Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors
	* The Ternary operator, ?: , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated
	* Init methods should follow the convention provided by Apple's generated code template. A return type of 'instancetype' should also be used instead of 'id'.
	* When accessing the x, y, width, or height of a CGRect, always use the CGGeometry functions instead of direct struct member access.
	* When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path.
	* Singleton objects should use a thread-safe pattern for creating their shared instance.
	* Never access self.view in your controller's initialization methods
	* Only expose public properties and methods in headers
5. Naming: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html
	* A method beginning with a verb indicates that it performs some side effects, but won't return anything
	* Any method starting with a noun, however, returns that object and should do so without side effects
	* It is good to be both clear and brief as possible, but clarity shouldn’t suffer because of brevity
	* In general, don’t abbreviate names of things. Spell them out, even if they’re long
		* An exception may be commonly used abreviations like VC for ViewController but even these could be confusing to someone coming from a different language. Here's a list of acceptable abreviations and acronyms: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html#//apple_ref/doc/uid/20001285-BCIHCGAE
	* Be consistent with your naming
	* Use prefixes when naming classes, protocols, functions, constants, and typedef structures.
	* Use camel-casing
	* The name of a class should contain a noun that clearly indicates what the class (or objects of the class) represent or do.
	* Protocols should be named according to how they group behaviors
	* For methods that represent actions an object takes, start the name with a verb
	* Do not use “do” or “does” as part of the name because these auxiliary verbs rarely add meaning. Also, never use adverbs or adjectives before the verb.
	* The use of “get” is unnecessary
	* Use keywords before all arguments
	* For delegate methods start the name by identifying the class of the object that’s sending the message
7. Code Structure within a file:
	* Use #pragma mark - to categorize methods in functional groupings
8. Comments: 
	* Block comments should generally be avoided
	* code should be as self-documenting as much as possible
9.  Variables
	* Always give a value. Declare and initialize them just before use.
10. Debugging
	* Use lldb for debugging
		* lldb allows you to inspect properties on classes that don't have explicit ivars declared in the object's interface.
	* Use NSZombieEnabled to find object leaks
11. Refactor constantly

## Build
1. define the minimum version supported
2. use continuous integration
	1. code is checked in
	2. jenkins kicks in
		* tests run
			* automated tests
			* unit tests
		* automated build
		* build automatically uploaded to continuos delivery service such as Crashlytics
	3. build availble to QA
		*  the app is thus maintained in a deployable state where a build always exists that’s been tested and is ready to deploy
3. debug vs release builds
	* you can add more build configurations on the "Info" tab of your project settings in Xcode.
	* use configuration settings files (“.xcconfig files”)  to set build settings
		* you can add comments to explain things
		* you can use #include to include other build settings files avoiding duplication
	* turn off code optimizations for Debug builds
	* enable code optimization for Release builds
		* there is a lot more optimization going on at compile time, at the expense of debugging possibilities
4. build configurations are used to specify builds that are signed for specific profiles
5. targets
	* a target resides conceptually below the project level
	* use target settings to override the project buuild settings for individual targets

## Deploy
1. use code signing to assign specific profiles to builds that can then be distributed for specific purposes
	* profiles are maintained on Apple’s developer portal and given an app ID
		* one profile for development
		* one profile for QA
		* one profile for app store release
2. during continuos integration push builds to a delivery service such as Crashlytics for deployment
