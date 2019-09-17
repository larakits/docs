# notification
* [Introduction](#introduction)
* [Creating Notification](#creating-notification)

## [Introduction](#introduction)
Notification allows you to notify your user about what is happening in your application. So, if you’re building a forum application, every time anyone mentions a user on a thread you will notify him that he is mentioned by someone in a thread. And every time a red indicator will pop up on the bell icon.

## [Creating Notification](#creating-notification)
To create notification you may use `Larakits\Notifications\Notification` class:

```
$user->notify(new \Larakits\Notifications\Notification(
    "someone mentioned you",
    "<strong>@$user->name</strong> How are you?",
    "fas fa-at", // fontawesome icon
    "/link/to/thread"
));
```