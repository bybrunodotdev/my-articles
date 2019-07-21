# Configuring absolute paths in React for Web without ejecting [en-US]

After you've done an article showing how to set up [absolute paths in React Native](https://dev.to/heybrunoandrade/configuring-paths-absolute-no-react-native-en-br-471o), now I've been showing you how to do this in React for Web.

While the React team does not implement this in the CRA, we'll set up with our own hands and without ejecting. Keep reading and you will see the magic happening.

## A small introduction ☕

### Why not eject the project?

Well, the reason for that is you're breaking CRA's “guarantees”. But calm, I've seen several projects that have been ejected and they are still working perfectly in production, the only problem is to eject, the settings will be mine and I have to support them.

"Stuff can break" - [Dan Abramov](https://twitter.com/dan_abramov/status/1045809734069170176)

But fortunately, using tools like [craco](https://github.com/sharegate/craco), we can easily go back to the CRA default settings if things go wrong. And this is incredible!

Since we will only touch _alias_, you do not have much to worry about, the craco will inject the new settings we make in the _craco.config.js_ file within the CRA default settings.

In case you do not know, the intention to configure absolute paths in a project made with Reactjs, is to facilitate the import of files. For this we can use a symbol to represent a root directory of our codes. See an example below:

```bash

Use this 😍
import Form from '@/components/Form'

E Avoid this 😤
import Form from '../../../../../components/Form'

```

## They say that it is practicing that you learne 🏊

☝ So come on, open your terminal and install the necessary dependencies:

```bash
# yarn
yarn add @craco/craco

# npm
npm i @craco/craco
```

✌ After performing the _craco_ installation, we will need to rename some _package.json_ command lines.

Replace the "react-scripts" with _"craco"_.

```json
{
  "scripts": {
  "start": "craco start",
  "build": "craco build",
  "test": "craco test",
}

```

This will cause the CRA scripts to be executed by _craco_ which will make injections of the settings that will be in the _craco.config.js_ file.

🛠 Create the file in the project root directory called: _craco.config.js_ and add the following settings:

```javascript
const path = require('path');

module.exports = {
  webpack: {
    alias: {
      '@': path.resolve(__dirname, 'src/')
    }
  },
  jest: {
    configure: {
      moduleNameMapper: {
        '^@(.*)$': '<rootDir>/src$1'
      }
    }
  }
};
```

## A pinch of VueJS, please! 🍲

I'm using _alias_ "@" copying as Vuejs uses. You can use _alias_ that you find interesting, such as "~" or "#", for example.

## My VSCode is not understanding anything 😢

When doing this we will come across the first problem, the autocomplete. We are already accustomed to having autocomplete when we are going to import the files using relative paths.

This error happens because VSCode does not understand that the "@" is the "src" folder of our project. To enable autocomplete we will need to configure the VSCode so that it understands. And for this we need to create a file called _jsconfig.json_ in the project root directory.

Learn more about [jsconfig.json](https://code.visualstudio.com/docs/languages/jsconfig).

Include the following properties within the file:

```json
{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": {
      "@/*": ["./*"]
    }
  },
  "exclude": ["node_modules", "**/node_modules/*"]
}
```

Amazing!

![](https://thepracticaldev.s3.amazonaws.com/i/d8dnvamm9u4v3xhp0hl3.png)

Now it works perfectly, use the command: npm start to run your project.

## A tool that likes to complain all the time! 😡

If you are using _eslint_, you will notice that it will complain on all imports that you perform using "@".

Thank God, there is a way to calm _eslint_ using [eslint-import-alias](https://www.npmjs.com/package/eslint-import-resolver-alias).

☝ First add the libraries below as development dependencies, for God's sake! 😰

```bash
# yarn
yarn add eslint-plugin-import eslint-import-resolver-alias -D

# npm
npm i eslint-plugin-import eslint-import-resolver-alias -D

```

✌ In your .eslintrc.json file include the following properties:

```bash
  "settings": {
    "import/resolver": {
      "alias": {
        "map": [["@", "./src"]]
      }
    }
  }

```

## Questions that look stupid but are not 😳💬

#### Can I use this technique in projects that go to production?

Answer: Yes, you can use it without any problem!

#### Can I use it in React Native?

Answer: No, the setting in React Native is different, I show you how to do this article:
[Configuring Absolute Paths in React Native](https://dev.to/heybrunoandrade/configuring-subways-no-react-native-en-471o).

#### Can my test files fail?

Answer: If you have followed step by step correctly, it probably will not give you an error. If you have changed the symbol that you are going to use as _alias_, make sure you have also placed this in the jest configuration there in the craco.config.js file in the _moduleNameMapper_ property.

#### Why are not you using the Babel plugin root import?

Answer: Unlike the Babel plugin root import, we only import a library that solves the problem, as well as being simple to use. Another thing that has been bothering me is that it is not supporting CRA 3.0, so the use of Craco.

## Imagine if everything in life worked perfectly 🦄

Like any lib, you may find bugs in @craco, if found please open a [issue in the official project](https://github.com/sharegate/craco/issues) for the community to improve the library and make it functional for everyone.

But cool, use it fearlessly for this tutorial.

## It's time to say goodbye 😩

I was enjoying having to spend this time with you. If you want to know what I'm up to, follow me on Twitter [@heybrunoandrade](https://twitter.com/heybrunoandrade).

Help your developer friends to stop suffering with relative imports by sharing this article!

Please help me make corrections or translate this article to other languages.
[Access Repository](https://github.com/heybrunoandrade/my-articles/tree/master/Front-end/ReactJS/Absolute%20Imports).

A big hug and until next time!
