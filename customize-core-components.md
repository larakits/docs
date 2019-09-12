# Customize Core Components
* [Introduction](#introduction)
* [Overriding Method](#overriding-method)

## [Introduction](#introduction)
`resources/js/larakits-components` directory will be created when you install Larakits. The `larakits-components` directory contains empty classes that extends  `Larakits Core Components`. You are free to customize any of Larakits core components from here.

## [Overriding Method](#overriding-method)
Let’s assume you want to override `update` method of `larakits/components/settings/navbar-items/personal/profile-photo` component. You need to place the `update` method in the `resources/js/larakits-components/settings/navbar-items/personal/profile-photo.jsx` file:

```
import Base from 'larakits/components/settings/navbar-items/personal/profile-photo'

export default class ProfilePhoto extends Base {
  
  /**
   * Upload profile photo.
   * 
   * @param {object} event 
   */
  upload(event) {
    // Your Custom Implementation...
  }
}
```