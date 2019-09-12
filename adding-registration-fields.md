# Adding Registration Fields
* [Introduction](#introduction)
* [Modifying View](#modifying-view)
* [Overriding Register Controller](#overriding-register-controller)
* [Overriding Register Route](#overriding-register-route)

## [Introduction](#introduction)
Larakits registration page only accepts `Name`, `Email`, `Password`, and `Terms & Conditions` fields. You can include extra fields too. For that, follow the below instructions.

## [Modifying View](#modifying-view)
You can add `website` (namely) form field in the `resources/views/vendor/larakits/auth/register.blade.php` file under the `Email` field:

```
<!-- website -->
<div class="form-group">
	<input class="form-control {{ (bool) $errors->has('website') ? 'is-invalid' : '' }}" name="website" placeholder="{{ __('Website') }}" type="text" value="{{ old('website') }}">
	@if((bool)$errors->has('website'))
	<div class="invalid-feedback">{{ $errors->first('website') }}</div>
	@endif
</div>
<!-- end of website -->
```

## [Overriding Register Controller](#overriding-register-controller)
To override RegisterController, create `RegisterController.php` file in the  `app/Http/Controller/Larakits/Auth` directory and Paste the code snippet  as given below in the `RegiserController.php` file:

```
<?php

namespace App\Http\Controllers\Larakits\Auth;

use Carbon\Carbon;
use Larakits\Larakits;
use Larakits\Rules\FullName;
use Illuminate\Support\Facades\Hash;
use Larakits\Events\Auth\UserRegistered;
use Illuminate\Support\Facades\Validator;
use Larakits\Http\Controllers\Auth\RegisterControllerasBase;

class RegisterController extends Base
{
	/**
	 * Get a validator for an incoming registration request.
	 *
	 * @param array $data
	 * @return \Illuminate\Contracts\Validation\Validator
	 */
	 protected function validator(array $data)
	 {
		$messages = [
			'agreed.required' => 'You have to accept our Terms & Conditions.'
		];

		return Validator::make($data, [
			'name' => ['required', 'string', 'max:255', new FullName],
			'email' => ['required', 'string', 'email', 'max:255', 'unique:users'],
			'password' =>['required', 'string', 'min:6'],
			'agreed' => ['required'],
			'website' => ['required'] // for website form field
		], $messages);
	 }

	/**
	 * Create a new user instance after a valid registration.
	 *
	 * @param array $data
	 * @return Object
	 */
	 protected function create(array $data) : Object
	 {
		$user = Larakits::user()->create([
			'name' => $data['name'],
			'email' => $data['email'],
			'password' => Hash::make($data['password']),
			'trial_ends_at' => Larakits::trialDays() > 0 ?Carbon::now()->addDays(Larakits::trialDays()) : null,
			'country' => geoip()->getLocation(request()->ip)->iso_code,
			'website' => $data['website'] // for website form field
		]);

		event(new UserRegistered($user));

		return $user;
	}
}
```

> You may include validation rules for the `website` form field in the `validator` method and `'website' => $data['website']` in the `create` method of `RegisterController` class.   

## [Overriding Register Route](#overriding-register-route)
Finally you have to override `/register` route. By default, the `/register` route uses `Larakits\Http\Controller\Auth\RegisterController` as controller class. So you have to place another `/register` route in `routes/web.php` file of your application that will use your overridden `RegisterController`:
 
```
Route::post('/register','Larakits\Auth\RegisterController@register');
```

