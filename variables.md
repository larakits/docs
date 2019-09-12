# variables

* [Introduction](#introduction)
* [Larakits Global Object](#larakits-global-object)
* [Adding Custom Properties](#adding-custom-properties)
	* [To Larakits Global Object](#to-larakits-global-object)
	* [To Larakits User Object](#to-larakits-user-object)
	* [To Larakits Config Object](#to-larakits-config-object)

## [Introduction](#introduction)
When you are building JavaScript based application, you may want to access currently logged-in user’s information, config information, and many more on the clients-side of your application. To make this possible Larakits includes a global object `Larakits.state`. By default Larakits includes `users` and `config` properties in the `Larakits.state` global object. You may add your own custom properties on `Larakits.state` if it’s required for your application.

## [Larakits Global Object](#larakits-global-object)
You may wish to access your currently logged-in users information on your `ReactJS` or `VueJS` component. For that you only have to call `Larakits.state.user`.

### [In ReactJS Component](#):

```javascript
class Example extends Component {

	componentDidMount() {
		console.log(Larakits.state.user);
	}

	render() {
 		return <div>ExampleComponent</div>
	}
}
```

### [In VueJS Component](#):

```html
<template>
	<div>ExampleComponent</div>
</template>

<script>
export default {
	mounted() {
		console.log(Larakits.state.user);
	}
}
</script>
```

## [Adding Custom Properties](#adding-custom-properties)
You can include custom properties to `Larakits`, `Larakits.user`, or `Larakits.config` object. 

### [To Larakits Global Object](#to-larakits-global-object)
To register your custom properties to Larakits global object, you have to include your custom properties to `Larakits.state` object in `resources/views/vendor/larakits/layouts/app.blade.php`:

```html
<script>
var Larakits = {
	state: {
		user: {!! auth()->user() ? json_encode(auth()->user()->state()) : 'null' !!},
		config:{!! $larakits !!},
		custom: 'value',
		anotherCustom: 'another-custom-value'
	}
}
</script>
```

### [To Larakits User Object](#to-larakits-user-object)
The `Larakits.state.user` object includes all `non-hidden` attributes of `App\User` class. You can include `relationship` data or `computed values` to `Larakits.state.user`. 

Include your custom user properties in the array inside `mergeState` function of `App\User` class:

```
/**
 * Get all projects belongs to the user.
 *
 * @return \Illuminate\Database\Eloquent\Relations\HasMany
 */
public function projects() : HasMany
{
	return $this->hasMany(\App\Project::class)->latest();
}

/**
 * Define user state here.
 * You can access the defined state on client-side via Larakits.state.user
 *
 * @return array
 */
protected function mergeState() : array
{
	return [
		'projects' => $this->projects()->get()
	];
}
```

### [To Larakits Config Object](#to-larakits-config-object)
You may include your custom config’s properties in the array inside `mergeConfig` function of `App\Providers\LarakitsServiceProvider` class:

```
/**
 * Define config state here.
 * You can access the defined state on client-side via Larakits.state.config
 *
 * @return array
 */
protected function mergeConfig() : array
{
	return [
		'app_name'=>config('app.name')
	];
}
```