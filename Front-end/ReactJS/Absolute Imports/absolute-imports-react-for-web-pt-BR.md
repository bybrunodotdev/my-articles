# Configurando caminhos absolutos no React para Web sem ejetar [pt-BR]

Depois de ter feito um artigo mostrando como realizar a configuração de [caminhos absolutos no React Native](https://dev.to/heybrunoandrade/configurando-caminhos-absolutos-no-react-native-pt-br-471o), agora venho mostrar como realizar essa configuração no React para Web.

Enquanto a equipe do React não implementa isso no CRA, vamos configurar com as nossas próprias mãos e sem ejetar. Continue lendo e verá a mágica acontecer.

## Uma pequena introdução ☕

### Por que não ejetar o projeto?

Bom, o motivo disso é que você estará quebrando as "garantias" do CRA. Mas calma, pego projetos o tempo todo que foram ejetados e eles estão até hoje funcionando perfeitamente em produção, o único problema de ejetar, é que as configurações serão minhas e tenho que dá suporte a elas.

"Coisas podem quebrar" - [Dan Abramov](https://twitter.com/dan_abramov/status/1045809734069170176)

Mas felizmente, utilizando ferramentas como o [craco](https://github.com/sharegate/craco), podemos voltar facilmente para as configurações padrões do CRA caso as coisas deem erradas. E isso é incrível!

Já que vamos mexer somente no _alias_, você não tem muito com o que se preocupar, o craco irá injetar as novas configurações que fizermos no arquivo _craco.config.js _ dentro das configurações padrões do CRA.

Caso você não saiba, o intuito de configurar caminhos absolutos em um projeto feito com Reactjs, é para facilitar a importação de arquivos. Para isso podemos utilizar um símbolo para representar um diretório root dos nossos códigos. Veja um exemplo abaixo:

```bash

Use isto 😍
import Form from '@/components/Form'

E Evite isto 😤
import Form from '../../../../../components/Form'

```

## Dizem que é praticando que se aprende 🏊

☝ Então vamos lá, abra o seu terminal e instale as dependências necessárias:

```bash
# yarn
yarn add @craco/craco

# npm
npm i @craco/craco
```

✌ Após realizar a instalação do _craco_, precisaremos renomear algumas linhas de comando do _package.json_.

Substitua o "react-scripts" por _"craco"_.

```json
{
  "scripts": {
  "start": "craco start",
  "build": "craco build",
  "test": "craco test",
}

```

Isso fará com que os scripts do CRA seja executado pelo _craco_ o qual irá realizar injeções das configurações que estaram no arquivo _craco.config.js_.

🛠 Crie o arquivo no diretório raiz do projeto chamado: _craco.config.js_ e inclua as configurações abaixo:

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

## Uma pitada de VueJS, por favor! 🍲

Estou utilizando o _alias_ "@" para imitar o Vuejs. Você pode utilizar o _alias_ que achar interessante, tais como "~" ou "#", por exemplo.

## Meu VSCode não está entendendo nada 😢

Ao fazer isso iremos nos deparar com o primeiro problema, o autocomplete. Já estamos acostumados a ter autocomplete quando vamos importar os arquivos utilizando caminhos relativos.

Esse erro acontece porque o VSCode não entende que o "@" é a pasta "src" do nosso projeto. Para ativarmos o autocomplete precisaremos configurar o VSCode para que ele entenda. E para isso precisaremos criar um arquivo chamado _jsconfig.json_ no diretório raiz do projeto.

Saiba mais sobre o [jsconfig.json](https://code.visualstudio.com/docs/languages/jsconfig).

Inclua as seguintes propriedades dentro do arquivo:

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

Incrível!

Agora está funcionando perfeitamente, utilize o comando: npm start para executar o seu projeto.

## Uma ferramenta que gosta de reclamar o tempo todo! 😡

Se você estiver utilizando o _eslint_, irá perceber que ele irá reclamar em todas as importações que você realizar utilizando "@".

Graças a Deus, existe uma forma de acalmar o _eslint_ utilizando o [eslint-import-alias](https://www.npmjs.com/package/eslint-import-resolver-alias).

☝ Primeiramente adicione as bibliotecas abaixo como dependências de desenvolvimento, pelo amor de Deus! 😰

```bash
# yarn
yarn add eslint-plugin-import eslint-import-resolver-alias -D

# npm
npm i eslint-plugin-import eslint-import-resolver-alias -D

```

✌ No seu arquivo .eslintrc.json inclua as seguintes propriedades:

```bash
  "settings": {
    "import/resolver": {
      "alias": {
        "map": [["@", "./src"]]
      }
    }
  }

```

## Perguntas que parecem idiotas mas não são 😳💬

### Posso utilizar esta técnica em projetos que vão para produção?

Resposta: Sim, você pode utilizar sem problema nenhum!

### Posso usar no React Native?

Resposta: Não, a configuração no React Native é diferente, mostro como fazer neste artigo:
[Configurando Caminhos absolutos no React Native](https://dev.to/heybrunoandrade/configurando-caminhos-absolutos-no-react-native-pt-br-471o).

### Meus arquivos de testes podem dar erro?

Resposta: Se você tiver seguido corretamente o passo a passo, provavelmente não. Se você mudou o símbolo que vai utilizar como _alias_, certifique-se de que tenha colocado isso também na configuração do jest lá no arquivo craco.config.js na propriedade _moduleNameMapper_.

### Por que não está utilizando o Babel plugin root import?

Resposta: Diferentemente do Babel plugin root import, importamos somente uma biblioteca que resolve o problema, além de ser simples de utilizar. Outra coisa que andou me incomodando é que não está dando suporte ao CRA 3.0, por isso a utilização do Craco.

## Imagine se tudo na vida funcionasse perfeitamente 🦄

Assim como qualquer lib, é possível que se encontre bugs no @craco, caso encontre por favor abra uma
[issue no projeto oficial](https://github.com/sharegate/craco/issues) para que a comunidade melhore a biblioteca e torne-a funcional para todos.

Mas calma, use-a sem medo para realização desse tutorial.

## É hora de dar tchau 😩

Estava gostando tando de passar esse tempo com você 😩. Caso queira saber o que ando aprontando por ai, me siga no Twitter [@heybrunoandrade](https://twitter.com/heybrunoandrade).

Ajude sua rede de amigos desenvolvedores a pararem de sofrer com importações relativas compartilhando este artigo!

Ahhh, se encontrou algo errado, você pode realizar a correção. Estou colocando os artigos no Github e você pode acessar [Clicando aqui](https://github.com/heybrunoandrade/my-articles).

Um grande abraço e até a próxima!
