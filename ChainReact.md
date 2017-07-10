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
