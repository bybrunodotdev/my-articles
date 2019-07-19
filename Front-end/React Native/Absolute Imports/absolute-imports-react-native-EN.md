# Configuring Absolute Paths in React Native

One of the things I miss about React / React Native is the absolute path. It is very common to use relative paths to import files.

The problem is when the project grows and the folders are deeply nested, I believe you may have already seen or already done so:

    ../../../.../../../../Utils/Breadcrumbs.js

    ../../../../../Components/Form/TextField.js

Now imagine that the Utils folder has changed directory. 😢

Para resolvermos esse problema, utilize uma biblioteca chamada _Babel Plugin Root Import_. Com essa biblioteca podemos utilizar caracteres coringas para apontar o diretório root de nossa aplicação, que geralmente é a pasta “src”. 😍

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

Com tudo configurado, já é possível realizar as importações dos seus arquivos utilizando “@” como prefixo. Segue um exemplo abaixo:

**@/Components/Form**

**@/Pages/Auth/SignIn**

## Uma pitada de _VueJS_ por favor

Estou utilizando o “@” para imitar o _Vuejs_. Utilize o prefixo que você achar interessante. Pode ser o ‘~’ ou ‘#’ por exemplo.

## Com licença, você poderia me mostrar o Caminho? 🚶

Utilizando esta técnica teremos o nosso primeiro problema, a ausência do autocomplete. Isso acontece porque o VSCode ainda não entende que o “@” faz referência a pasta “src” do nosso projeto. Para resolver isso vamos criar no diretório raiz um arquivo de configuração que o **_VSCode_** entende, chamado de _jsconfig.json_.

Dentro dele inclua as configurações abaixo:

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

Saiba mais sobre o arquivo _jsconfig.json_:  
[https://code.visualstudio.com/docs/languages/jsconfig](https://code.visualstudio.com/docs/languages/jsconfig)

Agora sim! 😎

![](https://thepracticaldev.s3.amazonaws.com/i/1rbf5ujyinvkv5rirjv2.png)

## Uma ferramenta que gosta de reclamar! 😡

Caso esteja utilizando o _eslint_ em seu projeto, irá receber muitas reclamações das importações que você faz utilizando o prefixo ‘@’.

Felizmente, existe uma biblioteca que serve para avisarmos ao _eslint_ que está tudo certo.

Adicione:

```console
    babylu@Project: ~$ yarn add eslint-import-resolver-babel-plugin-root-import -D

    ou

    babylu@Project: ~$ npm install eslint-import-resolver-babel-plugin-root-import -D
```

No arquivo de configuração do _eslint_ inclua as seguintes propriedades.

```json
  "settings": {
    "import/resolver": {
      "babel-plugin-root-import": {}
    }
  }
```

## Perguntas que parecem idiotas mas não são

#### Posso usar isso para aplicativos que vão para produção?

R: Sim, se você tiver seguido corretamente os passos verá que configuramos para produção no babel.config.js

#### Posso usar no React para web?

R: Para utilizar o babel plugin root import para Web é necessário realizar algumas outras configurações.

## Quem disse que tudo na vida são flores?

É possível que se encontre bugs na biblioteca. Caso você encontre por favor relate no repositório oficial do babel plugin root import e ajude a comunidade a criar uma biblioteca melhor.

[https://github.com/entwicklerstube/babel-plugin-root-import/issues](https://github.com/entwicklerstube/babel-plugin-root-import/issues)

Me siga no Twitter [@heybrunoandrade](https://twitter.com/heybrunoandrade)

Me ajude a traduzir esse artigo para outros idiomas.
[Acessar Repositório](https://github.com/heybrunoandrade/my-articles/tree/master/Front-end/React%20Native/Absolute%20Imports)
