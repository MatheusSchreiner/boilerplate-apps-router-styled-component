# .editorconfig

<br>
<br>

- Criar na raiz o arquivo .editorconfig, este arquivo define alguma configurações básicas do arquivo como espaçamento, nova linha ao final.
  Colar este código dentro do arquivo, caso queira remover os comentários fique avontade:

```
# editorconfig.org

root = true

# Será aplicado em todos os arquivos

[*]

# A indentação será com espaços

indent_style = space

# A indentação será com dois espaços

indent_size = 2
end_of_line = lf
charset = utf-8

# Irá remover espaços em branco ao final da linha

trim_trailing_whitespace = true

# Irá inserir uma nova linha ao final

insert_final_newline = true
```

<br>
<br>

### Eslint

<br>
<br>

- Instalar o Eslint para modificar algumas configurações e deixar o código mais limpo, pois apenas as configurações do next que estão dentro do arquivo .eslintrc.json não são suficientes como por exemplo alertar sobre uma variável que não está sendo utilizada

* npm init @eslint/config (Ao instalar ele fará algumas perguntas)

-How would you like to use ESLint?
To check syntax and find problems (Não marque para forçar o estilo de código pois iremos utilizar o prettier para isso)

-What type of modules does your project use?
Javascript modules

-Which framework does your project use?
React

-Does your project use TypeScript?
Yes

-Where does your code run?
Browser

-What format do you want your config file to be in?
JSON

-Would you like to install them now?
Yes

- Dentro do arquivo .eslintrc.json no campo rules cole estes comandos:

```
"rules": {
"react/react-in-jsx-scope": "off",
"react-hooks/rules-of-hooks": "error",
"react-hooks/exhaustive-deps": "warn",
"react/prop-types": "off",
"@typescript-eslit/explicit-module-boundary-types": "off"
}
```

<br>
<br>

# Prettier

<br>
<br>

OBS: Entre no site do Prettier para ver os comandos atualizados

- Instalar o prettier em modo dev e configurar ele para que toda vez que salve seus arquivos eles sejam modificados de acordo com as regras do lint automaticamente

* npm install --save-dev --save-exact prettier

* Criar arquivo com este comando:

```
  node --eval "fs.writeFileSync('.prettierrc','{}\n')"
  Cole dentro do arquivo:
  {
  "trailingComma": "none",
  "semi": false,
  "singleQuote": true
  }
```

- Criar arquivo .prettierignore
  Colar dentro do arquivo:

```
# A exclamação é para dizer para ele "Não" ignorar
!.storybook
!.jest
build
coverage
.next
```

- Rodar comando npx prettier src/ --write
  Dessa forma ele irá formatar todos os arquivos já existentes conforme as regras

- Instalar o plugin Prettier - Code formatter

- Criar pasta na raiz do projeto .vscode
- Criar arquivo settings.json dentro da pasta .vscode
  Colar dentro do arquivo:
  {
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
  }

- Rodar comando para que dessa forma não haja conflito do prettier com o eslint:
  npm install --save-dev eslint-config-prettier
- Colar no arquivo .eslintrc.json:

```
  "extends": ["prettier"]
```

<br>
<br>

# Husky

<br>
<br>

O Husky é utilizado para rodar comandos/configurações junto com o git, por exemplo rodar o lint e o prettier sempre que você for dar push e caso tenha algum erro ele não permite o push, pode fazer a mesma coisa com testes enfim

- npm install --save-dev husky lint-staged (O lint-staged é para rodar o lint apenas nos arquivos em staged do git e não rodar em todos arquivos do projeto)
- npx husky-init
- npm install

- Abrir o arquivo pre-commit dentro da pasta .husky
  Apagar npm test e colar: npx --no-install lint-staged

- Crie o arquivo na pasta raiz .lintstagedrc.js
  Cole:

```
  module.exports = {
  // Lint & Prettify TS and JS files
  '\*.{js,jsx,ts,tsx}': (filenames) => [
  `prettier --write ${filenames.join(' ')}`,
  `npm run lint --fix . ${filenames.join(' --file')}`
  ]
  }
```

Pronto com essa configuração toda vez que você der um git add . e fizer um commit o husky irá rodar a lib lint-staged por causa do comando npx --no-install lint-staged e esta lib irá ler o arquivo .lintstagerc.js que por sua vez irá rodar os comandos acima de prettier e lint caso de algum erro o husky não deixará fazer o commit e irá dizer os erros apontados por estes comandos.

<br>
<br>

# Jest

<br>
<br>

- npm install --save-dev jest @types/jest jest-environment-jsdom
- Criar arquivo na raiz jest.config.js
  Colar:

