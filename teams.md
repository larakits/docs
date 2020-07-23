# teams
* [Introduction](#introduction)
* [Refer To Team As](#refer-to-team-as)
* [Accessing The User’s Teams](#accessing-the-user-teams)
* [Accessing The User’s Current Team](#accessing-the-user-current-team)
* [Team Roles](#team-roles)
* [Helper Methods](#helper-methods)
* [Events](#events)

## [Introduction](#introduction)
Larakits supports team billing that allows your user’s to create and join teams. The team is a great way for sharing same resources among the team member’s.

To enable team, you have to use `Larakits\CanJoinTeam` trait into your `User` model.

```
<?php

namespace App;

use Larakits\Billable;
use Larakits\CanJoinTeam;
use Larakits\HasApiToken;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use Notifiable, Billable, HasApiToken, CanJoinTeam;
}
```

> You may also need to configure your team billing plans. Please consult [the configuration team billing plans]((null)) documentation.  

## [Refer To Team As](#refer-to-team-as)
By default, Larakits uses `/teams` URI and `teams` word to refer your team in the whole application. You may change it by simply calling `Larakits::referToTeamAs` method in the `registered` method of `App\Providers\LarakitsServiceProvider`.

```
Larakits::referToTeamAs('workspace');
```

Now, all of the `/teams` URI will be `/workspaces` and all of the `team` and `teams` words will be `workspace` and `workspaces` for your application.

## [Accessing The User’s Teams](#accessing-the-user-teams)
The `Larakits\CanJoinTeam` trait will give you many helpful method to access user’s teams.

```
foreach($user->teams as $team) {
	echo $team->name;
}
```

For every iteration, it will return the instance of `App\Team` model. If you want to use different `namespace` for your team model, make sure you are registering your team model in the `booted` method of `App\Providers\LarakitsServiceProvider`.

```
Larakits::useTeamModel(\App\Models\Team::class)
```

## [Accessing The User’s Current Team](#accessing-the-current-team)
Larakits allows your user to switch one team to another team from the user profile dropdown menu at the top right of the screen. Every time a user switches team, Larakits will make a record of the current team.

You can access the current team by call `currentTeam` property of the `App\User` model.

```
echo $user->currentTeam->name
```

The `currentTeam` property will help you to show current team related data in your application.

## [Team Roles](#team-roles)
By default Larakits only offer `owner` and `member` two roles. You may want to define your own roles with default roles.

### [Define Roles](#)
To define roles you have to call  `Larakits::useRoles` method in the `booted` method of `App\Providers\LarakitsServiceProvider`.

```
Larakits::useRoles([
	'author' => 'Author',
	'editor' => 'Editor'
]);
```

### [Get User’s Role](#)

```
$user->roleOn($team);
```

## [Helper Methods](#helper-methods)
The helpful methods you will get when you inlcude `Larakits\CanJoinTeam` trait in the `App\User` model:

### [Get All Teams Owned By The User](#)

```
foreach($user->ownedTeams as $team) {
	//
}
```

### [Determine Ownership Of The Given Team](#)

```
if($user->ownsTeam()) {
	//
}
```

### [Determine Ownership Of The Current Team](#)

```
if($user->ownsCurrentTeam()) {
	//
}
```

### [Determine The User’s On The Given Team](#)

```
if($user->onTeam($team)) {
	//
}
```

## [Events](#events)
Larakits dispatches many events when processing team related actions. All the events listed below are also registered in the `$listen` array of the `App\Providers\EventServiceProvider` class:

| Event                              | Description |
|------------------------------------|-------------|
| `Larakits\Events\Team\Created` | A team has been created |
| `Larakits\Events\Team\Deleted` | A team has been deleted |
| `Larakits\Events\Team\Switched ` | A user has been switched current team |
| `Larakits\Events\Team\Member\Addedd` | A user has joined on a team |
| `Larakits\Events\Team\Member\Removed` | A user has been removed from a team |
| `Larakits\Events\Team\Member\Left` | A user has left from a team |
| `Larakits\Events\Team\Member\Invited` | A user has been invited to join a team |
| `Larakits\Events\Team\Subscription\TrialPeriodExpired` | A team’s trial period was expired |
| `Larakits\Events\Team\Subscription\TeamSubscribed` | A team was subscribed|
| `Larakits\Events\Team\Subscription\SubscriptionUpdated` | A team’s subscription was updated |
| `Larakits\Events\Team\Subscription\SubscriptionCancelled` | A team’s subscription was cancelled|
| `Larakits\Events\Team\Subscription\InvoiceCreated` | Invoice is created for a team|
| `Larakits\Events\Team\Plan\Purchased` | A team purchased a product|