# Adding Contact Fields
* [Introduction](#introduction)
* [Modifying Contact Information Component](#modifying-contact-information-component)
* [Overriding Controller](#overriding-controller)
* [Overriding Route](#overriding-route)

## [Introduction](#introduction)
Once you include `website` form field in the `registration` page, you can add the form field to `contact information` section. 

Larakits `contact information` section of the `/settings` page only accepts `Name`, `Email`, `Phone`, and `Country` fields. You may want to include extra fields for your application. Follow the bellow instructions to customize default Larakits `contact information` section.

## [Modifying Contact Information Component](#modifying-contact-information-component)
You can add `website`  form field in the `resources/js/larakits-components/settings/navbar-items/personal/contact-information.jsx` file under the `Email` field:

```
<Input label="Website" name="website" value={this.state.website} onChange={website => this.setState({website})} errors={errors} />
```

> Don’t forget to run `npm run dev` in your terminal to compile the `Contact Information` component.  

## [Overriding Controller](#overriding-controller)
To override  `contactInformation` method of `PersonalController`, Create `PersonalController.php` file in the  `app/Http/Controller/Larakits/Settings` directory and Paste the code snippet as given below:

```
<?php

namespace App\Http\Controllers\Larakits\Settings;

use Larakits\Rules\FullName;
use Illuminate\Http\Request;
use Illuminate\Http\JsonResponse;
use Larakits\Events\Profile\ContactInformationUpdated;
use Larakits\Http\Controllers\Settings\PersonalController as Base;

class PersonalController extends Base
{
    /**
     * Update currently logged in user contact information.
     *
     * @param Request $request
     * @return \Illuminate\Http\JsonResponse
     */
    public function contactInformation(Request $request) : JsonResponse
    {
        $this->validate($request, [
            'name' => ['required', 'string', 'max:255', new FullName],
            'country' => 'required',
            'email' => 'required|email|unique:users,email,' . auth()->user()->id,
            'website' => 'required' // for website form field

        ]);

        $request->user()->update([
            'name' => request('name'),
            'email' => request('email'),
            'phone' => request('phone'),
            'country' => request('country'),
            'website' => request('website'), // for website form field
        ]);

        event(new ContactInformationUpdated($request->user()));

        return response()->json([
            'message' => 'Contact information updated'
        ]);
    }
}
```

> You may include validation rules for the `website` form filed in the `validate` method and `'website' => request('website')` in the `update` method.  

## [Overriding Route](#overriding-route)
Finally you have to override `/contact-information` route. By default the `/contact-information` route uses `Larakits\Http\Controller\Settings\PersonalController`. So you have to place another `/contact-information` route in `routes/web.php` file that will use your overridden  `PersonalController`.

```
Route::post('/contact-information', 'Larakits\Settings\PersonalController@contactInformation');
```