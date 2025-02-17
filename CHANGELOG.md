# Change Log
All notable changes to this project will be documented in this file.

## 9.0.7

### Fixes

* #527 Crash while clicking two times to hide the presenting controller
* #517 Prevent orphaned views from blocking the queue
* Prevent orphaned `SwiftMessagesSeque`s from retaining the presenting view controller

## 9.0.6

### Features

* Add `UIView` associated type to `Event`, e.g. `willShow(UIView)` so that event listeners can inspect the view.
* Add `Event.id: String?` property so that event listeners can reason about the view's ID.

## 9.0.5

### Fixes

* #482 Fix timing of `KeyboardTrackingView` callbacks.
* #483 KeyboardTrackingView causes a small space under bottom-style view

## 9.0.4

* #471 Xcode 13 issue - Enum cases with associated values cannot be marked potentially unavailable with '@available'
* Improve colors for dark mode.

## 9.0.3

### Fixes

* #467 Lower or equal level window's views disappear upon hide
* #466 Alert not shown after Biometry check
* #465 Fix broken Carthage build. The Carthage build was broken due to the `iMessageDemo` project's use of CocoaPods and the automatically generated `SwiftMessages` framework scheme created by CocoaPods. The podfile was modified to delete this scheme, but Carthage users may need to run `pod install` on the `iMessagesDemo` project, if they have CocoaPods installed, or manually delete the `iMessageDemo/Pods/Pods.xcodeproj/xcuserdata` folder.

## 9.0.2

### Fixes

* Fix app extension compile error when using CocoaPods.

## 9.0.1

### Fixes

* #455 #458 Restore key window after message is interacted with. When a message becomes the key window, such as if the user interacts with the message, iOS does not automatically restore the previous key window when the message is dismissed. SwiftMessages has some logic in `WindowViewController` to restore the key window. This change makes that logic more robust.

## 9.0.0

### Features

* #447 Add the ability to show view controller in a new window with `SwiftMessagesSegue`.
This capability is available when using `SwiftMessagesSegue` programmatically by supplying
an instance of `WindowViewController` as the segue's source view controller.

### Changes

* This release has minor breaking changes in the `WindowViewController` initializers.
The `windowLevel` is no longer accepted as an argument because the `config` parameter
should specify the window level in the `presentationContext` property.

### Fixes

* #451 Fix app extension crash

## 8.0.5

### Fixes

* #446 Restore previous key window on dismissal if the message assumed key window status.

## 8.0.4

### Features

* #442 Add `MarginAdjustable.respectSafeArea` option to exclude safe area from layout margins.
* #430 Support disable `becomeKeyWindow` from SwiftMessages.Config. This is a workaround for potential issues with apps that display additional windows.

### Fixes

* #437 Revert to explicitly specifying "SwiftMessages" as the module in nib files.
* #440 Fix crash when using SwiftMessages in app extension

## 8.0.3

### Features

* Full support for Swift Package Manager

### Fixes

* #328 ignoreDuplicates is not working
* #412 Fix deployment target on nib files to match target

## 8.0.2

### Changes

