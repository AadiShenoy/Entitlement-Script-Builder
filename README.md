
# Tailwind web components starter kit

This is a starter kit to develop web components using Tailwind CSS. 

Tailwind and web components do not play well together. 

We managed to find a way to make them work without hacks or weird tech: just common technologies combined in a elegant way.

No dependencies, based on [lit-element](https://lit.dev/docs/).

## How will you create a tailwind component?
Here is a sample code:

```typescript
import {html} from 'lit';
import {customElement, property} from 'lit/decorators.js';
import {TailwindElement} from '../shared/tailwind.element';

import style from './test.component.scss?inline'; // #1

@customElement('test-component')
export class TestComponent extends TailwindElement(style) { // #2

  @property()
  name?: string = 'World';

  render() {
    return html`
      <p>
        Hello,
        <b>${this.name}</b>
        !
      </p>
      <button class="bg-blue-200 text-yellow-200 p-2 rounded-full text-2xl">Hello world!</button>
    `;
  }
}
```
It is based on the [lit element](https://lit.dev/docs/) technology: if you wrote a lit component before, you'll find it familiar.  

There are only two differences to a standard _LitElement_:
1) You must import your styles from a separate file. And this is good for two reasons:
   - it separates the CSS from the logic
   - you can decide to use CSS or SCSS
   - note the `?inline` at the end of the file path: if you don't add it, then vite will add the style to the head of the html. If you add it, the style is scoped into the component only  
2) the class extends a _TailwindElement_ rather than a LitElement

A _TailwindElement_ extends a _LitElmement_ (see below) and adds the logic to integrate tailwind and your styles.

## Get started

To run the project:
1) `pnpm install` (only the first time)
2) `pnpm start` to run the server
3) to develop the library, run `pnpm build` and copy the static assets where you need them.

You may clone this repo and start developing your components by looking at the _test.component_ as reference.

As an alternative, and if you like to have control over every piece, do the following:

1) copy the files in the shared folder: 
   - _tailwind.element.ts_ extends LitElement by adding the tailwind support
   - _tailwind.global.css_ includes tha Tailwind base classes into each component
   - _globals.d.ts_ is used to avoid TypeScript errors whe nimporting CSS/Scss files in typescript files (thanks [@emaant96](https://github.com/emaant96))
2) copy the _package.json_ or the devDependencies inside into your own _package.json_  (**there are no dependencies**)
3) copy _postcss.config.js_, _tailwind.config.js_ and _tsconfig.js_ 

That's all.




## Show me the pieces
If you want to understand how it works, it's simple:

- the **package.json** integrates these technolgies:
```json
"autoprefixer": "^10.4.12",
"postcss": "^8.4.18",
"lit": "^2.4.0",
"tailwindcss": "^3.2.0",
"typescript": "^4.8.4",
"vite": "^3.1.8",
"sass": "^1.55.0"
```

- **vite** does almost all the work automatically
- to integrate tailwind, the most important file is in _src/shared/tailwind.element.ts_

```typescript
import {LitElement, unsafeCSS} from "lit";

import style from "./tailwind.global.css";

const tailwindElement = unsafeCSS(style);

export const TailwindElement = (style) =>
    class extends LitElement {

        static styles = [tailwindElement, unsafeCSS(style)];
    
    };

```

It extends a _LitElement_ class at runtime and adds the component tailwind classes.

The _style_ variable comes from your component, where it is imported from an external CSS (or SCSS) file.

Then it is combined with the default tailwind classes.

If you add more components, the common parts are reused.

## Who uses it?

