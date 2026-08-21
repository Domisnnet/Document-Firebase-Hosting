![GitHub repo size](https://img.shields.io/github/repo-size/Domisnnet/Shadow-Flip-Angular?style=for-the-badge)
![GitHub stars](https://img.shields.io/github/stars/Domisnnet/Shadow-Flip-Angular?style=for-the-badge)
![GitHub last commit](https://img.shields.io/github/last-commit/Domisnnet/Shadow-Flip-Angular?style=for-the-badge)

<h2 id="sobre-o-projeto">1. 🚀 Deploy de Angular 20 no Firebase Hosting 🚀</h2>

![Status](https://img.shields.io/badge/Status-Documentação-4CAF50?style=flat-square)
![Angular](https://img.shields.io/badge/Angular-20-DD0031?style=flat-square\&logo=angular\&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-Hosting-FFCA28?style=flat-square\&logo=firebase\&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-20.19%2B-339933?style=flat-square\&logo=node.js\&logoColor=white)
![Firebase CLI](https://img.shields.io/badge/Firebase_CLI-15%2B-FFCA28?style=flat-square\&logo=firebase\&logoColor=black)
[![Licença MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/Domisnnet/Document-Firebase-Hosting/blob/main/license)

> Guia técnico para conectar uma aplicação **Angular 20** a um projeto existente no **Firebase** e publicá-la utilizando o **Firebase Hosting**.
>
> **Nível:** intermediário → avançado
> **Última atualização:** agosto de 2026

---

## 📚 Tabela de Conteúdo



---

<h2 id="objetivo">2. 🎯 Objetivo</h2>

Este guia mostra como conectar um projeto **Angular 20 existente** a um projeto já criado no **Firebase** e publicá-lo utilizando o **Firebase Hosting**.

O processo inclui:

* Instalação do Firebase CLI.
* Autenticação da conta Google.
* Associação do projeto Angular ao projeto Firebase.
* Geração do build de produção.
* Localização do `index.html` gerado pelo Angular.
* Configuração do Firebase Hosting.
* Configuração de rotas para uma aplicação Single-Page Application.
* Teste local.
* Deploy em produção.

O Firebase utiliza o arquivo `firebase.json` para definir o comportamento do Hosting, incluindo a pasta pública, arquivos ignorados, rewrites e redirects.

> **Importante:** este tutorial considera uma aplicação Angular client-side/SPA. Projetos Angular que utilizam SSR ou uma arquitetura full-stack podem exigir uma configuração diferente, incluindo o Firebase App Hosting.

---

<h2 id="como-funciona">3. 🔄 Como funciona</h2>

O Angular não publica diretamente os arquivos presentes em `src/`.

Primeiro, o projeto precisa ser compilado com:

```bash
ng build --configuration production
```

O build transforma o código TypeScript e demais recursos da aplicação em arquivos prontos para publicação.

O Firebase Hosting publica somente a pasta definida na propriedade `hosting.public` do arquivo `firebase.json`.

### Fluxo

```text
Código-fonte Angular
        ↓
ng build --configuration production
        ↓
Pasta dist/
        ↓
Localização do index.html
        ↓
firebase.json
        ↓
Firebase Hosting
        ↓
Aplicação publicada
```

> **Regra de ouro:** o valor de `hosting.public` deve apontar para a pasta que contém diretamente o `index.html` gerado pelo build.

Em projetos Angular recentes, essa pasta pode ser:

```text
dist/nome-do-projeto/browser
```

ou, dependendo da configuração do projeto:

```text
dist/nome-do-projeto
```

Por isso, **não presuma o caminho**. Sempre verifique onde o Angular realmente gerou o `index.html`.

---

<h2 id="tecnologias-utilizadas">4. ⚙️ Tecnologias Utilizadas</h2>

### Requisitos

| Requisito        | Recomendação                       |
| :--------------- | :--------------------------------- |
| Angular          | Angular 20                         |
| Node.js          | 20.19+                             |
| npm              | Compatível com o Node.js instalado |
| Angular CLI      | Compatível com Angular 20          |
| Firebase CLI     | Versão atual                       |
| Conta Google     | Com acesso ao projeto Firebase     |
| Projeto Firebase | Criado no Firebase Console         |
| Projeto Angular  | Existente e compilando localmente  |
| VS Code          | Recomendado, mas opcional          |

Para Angular 20, utilize uma versão do Node.js compatível com a versão específica do Angular 20 utilizada no projeto.

Verifique as ferramentas instaladas:

```bash
node --version
npm --version
ng version
firebase --version
```

---

<h2 id="exemplo-utilizado">5. 🧪 Exemplo Utilizado</h2>

Este tutorial utiliza como exemplo o projeto:

| Item             | Valor                            |
| :--------------- | :------------------------------- |
| Projeto Angular  | `Shadow-Flip-Angular`            |
| Projeto Firebase | `shadow-angular`                 |
| URL esperada     | `https://shadow-angular.web.app` |
| Angular          | `20`                             |

Os nomes acima são utilizados como referência no tutorial.

Substitua-os pelos nomes reais do seu projeto quando necessário.

---

<h2 id="configuração-e-deploy">6. 📦 Configuração e Deploy</h2>

## ⚙️ Instalar o Firebase CLI

Abra o terminal integrado do VS Code e execute:

```bash
npm install -g firebase-tools
```

Verifique a instalação:

```bash
firebase --version
```

Exemplo:

```text
15.27.0
```

Se o comando `firebase` não for reconhecido, reinicie o terminal do VS Code.

Caso o problema persista, verifique o diretório global do npm:

```bash
npm prefix -g
```

* E se precisar , consulte a Documentação:

[![Firebase CLI](https://img.shields.io/badge/Firebase%20CLI%20-%20Documentação%20Oficial-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/docs/cli)

---

## 🔐 Fazer login no Firebase

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

## 📂 Entrar na pasta do projeto

Navegue até a pasta raiz do projeto Angular.

### Windows

```powershell
cd "C:\Projects\Shadow-Flip-Angular"
```

### macOS ou Linux

```bash
cd ~/Projects/Shadow-Flip-Angular
```

Confirme se está na pasta correta.

### Windows

```powershell
dir
```

### macOS ou Linux

```bash
ls
```

A pasta deverá conter arquivos ou diretórios semelhantes a:

```text
angular.json
package.json
src/
```

> Execute os comandos do Firebase a partir da raiz do projeto Angular.

---

## 🏗️ Gerar o build

Antes de configurar o diretório público do Hosting, gere o build da aplicação.

Execute:

```bash
ng build --configuration production
```

Também é possível utilizar o script definido no `package.json`:

```bash
npm run build
```

Após o build, o Angular exibirá informações sobre os arquivos gerados.

Uma estrutura possível em Angular 20 é:

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

> **Não configure `hosting.public` com base apenas no nome da pasta exibida no `dist`. Primeiro confirme onde o `index.html` foi realmente gerado.**

---

## 🔎 Localizar o `index.html`

### PowerShell

Execute na raiz do projeto:

```powershell
Get-ChildItem . -Filter index.html -Recurse | Select-Object FullName
```

Exemplo:

```text
C:\Projects\Shadow-Flip-Angular\dist\shadow-flip-angular\browser\index.html
```

Nesse caso, a pasta correta para o Firebase Hosting será:

```text
dist/shadow-flip-angular/browser
```

### macOS ou Linux

```bash
find . -type f -name "index.html"
```

### Correto

```text
dist/shadow-flip-angular/browser
```

### Incorreto

```text
dist/shadow-flip-angular/browser/index.html
```

A propriedade `public` recebe **a pasta**, não o caminho completo do arquivo.

---

## 🔗 Associar o projeto ao Firebase

Execute:

```bash
firebase use --add
```

Selecione o projeto Firebase desejado.

Defina um alias, por exemplo:

```text
default
```

Esse processo cria ou atualiza o arquivo:

```text
.firebaserc
```

Exemplo:

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

Exemplo:

```text
Active Project: shadow-angular
```

---

## 🔥 Inicializar o Firebase Hosting

Execute:

```bash
firebase init hosting
```

Durante o assistente, selecione ou informe:

| Pergunta                             | Resposta                        |
| :----------------------------------- | :------------------------------ |
| Use an existing project?             | Sim                             |
| Projeto Firebase                     | `shadow-angular`                |
| Public directory                     | Pasta que contém o `index.html` |
| Configure as a single-page app?      | Sim                             |
| Set up automatic builds with GitHub? | Opcional                        |

Se o projeto já estiver associado ao Firebase e você utilizar:

```bash
firebase init
```

selecione apenas o recurso:

```text
Hosting: Configure files for Firebase Hosting and optionally set up GitHub Action deploys
```

Não é necessário selecionar Firestore, Functions, Storage ou outros produtos quando o objetivo for apenas publicar o front-end Angular.

> **Atenção:** se você executar `firebase init` novamente e selecionar Hosting, revise o `firebase.json` depois. A inicialização pode alterar a seção `hosting` da configuração existente.

---

## 🔧 Configurar o `firebase.json`

Abra:

```text
firebase.json
```

na raiz do projeto.

O valor de `hosting.public` deve corresponder à pasta que contém diretamente o `index.html`.

### Exemplo com `browser`

Se o build gerar:

```text
dist/shadow-flip-angular/browser/index.html
```

utilize:

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

### Exemplo sem `browser`

Se o build gerar:

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

### Significado das propriedades

| Propriedade   | Finalidade                                |
| :------------ | :---------------------------------------- |
| `public`      | Define a pasta que será publicada         |
| `ignore`      | Impede o envio de arquivos desnecessários |
| `rewrites`    | Define regras de reescrita de URLs        |
| `source`      | Define o padrão de URL correspondente     |
| `destination` | Define o recurso que será entregue        |

---

# 🏗️ Validar o build

Antes do deploy, confirme novamente que o build foi gerado:

```bash
ng build --configuration production
```

Depois, confirme a existência do `index.html`.

### PowerShell

```powershell
Get-ChildItem . -Filter index.html -Recurse | Select-Object FullName
```

Exemplo:

```text
C:\Projects\Shadow-Flip-Angular\dist\shadow-flip-angular\browser\index.html
```

Nesse cenário:

```json
"public": "dist/shadow-flip-angular/browser"
```

> Se `hosting.public` estiver apontando para a pasta errada, o Firebase poderá publicar uma estrutura incorreta ou não encontrar o `index.html`.

---

# 🧪 Testar localmente

Antes do deploy, você pode testar o Hosting localmente.

Execute:

```bash
firebase emulators:start --only hosting
```

O Firebase exibirá uma URL local, normalmente semelhante a:

```text
http://127.0.0.1:5000
```

Valide:

* Página inicial.
* Rotas internas.
* Atualização da página em uma rota profunda.
* Imagens.
* Arquivos da pasta `assets`.
* Responsividade.
* Console do navegador.
* Links externos.
* Integrações com APIs.

Para encerrar o emulador:

```text
Ctrl + C
```

---

<h2 id="fazer-o-deploy">7. 🚀 Fazer o deploy</h2>

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

O comando:

```bash
firebase deploy --only hosting
```

publica somente os recursos relacionados ao Firebase Hosting.

---

<h2 id="proximos-deploys">8. 🔁 Próximos deploys</h2>

Depois que a configuração inicial estiver pronta, os próximos deploys ficam muito mais simples.

Execute:

```bash
ng build --configuration production
firebase deploy --only hosting
```

Ou utilize:

```bash
npm run build
firebase deploy --only hosting
```

### Automatizando pelo `package.json`

Você também pode criar um script:

```json
{
  "scripts": {
    "build:production": "ng build --configuration production",
    "deploy:hosting": "npm run build:production && firebase deploy --only hosting"
  }
}
```

Depois, execute:

```bash
npm run deploy:hosting
```

### Deploy de todos os recursos Firebase

O comando:

```bash
firebase deploy
```

pode publicar outros recursos Firebase configurados no projeto.

Para publicar somente o site, prefira:

```bash
firebase deploy --only hosting
```

---

<h2 id="estrutura-do-projeto">9. 📁 Estrutura do projeto</h2>

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

A pasta `dist/` é gerada automaticamente pelo processo de build.

> **Não edite manualmente os arquivos dentro de `dist/`.**

---

<h2 id="troubleshooting">10. 🛠️ Troubleshooting

## `firebase` não é reconhecido

Instale ou reinstale o Firebase CLI:

```bash
npm install -g firebase-tools
```

Depois, reinicie o terminal e valide:

```bash
firebase --version
```

Se o problema persistir:

```bash
npm prefix -g
```

---

## `Page Not Found`

Verifique:

* Se o build foi executado.
* Se o `index.html` existe.
* Se `hosting.public` aponta para a pasta correta.
* Se o `firebase.json` correto está sendo utilizado.
* Se o deploy foi executado depois da alteração.
* Se o projeto Firebase ativo é o correto.

Localize o arquivo:

```powershell
Get-ChildItem . -Filter index.html -Recurse | Select-Object FullName
```

Se o resultado for:

```text
...\dist\shadow-flip-angular\browser\index.html
```

o `firebase.json` deve utilizar:

```json
"public": "dist/shadow-flip-angular/browser"
```

---

## `Could not detect project root`

Execute os comandos dentro da pasta que contém:

```text
angular.json
package.json
src/
```

---

## Rotas internas retornam 404

Confirme se o `firebase.json` possui:

```json
"rewrites": [
  {
    "source": "**",
    "destination": "/index.html"
  }
]
```

Depois, gere o build novamente:

```bash
ng build --configuration production
```

e faça um novo deploy:

```bash
firebase deploy --only hosting
```

O rewrite permite que o Firebase entregue o `index.html` para URLs que serão interpretadas pelo Angular Router.

---

## Alterações não aparecem no navegador

Tente:

* Atualizar com `Ctrl + F5`.
* Abrir uma janela anônima.
* Limpar o cache do navegador.
* Confirmar se o deploy foi concluído.
* Confirmar a URL do projeto Firebase.
* Verificar se o build foi executado antes do deploy.

---

## Arquivos estáticos não carregam

Confirme:

* Se os arquivos estão dentro de `src/assets/ ou outro caminho de pasta que você utilize`.
* Se o caminho utilizado no código está correto.
* Se os assets estão configurados no `angular.json`.
* Se o caminho respeita letras maiúsculas e minúsculas.
* Se os arquivos foram incluídos no build.

Exemplo:

```html:
  <img src="images/firebase_badge.svg" width="90" alt="Badge do Firebase">
```

* A imagem deve aparecer como no exemplo Abaixo: 

<p align="center">
  <img src="images/firebase_badge.svg" width="120" alt="Badge do Firebase">
</p>

---

<h2 id="boas-praticas">11. 🧠 Boas práticas

## ⚡ Utilize o build de produção

Para publicar, prefira:

```bash
ng build --configuration production
```

O build de produção aplica otimizações adequadas para publicação.

---

## 📁 Não versione `dist`

Adicione ao `.gitignore`:

```gitignore
dist/
.firebase/
```

---

## 🧪 Teste antes de publicar

Utilize o emulador do Hosting:

```bash
firebase emulators:start --only hosting
```

---

## 🔬 Utilize canais de preview

Para criar uma publicação temporária:

```bash
firebase hosting:channel:deploy preview
```

O Firebase fornecerá uma URL de preview que pode ser utilizada para validação antes do deploy em produção.

---

## 🌎 Utilize aliases para ambientes

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

Depois:

```bash
firebase deploy --only hosting
```

Antes de executar um deploy, confirme sempre o projeto ativo:

```bash
firebase use
```

---

## 🔐 Evite publicar arquivos sensíveis

Nunca coloque no diretório público:

* Chaves privadas.
* Arquivos `.env` contendo segredos.
* Credenciais de service account.
* Tokens de acesso.
* Arquivos administrativos do Firebase.

> A configuração do Firebase utilizada no front-end não deve ser confundida com credenciais privadas. Segredos devem permanecer em ambientes protegidos, no back-end ou no pipeline de CI/CD.

---

## Angular client-side × SSR

Este tutorial utiliza o **Firebase Hosting clássico** para uma aplicação Angular client-side/SPA.

Se o projeto utiliza Angular SSR, prerenderização ou uma arquitetura full-stack, o processo de publicação pode ser diferente.

Para aplicações Angular com necessidades de renderização no servidor e integração com GitHub, considere o **Firebase App Hosting**.

---

<h2 id="como-contribuir">12. 🤝 Contribuindo para o Projeto</h2>

Adicione este projeto ao seu "deck" de desenvolvedor!  

| Fase | Ação | Link / Comando |
| :---: | :--- | :--- |
| **01** | **Fork** | [![Fork](https://img.shields.io/badge/-Fazer%20Fork-blue?style=flat-square&logo=github)](https://github.com/Domisnnet/Document-Firebase-Hosting/fork) |
| **02** | **Branch** | `git checkout -b feature/MinhaMelhoria` |
| **03** | **Commit** | `git commit -m 'feat: nova seção de Documentação'` |
| **04** | **Push** | `git push origin feature/MinhaMelhoria` |
| **05** | **PR** | [![Abrir PR](https://img.shields.io/badge/-Abrir%20PR-green?style=flat-square&logo=git)](https://github.com/Domisnnet/Document-Firebase-Hosting/compare) 

### 🐛 Encontrou um problema?

[![Issues Abertas](https://img.shields.io/github/issues/Domisnnet/Document--Firebase--Hosting?style=flat-square&color=red&logo=github)](https://github.com/Domisnnet/Document-Firebase-Hosting/issues)
[![Report Bug](https://img.shields.io/badge/Reportar-Erro-critical?style=flat-square&logo=github)](https://github.com/Domisnnet/Document-Firebase-Hosting/issues/new) 

---

<h2 id="faq">13. 🧠 Perguntas Frequentes</h2>

<details>
<summary><strong>Por que preciso executar o `ng build` antes do deploy? ❓</strong></summary>
<p>🚀 O Firebase Hosting publica arquivos estáticos. O comando <code>ng build --configuration production</code> transforma o projeto Angular em arquivos prontos para publicação dentro da pasta <code>dist/</code>.</p>
</details>

<details>
<summary><strong>Por que o Firebase não encontra minha aplicação? ❓</strong></summary>
<p>🔎 Na maioria dos casos, verifique primeiro o caminho definido em <code>hosting.public</code>. Ele precisa apontar para a pasta que contém diretamente o <code>index.html</code> gerado pelo build.</p>
</details>

<details>
<summary><strong>Por que existe uma pasta `browser` dentro de `dist`? ❓</strong></summary>
<p>📁 Dependendo da configuração e da versão do Angular, os arquivos finais podem ser gerados dentro de <code>dist/nome-do-projeto/browser</code>. O caminho correto deve sempre ser confirmado localizando o <code>index.html</code>.</p>
</details>

<details>
<summary><strong>Para que serve o `rewrites`? ❓</strong></summary>
<p>🔀 O <code>rewrites</code> permite que rotas de uma SPA Angular sejam encaminhadas para o <code>index.html</code>, permitindo que o Angular Router processe a URL.</p>
</details>

<details>
<summary><strong>Posso executar apenas `firebase deploy`? ❓</strong></summary>
<p>🚀 Sim. Porém, quando o objetivo é publicar somente o front-end no Firebase Hosting, <code>firebase deploy --only hosting</code> é mais específico e evita publicar outros recursos Firebase configurados no projeto.</p>
</details>

<details>
<summary><strong>Preciso configurar Firestore ou Functions para utilizar Firebase Hosting? ❓</strong></summary>
<p>🔥 Não. Se o objetivo for somente hospedar uma aplicação Angular, o Firebase Hosting é suficiente. Firestore, Functions, Storage e outros serviços são opcionais.</p>
</details>

---

<h2 id="checklist-final">14. ✅ Checklist final</h2>

Antes do primeiro deploy:

* [ ] Angular 20 configurado.
* [ ] Node.js compatível com Angular 20.
* [ ] npm funcionando.
* [ ] Angular CLI disponível.
* [ ] Firebase CLI instalado.
* [ ] Login realizado com `firebase login`.
* [ ] Projeto Firebase criado.
* [ ] Projeto Firebase selecionado com `firebase use`.
* [ ] Projeto Angular compilando localmente.
* [ ] Build de produção gerado.
* [ ] `index.html` localizado.
* [ ] `hosting.public` apontando para a pasta correta.
* [ ] `rewrites` configurado para o Angular Router.
* [ ] Emulador local validado, quando necessário.
* [ ] Deploy executado com `firebase deploy --only hosting`.
* [ ] URL de produção testada.
* [ ] Rotas internas testadas.
* [ ] Assets e imagens validados.
* [ ] Console do navegador revisado.

---

<h2 id="referencias-oficiais">15. 📚 Referências oficiais:

&nbsp;

[![Firebase Hosting](https://img.shields.io/badge/Firebase%20Hosting-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/docs/hosting)

[![Firebase CLI - Documentação Oficial](https://img.shields.io/badge/Firebase%20CLI%20-%20Documentação%20Oficial-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/docs/cli)

[![Get Started with Firebase Hosting](https://img.shields.io/badge/Get%20Started%20with%20Firebase%20Hosting-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/docs/hosting/quickstart)

[![Configuração Completa do Firebase Hosting](https://img.shields.io/badge/Configuração%20Completa%20do%20Firebase%20Hosting-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/docs/hosting/full-config)

[![Integração Angular + Firebase Hosting](https://img.shields.io/badge/Integração%20Angular%20%2B%20Firebase%20Hosting-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://firebase.google.com/docs/hosting/frameworks/angular)

[![Firebase App Hosting](https://img.shields.io/badge/Firebase%20App%20Hosting-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/docs/app-hosting)

[![Angular](https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.dev/)

[![Compatibilidade de Versões do Angular](https://img.shields.io/badge/Compatibilidade%20de%20Versões%20do%20Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.dev/reference/versions)

[![Angular CLI](https://img.shields.io/badge/Angular%20CLI-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.dev/tools/cli)

[![Build de Aplicações Angular](https://img.shields.io/badge/Build%20de%20Aplicações%20Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.dev/tools/cli/build)

---

<h2 id="regra-mais-importante">16. 📌 Regra mais importante

O valor de: `hosting.public` no `firebase.json` deve apontar para a pasta que contém diretamente o `index.html` gerado pelo build.

Exemplo:

```text
dist/
└── shadow-flip-angular/
    └── browser/
        └── index.html
```

Então:

```json
{
  "hosting": {
    "public": "dist/shadow-flip-angular/browser"
  }
}
```

### Se aparecer `Page Not Found`

Verifique, nesta ordem:

1. O resultado do `ng build`.
2. A localização real do `index.html`.
3. O valor de `hosting.public`.
4. A configuração de `rewrites`.
5. O projeto Firebase ativo.
6. Se o deploy foi executado novamente após as alterações.

### Comando útil no Windows

```powershell
Get-ChildItem . -Filter index.html -Recurse | Select-Object FullName
```

Esse comando permite descobrir rapidamente qual deve ser o valor de:

```json
"hosting": {
  "public": "..."
}
```

---

<h2 id="créditos">17. 📝 Créditos & Reconhecimentos</h2>

| Atribuição       | Responsável / Recurso | Descrição                                                      |
| :--------------- | :-------------------- | :------------------------------------------------------------- |
| **Framework**    | **Angular**           | Framework utilizado para desenvolvimento da aplicação.         |
| **Hosting**      | **Google Firebase**   | Infraestrutura utilizada para hospedagem da aplicação.         |
| **CLI**          | **Firebase CLI**      | Ferramenta utilizada para autenticação, configuração e deploy. |
| **Documentação** | **DomisDev**          | Organização e documentação do processo de deploy.              |

---

<h2 id="licença">18. 📄 Licença</h2>

Este tutorial pode ser utilizado como material de estudo e referência para projetos Angular publicados utilizando Firebase Hosting. Só não se esqueça de dar os **Devidos Créditos** ao **Desenvolvedor do Projeto.**

Este projeto está sob a [![Licença MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/Domisnnet/Document-Firebase-Hosting/blob/main/license)

---

<h2 id="perfil-do-github">19. 👨‍💻 Perfil do GitHub</h2>

Para conhecer os Projetos do **Desenvolvedor : DomisDev** , acesse o Repositório Oficial ,no ícone abaixo:

<a href="https://github.com/Domisnnet"> 
  <img src="images/DomisDev.png" width="90" style="border-radius: 50%" alt="DomisDev GitHub"> 
</a>

***

<p align="center">
<a href="#1--deploy-de-angular-20-no-firebase-hosting">
  <img src="https://img.shields.io/badge/⬆️%20Voltar%20ao%20topo-1A73E8?style=for-the-badge" alt="Voltar ao topo">
</a>
</p>