* [#395](https://github.com/SwiftKickMobile/SwiftMessages/pull/395) Add preliminary support for Swift Package Manager.

## 8.0.1

### Fixes

* #401 UIAlertController pops up but SwiftMessage layer absorbs all touches.

## 8.0.0

### Changes

* Add `SwiftMessages.PresentationContext.windowScene` option for targeting a specific window scene.
* Changed the behavior of the default `presentationContext`, `.automatic`. Previously, if the root view controller was presenting, the message would only be displayed over the presented view controller if the `modalPresentationStyle` was `fullScreen` or `overFullScreen`. Now, messages are always displayed over presented view controllers.
* Made `showDuraton` and `hideDuration` on `Animator` non-optional.
* Made `showDuraton` and `hideDuration` writable options on `TopBottomAnimation` and `PhysicsAnimation`.

### Fixes

* #365 Fix an issue with customized `TopBottomAnimation` where messages weren't properly displayed under navigation and tab bars.
* #352 Fix accessibility for view controllers presented with `SwiftMessagesSegue`.
* #355 Update card view layout to support centering of pure textual content 
* #354 Support `overrideUserInterfaceStyle` when view presented in its own window
* #360 Fix touch handing issue in iOS 13.1.3
* #382 Fix warnings in Xcode 11.4

## 7.0.1

### Changes

* Support iOS 13.

### Features
* #335 Add option to hide status bar when view is displayed in a window. As of iOS 13, windows can no longer cover the status bar. The only alternative is to set `Config.prefersStatusBarHidden = true` to hide it.

## 7.0.0

### Changes

* Swift 5
* Remove deprecated APIs

### Features

* #313 Improved sizing on iPad
  
>`SwiftMessagesSegue` provides default view controller sizing based on device, with width on iPad being limited to 500pt max. However, it is recommended that you explicitly specify size appropriate for your content using one of the following methods.
>  1. Define sufficient width and height constraints in your view controller such that it sizes itself.
>  1. Set the `preferredContentSize` property (a.k.a "Use Preferred Explicit Size" in Interface Builder's attribute inspector). Zeros are ignored, e.g. `CGSize(width: 0, height: 350)` only affects the height.
>  1. Add explicit width and/or height constraints to `segue.messageView.backgroundView`.  
>
>Note that `Layout.topMessage` and `Layout.bottomMessage` are always full screen width. For other layouts, the there is a maximum 500pt width on for regular horizontal size class (iPad) at 950 priority. This limit can be overridden by adding higher-priority constraints.

* #275 Add ability to avoid the keyboard.

>The `KeyboardTrackingView` class can be used to cause the message view to avoid the keyboard by sliding up when the keyboard gets too close.
>
>````swift
>// Message view
>var config = SwiftMessages.defaultConfig
>config.keyboardTrackingView = KeyboardTrackingView()
>
>// Or view controller
>segue.keyboardTrackingView = KeyboardTrackingView()
>````
>You can incorporate `KeyboardTrackingView` into your app even when you're not using SwiftMessages. Install into your view hierarchy by pinning `KeyboardTrackingView` to the bottom, leading, and trailing edges of the screen. Then pin the bottom of your content that should avoid the keyboard to the top `KeyboardTrackingView`. Use an equality constraint to strictly track the keyboard or an inequality constraint to only move when the keyboard gets too close. `KeyboardTrackingView` works by observing keyboard notifications and adjusting its height to maintain its top edge above the keyboard, thereby pushing your content up. See the comments in `KeyboardTrackingView` for configuration options.


* #276 Add ability to hide message without animation
* #272 Add duration for `SwiftMessagesSegue`
* #278 Make pan gesture recognizers public

## 6.0.2

### Features

* #262 Add event listeners to `SwiftMessagesSegue`.

## 6.0.1

### Features
* #257 The `.centered` presentation style, which is a shortcut for a specific configuration of the `PhysicsAnimation` animator, provides a physics-based dismissal gesture where the view can be flung off screen. When the view goes out of the container view's bounds, the animator calls `SwiftMessages.hide()`, which animates the dim view away and concludes the message view's lifecycle. There is currently a small delay of 0.2s before calling `hide()`.

This change adds the ability to configure the delay by customizing the animator. For example, to set the delay to zero, one would do:

````swift
let animation = PhysicsAnimation()
animation.panHandler.hideDelay = 0
config.presentationStyle = .custom(animator: animation)
````

## 6.0.0

### Changes

* Migrate to Swift 4.2

### Fixes

* Fix #228 restore shared SwiftMessages scheme

## 5.0.1

### Fixes

* Remove debug code that broke the view controller's section of the Demo app.

## 5.0.0

### Breaking Changes

* Removed support for iOS 8.

### Features
* Add support for modal view controller presentation using [`SwiftMessagesSegue`](./SwiftMessages/SwiftMessagesSegue.swift) custom segue subclass. Try it out in the "View Controllers" section of the Demo app. In addition to the class documentation, more can be found in the [View Controllers](./ViewControllers.md) readme.
* Update nib files to be more visually consistent with iPhone X:
  * Introduce [`CornerRoundingView`](./SwiftMessages/CornerRoundingView.swift), which provides configurable corner rounding using squircles (the smoother method of rounding corners that you see on app icons). Nib files that feature rounded corners have their `backgroundView` assigned to a `CornerRoundingView`. `CornerRoundingView` provides a `roundsLeadingCorners` option to dynamically round only the leading corners of the view when presented from top or bottom (a feature used for the tab-style layouts).
  * Increased the default corner radius to 20. Corner radius can be changed by either modifying the nib file or 
* Reworked the [`MarginAdjustable`](./SwiftMessages/MarginAdjustable.swift) to improve configurability of layout margins.
* Add rubber-banding to the interactive dismissal gesture. Rubber banding is automatically applied for views where `backgroundView` is inset from the message view's edges.
* Added `showDuration` and `hideDuration` properties to the `Animator` protocol (with default implementation that returns `nil`). These values enable animations to work for view controller presentation.

### Fixes

* #202 bodyLabel should set textAlignment to .natural
* #200 Automatic Presentation Context Broken
* Fix default value of `TopBottomAnimation.closePercentThreshold`

## 4.1.4

### Bug Fixes
* Fix #191 Prevent usage of UIApplication.shared when building for extensions

### Improvements
* #192 Add a way to test compilation with app extension

## 4.1.3

### Features
* #183 Added iOS app extension support at compile time.

### Bug Fixes
* Fix #185 Incorrect margin adjustments in landscape
* Fix #188 Physics animation visual glitch

## 4.1.2

### Features
* Updates for Swift 4.1
* #164 Added an optional `windowViewController` property to `SwiftMessages.Config` for supplying a custom subclass of `WindowViewController`.

### Bug Fixes
* Custom presentation styles using `TopBottomAnimation` now display properly under top and bottom bars.

## 4.1.1

### Features
* #152 Get current message being displayed without specifying an `id`

## 4.1.0

### Features

* Fix #134 add support for `CenterAnimation` displayed on top or bottom instead of center (renamed to `PhysicsAnimation`).

### Fixes

* Fix #128 move icons out of asset catalog to prevent mysterious crash
* Fix #129 adjust layout margins on orientation change to preserve layout when iOS hides status bar in landscape.
* Fix #131 by always completing hide/show animations if application isn't active.


## 4.0.0

### Features
* Swift 4.0 syntax
* Added support for iOS 11 and iPhone X. From the readme:

  SwiftMessages 4 supports iOS 11 out-of-the-box with built-in support   for safe areas. To ensur  that message view layouts look just right when overlapping safe areas, views that adopt the `MarginAdjustable` protocol (like `MessageView`) will have their layout margins automatically adjusted by SwiftMessages. However, there is no one-size-fits-all adjustment, so the following properties were added to `MarginAdjustable` to allow for additional adjustments to be made to the layout margins:

  ````swift
  public protocol MarginAdjustable {
      ...
      /// Safe area top adjustment in iOS 11+
      var safeAreaTopOffset: CGFloat { get set }
      /// Safe area bottom adjustment in iOS 11+
      var safeAreaBottomOffset: CGFloat { get set }
  }
  ````

  If you're using using custom nib files or view classes and your layouts don't look quite right, try adjusting the values of these properties. `BaseView` (the super class of `MessageView`) declares these properties to be `@IBDesignable` and you can find sample values in the nib files included with SwiftMessages.

### Bug Fixes
* Fix #100 memory leak.
* Change `Layout` enum capitalization to current Swift conventions.

## [3.5.1](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.5.0)

### Bug Fixes
* Undo change that broke `MessageView` class reference on nib files copied out of the SwiftMessages framework.

## [3.5.0](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.5.0)

### Features
* Added `SwiftMessages.hideCounted(id:)` method of hiding. The counted method hides when the number of calls to `show()` and `hideCounted(id:)` for a 
given message ID are equal. This can be useful for messages that may be
shown from  multiple code paths to ensure that all paths are ready to hide.

  Also added `SwiftMessages.count(id:)` to get the current count and `SwiftMessages.set(id:count:)` to set the current count.

* Added ways to retrieve message views currently being shown, hidden, or queued to be shown.

  ````swift
  // Get a message view with the given ID if it is currently 
  // being shown or hidden.
  if let view = SwiftMessages.current(id: "some id") { ... }
  
  // Get a message view with the given ID if is it currently 
  // queued to be shown. 
  if let view = SwiftMessages.queued(id: "some id") { ... }
  
  // Get a message view with the given ID if it is currently being
  // shown, hidden or in the queue to be shown.
  if let view = SwiftMessages.currentOrQueued(id: "some id") { ... }
  ````

### Bug Fixes
* Fix #116 for message views that don't adopt the `Identifiable` protocol by using the memory address as the ID.
* Fix #113 MessageView not hiding
* Fix #87 Support manual install

## [3.4.0](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.4.0)

### Features
* Added `.center` presentation style with a physics-based dismissal gesture.
* Added `.custom(animator:)` presentation style, where you provide an instance of the `Animator` protocol. The `TopBottomAnimation` and `CenterAnimation` animations both implement `Animator` and may be subclassed (configuration options will be added in a future release). `PhysicsPanHandler` class to provide a physics-based dismissal gesture.
* Added `.centered` message view layout with elements centered and arranged vertically.
* Added `configureBackgroundView(width:)` and `configureBackgroundView(sideMargin:)` convenience methods to `MessageView`.

## [3.3.4](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.3.4)

### Features
* #89 Add `blur` dim mode option.

### Bug Fixes
* #98 Fix touch handling in message view's background view.
* #97 Fix main thread checker warning 

## [3.3.3](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.3.3)

### Bug Fixes
* Fix an issue where rapidly showing and hiding messages could result in messages becoming orphaned on-screen.

## [3.3.2](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.3.2)

### Improvements
* `MessageView` is smarter about including additional accessibility views for cases where you've added accessible elements to the view. Previously only the `button` was included. Now all views where `isAccessibilityElement == true`.

    Note that all nib files now have `isAccessibilityElement == false` for `titleLabel`, `bodyLabel` and `iconLabel` (`titleLabel` and `bodyLabel` are read out as part of the overall message view's text). If any of these need to be directly accessible, then copy the nib file into your project and select "Enabled" in the Accessibility section of the Identity Inspector.

## [3.3.1](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.3.1)

### Bug Fixes
* Fix regression where the UI was being blocked when using `DimMode.none`.

## [3.3.0](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.3.0)

### Features
* Add proper support for VoiceOver. See the [Accessibility section](README.md#accessibility) of the readme.

## [3.2.1](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.2.1)

### Bug Fixes
* Fix infinite loop bug introduced in 3.2.0.

## [3.2.0](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.2.0)

### Features
* Added the ability to display messages for an indefinite duration while enforcing a minimum duration using `Duration.indefinite(delay:minimum)`.

This option is useful for displaying a message when a process is taking too long but you don't want to display the message if the process completes in a reasonable amount of time.
         
For example, if a URL load is expected to complete in 2 seconds, you may use the value `unknown(delay: 2, minimum 1)` to ensure that the message will not be displayed most of the time, but will be displayed for at least 1 second if the operation takes longer than 2 seconds. By specifying a minimum duration, you can avoid hiding the message too fast if the operation finishes right after the delay interval.

### Bug Fixes
* Prevent views below the dim view from receiving accessibility focus.
* Prevent taps in the message view from hiding when using interactive dim mode.
* Fix memory leak of single message view

## [3.1.5](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.1.5)

### Bug Fixes

* Fix memory leak of `MessageViews`.

## [3.1.4](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.1.4)

### Bug Fixes

* Fixed an issue where UIKit components were being instantiated off the main queue.

## [3.1.3](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.1.3)

### Features

* Added the ability to set a custom value on `MessageView.id`. This can be useful for dismissing specific messages using a pre-defined `id`.

## [3.1.2](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.1.2)

### Features

* Changed the default behavior when a message is displayed in its own window (such as with `.window` presentation context) to no longer become the key window in order to prevent the keyboard from dismissing. If one needs the message's window to become key, this can be done by setting `SwiftMessages.Config.becomeKeyWindow` to `true`. See 

### Bug Fixes

* Changed the internal logic of hiding a message view to always succeed to work around the problem of the hide animation failing, such as when started while the app is not active.
* Improved reliability of the automatic adjustments made to avoid message views overlapping the status bar, particularly when using the `.view` presentation context.

## [3.1.1](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.1.1)

### Features

* Added a `view` case to `presentationContext` for displaying the message in a specific container view.

## [3.1.0](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.1.0)

### Features

* Add `eventListeners` option to `SwiftMessages.Config` to specify a list of event listeners to be notified of message show and hide events.

## [3.0.3](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.0.3)

### Features

* Add `ignoreDuplicates` option to `SwiftMessages.Config` to specify whether or not to ignore duplicate `Identifiable` message views.

## [3.0.2](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.0.2)

### Features

* Add `shouldAutorotate` option to `SwiftMessages.Config` for customizing device rotation behavior when messages are presented in an overlay window. By default, message will auto-rotate.

## [3.0.1](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.0.1)

### Improvements

* Enable automatic provisioning on framework target

## [3.0.0](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/3.0.0)

### Breaking Changes

* Support for Swift 3.0.

## [2.0.0](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/2.0.0)

### Breaking Changes

* Support for Swift 2.3 and Xcode 8.

  The nib files needed to be updated to Xcode 8 format to work 
  around an iOS bug. Unfortunately, this makes it necessary 
  to break backward compatibility with Swift 2.2 and Xcode 7.

## [1.1.4](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/1.1.4)

### Bug Fixes

* Fix #16 Preserve status bar visibility when displaying message in a new window.

## [1.1.3](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/1.1.3)

### Features

* Add default configuration `SwiftMessages.defaultConfig` that can be used when calling the variants of `show()` that don't take a `config` argument or as a global base for custom configs.
* Add `Array.sm_random()` function that returns a random element from the array. Can be used to create a playful
     message that cycles randomly through a set of emoji icons, for example.

### Bug Fixes

* Fix #5 Emoji not shown!
* Fix #6 There is no way to create SwiftMessages instance as there is no public initializer

## [1.1.2](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/1.1.2)

### Bug Fixes

* Fix Carthage-related issues.

## [1.1.1](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/1.1.1)

### Features

* New layout `Layout.TabView` — like `Layout.CardView` with one end attached to the super view.

### Bug Fixes

* Fix spacing between title and body text in `Layout.CardView` when body text wraps.

## [1.1.0](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/1.1.0)

### Improvements

### API Changes

* The `BaseView.contentView` property of was removed because it no longer had any functionality in the framework.

    This is a minor backwards incompatible change. If you've copied one of the included nib files from a previous release, you may get a key-value coding runtime error related to contentView, in which case you can subclass the view and add a `contentView` property or you can remove the outlet connection in Interface Builder.

## [1.0.3](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/1.0.2)

### Improvements

* Remove the `iconContainer` property from `MessageView`.

### Bug Fixes

* Fix constraints generated by `BaseView.installContentView()`.

## [1.0.2](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/1.0.2)

### Features

* Add support for specifying an `IconStyle` in the `MessageView.configureTheme()` convenience function.

## [1.0.1](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/1.0.1)

* Add code comments.

## [1.0.0](https://github.com/SwiftKickMobile/SwiftMessages/releases/tag/1.0.0)

* Initial release.