We developed this starter kit to implement a web session player for our open source SaaS [browserbot](https://browserbot.io/).

If you want to contribute or share soem thoughts, just get in touch with us.

Enjoy.


# header-component

## Install
 
```bash
yarn add header-component
```

```bash
npm i header-component
```

## Usage

### With React

```js
import React from 'react'
Install the package - npm i @lit-labs/react

Usage
import * as React from 'react';
import {createComponent} from '@lit-labs/react';
import {MyElement} from './my-element.js';

export const MyElementComponent = createComponent({
tagName: 'my-element',
elementClass: MyElement,
react: React,
events: {
myEvent: 'myCustumEventName',
},
});

// where myEvent is the event name given to the custom event defined in the lit component.
// In the above example 'myCustumEventName' is the custom event.

// After defining the React component, you can use it just as you would use any other React component.
// <MyElementComponent
// myEvent: {myEventHandler}
// />
// where my myEventHandler is a function that needs to fired when myEvent is triggered.
```

### With Vue

```js
<template>
  <header-component
    :navOptions="navOptions"
    @map-click="handleMapClick"
    @setting-click="handleSetting"
  >
    <svg
      slot="slot-div"
      class="h-6 w-6"
      xmlns="http://www.w3.org/2000/svg"
      fill="white"
      viewBox="0 0 24 24"
      stroke-width="1.5"
      stroke="currentColor"
      aria-hidden="true"
      @click="handleNotification"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        d="M14.857 17.082a23.848 23.848 0 005.454-1.31A8.967 8.967 0 0118 9.75v-.7V9A6 6 0 006 9v.75a8.967 8.967 0 01-2.312 6.022c1.733.64 3.56 1.085 5.455 1.31m5.714 0a24.255 24.255 0 01-5.714 0m5.714 0a3 3 0 11-5.714 0"
      />
    </svg>
  </header-component>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      navOptions: {
        mode: "dark",
        logo: "https://ymedialabs.atlassian.net/s/1jmxwi/b/8/d35727372e299c952e88a10ef82bbaf6/_/jira-logo-scaled.png",
        logoAltText: "Logo",
        headerText: "YML",
        menuLinks: [
          {
            label: "Home",
            type: "link",
            url: "home",
          },
          {
            label: "Company",
            type: "link",
            url: "company",
          },
          {
            label: "Team",
            type: "link",
            url: "team",
          },
          {
            label: "Map",
            type: "button",
            eventName: "map-click",
          },
        ],
        topRightSlot: {
          slotName: "slot-div",
        },
      },
    };
  },
  methods: {
    handleMapClick() {
      console.log("Map");
    },
    handleSetting() {
      console.log("Settings");
    },
    handleNotification() {
      alert("New Notification");
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
```

## Compatibility

| [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png" alt="IE / Edge" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>IE / Edge | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png" alt="Firefox" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>Firefox | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png" alt="Chrome" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>Chrome | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png" alt="Safari" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>Safari | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/electron/electron_48x48.png" alt="Electron" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>Electron |
| --- | --- | --- | --- | --- |
| IE11, Edge | last 2 versions | last 2 versions | last 2 versions | last 2 versions |

### Props

| prop                                     | type             | required | default           | possible values                                      | description                                                                                                                                                                                                                                                                           |
| ---------------------------------------- | ---------------- | -------- | ----------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mode                                | String           | no      | dark  |         dark, light                                           | This will apply dark or light theme to header component                                                                                                                                                                                                                                    |
| logo                         | Url         | no       | https://ymedialabs.atlassian.net/s/1jmxwi/b/8/d35727372e299c952e88a10ef82bbaf6/_/jira-logo-scaled.png"             |                                                      | Brand/Company logo                                                                                                                                                                     |
| logoAltText                         | String           | no       | Company Logo               |                                                      | Add "alt" text to logo                                                                                                                                                                                             |
| brandImagePath                           | String or Object | no       | '/'               |                                                      | Link path of menu-option. If you have `isUsingVueRouter === true`, then this needs to be an `Object` with a `name` property or just a `String` of your path. Otherwise, just provide a `String`. link                                                                                 |
| brandImage                               | Image            | no       |                   |                                                      | `import` your image here to use your brand image                                                                                                                                                                                                                                      |
| brandImageAltText                        | String           | no       | 'brand-image'     |                                                      | The `alt` tag text for your brand image                                                                                                                                                                                                                                               |
| collapseButtonImageOpen                  | Image            | no       | A hamburger icon  |                                                      | `import` your image here                                                                                                                                                                                                                                                              |
| collapseButtonImageClose                 | Image            | no       | A times icon      |                                                      | `import` your image here                                                                                                                                                                                                                                                              |
| collapseButtonOpenColor                  | String           | no       | `#373737`         |                                                      | CSS hex - `#FFF`. Only applicable if you don't supply a `collapseButtonImageOpen`.                                                                                                                                                                                                    |
| collapseButtonCloseColor                 | String           | no       | `#373737`         |                                                      | CSS hex - `#FFF`. Only applicable if you don't supply a `collapseButtonImageClose`.                                                                                                                                                                                                   |
| showBrandImageInMobilePopup              | Boolean          | no       | false             |                                                      | If you want to show your brand logo in the mobile popup                                                                                                                                                                                                                               |
| ariaLabelMainNav                         | String           | no       | 'Main Navigation' |                                                      | The `aria-label` value for the main navbar element                                                                                                                                                                                                                                    |
| tooltipAnimationType                     | String           | no       | 'shift-away'      | 'shift-away', 'shift-toward', 'scale', 'perspective' | See [tippy.js docs](https://atomiks.github.io/tippyjs/all-options/)                                                                                                                                                                                                                   |
| tooltipPlacement                         | String           | no       | 'bottom'          | 'top', 'bottom', 'left', 'right' ... and more.       | See [tippy.js docs](https://atomiks.github.io/tippyjs/v6/all-props/#placement) for the complete list. Also, make sure to cross reference with popper.js's options. The tooltip dropdown will always drop in the direction you set here.                                               |
| menuOptionsLeft                          | Object           | no       | {}                |                                                      | Menu options that will be _pulled_ to the left towards the `brand-image`                                                                                                                                                                                                              |
| menuOptionsLeft.type                     | String           | yes      |                   | 'link', 'button', 'spacer', 'dropdown'               | What type of link will this menu-option be? `link` will be a link, `button` will be a button, `spacer` will be a spacer with a width of `30px` , `dropdown` will create a dropdown on desktop and a `ul/li` list on mobile. `dropdown` only works on menuOptions, not subMenuOptions. |
| menuOptionsLeft.text                     | String           | yes      |                   |                                                      | Text of menu-option                                                                                                                                                                                                                                                                   |
| menuOptionsLeft.path                     | String or Object | yes      |                   |                                                      | Link path of menu-option. If you have `isUsingVueRouter === true`, then this needs to be an `Object` with a `name` property or just a `String` of your path. Otherwise, just provide a `String`. Not applicable to `dropdown` menuOption types                                        |
| menuOptionsLeft.arrowColor               | String           | no       |                   |                                                      | CSS hex - `#FFF`. This styles the little chevron icon.                                                                                                                                                                                                                                |
| menuOptionsLeft.class                    | String           | no       |                   |                                                      | Only for `menuOptionsLeft.type === 'button'` - provide a class name so you can style your buttons                                                                                                                                                                                     |
| menuOptionsLeft.isLinkAction             | Boolean          | no       | false             |                                                      | When `true` , the `path` option of the `menuOption` will not fire - instead, you'll be able to register for the `@vnb-item-clicked` event which will spit you out the `text` value of your `menuOption` . That way, you can do an action you may want to trigger.                     |
| menuOptionsLeft.iconLeft                 | HTML String      | no       |                   |                                                      | Only for `menuOptionsLeft.type === 'link or menuOptionsLeft.type === 'dropdown'`. HTML string of the icon you want to use. See more info on the `Icon` section of the README.                                                                                                         |
| menuOptionsLeft.iconRight                | HTML String      | no       |                   |                                                      | Only for `menuOptionsLeft.type === 'link or menuOptionsLeft.type === 'dropdown'`. HTML string of the icon you want to use. See more info on the `Icon` section of the README.                                                                                                         |
| menuOptionsLeft.subMenuOptions           | Object           | no       |                   |                                                      | Sub-menu-options that will be shown                                                                                                                                                                                                                                                   |
| menuOptionsLeft.subMenuOptions.type      | String           | yes      |                   | 'link', 'hr'                                         | What type of link will this sub-menu-option be? `link` will be a link, `hr` will be a `hr` spacer                                                                                                                                                                                     |
| menuOptionsLeft.subMenuOptions.text      | String           | yes      |                   |                                                      | Text of sub-menu-option                                                                                                                                                                                                                                                               |
| menuOptionsLeft.subMenuOptions.subText   | String           | no       |                   |                                                      | Sub text of sub-menu-option                                                                                                                                                                                                                                                           |
| menuOptionsLeft.subMenuOptions.path      | String           | yes      |                   |                                                      | Link path of sub-menu-option                                                                                                                                                                                                                                                          |
| menuOptionsLeft.subMenuOptions.iconLeft  | HTML String      | no       |                   |                                                      | HTML string of the icon you want to use. See more info on the `Icon` section of the README.                                                                                                                                                                                           |
| menuOptionsLeft.subMenuOptions.iconRight | HTML String      | no       |                   |                                                      | HTML string of the icon you want to use. See more info on the `Icon` section of the README.                                                                                                                                                                                           |
| menuOptionsRight                         | Object           | no       | {}                |                                                      | Menu options that will be pushed to the right of the navbar. See above - all `menuOptionsLeft` apply                                                                                                                                                                                  |


### Menu Items Props

<table class="table table-bordered table-striped">
    <thead>
    <tr>
        <th style="width: 100px;">name</th>
        <th style="width: 50px;">type</th>
        <th style="width: 50px;">default</th>
        <th>description</th>
    </tr>
    </thead>
    <tbody>
        <tr>
          <td>MenuNumber</td>
          <td>Number</td>
          <td></td>
          <td>The MenuNumber denotes 'how many menu-items you want. Although Maximum is 5 and minimum is 1'</td>
        </tr>
        <tr>
          <td>Font</td>
          <td>Number</td>
          <td></td>
          <td>{optional}Specify the font-size. As the icons increase and decrease, Height will adjust automatically but Width will need to be adjusted maually</td>
        </tr>
        <tr>
            <td>IconColor</td>
            <td>String</td>
            <th>""</th>
            <td>Specify the color for the menu items/ navbar menu items e.g. "blue","#a595e9"</td>
        </tr>
        <tr>
            <td>activeColor</td>
            <td>String</td>
            <th>""</th>
            <td>Specify the background color for the active menu items/ navbar menu items e.g. "blue","#a595e9"</td>
        </tr>
        <tr>
            <td>activeIconColor</td>
            <td>String</td>
            <th>""</th>
            <td>Specify the color for the menu item/ navbar menu item e.g. "blue","#a595e9"</td>
        </tr>
        <tr>
            <td>degree</td>
            <td>String</td>
            <th>""</th>
            <td>{Optional} This is part of linear-gradient(degree, gradcolor1,gradcolor2), this will inturn overwrite activeColor and become the active background color. Specify the direction of background color for the active menu item/ navbar menu items e.g. "to left", "to right", "to bottom left" etc.</td>
        </tr>
        <tr>
            <td>gradcolor1</td>
            <td>String</td>
            <th>""</th>
            <td>{Optional} This is part of linear-gradient(degree, gradcolor1,gradcolor2), this will inturn overwrite activeColor and become the active background color. Specify the direction of background color for the active menu item/ navbar menu items e.g. "blue" etc.</td>
        </tr>
        <tr>
            <td>gradcolor2</td>
            <td>String</td>
            <th>""</th>
            <td>{Optional} This is part of linear-gradient(degree, gradcolor1,gradcolor2), this will inturn overwrite activeColor and become the active background color. Specify the direction of background color for the active menu item/ navbar menu items e.g. "green" etc.</td>
        </tr>
        <tr>
            <td>icon1, icon2... icon5</td>
            <td>String</td>
            <th>""</th>
            <td>Specify the name of the icon for each specific icon number(icon1, icon2) for the menu item/ navbar menu items. e.g. "bx bx-home", "fa fa-house" etc.</td>
        </tr>
        <tr>
            <td>url1, url2... url5</td>
            <td>String</td>
            <th>""</th>
            <td>Specify the name of the url for each specific url number(url1, url2) for the menu item/ navbar menu items. e.g. "/", "#contact" etc.</td>
        </tr>
    </tbody>
</table>


## Installation

```npm
  npm install react-navbar-menu
```

## Live Examples

https://ri4w0d.csb.app/
## More Info

https://yusuflateefblog.vercel.app/post/react-navbar-menu

## Code Installation

```npm
  npm install
```

## License

react-navbar-menu is released under the MIT license.
