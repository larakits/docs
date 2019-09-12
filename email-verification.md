# email verification
By default email verification service is disabled on Larakits. To enable email verification service you must set `TRUE` for the `$emailVerification` property of `App\Providers\LarakitsServiceProvider` class.

> As Larakits uses Laravel’s email verification service, your `App\User` class must implement the `Illuminate\Contracts\Auth\MustVerifyEmail` contract.  