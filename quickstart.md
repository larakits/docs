# quickstart
* [Introduction](#introduction)
* [Installing Larakits](#installing-larakits)
* [Building Assets](#building-assets)
* [Lighthouse Access](#lighthouse-access)
* [Configuring Billing Plans](#configuring-billing-plans)
* [Building Your Application](#building-your-application)
* [Customize Core Components](#customize-core-components)
* [Overring Core Implementation](#overriding-core-implementation)

## [Introduction](#introduction)
Every SaaS based project requires authentication, API token, subscription billing, performance metrics and many common functionalities. To get started, Larakits will give you those functionality out of the box. Larakits also supports `one-time charge`. You are free to swap them any time if you like!

By following this guide you can build a SaaS based project from scratch.

## [Installing Larakits](#installing-larakits)
You need Larakits installer to start a new project. Please follow the [Larakits installation guide](/docs/{version}/installation).

## [Building Assets](#building-assets)
Larakits comes with sass & javascript (ReactJS) assets to give interactive UI. Those assets need to compile before sending to the browser. For compiling assets you must install NodeJS on your machine. Please follow the [NodeJS installation guide](/docs/{version}/installation#installing-dependencies).

Once you are done, hit the command given below in your terminal to compile your assets:

```
npm install && npm run dev
```

## [Lighthouse Access](#lighthouse-access)
Larakits also includes admin functionality to see what’s going on in your application called [Lighthouse](/docs/{version}/lighthouse). Only registered developers can access the Lighthouse.

To register yourself as a developer, you have to include your email address on the `$developers` property in your `App\Providers\LarakitsServiceProvider` class.

## [Configuring Billing Plans](#configuring-billing-plans)
Larakits supports both `recurring billing` and `one-time charge`. By default Larakits is configured for recurring billing. Let’s introduce you with recurring billing first.

In the `booted` method of  `App\Providers\LarakitsServiceProvider`, you will see three plans are defined. One free and two paid plans. You will see both paid plans has a unique ID `larakits-test-1` and `larakits-test-2`. These are dummy IDs and matched with FastSpring [Subscription ID](https://dashboard.fastspring.com/2/product/all_subscriptions.xml). You have to change it with your own subscription ID that you’ll create on your own [FastSpring](https://fastspring.com) account.

For `one-time charge` you have to use `Larakits::useFastSpring()->oneTimeCharge();` instead of  `Larakits::useFastSpring()->onTrialDays(7);`. That’s it. Your application is ready for one-time charge. One thing keep on mind that you have to use FastSpring [Product ID](https://dashboard.fastspring.com/2/product/all.xml) instead of [Subscription ID](https://dashboard.fastspring.com/2/product/all_subscriptions.xml).

> Don’t forget to configure `FastSpring API token`, `Webhook`, and `Mail`. Please follow [the billing guide](/docs/{version}/billing#configuration). 

## [Building Your Application](#building-your-application)
It’s time to build your own application. When you create your project via Larakits installer, it creates a `/home` route. Basically it’ll show a blade file that is located on `/resources/views/home.blade.php`. You are free to add any routes and views. 

> Follow the [subscribed middleware](/docs/{version}/billing#subscribed-middleware) guide to secure your route subscribers only.  

## [Customize Core Components](#customize-core-components)
Larakits uses ReactJS as JavaScript library to show interactive UI. Sometimes your business might need to modify ReactJS components that comes with Larakits. Follow the [customize core components](/docs/{version}/customize-core-components) guide.

## [Overriding Core Implementation](#overriding-core-implementation)
You may wish to extend or override core implementation that’s written on the Larakits. Follow the [adding contact fields](/docs/{version}/adding-contact-fields) guide.