```
  module.exports = {
  testEnvironment: 'jsdom',
  testPathIgnorePatterns: ['/node_modules', '/.next/'],
  collectCoverage: true,
  collectCoverageFrom: [
  'src/**/*.ts(x)?',
  '!src/app/**',
  '!src/lib/registry.tsx'
  ],
  setupFilesAfterEnv: ['<rootDir>/.jest/setup.ts'],
  modulePaths: ['<rootDir>/src/'],
  transform: {
  '^.+\\.(js|jsx|ts|tsx)$': ['babel-jest', { presets: ['next/babel'] }]
  }
  }
```

- Colar no arquivo package.json

```
  "scripts": { "test": "jest --passWithNoTests --maxWorkers=50%", "test:watch": "jest --watch --maxWorkers=25%", "test:ci": "jest --runInBand" }
```

- Criar a pasta .jest criar o arquivo setup.ts
  <br>
  <br>

# React Testing Library

<br>
<br>

OBS: Entrar no site para ver comando atualizados

- Rodar comandos:
  npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event

- Colar no arquivo setup.ts da pasta .jest:
  import '@testing-library/jest-dom'

- Colar no arquivo tsconfig.json no campo include o comando:

```
  "include": [".jest/setup.ts"]
```

- Colar no arquivo .lintstagerc.js o comando para rodar os testes nos arquivos em staged:
  // O -- vazio antes do --findRelatedTests é usado para quando ele rodar o npm test ele irá ler o arquivo package.json e irá colocar o comando que está lá no lugar deste --

```
  `npm test -- --findRelatedTests ${filenames.join(' ')}`
```

<br>
<br>

# Styled Components

<br>
<br>

- Deletar os arquivos do tailwind: tailwind.config.ts postcss.config.js globals.css remover import do globals no arquivo layout.ts
- Rodar comando para desinstalar as libs:
  npm uninstall --save autoprefixer postcss tailwindcss

- Vá no site do next, get starter, no menu esquerdo: Building Your Application - styling - CSS-in-JS siga as instruções:
- Crie a pasta lib dentro da pasta src e crie o arquivo dentro da pasta lib: registry.tsx e cole todo o conteudo que está no site
- Rodar o comando para instalar o styled-components:
  npm install styled-components

- Seguindo o site cole a tag <StyledComponentsRegistry> no rootLayout

- Ao criar os arquivos styles coloque eles com o 'use client' pois eles são renderizados pelo lado do cliente já que o css-in-js utiliza o javascript para rodar o css. Exemplo:

```
'use client'

import styled from 'styled-components'

export const Wrapper = styled.main`  background-color: #06092b;
  color: #fff;
  width: 100%;
  height: 100%;
  padding: 3rem;
  text-align: center;
  display: flex;
  flex-direciton: column;
  align-items: center;
  justify-content: center;`

import \* as S from './styles'

const Main = () => (
<S.Wrapper>

<h1>React Avançado</h1>
</S.Wrapper>
)
export default Main
```

Utilizo a importação com o S pois assim saberei que a tag que utilizei vem do Styled Components e não de outra lib como MUI que utiliza nomes de tags também como o <Title> por exemplo

<br>
<br>

# Jest Styled Components

<br>
<br>

Para que o jest funcione nos styled components e assim garantindo também o css é necessáior instalar uma lib do jest para analisar o styled components

- npm install --save-dev jest-styled-components

- Crie a pasta no src chamada types e crie o arquivo jest-styled-components.d.ts e cole dentro do arquivo:

```
// Types provided from the official repo:
// https://github.com/styled-components/jest-styled-components/blob/master/typings/index.d.ts

/_ eslint-disable @typescript-eslint/no-explicit-any _/
/_ eslint-disable @typescript-eslint/ban-types _/
import { NewPlugin } from 'pretty-format'
import { css } from 'styled-components'

declare global {
namespace jest {
interface AsymmetricMatcher {

$$
typeof: Symbol
sample?: string | RegExp | object | Array<any> | Function
}

    type Value = string | number | RegExp | AsymmetricMatcher | undefined

    interface Options {
      media?: string
      modifier?: string | ReturnType<typeof css>
      supports?: string
    }

    // eslint-disable-next-line @typescript-eslint/no-unused-vars
    interface Matchers<R, T> {
      toHaveStyleRule(property: string, value?: Value, options?: Options): R
    }

}
}

export interface StyledComponentsSerializerOptions {
addStyles?: boolean
classNameFormatter?: (index: number) => string
}

export declare const styleSheetSerializer: NewPlugin & {
setStyleSheetSerializerOptions: (
options?: StyledComponentsSerializerOptions
) => void
}
```

- Na pasta .jest no arquivo setup.ts passe o seguinte import:

```
  import 'jest-styled-components'
```

- No arquivo jest.config.js cole este comando:

```
  // https://github.com/styled-components/styled-components/issues/4081
  // v6 of styled-components doesn't inject styles in test environment
  // we should to force it to use the browser version
  moduleNameMapper: {
  '^styled-components':
  'styled-components/dist/styled-components.browser.cjs.js'
  }
```

