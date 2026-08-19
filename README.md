TUTORIAL BÁSICO Conectando um projeto Angular ao Firebase Hosting

Este tutorial mostra como conectar um projeto Angular existente no VS Code a um projeto já criado no Firebase e publicá-lo usando Firebase Hosting.

##1. PRÉ-REQUISITOS
Você precisa ter:
-   Node.js instalado
-   npm instalado
-   Angular CLI/projeto Angular
-   Uma conta Google/Firebase
-   Um projeto criado no Firebase Console
-   VS Code ou outro terminal

Exemplo:
Projeto Angular: Shadow-Flip-Angular
Projeto Firebase: shadow-angular

---

##2. INSTALAR O FIREBASE CLI

Abra o terminal do VS Code e execute:
npm install -g firebase-tools
Verifique a instalação:
firebase –version
Exemplo:
15.27.0

---

##3. FAZER LOGIN NO FIREBASE

Execute:
firebase login
O navegador será aberto para autenticação. Depois, verifique os projetos disponíveis:
firebase projects:list
O projeto criado no Firebase deverá aparecer na lista.
Exemplo:
Project Display Name Project ID shadow-angular shadow-angular

---

##4. ENTRAR NA PASTA DO PROJETO ANGULAR

No terminal:
```
cd caminho/do/seu/projeto
Exemplo:
cd C:-Flip-Angular
```
---

##5. ASSOCIAR O PROJETO ANGULAR AO FIREBASE

Execute:
firebase use –add
Selecione o projeto Firebase desejado.
Exemplo:
shadow-angular
Quando for solicitado um alias, pode usar:
default
Isso cria ou atualiza o arquivo:
.firebaserc
Exemplo de conteúdo:
{ “projects”: { “default”: “shadow-angular” } }
Para confirmar a associação:
firebase use
O projeto deverá aparecer como projeto ativo.

---

##6. CONFIGURAR O FIREBASE HOSTING

Execute:
firebase init
Na lista de recursos, selecione:
Hosting: Set up deployments for static web apps. Use a tecla Space para selecionar a opção e Enter para confirmar. Não é necessário selecionar Firestore, Functions, Storage ou outros recursos se o objetivo for apenas publicar o projeto Angular.

---

##7. GERAR O BUILD DO ANGULAR

Execute:
```
ng build
O Angular mostrará o local onde o build foi gerado.
Exemplo:
Output location: dist/shadow-flip-angular
ATENÇÃO:
Em versões recentes do Angular, o index.html pode estar dentro de uma
subpasta chamada “browser”.
Exemplo:
dist/ └── shadow-flip-angular/ └── browser/ ├── index.html ├──
main-XXXXXXXX.js ├── styles-XXXXXXXX.css └── …
```
---

##8. VERIFICAR ONDE ESTÁ O index.html

No PowerShell, execute:
Get-ChildItem .-Filter index.html -Recurse | Select-Object FullName
Exemplo de resultado:
C:-Flip-Angular-flip-angular.html
O caminho encontrado é importante porque o Firebase Hosting precisa
publicar exatamente a pasta que contém o index.html.

---

##9. CONFIGURAR O : firebase.json

O arquivo firebase.json deve apontar para a pasta que contém o index.html.
Exemplo para o projeto Shadow-Flip-Angular:
{ “hosting”: { “public”: “dist/shadow-flip-angular/browser”, “ignore”: [
“firebase.json”, “**/.*“,”/node_modules/” ], “rewrites”: [ { “source”:
“**“,”destination”: “/index.html” } ] } }
IMPORTANTE:
Se o index.html estiver diretamente em:
dist/shadow-flip-angular/
use:
“public”: “dist/shadow-flip-angular”
Se estiver em:
dist/shadow-flip-angular/browser/
use:
“public”: “dist/shadow-flip-angular/browser”

---

##10. POR QUE USAR REWRITES?

Para aplicações Angular que utilizam rotas, o Firebase precisa encaminhar as URLs para o index.html para que o Angular Router possa assumir o controle.
Exemplo:
/ /projects /about /contact
A configuração:
“rewrites”: [ { “source”: “**“,”destination”: “/index.html” }]
faz com que as rotas da aplicação sejam entregues ao Angular.

---

##11. FAZER O DEPLOY

Depois de gerar o build e configurar o firebase.json:
firebase deploy –only hosting
No final, deverá aparecer algo semelhante a:
Deploy complete!
Hosting URL: https://shadow-angular.web.app
Abra a Hosting URL no navegador para acessar a aplicação publicada.

---

##12. FLUXO PARA OS PRÓXIMOS DEPLOYS

Depois que a configuração estiver pronta, sempre que fizer alterações no
Angular:
1.  Gere o build:
ng build
2.  Publique:
firebase deploy –only hosting
Pronto.
Também é possível usar:
firebase deploy
Esse comando publica todos os recursos Firebase configurados no projeto.

---

##13. ESTRUTURA BÁSICA DO PROJETO

Shadow-Flip-Angular/ │ ├── dist/ │ └── shadow-flip-angular/ │ └──
browser/ │ ├── index.html │ ├── main-XXXXXXXX.js │ └──
styles-XXXXXXXX.css │ ├── src/ │ ├── app/ │ ├── assets/ │ ├── index.html
│ ├── main.ts │ └── styles.scss │ ├── .firebaserc ├── firebase.json ├──
angular.json ├── package.json └── tsconfig.json

---

##14. RESUMO RÁPIDO

Se o projeto Firebase já estiver criado:
1.  Instalar o Firebase CLI:
npm install -g firebase-tools
2.  Fazer login:
firebase login
3.  Entrar na pasta do Angular:
cd meu-projeto
4.  Associar ao Firebase:
firebase use –add
5.  Configurar o Hosting:
firebase init
6.  Gerar o build:
ng build
7.  Verificar onde está o index.html:
Get-ChildItem .-Filter index.html -Recurse | Select-Object FullName
8.  Configurar o public do firebase.json para a pasta que contém o
    index.html.
9.  Publicar:
firebase deploy –only hosting

---

##REGRA MAIS IMPORTANTE

O valor de “public” no firebase.json deve apontar para a pasta que realmente contém o index.html gerado pelo build do Angular.
Exemplo:
“public”: “dist/shadow-flip-angular/browser”
Se o Firebase mostrar:
Page Not Found e informar que não encontrou index.html, verifique primeiro o caminho configurado em “public”.
