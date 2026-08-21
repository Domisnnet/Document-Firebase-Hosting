📘 Deploy de Angular no Firebase Hosting
Documentação técnica premium para publicar uma aplicação Angular no Firebase Hosting.
Nível: intermediário → avançado
Última atualização: agosto de 2026
📑 Tabela de conteúdos
Objetivo
Como funciona
Pré-requisitos
Exemplo utilizado
Passo a passo
1. Instalar o Firebase CLI
2. Fazer login no Firebase
3. Entrar na pasta do projeto
4. Associar o projeto Angular ao Firebase
5. Inicializar o Firebase Hosting
6. Gerar o build de produção
7. Localizar o index.html
8. Configurar o firebasejson
9. Testar localmente
10. Fazer o deploy
Fluxo dos próximos deploys
Estrutura do projeto
Troubleshooting
Boas práticas
Checklist final
Referências oficiais
🎯 Objetivo
Este guia mostra como conectar um projeto Angular existente a um projeto já criado no Firebase e publicá-lo utilizando o Firebase Hosting.
O processo inclui:
Instalação e autenticação do Firebase CLI.
Associação do projeto Angular ao projeto Firebase.
Configuração do Firebase Hosting.
Geração do build de produção.
Configuração de rotas para aplicações Single-Page Application.
Deploy e validação da aplicação publicada.
🔄 Como funciona
O Angular não publica diretamente os arquivos presentes em src/. Primeiro, o projeto precisa ser compilado com ng build.
Esse processo gera uma versão otimizada da aplicação dentro da pasta dist/. O Firebase Hosting publica somente a pasta indicada na propriedade public do arquivo firebase.json.
Código-fonte Angular
        ↓
ng build --configuration production
        ↓
Pasta dist/
        ↓
Firebase Hosting
        ↓
Aplicação publicada
Regra de ouro: o valor de hosting.public precisa apontar para a pasta que realmente contém o index.html gerado pelo build.
✅ Pré-requisitos
Valide as ferramentas instaladas:
node --version
npm --version
ng version
🧪 Exemplo utilizado
Neste documento, serão utilizados os seguintes nomes fictícios:
Substitua esses valores pelos nomes reais do seu projeto.
🚀 Passo a passo
1. Instalar o Firebase CLI
Abra o terminal integrado do VS Code e execute:
npm install -g firebase-tools
Verifique se a instalação foi concluída:
firebase --version
Exemplo de saída:
15.27.0
Se o comando firebase não for reconhecido, reinicie o terminal do VS Code ou verifique se o diretório global do npm está configurado no PATH.
2. Fazer login no Firebase
Execute:
firebase login
O navegador será aberto para autenticação com sua conta Google.
Depois do login, liste os projetos disponíveis:
firebase projects:list
O projeto criado no Firebase Console deverá aparecer na lista.
Exemplo:
Project Display Name    Project ID
shadow-angular          shadow-angular
3. Entrar na pasta do projeto
Navegue até a pasta raiz do projeto Angular:
cd caminho/do/seu/projeto
Exemplo no Windows:
cd "C:ProjectsShadow-Flip-Angular"
Exemplo no macOS ou Linux:
cd ~/Projects/Shadow-Flip-Angular
Confirme se está na pasta correta:
dir
No macOS ou Linux:
ls
A pasta deverá conter arquivos como:
angular.json
package.json
src/
4. Associar o projeto Angular ao Firebase
Execute:
firebase use --add
Selecione o projeto Firebase desejado e defina um alias, por exemplo:
default
Esse processo cria ou atualiza o arquivo .firebaserc:
{
  "projects": {
    "default": "shadow-angular"
  }
}
Confirme o projeto atualmente selecionado:
firebase use
A saída deverá indicar o projeto ativo:
Active Project: shadow-angular
5. Inicializar o Firebase Hosting
Execute:
firebase init hosting
Durante o assistente, selecione ou informe:
Se preferir inicializar todos os recursos manualmente, execute:
firebase init
Nesse caso, selecione apenas:
Hosting: Configure files for Firebase Hosting and optionally set up GitHub Action deploys
Não é necessário selecionar Firestore, Functions, Storage ou outros produtos quando o objetivo for apenas publicar o front-end Angular.
6. Gerar o build de produção
Execute:
ng build --configuration production
Também é possível utilizar:
npm run build
O Angular exibirá o diretório de saída do build.
Em projetos recentes, a estrutura pode ser semelhante a:
dist/
└── shadow-flip-angular/
    └── browser/
        ├── index.html
        ├── main-XXXXXXXX.js
        ├── styles-XXXXXXXX.css
        └── assets/
