# Central de Qualidade e Operações · publicação no Firebase

Este pacote tem tudo o que é preciso para colocar o sistema no ar:

- `public/index.html`: o sistema completo, num arquivo só
- `firestore.rules`: as regras de segurança do banco de dados
- `firebase.json` e `.firebaserc`: a configuração da publicação

O plano gratuito do Firebase (Spark) é suficiente. Não é preciso cadastrar cartão.

---

## 1. Criar o projeto

1. Acesse https://console.firebase.google.com e clique em **Criar um projeto**.
2. Dê um nome (por exemplo, `central-igreen`). O Google Analytics pode ficar desativado.
3. Anote o **ID do projeto**, que aparece embaixo do nome (por exemplo, `central-igreen-a1b2c`).

## 2. Ativar o login por usuário e senha

1. No menu lateral, abra **Criação › Authentication** e clique em **Vamos começar**.
2. Em **Método de login**, ative **E-mail/senha** e salve.
3. Na aba **Usuários**, clique em **Adicionar usuário** e cadastre:
   - E-mail: `processos@central-igreen.app`
   - Senha: `processos2026`

O sistema transforma o usuário digitado na tela de login em e-mail: quem digita `processos` entra com `processos@central-igreen.app`. Esse e-mail não precisa existir de verdade.

## 3. Criar o banco de dados

1. Abra **Criação › Firestore Database** e clique em **Criar banco de dados**.
2. Escolha o local **southamerica-east1 (São Paulo)** e o **modo de produção**.
3. Quando terminar, abra a aba **Regras**, apague o conteúdo, cole todo o conteúdo do arquivo `firestore.rules` e clique em **Publicar**.

## 4. Ligar o sistema ao projeto

1. Abra **Configurações do projeto** (ícone de engrenagem › Configurações do projeto).
2. Em **Seus apps**, clique no ícone **Web** (`</>`), dê um apelido (por exemplo, `central`) e registre o app. Não marque o Firebase Hosting nessa tela.
3. Copie o objeto `firebaseConfig` que aparece. Ele tem este formato:

   ```js
   {
     apiKey: "...",
     authDomain: "....firebaseapp.com",
     projectId: "...",
     storageBucket: "...",
     messagingSenderId: "...",
     appId: "..."
   }
   ```

4. Abra `public/index.html` num editor de texto (Bloco de Notas ou VS Code) e procure por `const FIREBASE_CONFIG = null;`, logo no início do código.
5. Troque o `null` pelo objeto copiado, mantendo o ponto e vírgula no final:

   ```js
   const FIREBASE_CONFIG = { apiKey: "...", authDomain: "...", projectId: "...", storageBucket: "...", messagingSenderId: "...", appId: "..." };
   ```

6. Salve o arquivo.
7. Abra `.firebaserc` e troque `COLE-AQUI-O-ID-DO-PROJETO` pelo ID anotado no passo 1.

## 5. Publicar

É preciso ter o Node.js instalado (https://nodejs.org, versão LTS). Depois, abra o terminal (no Windows, o Prompt de Comando) na pasta deste pacote e rode, uma linha por vez:

```
npm install -g firebase-tools
firebase login
firebase deploy
```

O `firebase login` abre o navegador para você entrar com a conta Google dona do projeto. No final do `firebase deploy`, o terminal mostra o endereço do sistema, no formato `https://SEU-PROJETO.web.app`. O comando publica o site e também as regras do banco.

Para publicar uma nova versão no futuro, basta trocar o `public/index.html` e rodar `firebase deploy` de novo.

## 6. Primeiro acesso

1. Abra o endereço do sistema e entre com `processos` / `processos2026`. No primeiro acesso, esse usuário vira administrador automaticamente. Isso acontece uma única vez.
2. Em **Configurações › Usuários**, cadastre o seu próprio usuário como administrador.
3. Se quiser proteger as Configurações do time, abra o card do `processos` e desmarque **Administrador**. Assim o time continua entrando com `processos`, mas sem acesso total.
4. Em **Configurações › Colaboradores**, cadastre o time. Os colaboradores ativos aparecem automaticamente na Palavra da Semana de toda semana.

## 7. Trazer os dados do sistema atual

1. No sistema atual, abra **Configurações › Backup › Baixar backup**.
2. No sistema novo, entre como administrador e abra **Configurações › Backup › Escolher arquivo de backup**.

As semanas e os setores são importados. Metas, alertas, prazos e as palavras antigas da Palavra da Semana são descartados, porque essas funções foram removidas ou mudaram de formato. Os colaboradores são cadastrados do zero.

---

## Limites do plano gratuito, que valem para este sistema

- **Senha de outra pessoa:** sem um servidor próprio, o Firebase não permite que um administrador troque a senha de outro usuário pelo sistema. Cada pessoa troca a própria senha em **Configurações › Usuários** (precisa ser administradora) ou você redefine pelo Console, em **Authentication › Usuários**.
- **Excluir usuário:** excluir no sistema remove o acesso na hora. A conta continua listada no Authentication; para reutilizar o mesmo nome de usuário, exclua a conta lá também.
- **Exportar Excel:** precisa de internet, porque a biblioteca do Excel é carregada na hora.

## Testar sem Firebase

Com `FIREBASE_CONFIG = null`, o `index.html` funciona aberto direto no navegador, em modo demonstração: os dados ficam só naquele navegador. Usuário `processos`, senha `processos2026`.
