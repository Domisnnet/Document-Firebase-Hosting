Perfeito. Agora tenho o conteúdo completo. Abaixo está o **mesmo README integral**, padronizado em **Markdown**, com badges consistentes, tabela premium em três colunas e links internos corrigidos com âncoras explícitas.

```markdown
<a id="topo"></a>

# 📘 Deploy de Angular no Firebase Hosting

![Documentação](https://img.shields.io/badge/Documentação-Técnica-1A73E8?style=for-the-badge&logo=markdown&logoColor=white)
![Nível](https://img.shields.io/badge/Nível-Intermediário%20→%20Avançado-7B1FA2?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Atualizado-4CAF50?style=for-the-badge)
![Atualização](https://img.shields.io/badge/Atualizado-Agosto%202026-607D8B?style=for-the-badge)

[![Angular](https://img.shields.io/badge/Angular-17%2B-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.dev/)
[![Firebase Hosting](https://img.shields.io/badge/Firebase-Hosting-FFCA28?style=for-the-badge&logo=firebase&logoColor=000000)](https://firebase.google.com/docs/hosting)
[![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org/)
[![Firebase CLI](https://img.shields.io/badge/Firebase_CLI-15.x%2B-FFCA28?style=for-the-badge&logo=firebase&logoColor=000000)](https://firebase.google.com/docs/cli)

> Documentação técnica para publicar uma aplicação Angular no Firebase Hosting.  
> **Nível:** intermediário → avançado  
> **Última atualização:** agosto de 2026

---

## 📚 Tabela de Conteúdo

| 🚀 Configuração | 🏗️ Build & Publicação | 🛠️ Suporte & Referências |
| :---: | :---: | :---: |
| [![1. Objetivo](https://img.shields.io/badge/1%20-%20Objetivo-4CAF50?style=flat-square)](#objetivo) | [![5. Build](https://img.shields.io/badge/5%20-%20Build-607D8B?style=flat-square)](#build-producao) | [![9. Erros](https://img.shields.io/badge/9%20-%20Erros-F44336?style=flat-square)](#troubleshooting) |
| [![2. Requisitos](https://img.shields.io/badge/2%20-%20Requisitos-2196F3?style=flat-square)](#pre-requisitos) | [![6. Hosting](https://img.shields.io/badge/6%20-%20Hosting-FF9800?style=flat-square)](#firebase-json) | [![10. Práticas](https://img.shields.io/badge/10%20-%20Práticas-9C27B0?style=flat-square)](#boas-praticas) |
| [![3. Firebase CLI](https://img.shields.io/badge/3%20-%20Firebase%20CLI-FFCA28?style=flat-square&logo=firebase&logoColor=000000)](#firebase-cli) | [![7. Testar](https://img.shields.io/badge/7%20-%20Testar-00BCD4?style=flat-square)](#testar-localmente) | [![11. Checklist](https://img.shields.io/badge/11%20-%20Checklist-4CAF50?style=flat-square)](#checklist-final) |
| [![4. Associar](https://img.shields.io/badge/4%20-%20Associar-3F51B5?style=flat-square&logo=firebase&logoColor=white)](#associar-projeto) | [![8. Deploy](https://img.shields.io/badge/8%20-%20Deploy-009688?style=flat-square&logo=firebase&logoColor=white)](#fazer-deploy) | [![12. Referências](https://img.shields.io/badge/12%20-%20Referências-795548?style=flat-square)](#referencias) |

---

<a id="objetivo"></a>

## 🎯 Objetivo

[![Foco](https://img.shields.io/badge/Foco-Deploy%20Angular%20%2B%20Firebase-4CAF50?style=flat-square)](#objetivo)
[![Aplicação](https://img.shields.io/badge/Aplicação-Single--Page%20Application-673AB7?style=flat-square)](#como-funciona)

Este guia mostra como conectar um projeto Angular existente a um projeto já criado no Firebase e publicá-lo utilizando o Firebase Hosting.

O processo inclui:

- Instalação e autenticação do Firebase CLI.
- Associação do projeto Angular ao projeto Firebase.
- Configuração do Firebase Hosting.
- Geração do build de produção.
- Configuração de rotas para uma aplicação Single-Page Application.
- Teste local e deploy da aplicação.

O Firebase utiliza o arquivo `firebase.json` para definir o comportamento do Hosting, incluindo a pasta pública, arquivos ignorados, rewrites e redirects.

---

<a id="como-funciona"></a>

## 🔄 Como funciona

[![Fluxo](https://img.shields.io/badge/Fluxo-Build%20→%20Hosting-607D8B?style=flat-square)](#como-funciona)

O Angular não publica diretamente os arquivos presentes em `src/`. Primeiro, o projeto precisa ser compilado com `ng build`, que transforma o código TypeScript em JavaScript e gera os arquivos otimizados da aplicação.

O Firebase Hosting publica somente a pasta definida na propriedade `hosting.public` do arquivo `firebase.json`.

```text
Código-fonte Angular
        ↓
