# announcement
* [Introduction](#introduction)
* [Event](#event)

## [Introduction](#introduction)
The announcement will help you to notify your user about any changes you made on your application. You can announce about your new blog post and almost everything you want.

Announcement has three parts: `title`, `body`, and `link`. The title is the announcement title that will appear when user’s click on bell icon on the top-right of navbar. The body is the content you want to show under the title. It will be short description and you are free to use `markdown` syntax. The link will be the more details of your announcement.

> Announcement is the feature of [**Lighthouse**](/docs/{version}/lighthouse).  So, you have to include your email address on `$developer` property in the `App\Providers\LarakitsServiceProvider` class.

If you registered yourself as a Larakits developer, you will see `Lighthouse` dropdown menu item when you click on your name from top-right of the navbar. In the lighthouse page you will see the `announcement` link under `SETTINGS`. 

When you publish new announcement, every users on your system will see a red indicator on the bell icon situated on the navbar.  When they will click the bell icon they will see a list of announcements. It may also have [Notification](/docs/{version}/notification). When they click on any announcement, they will be redirected to the detailed link page that you will include when creating announcement.

## [Event](#event)
Larakits will dispatch `Larakits\Events\Announcement\Published` event, every time you published a new announcement. It’ll allow you to notify your user on stack, email, etc.