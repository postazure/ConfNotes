# Chain React Conference Notes
React Native Conference

Slack `community.infinite.red`

The Jump?

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

[Slides](http://www.benmvp.com/slides/2017/chainreact/react-native-esnext.html#/)


## - Nader Dabit
