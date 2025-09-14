
# 🗺️ Mapa de Quadras - Alexânia

Aplicação web que exibe um mapa interativo de quadras em **Alexânia (GO)** usando **Leaflet.js** e **OpenStreetMap**, com autenticação e controle de permissões via **Firebase**.

## 🚀 Funcionalidades

* Visualização de todas as quadras no mapa (sem necessidade de login).
* Login de usuários com autenticação Firebase.
* Permissão diferenciada por papel:

  * 🔒 **viewer** → apenas visualiza as quadras.
  * ✏️ **editor** → pode clicar nas quadras e alternar a cor (verde/cinza).
* As cores são salvas no **Firestore** e recarregam ao abrir a página.
* Atualizações em tempo real: se um editor altera uma quadra, todos os usuários veem a mudança instantaneamente.

## 🛠️ Tecnologias

* [Leaflet.js](https://leafletjs.com/) – mapas interativos.
* [OpenStreetMap](https://www.openstreetmap.org/) – tiles do mapa.
* [Firebase](https://firebase.google.com/) – autenticação e banco de dados (Firestore).
* HTML, CSS, JavaScript.

## 📂 Estrutura do Projeto

```
📦 mapa-alexania
 ┣ 📜 index.html   # Página principal (mapa e autenticação)
 ┗ 📜 README.md    # Documentação
```

## ⚙️ Configuração

### 1. Clonar o repositório

```bash
git clone https://github.com/seu-usuario/mapa-alexania.git
cd mapa-alexania
```

### 2. Configurar Firebase

* Crie um projeto no [Firebase Console](https://console.firebase.google.com/).
* Habilite **Authentication (Email/Senha)**.
* Crie uma coleção no **Firestore** chamada `usuarios`.
* Dentro dela, crie um documento para cada usuário (o **UID** deve ser igual ao UID do usuário em *Authentication*).

  * Campo: `role` → valor: `"viewer"` ou `"editor"`.

Exemplo no Firestore:

```
usuarios (coleção)
   └── wA8Jk3VhR2nZQ4y... (UID do usuário)
         └── role : "editor"
```

### 3. Inserir Configuração do Firebase

No `index.html`, substitua a configuração pelo seu projeto:

```js
const firebaseConfig = {
  apiKey: "SUA_API_KEY",
  authDomain: "SEU_PROJETO.firebaseapp.com",
  projectId: "SEU_PROJETO",
  storageBucket: "SEU_PROJETO.appspot.com",
  messagingSenderId: "ID_REMETENTE",
  appId: "SUA_APP_ID"
};
```

### 4. Rodar o projeto

Basta abrir o `index.html` no navegador (não precisa de servidor).

Se preferir rodar com servidor local:

```bash
npx serve .
```

## 🔑 Regras de Segurança do Firestore (exemplo)

Recomenda-se configurar as regras assim:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Quadras
    match /quadras/{id} {
      allow read: if true; // todos podem visualizar
      allow write: if request.auth != null 
                   && get(/databases/$(database)/documents/usuarios/$(request.auth.uid)).data.role == "editor";
    }

    // Usuários
    match /usuarios/{userId} {
      allow read: if request.auth != null;
      allow write: if false; // apenas admins via console
    }
  }
}
```

## 📸 Demonstração

🔲 Cinza = quadra não marcada
🟩 Verde = quadra marcada

## 👨‍💻 Autor

Emanuel Trindade

---
