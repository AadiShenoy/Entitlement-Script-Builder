
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

## With Vue

### With default value

```js
<template>
  <header-component>
  </header-component>
</template>

<script>
export default {
  name: "App"
};
</script>

<style>
#app {
}
</style>
```
### With Props

```js
<template>
  <header-component
    :navOptions="navOptions"
  >
  <button slot="notification">Notification</button>
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
          slotName: "notification",
        },
      },
    };
  }
};
</script>

<style>
#app {
}
</style>
```


## Compatibility

| [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png" alt="IE / Edge" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>IE / Edge | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png" alt="Firefox" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>Firefox | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png" alt="Chrome" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>Chrome | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png" alt="Safari" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>Safari | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/electron/electron_48x48.png" alt="Electron" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)<br>Electron |
| --- | --- | --- | --- | --- |

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


### Menu Links Props

<table class="table table-bordered table-striped">
    <thead>
    <tr>
        <th style="width: 100px;">name</th>
        <th style="width: 50px;">type</th>
      <th style="width: 100px;">required</th>
       <th style="width: 50px;">possible values</th>
       <th style="width: 50px;">description</th>
      
    </tr>
    </thead>
    <tbody>
        <tr>
          <td>label</td>
          <td>String</td>
            <td>yes</td>
           <td></td>
          <td>Name of the menu link</td>
         
        </tr>
        <tr>
          <td>type</td>
          <td>String</td>
          <td>yes</td>
          <td>link | button</td>
          <td>If given 'link', user must provide an url. If given 'button', user must provide an custom event name ('eventName')  </td>
        </tr>
        <tr>
            <td>url</td>
            <td>String</td>
            <td>If type='link', yes else no</td>
            <td></td>
            <td>url</td>
        </tr>
        <tr>
            <td>eventName</td>
            <td>String</td>
            <td>If type='button', yes else no</td>
            <td></td>
            <td>custom event name triggered on click of a button</td>
      </tr>
    </tbody>
</table>

### Props

| prop                                     | type             | required | default           | possible values                                      | description                                                                                                                                                                                                                                                                           |
| ---------------------------------------- | ---------------- | -------- | ----------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mode                                | String           | no      | dark  |         dark, light                                           | This will apply dark or light theme to header component                                                                                                                                                                                                                                    |
| logo                         | Url         | no       | https://example.png            |                                                      | Brand / Company logo                                                                                                                                                                     |
| logoAltText                         | String           | no       | Company Logo               |                                                      | Adds "alt" text to logo                                                                                                                                                                                             |
| headerText                           | String  | no       | YML               |                                                      | Brand / company name                                                                                 |
|  openMenuIcon.slotName                    | String            | no       |                   |                                                      | A Slot which can render a HTMLElement or a component                                                                                                                                                                                                                                   |
| closeMenuIcon.slotName                        | String           | no       |      |                                                      | A Slot which can render a HTMLElement or a component                                                                                                                                                                                                                                                |
| menuLinks                  | Array<Object>            | no       |   |                                                      | An object that allows user to pass menu links                                                                                                                                                                                                                                                              |
| topRightSlot.slotName                 | String            | no       |     |                                                      | A Slot which can render a HTMLElement or a component                                                                                                                                                                                                                                                              |



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
