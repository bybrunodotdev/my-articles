# Configuring Absolute Paths in React Native

One of the things I miss about React / React Native is the absolute path. It is very common to use relative paths to import files.

The problem is when the project grows and the folders are deeply nested, I believe you may have already seen or already done so:

    ../../../.../../../../Utils/Breadcrumbs.js

    ../../../../../Components/Form/TextField.js

Now imagine that the Utils folder has changed directory. 😢

To resolve this issue, use a library called _Babel Plugin Root Import_. With a list that can be used to encode the root of our application, which is a "src" folder. 😍

## It is practicing that you learn

☝ Add the library to your project.

```console
    babylu@Project: ~$ yarn add babel-plugin-root-import -D

    ou

    babylu@Project: ~$  npm install babel-plugin-root-import -D
```

✌ After installation, set up the _*babel.config.js*_ file that is located in the root directory.

```javascript
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    [
      'babel-plugin-root-import',
      {
        rootPathPrefix: '@',
        rootPathSuffix: 'src'
      }
    ]
  ],
  env: {
    production: {
      plugins: [
        'babel-plugin-root-import',
        {
          rootPathPrefix: '@',
          rootPathSuffix: 'src'
        }
      ]
    }
  }
};
```

With everything set up, it is now possible to import your files using "@" as a prefix. Here's an example below:

**@/Components/Form**

**@/Pages/Auth/SignIn**

## A pinch of _VueJS_ please 🍲

I am using the "@" copying the _Vuejs_. Use the prefix that you find interesting. It can be the '~' or '#' for example.

## Excuse me, could you show me the Way? 🚶

Using this technique we will have our first problem, the absence of autocomplete. This is because VSCode still does not understand that the "@" refers to the "src" folder of our project. To solve this we will create in the root directory a configuration file that **_VSCode_** understands, called _jsconfig.json_.

Inside it include the settings below:

```json
{
  "compilerOptions": {
    "target": "es6",
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "exclude": ["node_modules"]
}
```

Learn more about the _jsconfig.json_ file:
[https://code.visualstudio.com/docs/languages/jsconfig](https://code.visualstudio.com/docs/languages/jsconfig)

Agora sim! 😎

![](https://thepracticaldev.s3.amazonaws.com/i/1rbf5ujyinvkv5rirjv2.png)

## A tool that likes to complain! 😡

If you are using _eslint_ in your project, you will receive many complaints about the imports you make using the prefix '@'.

Luckily, there is a library that lets you tell _eslint_ that everything is okay.

Add:

```console
    babylu@Project: ~$ yarn add eslint-import-resolver-babel-plugin-root-import -D

    ou

    babylu@Project: ~$ npm install eslint-import-resolver-babel-plugin-root-import -D
```

In the _eslint_ configuration file include the following properties.

```json
  "settings": {
    "import/resolver": {
      "babel-plugin-root-import": {}
    }
  }
```

## Questions that look stupid but are not 🤔

#### Can I use this for applications going to production?

Answer: Yes, if you have followed the steps correctly you will see that we configured for production in babel.config.js

#### Can I use React for web?

Answer: To use the babel plugin root import for Web, you need to perform some other settings

## But not everything in life is flowers 🔴

You may encounter bugs in the library. If you find it please report it in the official babel plugin root import repository and help the community create a better library.

[https://github.com/entwicklerstube/babel-plugin-root-import/issues](https://github.com/entwicklerstube/babel-plugin-root-import/issues)

Follow me on twitter [@heybrunoandrade](https://twitter.com/heybrunoandrade)

Help me translate this article into other languages.
If you have received an error in the translation please make the repository to make a correction. I'll be very grateful.
[Access Repository](https://github.com/heybrunoandrade/my-articles/tree/master/Front-end/React%20Native/Absolute%20Imports)
