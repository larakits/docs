# billing
* [Introduction](#introduction)
* [Configuration](#configuration)
	* [Webhook](#webhook)
	* [Mail](#mail)
* [Recurring Billing](#recurring-billing)
* [One-Time Charge](#one-time-charge)
* [Configuring Billing Plans](#configuring-billing-plans)
	* [Defining Monthly Plan](#defining-monthly-plan)
	* [Defining Yearly Plan](#defining-yearly-plan)
* [Coupon](#coupon)
* [Middleware](#middleware)
	* [Subscribed Middleware](#subscribed-middleware)
	* [Purchased Middleware](#purchased-middleware)
* [Customization Currency](#customization-currency)
* [Events](#events)

## [Introduction](#introduction)
Larakits supports both `Recurring Billing` and `One-Time Charge`. Currently, Larakits uses [FastSpring](https://fastspring.com/) as payment provider. If you do not have FastSpring account, create your account before moving to configuration section.

## [Configuration](#configuration)
Once you created your FastSpring account, you can create `API Credentials` under `Integrations > API Credentials` from your FastSpring dashboard. After that, open `.env` file of your application and populate the `FASTSPRING_USERNAME`, `FASTSPRING_PASSWORD`, `FASTSPRING_STORE_ID`, `FASTSPRING_SUB_DIRECTORY_STORE_ID`  environment variable values.

> You will get your `FASTSPRING_STORE_ID` and `FASTSPRING_SUB_DIRECTORY_STORE_ID` from **Storefronts** > **Popup Storefronts** > **PLACE ON YOUR WEBSITE**. When you click the **PLACE ON YOUR WEBSITE**, a modal will popup.
>
> You will see a line similar to `data-storefront="FASTSPRING_STORE_ID.onfastspring.com/popup-FASTSPING_SUB_DIRECTORY_STORE_ID"` where `FASTSPRING_STORE_ID` will be your store ID and `FASTSPRING_SUB_DIRECTORY_STORE_ID` your sub directory ID.  

### [Webhook](#webhook)
You must configure your webhook on FastSpring. The webhook will be `/webhook/fastspring` URI.

To configure your webhook, go to `Integrations > Webhooks > Add New Webhook` from your FastSpring dashboard. Here's the list of event types you need to configure:

* order.completed
* subscription.activated
* subscription.deactivated
* subscription.canceled
* subscription.updated
* subscription.charge.completed
* subscription.charge.failed

> Make sure you write a secret key in the form field of `HMAC SHA256 Secret` when you provide the webhook URL. Also populate the same secret key for `FASTSPRING_HMAC_SECRET` environment variable value in `.env` file of your application.  

### [Mail](#mail)
Larakits sends email to user when registration is complete, trial period is over, and new invoice is created. So you have to configure your mail setting from `.env`.

## [Recurring Billing](#recurring-billing)
By default, Larakits configured recurring billing when you created new project via Larakits installer. You will get the configuration in the `booted` method of  `App\Providers\LarakitsServiceProvider` class.

Larakits recurring billing is configured for `7 days` trial. You may change the trial days as your wish by calling `onTrialDays` method of `Larakits` instance:

```
Larakits::useFastSpring()->onTrialDays(15);
```

You may use `onTrial` method of `user` instance to know if the user is on trial period:

```
$user->onTrial();
```

## [One-Time Charge](#one-time-charge)
You may want to start your project with one-time charge or swap your recurring billing to one-time charge for a certain period of time `usecase: Lifetime Deals Campaign for 15 days`. Larakits offers a painless way to do this by calling `oneTimeCharge` method of `Larakits` instance:

```
Larakits::useFastSpring()->oneTimeCharge();
```

In general, Larakits will not redirect your user to any page after the purchase is complete. You can change this behaviour by calling `Larakits::redirectAfterPurchase` method in the `booted` method of `App\Providers\LarakitsServiceProvider` class:

```
Larakits::redirectAfterPurchase('/licenses');
```

## [Configuring Billing Plans](#configuring-billing-plans)
In the `booted` method of  `App\Providers\LarakitsServiceProvider`, you will see three plans are defined. One free and two paid plans. You will see both paid plans has a unique ID `larakits-test-1` and `larakits-test-2`. These are dummy IDs and matched with FastSpring [Subscription ID](https://dashboard.fastspring.com/2/product/all_subscriptions.xml). You have to change it with your own subscription ID that you will create on your own [FastSpring](https://fastspring.com) account.

> One thing keep in mind that for the `one-time charge` you have to use FastSpring [Product ID](https://dashboard.fastspring.com/2/product/all.xml) instead of [Subscription ID](https://dashboard.fastspring.com/2/product/all_subscriptions.xml)  

### [Defining Monthly Plan](#defining-monthly-plan)
The `Plan` method of `Larakits` instance accepts two arguments. The first argument is the name of your plan that will be displayed on pricing table on subscription or purchase page. The second argument is the ID of your FastSpring subscription or product ID:

```
Larakits::plan('Pro', 'monthly-pro')
            ->price(19)
            ->features([
                'Feature 1',
                'Feature 2',
                'Feature 3',
                'Feature 4'
            ]);
```

All the features name defined by you will be displayed on the subscription or purchase page under the plan name. Obviously you are free to include as many plans you want.

For the `one-time charge` you may pass sub-plan name for your plan. By calling `for` method of plan instance:

```
Larakits::plan('Personal', 'personal-license')
           ->price(99)
           ->for('Single Site')
           ->features([
               'Feature 1',
               'Feature 2',
               'Feature 3',
               'Feature 4'
           ]);
```

The sub-plan name will be shown as `$99/Single Site`.

### [Defining Yearly Plan](#defining-yearly-plan)
You may define your yearly plan by simply calling `yearly` method:

```
Larakits::plan('Pro', 'yearly-pro')
            ->price(199)
            ->yearly()
            ->features([
                'Feature 1',
                'Feature 2',
                'Feature 3',
                'Feature 4'
            ]);
```

## [Coupon](#coupon)
You may apply coupon on a plan by calling `coupon` method of plan instance:

```
Larakits::plan('Pro', 'monthly-pro')
            ->price(19)
            ->coupon('HOTSALE')
            ->features([
                'Feature 1',
                'Feature 2',
                'Feature 3',
                'Feature 4'
            ]);
```

> Before applying coupon, make sure you create coupon from `Coupons > Create Coupon` from your FastSpring Dashboard.  

## [Middleware](#middleware)
Larakits registeres `subscribed` and `purchased` middleware in the `app/Http/Kernel.php` file while creating new project.

> If you configured recurring billing for your application, use `subscribed` middleware. Otherwise `purchased` middleware.  

### [Subscribed Middleware](#subscribed-middleware)
The `subscribed` middleware is used for preventing access to a route if the user is not already subscribed. By default Larakits will redirect them to the `subscription` page:

```
Route::middleware('subscribed')->get('/projects', function() {
  //
});
```

In case of specifying the plan name to prevent access of a route from a user, you can follow the below format:

```
Route::middleware('subscribed:monthly-pro')->get('/projects', function() {
  //
});
```

### [Purchased Middleware](#purchased-middleware)
The `purchased` middleware is used for preventing access to a route if the user is not already purchased your product. By default Larakits will redirect them to the `purchase` page:

```
Route::middleware('purchased')->get('/courses', function() {
  //
});
```

In case of specifying the plan name to prevent access of a route from a user, you can follow the below format:

```
Route::middleware('purchased:personal-license')->get('/courses', function() {
  //
});
```

## [Customization Currency](#customization-currency)
You may change default currency by calling `useCurrency` method of `Larakits\Larakits` class:

```
Larakits::useCurrency("£");
```

Optionally, you may override FastSpring currency by **Storefronts** > **Popup Storefronts** > **More** > **Languages and Currencies** > **Override Store Currencies** from your FastSpring dashboard.

## [Events](#events)
Larakits dispatches many events when processing billings. All the events listed below are also registered in the `$listen` array of the `App\Providers\EventServiceProvider` class:

| Event                              | Description |
|------------------------------------|-------------|
| `Larakits\Events\Subscription\TrialPeriodExpired` | A user’s trial period was expired |
| `Larakits\Events\Subscription\UserSubscribed` | A user was subscribed|
| `Larakits\Events\Subscription\SubscriptionUpdated` | A user’s subscription was updated |
| `Larakits\Events\Subscription\SubscriptionCancelled` | A user’s subscription was cancelled|
| `Larakits\Events\Subscription\InvoiceCreated` | Invoice is created for a user|
|`Larakits\Events\Profile\ContactInformationUpdated` | A user’s contact information was updated|
| `Larakits\Events\Plan\Purchased` | A user purchased a product|