ng build --configuration production
        ↓
Pasta dist/
        ↓
Firebase Hosting
        ↓
Aplicação publicada
```

> **Regra de ouro:** o valor de `hosting.public` precisa apontar para a pasta que contém diretamente o `index.html` gerado pelo build.

---

<a id="pre-requisitos"></a>

## ✅ Pré-requisitos

[![Node.js](https://img.shields.io/badge/Node.js-Requerido-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org/)
[![Angular CLI](https://img.shields.io/badge/Angular_CLI-Requerido-DD0031?style=flat-square&logo=angular&logoColor=white)](https://angular.dev/tools/cli)
[![Firebase CLI](https://img.shields.io/badge/Firebase_CLI-Requerido-FFCA28?style=flat-square&logo=firebase&logoColor=000000)](https://firebase.google.com/docs/cli)

Antes de iniciar, certifique-se de que possui:

| Requisito | Recomendação |
| :--- | :--- |
| Node.js | Versão compatível com o Angular utilizado |
| npm | Instalado junto com o Node.js |
| Angular CLI | Compatível com a versão do projeto |
| Firebase CLI | Versão atual |
| Conta Google | Com acesso ao projeto Firebase |
| Projeto Firebase | Criado no Firebase Console |
| Projeto Angular | Existente e compilando localmente |
| VS Code | Recomendado, mas opcional |

Valide as ferramentas instaladas:

```bash
node --version
npm --version
ng version
```

[![Node.js](https://img.shields.io/badge/Verificar-Node.js-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org/)
[![Angular CLI](https://img.shields.io/badge/Verificar-Angular_CLI-DD0031?style=flat-square&logo=angular&logoColor=white)](https://angular.dev/tools/cli)
[![Firebase CLI](https://img.shields.io/badge/Verificar-Firebase_CLI-FFCA28?style=flat-square&logo=firebase&logoColor=000000)](https://firebase.google.com/docs/cli)

---

<a id="exemplo-utilizado"></a>

## 🧪 Exemplo utilizado

[![Exemplo](https://img.shields.io/badge/Projeto-Shadow--Flip--Angular-673AB7?style=flat-square)](#exemplo-utilizado)
[![Firebase](https://img.shields.io/badge/Firebase-shadow--angular-FFCA28?style=flat-square&logo=firebase&logoColor=000000)](#exemplo-utilizado)

Neste documento, serão utilizados os seguintes nomes fictícios:

| Item | Valor |
| :--- | :--- |
| Projeto Angular | `Shadow-Flip-Angular` |
| Projeto Firebase | `shadow-angular` |
| URL esperada | `https://shadow-angular.web.app` |

Substitua esses valores pelos nomes reais do seu projeto.

---

<a id="passo-a-passo"></a>

## 🚀 Passo a passo

