# Guia de criação

Template para monorepo de componentes React + Typescript + Material UI + Lerna + Turbo + Storybook

<details>
<summary>Índice</summary>

- [Git](#git)
- [Package.json e Workspaces](#packagejson-e-workspaces)
- [EditorConfig](#editorconfig)
- [Dependências](#dependências)
  - [Prettier](#prettier)
  - [Eslint](#eslint)
  - [Typescript](#typescript)
  - [Jest](#jest)
  - [Lerna](#lerna)
  - [Turbo](#turbo)
  - [Husky](#husky)
  - [Lint-staged](#lint-staged)
  - [Commitlint](#commitlint)
  - [React](#react)
  - [Material UI](#material-ui)
- [Commitizen](#commitizen)
- [Storybook](#storybook)
- [Criando componentes](#criando-componentes)

</details>

## Getting started

Crie um diretório de sua escolha para nosso projeto. Para este guia vou criar o diretório `react-lib-components`.

### Git

Inicie o repositório git caso não o tenha feito.

:bulb: _O comando `git init` cria um novo repositório Git com a branch principal nomeada **master**. Recentemente, muitos projetos e serviços, como o GitHub, mudaram o nome padrão da branch principal para **main** em resposta a considerações sobre a linguagem inclusiva. A palavra **master** pode evocar associações com a escravidão, e a mudança para **main** é um esforço para usar uma linguagem mais neutra e inclusiva na comunidade de tecnologia._

```bash
git init -b main
```

Crie o arquivo `.gitignore` com o comando abaixo.

```bash
echo -e "# Dependências\n/node_modules\n\n# Diretórios de build e distribuição\n/dist\n/build\n/lib\n/out\n\n# Arquivos de cache do compilador e da transpilação\n/.tscache\n/.turbo\n\n# Logs\nnpm-debug.log*\nyarn-debug.log*\nyarn-error.log*\npnpm-debug.log*\nlerna-debug.log*\n\n# Cobertura de testes\n/coverage\n/.nyc_output\n\n# Diretórios e arquivos de configuração do editor\n/.vscode\n/.idea\n*.sublime-workspace\n*.sublime-project\n\n# Outros arquivos e diretórios\n.DS_Store\nThumbs.db" > .gitignore
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```txt
# Dependências
/node_modules

# Diretórios de build e distribuição

/dist
/build
/lib
/out

# Arquivos de cache do cpompilador e da transpilação

/.tscache
/.turbo

# Logs

npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
lerna-debug.log\*

# Cobertura de testes

/coverage
/.nyc_output

# Diretórios e arquivos de configuração do editor

/.vscode
/.idea
_.sublime-workspace
_.sublime-project

# Outros arquivos e diretórios

.DS_Store
Thumbs.db

```

</details>

### Package.json e Workspaces

Em seguida, inicie o projeto npm dentro do diretório.

```bash
npm init -y
```

Vamos utilizar o `npm workspaces`, então edite o arquivo `package.json` criado com o comando anterior e substitua o conteúdo atual pelo conteúdo abaixo.

```json
{
  "name": "react-lib-components",
  "private": true,
  "workspaces": ["./packages/**/*"],
  "description": "Monorepo template for React component library + TypeScript + Material UI + Lerna + Turbo",
  "author": "SEU_NOME_AQUI"
}
```

## Editorconfig

[Editorconfig](https://editorconfig.org/) é uma ferramenta de codificação que ajuda a manter estilos de codificação consistentes em diferentes editores e IDEs. Ele funciona através de um arquivo `.editorconfig` no repositório do projeto, definindo regras para indentação, espaços finais, conjunto de caracteres e outros estilos de código.

Crie o arquivo `.editorconfig` com o comando abaixo.

```bash
echo -e "# http://editorconfig.org\n\n# top-most EditorConfig file\nroot = true\n\n# Unix-style newlines with a newline ending every file\n[*]\ncharset = utf-8\nend_of_line = lf\ninsert_final_newline = true\ntrim_trailing_whitespace = true\n\n# Indentation override for js(x), ts(x) and vue files\n[*.{js,jsx,ts,tsx,vue}]\nindent_style = space\nindent_size = 2\ntrim_trailing_whitespace = true\ninsert_final_newline = true\n\n# Indentation override for css related files\n[*.{css,styl,scss,less,sass}]\nindent_size = 2\nindent_style = space\n\n# Indentation override for html files\n[*.html]\nindent_size = 2\nindent_style = space\n\n# Trailing space override for markdown file\n[*.md]\nindent_size = 2\nindent_style = space\ntrim_trailing_whitespace = false\n\n# Indentation override for config files\n[*.{json,yml}]\nindent_size = 2\nindent_style = space" > .editorconfig
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```yml
# http://editorconfig.org

# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

# Indentation override for js(x), ts(x) and vue files
[*.{js,jsx,ts,tsx,vue}]
indent_style = space
indent_size = 2
trim_trailing_whitespace = true
insert_final_newline = true

# Indentation override for css related files
[*.{css,styl,scss,less,sass}]
indent_size = 2
indent_style = space

# Indentation override for html files
[*.html]
indent_size = 2
indent_style = space

# Trailing space override for markdown file
[*.md]
indent_size = 2
indent_style = space
trim_trailing_whitespace = false

# Indentation override for config files
[*.{json,yml}]
indent_size = 2
indent_style = space
```

</details>

## Dependências

Agora vamos instalar algumas dependências no projeto, elas facilitarão nossa vida. O comando abaixo instala todas elas de uma vez, então nos próximos passos você poderá pular os comandos deste tópico que instalam dependências e são precedidos pelo texto: `Instale as dependências com o comando abaixo.` Se decidir instalar todas de uma vez você poderá realizar apenas os passos de criação de arquivos e configurações. Caso prefira instalar as dependências uma por uma, pode pular o comando abaixo e seguir para os próximos passos.

```bash
npm i -D typescript jest ts-jest @types/jest prettier eslint eslint-config-prettier eslint-import-resolver-typescript eslint-plugin-import eslint-plugin-prettier  eslint-plugin-react @typescript-eslint/eslint-plugin @typescript-eslint/parser lerna turbo husky lint-staged @commitlint/{config-conventional,cli} react react-dom @mui/material @emotion/react @emotion/styled
```

### Prettier

[Prettier](https://prettier.io/) é uma ferramenta de formatação de código que suporta várias linguagens, incluindo JavaScript, CSS e HTML. Ele padroniza a formatação do código, melhorando a consistência e legibilidade. Pode ser integrado a editores de código e fluxos de trabalho de CI/CD, automatizando a formatação e garantindo um estilo de código uniforme entre os desenvolvedores.

Instale a dependência com o comando abaixo.

```bash
npm i -D prettier
```

Crie o arquivo `.prettierrc.js` com o comando abaixo.

```bash
echo -e "module.exports = {\n  arrowParens: \"always\",\n  useTabs: false,\n  printWidth: 79,\n  endOfLine: \"lf\",\n  tabWidth: 2,\n  semi: true,\n};" > .prettierrc.js
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```js
module.exports = {
  arrowParens: "always",
  useTabs: false,
  printWidth: 79,
  endOfLine: "lf",
  tabWidth: 2,
  semi: true,
};
```

</details>

### Eslint

[Eslint](https://eslint.org/) é uma ferramenta de análise de código estática para identificar padrões problemáticos encontrados em código JavaScript. Ele é amplamente configurável, permitindo a personalização de regras para garantir a qualidade e consistência do código. Também é comumente usado para fazer cumprir as diretrizes de estilo de codificação, trabalhando em conjunto com ferramentas como Prettier.

Instale as dependências com o comando abaixo.

```bash
npm i -D eslint eslint-config-prettier eslint-import-resolver-typescript eslint-plugin-import eslint-plugin-prettier  eslint-plugin-react @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

Crie o arquivo `.eslintrc.js` com o comando abaixo.

```bash
echo -e "module.exports = {\n  env: {\n    browser: true,\n    es2020: true,\n    jest: true,\n    node: true,\n  },\n  extends: [\n    \"eslint:recommended\",\n    \"plugin:react/recommended\",\n    \"plugin:import/recommended\",\n    \"plugin:import/typescript\",\n    \"plugin:@typescript-eslint/recommended\",\n    \"prettier\",\n  ],\n  parser: \"@typescript-eslint/parser\",\n  parserOptions: {\n    ecmaVersion: \"latest\",\n    sourceType: \"module\",\n  },\n  plugins: [\"@typescript-eslint\", \"prettier\", \"react\"],\n  rules: {\n    \"prettier/prettier\": \"error\",\n    \"@typescript-eslint/no-var-requires\": 0,\n    \"react/react-in-jsx-scope\": 0,\n    \"import/order\": [\n      \"error\",\n      {\n        groups: [\"builtin\", \"external\", \"internal\"],\n        pathGroups: [\n          {\n            pattern: \"react\",\n            group: \"external\",\n            position: \"before\",\n          },\n        ],\n        pathGroupsExcludedImportTypes: [\"react\"],\n        \"newlines-between\": \"always\",\n        alphabetize: {\n          order: \"asc\",\n          caseInsensitive: true,\n        },\n      },\n    ],\n  },\n  settings: {\n    \"import/resolver\": {\n      typescript: {},\n    },\n    react: {\n      version: \"detect\",\n    },\n  },\n};" > .eslintrc.js
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```js
module.exports = {
  env: {
    browser: true,
    es2020: true,
    jest: true,
    node: true,
  },
  extends: [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:import/recommended",
    "plugin:import/typescript",
    "plugin:@typescript-eslint/recommended",
    "prettier",
  ],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaVersion: "latest",
    sourceType: "module",
  },
  plugins: ["@typescript-eslint", "prettier", "react"],
  rules: {
    "prettier/prettier": "error",
    "@typescript-eslint/no-var-requires": 0,
    "react/react-in-jsx-scope": 0,
    "import/order": [
      "error",
      {
        groups: ["builtin", "external", "internal"],
        pathGroups: [
          {
            pattern: "react",
            group: "external",
            position: "before",
          },
        ],
        pathGroupsExcludedImportTypes: ["react"],
        "newlines-between": "always",
        alphabetize: {
          order: "asc",
          caseInsensitive: true,
        },
      },
    ],
  },
  settings: {
    "import/resolver": {
      typescript: {},
    },
    react: {
      version: "detect",
    },
  },
};
```

</details>

### Typescript

[TypeScript](https://www.typescriptlang.org/) é uma linguagem de programação desenvolvida pela Microsoft, um superset do JavaScript, que adiciona tipagem estática opcional. Isso permite que os desenvolvedores detectem erros mais facilmente e organizem o código de forma mais eficiente, especialmente em projetos grandes.

Instale a dependência com comando abaixo.

```bash
npm i -D typescript
```

Crie o arquivo `tsconfig.json` com o comando abaixo(esta é a configuração sugerida, mas você pode utilizar a de sua preferência).

```bash
echo -e "{\n  \"compilerOptions\": {\n    /* Output Formatting Options */\n    \"skipLibCheck\": true,\n\n    /* Module Resolution Options */\n    \"module\": \"esnext\",\n    \"moduleResolution\": \"node\",\n    \"resolveJsonModule\": true,\n\n    /* Interop Constraints Options */\n    \"allowSyntheticDefaultImports\": true,\n    \"esModuleInterop\": true,\n    \"forceConsistentCasingInFileNames\": true,\n    \"isolatedModules\": true,\n\n    /* Emit Options*/\n    \"noEmit\": true,\n\n    /* Type Checking Options */\n    \"strict\": true,\n    \"noFallthroughCasesInSwitch\": true,\n    \"noImplicitAny\": true,\n    \"noUnusedParameters\": true,\n    \"strictNullChecks\": true\n  },\n  \"exclude\": [\"node_modules\", \"lib\"]\n}" > tsconfig.json
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```json
{
  "compilerOptions": {
    /* Output Formatting Options */
    "skipLibCheck": true,

    /* Module Resolution Options */
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,

    /* Interop Constraints Options */
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "isolatedModules": true,

    /* Emit Options*/
    "noEmit": true,

    /* Type Checking Options */
    "strict": true,
    "noFallthroughCasesInSwitch": true,
    "noImplicitAny": true,
    "noUnusedParameters": true,
    "strictNullChecks": true
  },
  "exclude": ["node_modules", "lib"]
}
```

</details>

### Jest

[Jest](https://jestjs.io/) é uma popular ferramenta de testes para JavaScript, conhecida por sua simplicidade e suporte a testes de snapshots. Oferece uma experiência de teste integrada com funções para criar, executar e estruturar testes, além de mockar objetos. É amplamente usada em aplicações React.

Instale as dependências com o comando abaixo.

```bash
npm i -D jest ts-jest @types/jest
```

Crie o arquivo `jest.config.ts` com o comando abaixo.

```bash
echo -e "import type { Config } from \"jest\";\n\nconst config: Config = {\n  verbose: true,\n  preset: \"ts-jest\",\n  collectCoverage: true,\n};\n\nexport default config;" > jest.config.ts
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```ts
import type { Config } from "jest";

const config: Config = {
  verbose: true,
  preset: "ts-jest",
  collectCoverage: true,
};

export default config;
```

</details>

### Lerna

[Lerna](https://lerna.js.org/) é uma ferramenta de gerenciamento de projetos JavaScript que otimiza o fluxo de trabalho em monorepos. Ela facilita a manutenção de múltiplos pacotes em um único repositório, automatizando tarefas como versionamento, publicação de pacotes e gerenciamento de dependências.

Instale a dependência com o comando abaixo.

```bash
npm i -D lerna
```

Crie o arquivo `lerna.json` com o comando abaixo.

```bash
echo -e "{\n  \"\$schema\": \"node_modules/lerna/schemas/lerna-schema.json\",\n  \"version\": \"independent\",\n  \"command\": {\n    \"publish\": {\n      \"conventionalCommits\": true,\n      \"message\": \"chore(release): publish\"\n    }\n  }\n}" > lerna.json
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```json
{
  "$schema": "node_modules/lerna/schemas/lerna-schema.json",
  "version": "independent",
  "command": {
    "publish": {
      "conventionalCommits": true,
      "message": "chore(release): publish"
    }
  }
}
```

</details>

Adicione os scripts necessários ao `package.json` com o comando abaixo.

```bash
npm pkg set scripts.version:stable="lerna version --yes" scripts.publish:stable="lerna publish from-git --yes" scripts.publish:canary="lerna publish --canary --no-git-tag-version --no-push --yes"
```

### Turbo

[Turborepo](https://turborepo.org/) é uma ferramenta de construção e gerenciamento de projetos para JavaScript e TypeScript, otimizada para monorepos. Ela permite a construção eficiente de vários pacotes com cache inteligente e execução paralela, melhorando a velocidade e a eficiência do desenvolvimento.

Instale a dependência com o comando abaixo.

```bash
npm i -D turbo
```

Crie o arquivo `turbo.json` com o comando abaixo.

```bash
echo -e "{\n  \"\$schema\": \"https://turborepo.org/schema.json\",\n  \"pipeline\": {\n    \"build\": {\n      \"outputs\": [\"build/**\"],\n      \"dependsOn\": [\"^build\"]\n    },\n    \"test\": {\n      \"inputs\": [\"src/**/*.ts\"],\n      \"outputs\": [],\n      \"dependsOn\": [\"build\"]\n    }\n  }\n}" > turbo.json
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```json
{
  "$schema": "https://turborepo.org/schema.json",
  "pipeline": {
    "build": {
      "outputs": ["build/**"],
      "dependsOn": ["^build"]
    },
    "test": {
      "inputs": ["src/**/*.ts"],
      "outputs": [],
      "dependsOn": ["build"]
    }
  }
}
```

</details>

Adicione os scripts do Turbo ao `package.json` com o comando abaixo.

```bash
npm pkg set scripts.test="turbo run test" scripts.build="turbo run build"
```

### Husky

[Husky](https://typicode.github.io/husky/) é uma ferramenta de desenvolvimento para JavaScript que facilita a gestão de ganchos Git (Git hooks). Ela permite a configuração de scripts personalizados que são executados em eventos específicos do Git, como commit ou push, ajudando a manter a qualidade e a consistência do código.

Instale a dependência com o comando abaixo.

```bash
npm i -D husky
```

Habilite os hooks do Git e adicione o script `prepare` ao `package.json` com o comando abaixo.

```bash
npx husky install && npm pkg set scripts.prepare="husky install"
```

### Lint-staged

[Lint-staged](https://github.com/lint-staged/lint-staged) é uma ferramenta de desenvolvimento que executa linters em arquivos versionados pelo Git, mas que ainda não foram commitados. Ela melhora a eficiência ao focar apenas nos arquivos alterados, garantindo que o código atenda a padrões específicos antes dos commits.

Instale as dependências com o comando abaixo.

```bash
npm i -D lint-staged
```

Crie o arquivo `.lintstagedrc` com o comando abaixo.

```bash
echo -e "{\n  \"*.{js,jsx,ts,tsx,json,md}\": [\"prettier --write\"],\n  \"*.{js,jsx,ts,tsx}\": [\n    \"eslint --fix --report-unused-disable-directives --max-warnings 0\"\n  ]\n}" > .lintstagedrc
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```json
{
  "*.{js,jsx,ts,tsx,json,md}": ["prettier --write"],
  "*.{js,jsx,ts,tsx}": [
    "eslint --fix --report-unused-disable-directives --max-warnings 0"
  ]
}
```

</details>

<table>
  <tr>
    <th style="width: 300px;">Flag</th>
    <th>Descrição</th>
  </tr>
  <tr>
    <td><code>--fix</code></td>
    <td>Tenta corrigir automaticamente problemas encontrados que possuem correções disponíveis.</td>
  </tr>
  <tr>
    <td><code>--report-unused-disable-directives</code></td>
    <td>Reporta as diretivas <code>eslint-disable</code> que são desnecessárias porque as regras que elas desativavam não são violadas.</td>
  </tr>
  <tr>
    <td><code>--max-warnings</code></td>
    <td>Define o número máximo de avisos permitidos. Com <code>0</code>, até um único aviso resulta em falha de execução.</td>
  </tr>
</table>

Adicione o hook de pre-commit com o comando abaixo.

```bash
npx husky add .husky/pre-commit "npx lint-staged"
```

### Commitlint

[Commitlint](https://commitlint.js.org/) é uma ferramenta de desenvolvimento que ajuda a manter a consistência dos mensagens de commit no Git. Ela verifica se as mensagens seguem um formato predefinido, baseado em convenções como [Conventional Commits](https://www.conventionalcommits.org/), aumentando a legibilidade e a organização do histórico de commits.

Instale as dependências com o comando abaixo.

```bash
npm i -D @commitlint/{config-conventional,cli}
```

Crie o arquivo `commitlint.config.ts` com o comando abaixo.

```bash
echo -e "import type { UserConfig } from \"@commitlint/types\";\n\nconst Configuration: UserConfig = {\n  extends: [\"@commitlint/config-conventional\"],\n};\n\nmodule.exports = Configuration;" > commitlint.config.ts
```

<details>
<summary>Clique para ver todo o conteúdo.</summary>
<br/>

```ts
import type { UserConfig } from "@commitlint/types";

const Configuration: UserConfig = {
  extends: ["@commitlint/config-conventional"],
};

module.exports = Configuration;
```

</details>

Adicione o hook de commit-msg com o comando abaixo.

```bash
npx husky add .husky/commit-msg "npx --no -- commitlint --edit ${1}"
```

### React

[React](https://reactjs.org/) é uma biblioteca JavaScript de código aberto para construir interfaces de usuário. Desenvolvida pelo Facebook, é usada para criar componentes reutilizáveis e gerenciar o estado em aplicações web de uma página (SPA). React é conhecido por seu modelo de componentes declarativos e pela eficiente atualização do DOM através de um algoritmo de reconciliação, o Virtual DOM.

Instale as dependências com o comando abaixo.

```bash
npm i -D react react-dom
```

### Material UI

[Material UI](https://mui.com/) é uma biblioteca de componentes React popular que implementa o Material Design do Google. Oferece uma vasta gama de componentes UI prontos para uso, como botões, caixas de diálogo, cards, entre outros, facilitando o desenvolvimento de interfaces atrativas e funcionais. Além disso, é altamente personalizável e otimizado para acessibilidade.

Instale as dependências com o comando abaixo.

```bash
npm i -D @mui/material @emotion/react @emotion/styled
```

## Commitizen

[Commitizen](https://commitizen-tools.github.io/commitizen/) é uma ferramenta de linha de comando que padroniza as mensagens de commit do Git. Ele guia os desenvolvedores por um prompt de perguntas para criar um commit formatado de forma consistente e legível, facilitando a automação de versionamento e geração de changelogs. É amplamente usado em projetos que seguem as convenções de mensagens de commit, como o [Conventional Commits](https://www.conventionalcommits.org/).

Instale globalmente com o comando abaixo.

```bash
npm i -g commitizen
```

Inicialize a configuração do Commitizen com o comando abaixo.

```bash
npx commitizen init cz-conventional-changelog --npm --save-dev --exact
```

<table>
  <tr>
    <th style="width: 100px;">Flag</th>
    <th>Descrição</th>
  </tr>
  <tr>
    <td><code>--npm</code></td>
    <td>Especifica que o NPM deve ser usado para instalar o adaptador. Útil em ambientes com múltiplos gerenciadores de pacotes.</td>
  </tr>
  <tr>
    <td><code>--save-dev</code></td>
    <td>Adiciona o adaptador como uma dependência de desenvolvimento no arquivo <code>package.json</code>. Não será incluído em produção.</td>
  </tr>
  <tr>
    <td><code>--exact</code></td>
    <td>Garante a instalação da versão exata do adaptador, sem usar range de versões, para consistência entre os ambientes de desenvolvimento.</td>
  </tr>
</table>

## Storybook

[Storybook](https://storybook.js.org/) é uma ferramenta de desenvolvimento de interface do usuário (UI) para componentes de front-end. Ela permite aos desenvolvedores criar e visualizar componentes de UI isoladamente, facilitando o desenvolvimento e teste. Suporta frameworks como React, Vue, Angular e outros, tornando-se uma escolha popular para a documentação de componentes e construção de bibliotecas de design.

Inicialize a configuração do Storybook para React jutamente com o builder do vite.

```bash
npx sb init --type react --builder vite
```

@TODO: ajustar arquivo de configuração do Storybook

## Criando componentes

@TODO: descrever como criar um componente
