# lighthouse
* [Introduction](#introduction)
* [Configuration](#configuration)
* [Middleware](#middleware)
* [Features](#features)
	* [Users](#users)
	* [Metrics](#metrics)
	* [Announcement](#announcement)
* [Customization](#customization)

## [Introduction](#introduction)
Lighthouse offers the admin features to your application. Only registered developers can access the Lighthouse. It will give you the opportunity to impersonate user, monitor revenue and create [announcement](/docs/{version}/announcement).

## [Configuration](#configuration)
In the `$developers` property of `App\Providers\LarakitsServiceProvider` class, include developers email address to give them access to the Lighthouse.

## [Middleware](#middleware)
If you want to define routes that is only accessible for the registered developers, you may use `dev	` middleware:

```
Route::middleware('dev')->get('/newsletters', function() {
  //
});
```

## [Features](#features)
Lighthouse admin features are listed below:

### [Users](#users)
Under the users section, you can search any user by name or email address. You can view the total revenue generated from the user, current subscription plan for [recurring billing](/docs/{version}/billing#recurring-billing) or purchased plan for [one-time charge](/docs/{version}/billing#one-time-charge), and also impersonate that user by clicking little spy icon.

### [Metrics](#metrics)
Under the metrics section, you can view your application performance such as monthly recurring revenue, yearly recurring revenue, total volume, and currently trialing users. You can also see which plan is more popular among all the plans of your application.

In the metrics section, you may see a nice graph on daily volume & newly registered user of the last 15 days and monthly recurring revenue & yearly recurring revenue of the last 30 days. 

Larakits offers a `larakits:kpi` artisan command for capturing performance metrics. The command is already scheduled in your application’s `app/console/Kernel.php` file when you created new project via [Larakits Installer](/docs/{version}/installation#installer). 

> Don’t forget to run a cron job for `schedule:run` artisan command. You can consultant [Laravel Task Scheduling Docs](https://laravel.com/docs/6.0/scheduling).  

### [Announcement](#announcement)
The announcement will help you to notify your user about any changes you made on your application. You can also announce about your new blog post and almost everything you want. You may consultant [announcement](/docs/{version}/announcement) docs for more details.

## [Customization](#customization)
The Lighthouse frontend is built using ReactJS. If you want to include any additional section in the Lighthouse page, you have to extend Larakits ReactJS component. 

When you create new project via Larakits Installer, a `Lighthouse` component will be included in the `/resources/js/larakits-components/lighthouse/index.jsx` that will extend the base `Lighthouse` component located  in `larakits/resources/js/lighthouse`. You are free to add/modify any feature directly from extended Lighthouse component.

> If you still don’t understand property, you can check `resources/js/larakits-components/settings/index.jsx` component. It’s the extended version of `larakits/resources/js/settings/index.jsx` component.  