[![Tutorial](https://img.shields.io/badge/Fluxo-10%20etapas-1A73E8?style=flat-square)](#passo-a-passo)

---

<a id="firebase-cli"></a>

### 1. Instalar o Firebase CLI

[![Instalação](https://img.shields.io/badge/Etapa%201-Instalação-FFCA28?style=flat-square&logo=firebase&logoColor=000000)](#firebase-cli)

Abra o terminal integrado do VS Code e execute:

```bash
npm install -g firebase-tools
```

Verifique se a instalação foi concluída:

```bash
firebase --version
```

Exemplo de saída:

```text
15.27.0
```

Se o comando `firebase` não for reconhecido, reinicie o terminal do VS Code. Caso o problema persista, verifique o diretório global do npm:

```bash
npm prefix -g
```

[![Documentação Firebase CLI](https://img.shields.io/badge/Documentação-Firebase_CLI-FFCA28?style=flat-square&logo=firebase&logoColor=000000)](https://firebase.google.com/docs/cli)

---

<a id="login-firebase"></a>

### 2. Fazer login no Firebase

[![Login](https://img.shields.io/badge/Etapa%202-Autenticação-1A73E8?style=flat-square&logo=google&logoColor=white)](#login-firebase)

Execute:

```bash
firebase login
```

O navegador será aberto para autenticação com sua conta Google.

Depois do login, liste os projetos disponíveis:

```bash
firebase projects:list
```

O projeto criado no Firebase Console deverá aparecer na lista.

Exemplo:

```text
Project Display Name    Project ID
shadow-angular          shadow-angular
```

---

<a id="pasta-projeto"></a>

### 3. Entrar na pasta do projeto

[![Diretório](https://img.shields.io/badge/Etapa%203-Projeto%20Angular-3F51B5?style=flat-square)](#pasta-projeto)

Navegue até a pasta raiz do projeto Angular:

```bash
cd caminho/do/seu/projeto
```

#### Windows

```powershell
cd "C:\Projects\Shadow-Flip-Angular"
```

#### macOS ou Linux

```bash
cd ~/Projects/Shadow-Flip-Angular
```

Confirme se está na pasta correta.

#### Windows

```powershell
dir
```

#### macOS ou Linux

```bash
ls
```

A pasta deverá conter arquivos ou diretórios semelhantes a:

```text
angular.json
package.json
src/
```

---

<a id="associar-projeto"></a>

### 4. Associar o projeto Angular ao Firebase

[![Vincular](https://img.shields.io/badge/Etapa%204-Vincular%20projeto-3F51B5?style=flat-square&logo=firebase&logoColor=white)](#associar-projeto)

Execute:

```bash
firebase use --add
```

Selecione o projeto Firebase desejado e defina um alias, por exemplo:

```text
default
```

Esse processo cria ou atualiza o arquivo `.firebaserc`:

```json
{
  "projects": {
    "default": "shadow-angular"
  }
}
```

Confirme o projeto atualmente selecionado:

```bash
firebase use
```

A saída deverá indicar o projeto ativo:

```text
Active Project: shadow-angular
```

---

<a id="inicializar-hosting"></a>

### 5. Inicializar o Firebase Hosting

[![Hosting](https://img.shields.io/badge/Etapa%205-Hosting-FFCA28?style=flat-square&logo=firebase&logoColor=000000)](#inicializar-hosting)

Execute:

```bash
firebase init hosting
```

Durante o assistente, selecione ou informe:

| Pergunta | Resposta recomendada |
| :--- | :--- |
| Use an existing project? | Sim |
| Projeto Firebase | `shadow-angular` |
| Public directory | Será definido após verificar o build |
| Configure as a single-page app? | Sim |
| Set up automatic builds with GitHub? | Opcional |

Se preferir iniciar o Firebase manualmente, execute:

```bash
firebase init
```

Nesse caso, selecione apenas o recurso de Hosting:

```text
Hosting: Configure files for Firebase Hosting and optionally set up GitHub Action deploys
```

Não é necessário selecionar Firestore, Functions, Storage ou outros produtos quando o objetivo for apenas publicar o front-end Angular.

O comando `firebase init` cria o arquivo `firebase.json` na raiz do projeto, arquivo utilizado para configurar os recursos do Firebase.

---

<a id="build-producao"></a>

### 6. Gerar o build de produção

[![Build](https://img.shields.io/badge/Etapa%206-Build%20de%20produção-DD0031?style=flat-square&logo=angular&logoColor=white)](#build-producao)

Execute:

```bash
ng build --configuration production
```

Também é possível utilizar o script padrão do projeto:

```bash
npm run build
```

O Angular exibirá o diretório de saída do build.

Em projetos recentes, a estrutura poderá ser semelhante a:

```text
dist/
└── shadow-flip-angular/
    └── browser/
        ├── index.html
        ├── main-XXXXXXXX.js
        ├── styles-XXXXXXXX.css
        └── assets/
```

Em outros projetos, o `index.html` poderá estar diretamente em:

```text
dist/shadow-flip-angular/
```

> Não presuma o caminho da pasta `public`. Sempre confirme onde o build realmente gerou o `index.html`.

---

<a id="localizar-index"></a>

### 7. Localizar o `index.html`

[![Index](https://img.shields.io/badge/Etapa%207-Localizar%20index.html-607D8B?style=flat-square)](#localizar-index)

#### PowerShell

Execute na raiz do projeto:

```powershell
Get-ChildItem . -Filter index.html -Recurse | Select-Object FullName
```

Exemplo de resultado:

```text
C:\Projects\Shadow-Flip-Angular\dist\shadow-flip-angular\browser\index.html
```

Nesse caso, a pasta correta para o Firebase Hosting será:

```text
dist/shadow-flip-angular/browser
```

#### macOS ou Linux

```bash
find . -type f -name "index.html"
```

O valor configurado em `public` deve ser a pasta que contém o arquivo, e não o caminho completo do arquivo.

#### Correto

```text
dist/shadow-flip-angular/browser
```

#### Incorreto

```text
dist/shadow-flip-angular/browser/index.html
```

---

<a id="firebase-json"></a>

### 8. Configurar o `firebase.json`

[![Configuração](https://img.shields.io/badge/Etapa%208-firebase.json-FF9800?style=flat-square)](#firebase-json)

Abra o arquivo `firebase.json` na raiz do projeto e configure-o conforme a localização real do `index.html`.

#### Exemplo com a pasta `browser`

```json
{
  "hosting": {
    "public": "dist/shadow-flip-angular/browser",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

#### Exemplo sem a pasta `browser`

Se o arquivo estiver em:

```text
dist/shadow-flip-angular/index.html
```

utilize:

```json
{
  "hosting": {
    "public": "dist/shadow-flip-angular",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

#### Significado das propriedades

| Propriedade | Finalidade |
| :--- | :--- |
| `public` | Define a pasta que será publicada |
| `ignore` | Impede o envio de arquivos desnecessários |
| `rewrites` | Encaminha determinadas URLs para outro recurso |
| `source` | Define o padrão de URL correspondente |
| `destination` | Define o destino da requisição |

O Firebase permite configurar no `firebase.json` quais arquivos serão publicados e como as URLs serão processadas.

#### Por que usar `rewrites`?

O Angular utiliza roteamento no navegador. Rotas como as seguintes podem não existir como arquivos físicos:

```text
/
/projects
/about
/contact
```

Sem um rewrite, o Firebase poderá procurar um arquivo físico chamado `projects`, `about` ou `contact` e retornar uma página 404.

A regra abaixo entrega o `index.html` para essas rotas:

```json
"rewrites": [
  {
    "source": "**",
    "destination": "/index.html"
  }
]
```

Depois disso, o Angular Router interpreta a URL e renderiza o componente correspondente.

> A configuração acima é indicada para aplicações Angular client-side. Projetos com SSR possuem requisitos diferentes e devem ser configurados conforme a arquitetura utilizada.

---

<a id="testar-localmente"></a>

### 9. Testar localmente

[![Teste](https://img.shields.io/badge/Etapa%209-Emulador%20local-00BCD4?style=flat-square&logo=firebase&logoColor=000000)](#testar-localmente)

Antes do deploy, gere o build:

```bash
ng build --configuration production
```

Depois, inicie o emulador do Hosting:

```bash
firebase emulators:start --only hosting
```

O Firebase exibirá uma URL local, normalmente semelhante a:

```text
http://127.0.0.1:5000
```

Valide:

- Página inicial.
- Rotas internas.
- Atualização da página em uma rota profunda.
- Imagens e arquivos da pasta `assets`.
- Responsividade.
- Console do navegador.
- Links externos.
- Integrações com APIs.

Para encerrar o emulador:

```text
Ctrl + C
```

O Firebase Hosting oferece um emulador local para testar alterações antes da publicação em produção.

---

<a id="fazer-deploy"></a>

### 10. Fazer o deploy

[![Deploy](https://img.shields.io/badge/Etapa%2010-Deploy%20em%20produção-009688?style=flat-square&logo=firebase&logoColor=white)](#fazer-deploy)

Depois de validar o build e o `firebase.json`, execute:

```bash
firebase deploy --only hosting
```

Ao final, será exibida uma mensagem semelhante a:

```text
Deploy complete!

Hosting URL: https://shadow-angular.web.app
```

Abra a URL no navegador e valide a aplicação em produção.

O comando `firebase deploy --only hosting` publica somente os recursos relacionados ao Firebase Hosting.

[![Acessar Firebase Hosting](https://img.shields.io/badge/Acessar-Firebase_Hosting-FFCA28?style=for-the-badge&logo=firebase&logoColor=000000)](https://firebase.google.com/docs/hosting)

---

<a id="proximos-deploys"></a>

## 🔁 Fluxo dos próximos deploys

[![Automação](https://img.shields.io/badge/Fluxo-Build%20%2B%20Deploy-009688?style=flat-square)](#proximos-deploys)

Depois que a configuração inicial estiver pronta, o processo será:

```bash
ng build --configuration production
firebase deploy --only hosting
```

Você também pode criar scripts no `package.json`:

```json
{
  "scripts": {
    "build:production": "ng build --configuration production",
    "deploy:hosting": "npm run build:production && firebase deploy --only hosting"
  }
}
```

Depois, execute apenas:

```bash
npm run deploy:hosting
```

O comando abaixo poderá publicar outros recursos Firebase configurados no projeto:

```bash
firebase deploy
```

Para publicar somente o site, prefira:

```bash
firebase deploy --only hosting
```

---

<a id="estrutura-projeto"></a>

## 📁 Estrutura do projeto

[![Estrutura](https://img.shields.io/badge/Projeto-Angular%20%2B%20Firebase-673AB7?style=flat-square)](#estrutura-projeto)

Uma estrutura típica poderá ser:

```text
Shadow-Flip-Angular/
├── dist/
│   └── shadow-flip-angular/
│       └── browser/
│           ├── index.html
│           ├── main-XXXXXXXX.js
│           ├── styles-XXXXXXXX.css
│           └── assets/
├── src/
│   ├── app/
│   ├── assets/
│   ├── index.html
│   ├── main.ts
│   └── styles.scss
├── .firebaserc
├── firebase.json
├── angular.json
├── package.json
├── package-lock.json
└── tsconfig.json
```

A pasta `dist/` é gerada automaticamente pelo processo de build e não deve ser editada manualmente.

---

<a id="troubleshooting"></a>

## 🛠️ Troubleshooting

[![Suporte](https://img.shields.io/badge/Suporte-Resolução%20de%20erros-F44336?style=flat-square)](#troubleshooting)

### `firebase` não é reconhecido

Instale ou reinstale o Firebase CLI:

```bash
npm install -g firebase-tools
```

Depois, reinicie o terminal e valide:

```bash
firebase --version
```

Se o problema persistir, consulte o diretório global do npm:

```bash
npm prefix -g
```

---

### Erro `Page Not Found`

Verifique:

- Se o build foi executado.
- Se o `index.html` existe.
- Se `hosting.public` aponta para a pasta correta.
- Se o deploy foi executado depois da alteração.
- Se não existe outro `firebase.json` em uma pasta diferente.

Localize o arquivo:

```powershell
Get-ChildItem . -Filter index.html -Recurse | Select-Object FullName
```

---

### Erro `Could not detect project root`

Execute os comandos dentro da pasta que contém:

```text
angular.json
package.json
src/
```

---

### Rotas internas retornam 404

Confirme se o `firebase.json` possui:

```json
"rewrites": [
  {
    "source": "**",
    "destination": "/index.html"
  }
]
```

Depois, gere o build novamente e faça um novo deploy:

```bash
ng build --configuration production
firebase deploy --only hosting
```

---

### Alterações não aparecem no navegador

Tente:

- Atualizar a página com `Ctrl + F5`.
- Abrir uma janela anônima.
- Limpar o cache do navegador.
- Confirmar se o deploy foi concluído.
- Verificar a URL correta do projeto Firebase.

---

### Arquivos estáticos não carregam

Confirme:

- Se os arquivos estão dentro de `src/assets`.
- Se o caminho utilizado no código está correto.
- Se os assets estão configurados no `angular.json`.
- Se o caminho respeita letras maiúsculas e minúsculas.

Exemplo:

```html
<img src="assets/images/logo.svg" alt="Logo">
```

---

<a id="boas-praticas"></a>

## 🧠 Boas práticas

[![Qualidade](https://img.shields.io/badge/Foco-Qualidade%20e%20segurança-9C27B0?style=flat-square)](#boas-praticas)

### Use build de produção

Para publicar, prefira:

```bash
ng build --configuration production
```

Esse perfil normalmente aplica otimizações como:

- Minificação.
- Tree-shaking.
- Otimização de bundles.
- Remoção de código não utilizado.
- Geração de arquivos prontos para produção.

---

### Não versione a pasta `dist`

Adicione ao `.gitignore`:

```gitignore
dist/
.firebase/
```

---

### Teste antes de publicar

Use o emulador do Hosting:

```bash
firebase emulators:start --only hosting
```

---

### Use canais de preview

Para gerar uma publicação temporária:

```bash
firebase hosting:channel:deploy preview
```

O Firebase fornecerá uma URL de preview que poderá ser compartilhada para validação antes do deploy em produção.

---

### Use aliases para ambientes

Exemplo de `.firebaserc`:

```json
{
  "projects": {
    "development": "shadow-angular-dev",
    "staging": "shadow-angular-staging",
    "production": "shadow-angular"
  }
}
```

Para selecionar um ambiente:

```bash
firebase use production
```

Depois, execute:

```bash
firebase deploy --only hosting
```

Antes de executar um deploy, confirme sempre o projeto ativo:

```bash
firebase use
```

---

### Evite publicar arquivos sensíveis

Nunca coloque no diretório público:

- Chaves privadas.
- Arquivos `.env` com segredos.
- Credenciais de service account.
- Tokens de acesso.
- Arquivos administrativos do Firebase.

> A configuração do Firebase no front-end não deve conter credenciais privadas. Segredos devem permanecer em ambientes protegidos no back-end ou no pipeline de CI/CD.

---

<a id="checklist-final"></a>

## ✅ Checklist final

[![Pré-deploy](https://img.shields.io/badge/Validação-Pré--deploy-4CAF50?style=flat-square)](#checklist-final)

Antes do primeiro deploy:

- [ ] Node.js instalado.
- [ ] npm funcionando.
- [ ] Angular CLI disponível.
- [ ] Firebase CLI instalado.
- [ ] Login realizado com `firebase login`.
- [ ] Projeto Firebase selecionado.
- [ ] Projeto Angular compilando localmente.
- [ ] Build de produção gerado.
- [ ] `index.html` localizado.
- [ ] `hosting.public` apontando para a pasta correta.
- [ ] `rewrites` configurado para o Angular Router.
- [ ] Emulador local validado, quando necessário.
- [ ] Deploy executado com `firebase deploy --only hosting`.
- [ ] URL de produção testada.
- [ ] Rotas internas testadas.
- [ ] Assets e imagens validados.
- [ ] Console do navegador revisado.

---

<a id="referencias"></a>

## 📚 Referências oficiais

[![Firebase Hosting](https://img.shields.io/badge/Firebase-Hosting-FFCA28?style=for-the-badge&logo=firebase&logoColor=000000)](https://firebase.google.com/docs/hosting)
[![Firebase CLI](https://img.shields.io/badge/Firebase-CLI-FFCA28?style=for-the-badge&logo=firebase&logoColor=000000)](https://firebase.google.com/docs/cli)
[![Angular CLI](https://img.shields.io/badge/Angular-CLI-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.dev/tools/cli)

- [Firebase Hosting](https://firebase.google.com/docs/hosting)
- [Firebase CLI](https://firebase.google.com/docs/cli)
- [Get started with Firebase Hosting](https://firebase.google.com/docs/hosting/quickstart)
- [Integração entre Angular e Firebase Hosting](https://firebase.google.com/docs/hosting/frameworks/angular)
- [Configuração completa do `firebase.json`](https://firebase.google.com/docs/hosting/full-config)
- [Documentação do Angular CLI](https://angular.dev/tools/cli)
- [Build de aplicações Angular](https://angular.dev/tools/cli/build)

---

<a id="regra-mais-importante"></a>

## 📌 Regra mais importante

[![Regra de ouro](https://img.shields.io/badge/Regra%20de%20ouro-hosting.public%20%3D%20pasta%20do%20index.html-D32F2F?style=for-the-badge)](#regra-mais-importante)

O valor de `public` no `firebase.json` deve apontar para a pasta que contém diretamente o `index.html` gerado pelo build.

Exemplo:

```json
{
  "hosting": {
    "public": "dist/shadow-flip-angular/browser"
  }
}
```

Se o Firebase exibir uma página `Page Not Found` ou informar que não encontrou o `index.html`, verifique:

1. O resultado do `ng build`.
2. A localização real do `index.html`.
3. O valor de `hosting.public`.
4. A configuração de `rewrites`.
5. Se o deploy foi executado novamente.

---

<h2 id="perfil-do-github">. 👨‍💻 Perfil do GitHub</h2>

<a href="https://github.com/Domisnnet"> 
  <img src="images/DomisDev.png" width="90" style="border-radius: 50%" alt="DomisDev GitHub"> 
</a>

[![Voltar ao topo](https://img.shields.io/badge/⬆️%20Voltar%20ao%20topo-1A73E8?style=for-the-badge)](#topo)