- E na aba collectCoverageFrom do arquito jest.config.js cole estas pastas para ignorar também:

```
  '!src/types/\*\*',
  '!src/styles'
```

<br>
<br>

# StoryBook

<br>
<br>

- npx storybook@latest init
- Apagar a pasta stories (É uma pasta da documentação inicial do storybook, caso queira ver vá no package.json e veja o comando para inicializar o storybook que irá abrir em uma porta e veja a documentação que está nesta pasta)
- Va no arquivo main da pasta .storybook e cole:

```
import type { StorybookConfig } from '@storybook/nextjs'

const config: StorybookConfig = {
staticDirs: ['../public'],
stories: ['../src/components/**/stories.tsx'],
addons: ['@storybook/addon-essentials'],
framework: {
name: '@storybook/nextjs',
options: {}
},
docs: {
autodocs: true
},
webpackFinal: (config) => {
config.resolve?.modules?.push(`${process.cwd()}/src`)
return config
}
}
export default config
```

- Na raiz crie o arquivo .eslintignore e cole:

```
  !.storybook
  !.jest
```

- Dentro da pasta components/Main crie o arquivo stories.tsx e comece a escrever os dados do componente, exemplo:

```
  import { Meta, StoryObj } from '@storybook/react'

import Main from '.'

export default {
title: 'Main',
component: Main,
parameters: {
layout: 'fullscreen'
}
} as Meta

export const Default: StoryObj = {}
```

E rode o comando para iniciar o storybook pra ver este componente la no storybook

Após criar os componentes do storybook que servirão como documentação do front você pode rodar o comando "npm run build-storybook" e ele irá gerar uma pasta chamada storybook-static que são os arquivos estáticos que servem como documentação do storybook, não há necessidade de levar essa pasta pro projeto então pode colocar ela no arquivo .gitignore:

```
#storybook
storybook-static
```

Está pasta você gera ela e leva ele pra um site bem simples já que é tudo estatico para documentação do front para quem quiser ver.
<br>
<br>

# Plop component

<br>
<br>

É uma lib que cria arquivos padrões pra você, por exemplo, sempre que crio um componente novo eu precisaria criar os arquivos: page, style, layout; por exemplo e ele irá criar pra você automagicamente

- npm install --save-dev plop
- Na raiz cria a pasta generators e o arquivo plopfiles.js e cole:

```
  module.exports = (plop) => {
  // create your generators here
  plop.setGenerator('component', {
  description: 'Create a component',
  prompts: [
  {
  type: 'input',
  name: 'name',
  message: 'What is your component name?'
  }
  ],
  actions: [
  {
  type: 'add',
  path: '../src/components/{{pascalCase name}}/index.tsx',
  templateFile: 'templates/Component.tsx.hbs'
  },
  {
  type: 'add',
  path: '../src/components/{{pascalCase name}}/styles.ts',
  templateFile: 'templates/styles.ts.hbs'
  },
  {
  type: 'add',
  path: '../src/components/{{pascalCase name}}/stories.tsx',
  templateFile: 'templates/stories.tsx.hbs'
  },
  {
  type: 'add',
  path: '../src/components/{{pascalCase name}}/test.tsx',
  templateFile: 'templates/test.tsx.hbs'
  }
  ]
  })
  }
```

- Na pasta generators cria a pasta templates e crie os arquivos:

```
  Component.tsx.hbs:
  import \* as S from './styles'

const {{pascalCase name}} = () => (
<S.Wrapper>
<h1>{{pascalCase name}}</h1>
</S.Wrapper>
)

export default {{pascalCase name}}

styles.ts.hbs:
import styled from 'styled-components'

export const Wrapper = styled.main``

stories.tsx.hbs:
import { Meta, StoryObj } from '@storybook/react'
import {{pascalCase name}} from '.'

export default {
title: '{{pascalCase name}}',
component: {{pascalCase name}}
} as Meta

export const Default: StoryObj = {}

test.tsx.hbs:
import { render, screen } from '@testing-library/react'

import {{pascalCase name}} from '.'

describe('<{{pascalCase name}} />', () => {
it('should render the heading', () => {
const { container } = render(<{{pascalCase name}} />)

    expect(screen.getByRole('heading', { name: /{{pascalCase name}}/i })).toBeInTheDocument()

    expect(container.firstChild).toMatchSnapshot()

})
})
```

- No package.json cole o comando no scripts:

```
  "generate": "npx --no plop --plopfile generators/plopfile.js"
```

Sempre que rodar o comando npm run generate ele irá te perguntar a pergunta que você deixou la na propriedade prompst do arquivo dele "qual o nome do componente" e fará as ações que você descreveu no arquivo, lembrando ele irá gerar os arquivos de acordo com os arquivos template que você criou;

- No .eslintignore cole

```
  generators
```
