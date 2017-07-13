# Chain React Conference Notes
[React Native Conference](https://infinite.red/ChainReactConf)

Slack `community.infinite.red`

The Jump?

<!-- TOC depthFrom:2 depthTo:2 withLinks:1 updateOnSave:1 orderedList:0 -->

- [The Dark Arts of the bundlers - Mike Grabowski](#the-dark-arts-of-the-bundlers-mike-grabowski)
- [Open Source Contributing - Brent Vatne](#open-source-contributing-brent-vatne)
- [AWS/Runtime process - Richard Threlkeld](#awsruntime-process-richard-threlkeld)
- [Paypal Checkout with RN - Poornima Venkatakrishnan](#paypal-checkout-with-rn-poornima-venkatakrishnan)
- [RN + ES.next - Ben Ilegbodu](#rn-esnext-ben-ilegbodu)
- [Mobile Payments - Naoufal Kadhom](#mobile-payments-naoufal-kadhom)
- [Gestures Everywhere - Kyle Poole & Thomas Bruketta](#gestures-everywhere-kyle-poole-thomas-bruketta)
- [Gudog - Javier Cuevas](#gudog-javier-cuevas)
- [App Browser / Progressive Web Apps - Ken Wheeler](#app-browser-progressive-web-apps-ken-wheeler)
- [Break Down Bridging -  Peggy Rayzis](#break-down-bridging-peggy-rayzis)
- [Evolution of API Design - Eric Baer](#evolution-of-api-design-eric-baer)
- [DevOps - Ram Narasimhan](#devops-ram-narasimhan)
- [React as a Platform - Leland Richardson](#react-as-a-platform-leland-richardson)
- [Serverless Backend for RN Apps - Kevin Old](#serverless-backend-for-rn-apps-kevin-old)
- [Panel Discussion](#panel-discussion)
- [When to go Native over JavaScript - Harry Tormey](#when-to-go-native-over-javascript-harry-tormey)
- [How to make mobile apps feel good - Alex Kotliarskyi](#how-to-make-mobile-apps-feel-good-alex-kotliarskyi)
- [React Native on the Apple TV Platform - Doug Lowder](#react-native-on-the-apple-tv-platform-doug-lowder)
- [From Idea to App Store - Chris Ball](#from-idea-to-app-store-chris-ball)

<!-- /TOC -->

## The Dark Arts of the bundlers - Mike Grabowski
Takes all source files and turns them into a single file. But why?
In the beginning all required files where imported in html, but order is key.

Bundlers make sure that all files are available before the code requires it.

Bundlers can also provide:
* Transpiling
* Code Splitting
* Minification - uglify, tree shaking...

But what about react native?
The native devices can run a JS VM and create a data bridge between the native
device and the VM.

The only difference about how the code is run on the different devices is the
start command.

> Note: Look into Haste module for making modules available regardless of their
location.

RN really uses a 'Packager' which includes a bundler and many other tools. Their
focus is on providing a better DX, things like the debugger.

What is the packager? It's an HTTP server

Packager provides:
* Debugger
* Live Reloader
* Dev tools
* Symbolicate - provides the JS stack trace

Haul is a drop in solution for RN Packager powered by Webpack

[git:callstack-io/haul](https://github.com/callstack-io/haul)

## Open Source Contributing - Brent Vatne
Expo.io
Check out [`Snack`](https://snack.expo.io/)

Submitting PRs to the official docs will have more impact and the community will
keep it up to date.

If you're going to write a blog post to help some one understand the docs, you
might as well add that to the docs so that more people can benefit that those
that stumble upon your personal blog.

> Find and Evaluate libraries using `native.directory` on Expo

[git:expo/react-native-libraries](https://github.com/expo/react-native-libraries)

Anti-pitch: Let potential users know the limitations and constraints so that
they don't get frustrated trying to use something that isn't right for their
use case.

## AWS/Runtime process - Richard Threlkeld
SDK to support AWS with RN [git:awslabs/aws-sdk-react-native](https://github.com/awslabs/aws-sdk-react-native)

### Services:
 * Secure Auth
  1. Amazon Cognito (AuthN/AuthZ)

 * API Gateway/Serverless
  1. API Gateway
  1. "Front Door" for apps
  1. AWS Lambdas

 * PubSub
  1. AWS IoT

 * Analystics - Engagement
  1. PinPoint - Push, SMS, Email

> AWS Device Farm for device testing in the cloud

### APIs and Serverless

```
Client <-> Auth (Cognito)
        -> API (Gateway with greedy path to capture all traffic)
              -> AWS Lambdas
```

### PubSub

```
Client <-> AWS IoT PubSub with MQTT connection
```

### Amazon PinPoint

Push notification to groups or one-one

### Uploading

JavaScript SDK Managed Uploader support for RN for foreground Uploading

For background uploading, there are iOS and Android SDKs. Note: The iOS has limitations due to the os that limit upload to 30sec.

### Auth - Cognito

Cognito user pools - [JWT](https://jwt.io/) using [SRP](http://srp.stanford.edu/).
SRP removes the need for the client to send the password over the wire.
Essentially the backend doesn't store password, it store verifiers.

Federated identities - SAML, Social, etc. -> AWS Credentials

Sigv4 instead of simply using a Bearer token, the system uses fully signed tokens that are mathematically verified by the SDK.

## Paypal Checkout with RN - Poornima Venkatakrishnan
Problem: The only way for the app developers to receive payment via paypal requires opening a webview to complete the transaction and then returning to the app after payment.

Competitors such as Samsung, Google and Apple tap into the OS which allows for a native experience.

Paypal creates a SDK/package that can easily be dropped into a merchant's app.

For hybrid merchants:
This creates a webview with user information seeded in the view. Once the transaction is complete it deeplinks back to the merchant's app.

For native merchants:
The SDK takes over when the user selects to Checkout with Paypal and provides UI and triggers a callback of some kind.

> The module's size is a concern for app creators.
> The SDK's RN version could potentially conflict with the RN app's version.


## RN + ES.next - Ben Ilegbodu
DRY-er code through es6 syntax:
[Slides](http://www.benmvp.com/slides/2017/chainreact/react-native-esnext.html#/)

## Mobile Payments - Naoufal Kadhom
[naoufal/react-native-payments](https://github.com/naoufal/react-native-payments) - xplatform simplified purchase without a webform

### Web payments
Payment Request API - Make a request for a payment without presenting a form. Available in Chrome 60 (currently available in edge and chrome for android).

Pulls from the user's saved cards in their browser.

### Native
Apple Pay, Android Pay

### React Native Payments
Uses a polyfill to take advantage of the Payment Request API. This makes sharing code between web and RN much easier.

## Gestures Everywhere - Kyle Poole & Thomas Bruketta
Gestures simply UIs and reduce the amount of taps. Allows users to directly manipulate the thing they are looking at rather than using a control to impact something else on the page/app.

[Gestures](https://facebook.github.io/react-native/docs/gesture-responder-system.html)

### Touchables
Tap, Double Tap, Long Press

* TouchableWithoutFeedBack - just calls the callback but doesn't give the user an indication that it was pressed
* TouchableOpacity - Lowers capacity on press, and cb on release. User can slide of and cancle
* TouchableHighlight - Similar to opacity change, but sets a different color instead.

> `hitSlop` can increase the hit size without impacting layout

### ScrollViews
* ScrollView - Native scroll with all native hooks for you child content.
  - `Paging` can add multiple page carousel

> Plays nice with touchables and delegates the tap to scroll

### PanResponder
Impacts a single item rather than a container of objects. It is not a component. You'll need to pass this into a render of a component.

## Gudog - Javier Cuevas
Airbnb for dog walkers.

First used PhoneGap and then switched to react native.

Redux-saga is like a long running thread for side processes. Ignite generator for RN.

Need more convention in the project layouts.

## App Browser / Progressive Web Apps - Ken Wheeler
Most apps are aggregating content like facebook app. They are browsers.

Mobile web? Poor performance, poor capability its just not great for app development.

The first browser was actually called the 'WorldWideWeb'.

2006: There are document primitives, but not widget primitives. They aren't first class citizens.

HTML5 tries to address many of these concerns...

But web apps are still slow because they have to make up for all the missing functionality from the browsers.

Progressive Web Apps - Are web apps that are supposed to feel like responsive native apps. But they are really just a checklist of performance recommendations. They deal with platform limitations by imposing restrictions.

Web apps?
Discovery is good, distribution is free, but performance is not there.

What about native apps?
Hard for people to find them, distribution is a pain, but apps feel great.

What's an App Browser?
Uses URLs and loads websites, and opens native apps. Has fallbacks to website if app cant be loaded.

Polyfill your native app to put it into web.

TODOs
* Bridge communication for URL syncing and routing
* Handle local assets
* Handle permissions
* Standardized library

Exists on Expo.io

## Break Down Bridging -  Peggy Rayzis

> `react-native-create-bridge` can help generate the bridging code.

How we communicate between JS and native code.
You don't always need write it yourself. If performance is very important it can be best to write it as native and bridge to it.

There is a json representation for each native modules in the js vm. All events start on native and then it gets passed through the bridge to the js code.

The __message queue__ is responsible for handling that transfer of information. If its blocked then you wont get the event in js.
So you could...
* Restrict the amount of info getting passed across the bridge.
* Pass config once, and not with every event. Keep data flat.
* Write business on one side of the bridge.
* Keep data flat.

Debugging:
* __rn-snoopy__ is a debugging tool for bridging
* Triggering a yellow box warning can be a good tool for debugging

### Native UI Components
Only one instance of the Module is created per bridge.

View Manager is a factory that orchestrates the communication across the bridge.

Only one instance of View Manager is created per bridge. It declares data structures. Hooks up lifecycle events.

### Pain Points
* Context switching
* Boilerplate
* Not a lot of docs

> npm `react-native-create-bridge` generator for native bridging templates.

## Evolution of API Design - Eric Baer
GraphQL - Its where we've been going all along.
Its a spec not an implementation. There are implementations in many different languages.


## DevOps - Ram Narasimhan
[Mobile Center](https://mobile.azure.com)

### CodePush
Send packager output to cloud before sending it to the JS VM.
This can allow you to push your code to device.


## React as a Platform - Leland Richardson
RN and React are as easily compatible as one would hope.
These are separate __technologies__, not __concerns__.

Each component needs to select its dependency, implicitly html or explicitly native. A dependency injector could inject the correct dependency for the context (device).

[git:lelandrichardson/react-primitives](https://github.com/lelandrichardson/react-primitives) includes DI.

Use platform extensions \*.ios.js, \*.android.js

Render react components into Sketch using [react-sketchapp](https://github.com/airbnb/react-sketchapp). Could this eliminate the need for a styleguide?

## Serverless Backend for RN Apps - Kevin Old

AWS console can be difficult to configure. There is a cli or sdk that can help.

[Serverless](https://serverless.com/) framework can help, it provides many tools automation and convention.

Serverless breaks down into multiple components:
* Function
* Event
* Resource
* Service

```
Client  ->  Gateway ->  Lambda
        <-          <-
```

serverless.yml is the manifest of the lambda (function) and the api gateway.
Serverless includes tooling for deploy.

serverless-offline

## Panel Discussion
### yarn vs. npm
The aren't very many advantages over one another.
Yarn might be better for RN because of symlinks and packager interactions.
The disadvantage to yarn is that the whole group needs the same version of yarn.

### Navigation
react-navigation for RN

### Biggest Challenges with RN
* Breaking Changes and Backwards Compatibility Issues
* Facebook internal priorities vs. open source community use cases
* Performance issues over the bridge
* Accessibility support
* Security concerns in Android
* Enterprise adoption

## When to go Native over JavaScript - Harry Tormey
### Tooling
__Expo__ like rails for react native. Its great for pure JS and no native code. If you want to use some native code, then you will not have support from Expo. CodePush has similar problems around native code.

These native elements require changes to binaries.

### Thrid Party libraries
git:hgale - [medium](https://launchdrawer.com/)

Which ones contain native code?

ex. Navigation
Native libraries wrap underling navigation.
JS libraries compensate and only do it with JS.

With the JS version, in iOS renders all the screens, not just the one being displayed. Each screen has its own view controller.

This can confuse things like accessibility (android), but has other impacts.
* Native Feel
* Performance

Native libraries have cons too.
* Requires native knowledge
* Might require different tooling
* There are difference in the native platforms that you will need to account for.

### Native
Need to test on actual application, apps behave differently from simulator.
There are many use cases when you will need to write your own native code. Things that need to happen in the background.

So, write just the native code you need, but you probably will need it in some cases.

## How to make mobile apps feel good - Alex Kotliarskyi
What impacts what makes a good app?
Expectations - Mobile apps have higher expectations than web apps

Building for mobile is different for web. There are a lot of components that they are familiar with, but how you use them needs to be somewhat different than they would be on web.

TextInput for forms will feel clunky because they are in a different context with different controls. You don't have a physical keyboard, you have to tap on the input.

Direct Manipulation - cursor has visual feedback and can precisely select something. On mobile you are interacting with the thing you want to change, need instant feedback is require because it is expected.

Physics (checkout wix/react-native-interactable) things should feel smooth

Touch make the tap target much larger than the actual icon (slopHit)

Ability to Cancel an interaction. Push button but slide away allows the user to cancel an action.

Animation and Spatial Awareness - I don't see the screen behind, but i know its still there, even though the UI is 2D. On the desktop this works, but on mobile we don't have space. So we appear to be larger than the actual space. We can see things hanging off the edge. Animation is used to aid spacial awareness.

Loading - The app is loading and using the full screen to show a spinner. meh. Pass info from list to show and render as much as you can while you wait for rest of data to appear. The list will hold some of the information so show it and the user will feel like its fast while it waits for the rest of the data.

RN lets you build something really fast, but don't get fooled. That doesn't get it done. It's simple, but you still need to spend the time on the 'butter'.

These are all simple, but important. When its right, the user wont notice, but if any are wrong, it will stand out immediately.

"Stop Drawing Dead Fish" :: Bret V.

## React Native on the Apple TV Platform - Doug Lowder
RN for tvOS is about 90% inline with iOS .
There is no touch screen!! So the tv uses the 'focus engine' to figure out where the selection should go next.

ReactTvView detects focus changes that fires an event to touchable. This allows touchable (in all its forms) to work as normal.

Also handle trackpad events with a custom handler.

UI
* Larger screen - larger Front
* TabBarIOS for top level navigation
* List views need `removeClippedSubviews=false`

`victory-native` for data visualization.

## From Idea to App Store - Chris Ball
chain react photobomb

1. Optimize image sizes - use smaller images when appropriate.
1. Don't forget to add permissions.
1. Persistence should be minimal
1. Loading Screen
  `rn-toolbox` - generate icons.

  `rn-generator-toolbox` - generate splash screens

  `react-native-smart-splash-screen`

  __Avoid 'flash of white'__
  * iOS - rootView.BackgroundColor
  * Android needs something too...

1. Optimize screen size/orientation
1. Handle Offline

### Notes:

> 1. __CodePush__ can allow for hot fixes where deploying to the app store can't.
>
> 1. Fastlane precheck can help 'lint' your submittion
>
> 1. Create a new build, not a new version when you resubmit after a rejection.