Em outros projetos, o index.html pode estar diretamente em:
dist/shadow-flip-angular/
Não presuma o caminho da pasta public. Sempre confirme onde o build realmente gerou o index.html.
7. Localizar o index.html
PowerShell
Execute na raiz do projeto:
Get-ChildItem . -Filter index.html -Recurse | Select-Object FullName
Exemplo de resultado:
C:ProjectsShadow-Flip-Angulardistshadow-flip-angular\browserindex.html
Nesse caso, a pasta correta para o Firebase Hosting será:
dist/shadow-flip-angular/browser
macOS ou Linux
find . -type f -name "index.html"
O caminho configurado em public deve ser a pasta do arquivo, e não o caminho completo do arquivo.
Correto:
dist/shadow-flip-angular/browser
Incorreto:
dist/shadow-flip-angular/browser/index.html
8. Configurar o firebase.json
Abra o arquivo firebase.json na raiz do projeto e configure-o conforme o local real do index.html.
Exemplo com a pasta browser
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
Exemplo sem a pasta browser
Se o arquivo estiver em:
dist/shadow-flip-angular/index.html
utilize:
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
Significado das propriedades
Por que usar rewrites?
O Angular utiliza roteamento no navegador. Rotas como estas podem não existir como arquivos físicos:
/
 /projects
 /about
 /contact
A regra abaixo faz com que o Firebase entregue o index.html para essas rotas:
"rewrites": [
  {
    "source": "**",
    "destination": "/index.html"
  }
]
Depois disso, o Angular Router interpreta a URL e renderiza o componente correspondente.
A configuração acima é indicada para aplicações Angular client-side. Projetos com SSR possuem requisitos diferentes e devem ser configurados conforme a arquitetura utilizada.
9. Testar localmente
Antes do deploy, gere o build:
ng build --configuration production
Depois, inicie o emulador do Hosting:
firebase emulators:start --only hosting
O Firebase exibirá uma URL local, normalmente semelhante a:
http://127.0.0.1:5000
Valide:
Página inicial.
Rotas internas.
Atualização da página em uma rota profunda.
Imagens e arquivos da pasta assets.
Responsividade.
Console do navegador.
Links externos e APIs.
Para encerrar o emulador:
Ctrl + C
10. Fazer o deploy
Depois de validar o build e o firebase.json, execute:
firebase deploy --only hosting
Ao final, será exibida uma mensagem semelhante a:
Deploy complete!

