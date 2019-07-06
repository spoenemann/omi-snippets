# Omi Snippets

[Try it Now!](https://marketplace.visualstudio.com/items?itemName=Wscats.omi-snippets)

Visual Studio Code syntax highlighting for single-file Omi.js components (enabled by omil).

Or [Download Vsix!](https://github.com/Wscats/omi-snippets/releases/download/0.41/omi-snippets-0.4.1.vsix) to install in Visual Studio Code

# Contributors

| [<img src="https://avatars1.githubusercontent.com/u/17243165?s=460&v=4" width="60px;"/><br /><sub>Eno Yao</sub>](https://github.com/Wscats)| [<img src="https://avatars2.githubusercontent.com/u/5805270?s=460&v=4" width="60px;"/><br /><sub>Aaron Xie</sub>](https://github.com/aaron-xie)| [<img src="https://avatars3.githubusercontent.com/u/12515367?s=460&v=4" width="60px;"/><br /><sub>DK Lan</sub>](https://github.com/dk-lan)| [<img src="https://avatars1.githubusercontent.com/u/30917929?s=460&v=4" width="60px;"/><br /><sub>Yong</sub>](https://github.com/flowerField)| [<img src="https://avatars3.githubusercontent.com/u/33544236?s=460&v=4" width="60px;"/><br /><sub>Li Ting</sub>](https://github.com/Liting1)| <img src="https://wscats.github.io/omi-snippets/images/xin.jpg" width="60px;"/><br /><sub>Xin</sub>| [<img src="https://wscats.github.io/omi-snippets/images/lemon.jpg" width="60px;"/><br /><sub>Lemon</sub>](https://github.com/lemonyyye)  |  [<img src="https://wscats.github.io/omi-snippets/images/jing.jpg" width="60px;"/><br /><sub>Jing</sub>](https://github.com/vickySC)  |  [<img src="https://wscats.github.io/omi-snippets/images/lin.jpg" width="60px;"/><br /><sub>Lin</sub>](https://github.com/shirley3790)  | [<img src="https://avatars2.githubusercontent.com/u/23230108?s=460&v=4" width="60px;"/><br /><sub>Tian Fly</sub>](https://github.com/tiantengfly)| 
| - | - | - | - | - | - | - | - | - | - |

# Quick Start

- Install [Omi Snippets](https://marketplace.visualstudio.com/items?itemName=Wscats.omi-snippets).
- Try it with [omil](https://github.com/Wscats/omil), a Omi.js components loader based on webpack.
- Via Marketplace Control: search for `Omi Snippets` and click install.
- Manual: clone this repo and install `omi-snippets.vsix` into your Visual Studio Code.
> In addition, if you want to build and install the extension from source, you need to install [vsce](https://code.visualstudio.com/api/working-with-extensions/publishing-extension), like this.

```bash
# Then, clone the repository and compile it.
git clone https://github.com/Wscats/omi-snippets
cd omi-snippets
yarn
vsce package
```
![wscats](https://wscats.github.io/omi-snippets/images/omi-snippets.gif)

This will setup a basic `webpack` + [omil](https://github.com/Wscats/omil) project for you, with `*.omi` or `*.eno` files and hot-reloading working out of the box!

For example, you can create `test.omi` in Visual Studio Code before install [Omi Snippets](https://marketplace.visualstudio.com/items?itemName=Wscats.omi-snippets)
```html
<!-- test.omi -->
<template name="component-name">
    <div>
        {this.data.name}
    </div>
</template>
<script>
import style from './style.css';
export default class {
    static css = style
    install(){
        this.data = {
            name: 'Eno Yao',
        }
    }
}
</script>
<style lang="scss">
    div{
        color:red;
    }
</style>
```

After you save the code in editor`(Ctrl+S)`, it will be converted into `test.js`
```jsx
// test.js
import { WeElement, define, h } from "omi";
import style from "./style.css";
const componentName = class extends WeElement {
  render() {
    return h("div", null, this.data.name);
  }
  static css =
    `
      div{
          color:red;
      }
    ` + style;
  install() {
    this.data = {
      name: "Eno Yao"
    };
  }
};
define("component-name", componentName);
```

## Usage In Omi

A `*.omi` file is a custom file format that uses HTML-like syntax to describe a Omi component. Each `*.omi` file consists of three types of top-level language blocks: `<template>, <script>, and <style>`, and optionally additional custom blocks:

```html
<template lang="html" name="component-name">
  <!-- replace render function -->
  <header onClick="${this.test}">${this.data.title}</header>
</template>
<script>
import style from './_oHeader.css'
export default class {
  static css = style + `p{color:red}` // it will combine scoped css，only support static css = xxx
  test(){
    console.log('Hello Eno!')
  }
  install() {
    this.data = {
      title: 'Omi'
    }
  }
}
</script>
<style>
/* scoped css */
header {
  height: 50px;
  background-color: #07c160;
  color: white;
  text-align: center;
  line-height: 50px;
  width: 100%;
}
</style>
```
> [Single-File Components Demo](https://github.com/Wscats/omil/blob/master/src/components/oHeader.omi)

It also supports [JSX](https://github.com/facebook/jsx), if you want to do that, you only write `<template>` without `lang="html"` attribute in your component like this:
```html
<template>
  <header onClick={this.test}>{this.data.title}</header>
</template>
```
> [JSX Demo](https://github.com/Wscats/omil/blob/master/src/components/oPanel.omi)

`omil` supports using non-default languages, such as CSS pre-processors and compile-to-HTML template languages, by specifying the lang attribute for a language block. For example, you install [node-sass](https://www.npmjs.com/package/node-sass) after you can use [Sass](https://sass-lang.com/) for the style of your component like this:
```html
<style lang="scss">
$height: 50px;
$color: #07c160;
header {
  height: $height;
  background-color: $color;
}
</style>
```

> [Sass Demo](https://github.com/Wscats/omil/blob/master/src/components/oGallery.omi)

## Support React

You can also use an ES6 class to define a class component by omil.
```html
<template name="ComponentName">
    <p>{this.state.title}</p>
</template>
<script>
    export default class {
        constructor(props) {
            super(props)
            this.state.title = "Eno Yao"
        }
    }
</script>
<style lang="scss">
    p {color: #58bc58;}
</style>
```

A higher-order component (HOC) is an advanced technique in React for reusing component logic. HOCs are not part of the React API. Here's a concrete example.
```html
<template name="ComponentName">
    <div>
        <p>{this.state.title}</p>
    </div>
</template>
<script>
    const HOC = (props) => {
        return (WraooedComponent) => {
            return class HOC extends WeElement {
                state = {
                    name: 'Eno Yao',
                    ...props
                }
                render() {
                    return (
                        <div>
                            Hello World
                            <WraooedComponent name={{ ...this.state }} />
                        </div>
                    )
                }
            }
        }
    }
    export default HOC({
        age: 18
    })(class {
        constructor(props) {
            super(props)
            this.state = {
                title: 'Lemon'
            }
        }
        componentDidMount() {
            console.log(this)
        }
        handleChange() {}
    })
</script>
<style lang="scss">
    /* CSS */
    p {
        color: #58bc58;
    }
</style>
```

# Cooperate With TypeScript

A static type system can help prevent many potential runtime errors, especially as applications grow. You can use `Single File Components(SFC)` cooperate with `Higher Order Components(HOC)` to get support with `TypeScript`
```html
<template name="Eno">
    <div><p>{this.state.name}</p></div>
</template>
<script>
    // TypeScript Support
    import EnoType from './EnoType.tsx'
    export default EnoType(class {
        constructor(props) {
            super(props);
            this.state = { name: 'abc', age: 18}
        }
    })
</script>
<style lang="scss">
    p { color: #58bc58 };
</style>
```
Now, you can create `EnoType.tsx` in editor which provides TypeScript inference inside SFCs and many other great features.
```ts
// EnoType.ts
import React from 'react';
interface EnoTypeProps { }
interface EnoTypeState { name: string }
export default (Component: React.ComponentType) => {
    return class EnoType extends React.Component<EnoTypeProps, EnoTypeState> {
        constructor(props: EnoTypeProps) {
            super(props)
            this.state = { name: 'Eno Yao' }
        }
        render() { return <Component /> }
    }
}
```

There are many cool features provided by `omil`:

- Allows using other webpack loaders for each part of a Omi component, for example Sass for `<style lang="scss">` and JSX/HTML for `<template lang="html">` and ES5+ for `<script type="text/babel">`;
- Allows custom blocks in a `.omi` or `.eno` file that can have custom loader chains applied to them [Here Online Demo](https://github.com/Wscats/omil/tree/master/src/components);
- Treat static assets referenced in `<style>` and `<template>` as module dependencies and handle them with webpack loaders (Such as [htm](https://www.npmjs.com/package/htm), [to-string-loader](https://www.npmjs.com/package/to-string-loader));
- Simulate scoped CSS for each component (Use Shadow DOM);
- State-preserving hot-reloading during development.

In a nutshell, the combination of webpack and `omil` gives you a modern, flexible and extremely powerful front-end workflow for authoring Omi.js applications.


# Code Snippets

|trigger|snippet|
|-|-|
|import|`import { * } from 'omi'`|
|export default|`export default {}`|
|modult.export"|`modult.export = {}`|
|render|`render(){}`|
|css|`css(){}`|
|template|`<template></template>`|
|templateLang|`<template lang></template>`|
|script|`<script></script>`|
|style|`<style></style>`|
|styleLang|`<style lang></style>`|
|scaffold|`<template><script><style>`|
|...|...|

**NOTE:** You still need to install corresponding packages for pre-processors (e.g. `JSX, SASS, TypeScript`) to get proper syntax highlighting for them.

# Enabling JSX Highlighting

The `<script>` block uses the syntax highlighting currently active for you normal `.js` files. To support JSX highlighting inside Omi files, just set [Babel javascript highlighting package](https://packagecontrol.io/packages/Babel), which supports JSX, as your default JS highlighting. 

> Note you may need to explicitly disable Sublime's default `JavaScript` package to make it work.




# Thanks

- [omi](https://github.com/Tencent/omi)
- [omil](https://github.com/Wscats/omil)
- [babel](https://babeljs.io/)
- [prettier](https://prettier.io/)
- [html-snippets](https://github.com/abusaidm/html-snippets)
- [vsc-css-snippets](https://github.com/joy-yu/vsc-css-snippets)
- [vscode-javascript](https://github.com/xabikos/vscode-javascript)
- [vscode-javascript-snippets](https://github.com/nathanchapman/vscode-javascript-snippets)


# License

[MIT](http://opensource.org/licenses/MIT)