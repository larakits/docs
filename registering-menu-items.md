# Registering Menu Items
* [Introduction](#introduction)
* [Navbar](#navbar)
	* [Left Side](#navbar-left-side)
	* [Right Side](#navbar-right-side)
	* [Right Side Dropdown](#navbar-right-side-dropdown)
* [Settings Page](#settings-page)

## [Introduction](#introduction)
You may want to include page links for your application. In Larakits, there are two place for it: `Navbar` and `Settings Page`.

## [Navbar](#navbar)
In the navbar you may include page links in three places `Left Side`, `Right Side`, and `Right Side Dropdown`.

### [Left Side](#navbar-left-side)
To show page links on the navber’s left side, you have to include your page links on the `resources/views/vendor/larakits/partials/nav/left.blade.php`

### [Right Side](#navbar-right-side)
To show page links on the navber’s right side, you have to include your page links on the `resources/views/vendor/larakits/partials/nav/right.blade.php`. All page links on the right side will only show for the `Guest` users.

### [Right Side Dropdown](#navbar-right-side-dropdown)
To show page links on the navber’s right side dropdown, you have to include your page links on the `resources/views/vendor/larakits/partials/nav/right-dropdown.blade.php`. The dropdown page links will only show when user clicks on their `name` from the top-right navbar.

## [Settings Page](#settings-page)
When you navigate `/settings` page, you will see the page with two columns. On the left column you will see few links under the title `Personal`, and `Billing`. 

At first you need to register your links in the `constructor` method of the `resources/js/larakits-components/settings/navbar.jsx` file:

```
/**
 * Create a new navbar component.
 * 
 * @param {object} props 
 */
constructor(props) {
  super(props);

  // navbar menu items state
  this.state = {
    navbar: [
      ....
      {
        title: 'LINK GROUP NAME',
        items: [
          'Link 1', // uri will be /settings/link-1
          'Link 2', // uri will be /settings/link-2
          'Link 3', // uri will be /settings/link-3
        ]
      },
		....
    ]
  }
}
```

After that you have to register `ReactJS Component` for the registered links. You may register your component in the `renderNavbarItem` method of `resources/js/larakits-components/settings/navbar-item-preview.jsx` file:

```
/**
 * Rendering navbar item component.
 */
renderNavbarItem() {
  switch (this.props.activeNavItem) {
    ....
    case 'link-1':
      return <ComponentOne />
    case 'link-2':
      return <ComponentTwo />
    case 'link-3':
      return <ComponentThree />
    default:
      this.redirectToSettings();
  }
}
```

> If you registered your page link as `My Page Link`, you have to register your component for `my-page-link`. You can access your page by navigating `/settings/my-page-link`.