Hosting URL: https://shadow-angular.web.app
Abra a URL no navegador e valide a aplicação em produção.
🔁 Fluxo dos próximos deploys
Depois que a configuração inicial estiver pronta, o processo será:
ng build --configuration production
firebase deploy --only hosting
Você também pode criar um script no package.json:
{
  "scripts": {
    "build:production": "ng build --configuration production",
    "deploy:hosting": "npm run build:production && firebase deploy --only hosting"
  }
}
Depois, execute apenas:
npm run deploy:hosting
O comando firebase deploy sem --only hosting poderá publicar outros recursos Firebase configurados no projeto. Para publicar apenas o site, prefira firebase deploy --only hosting.
📁 Estrutura do projeto
Uma estrutura típica poderá ser:
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
A pasta dist/ normalmente é gerada automaticamente e não deve ser editada manualmente.
🛠️ Troubleshooting
firebase não é reconhecido
Tente:
npm install -g firebase-tools
Depois, reinicie o terminal e valide:
firebase --version
Se o problema persistir, verifique o diretório global do npm:
npm prefix -g
Erro Page Not Found
Verifique:
Se o build foi executado.
Se o index.html existe.
Se hosting.public aponta para a pasta correta.
Se o deploy foi executado depois da alteração.
Se não existe um segundo firebase.json em outra pasta.
Comando para localizar o arquivo:
Get-ChildItem . -Filter index.html -Recurse | Select-Object FullName
Erro Could not detect project root
Execute os comandos dentro da pasta que contém:
angular.json
package.json
src/
Rotas internas retornam 404
Confirme se o firebase.json possui:
"rewrites": [
  {
    "source": "**",
    "destination": "/index.html"
  }
]
Depois, gere o build novamente e faça um novo deploy:
ng build --configuration production
firebase deploy --only hosting
Alterações não aparecem no navegador
Tente:
Atualizar a página com Ctrl + F5.
Abrir uma janela anônima.
Limpar o cache do navegador.
Confirmar se o deploy foi concluído.
Verificar a URL correta do projeto Firebase.
Arquivos estáticos não carregam
Confirme:
Se os arquivos estão dentro de src/assets.
Se o caminho utilizado no código está correto.
Se os assets foram incluídos no angular.json.
Se o caminho é compatível com letras maiúsculas e minúsculas.
Exemplo:
<img src="assets/images/logo.svg" alt="Logo">
🧠 Boas práticas
Use build de produção
Para publicar, prefira:
ng build --configuration production
Esse perfil normalmente aplica otimizações como:
Minificação.
Tree-shaking.
Otimização de bundles.
Remoção de código não utilizado.
Geração de arquivos prontos para produção.
Não versionar a pasta dist
Adicione ao .gitignore:
dist/
.firebase/
Teste antes de publicar
Use o emulador:
firebase emulators:start --only hosting
Use canais de preview
Para gerar uma publicação temporária:
firebase hosting:channel:deploy preview
O Firebase fornecerá uma URL de preview que poderá ser compartilhada para validação.
Use aliases para ambientes
Exemplo de .firebaserc:
{
  "projects": {
    "development": "shadow-angular-dev",
    "staging": "shadow-angular-staging",
    "production": "shadow-angular"
  }
}
Para selecionar um ambiente:
firebase use production
Depois:
firebase deploy --only hosting
Antes de executar um deploy, confirme sempre o projeto ativo com firebase use.
Evite publicar arquivos sensíveis
Nunca coloque no diretório público:
Chaves privadas.
Arquivos .env com segredos.
Credenciais de service account.
Tokens de acesso.
Arquivos administrativos do Firebase.
✅ Checklist final
Antes do primeiro deploy:
�Node.js instalado.
�npm funcionando.
�Angular CLI disponível.
�Firebase CLI instalado.
�Login realizado com firebase login.
�Projeto Firebase selecionado.
�Projeto Angular compilando localmente.
�Build de produção gerado.
�index.html localizado.
�hosting.public apontando para a pasta correta.
�rewrites configurado para o Angular Router.
�Emulador local validado, quando necessário.
�Deploy executado com firebase deploy --only hosting.
�URL de produção testada.
📚 Referências oficiais
Firebase Hosting
Firebase CLI
Integração entre Angular e Firebase Hosting
Configuração completa do firebase.json
Documentação do Angular CLI
📌 Regra mais importante
O valor de public no firebase.json deve apontar para a pasta que contém diretamente o index.html gerado pelo build.
Exemplo:
{
  "hosting": {
    "public": "dist/shadow-flip-angular/browser"
  }
}
Se o Firebase exibir uma página Page Not Found ou informar que não encontrou o index.html, verifique primeiro:
1. O resultado do ng build.
2. A localização real do index.html.
3. O valor de hosting.public.
4. Se o deploy foi executado novamente.
Feito com ❤️ para projetos Angular e Firebase
⬆️ Voltar ao topo
