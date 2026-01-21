# 🔥 GUIDE DE CONFIGURATION - Geek'Zone Launcher

## 📋 CE QUE VOUS DEVEZ FAIRE

Vous avez maintenant 3 fichiers :
1. **index.html** - Page d'accueil (avec bouton "Mon Compte")
2. **login.html** - Page de connexion/inscription
3. **profile.html** - Page de profil utilisateur

---

## 🔥 ÉTAPE 1 : CONFIGURER FIREBASE (Base de données + Auth)

### 1️⃣ Créer un projet Firebase

1. Allez sur : https://console.firebase.google.com/
2. Cliquez sur **"Ajouter un projet"**
3. Nom du projet : `geekzone-launcher` (ou ce que vous voulez)
4. Activez Google Analytics si vous voulez (optionnel)
5. Cliquez sur **"Créer le projet"**

### 2️⃣ Activer l'authentification Email/Password

1. Dans votre projet Firebase, allez dans **"Authentication"** (menu gauche)
2. Cliquez sur **"Commencer"**
3. Dans l'onglet **"Sign-in method"** :
   - Cliquez sur **"Email/Password"**
   - Activez le bouton **"Activer"**
   - Cliquez sur **"Enregistrer"**

### 3️⃣ Créer Firestore Database

1. Dans le menu gauche, allez dans **"Firestore Database"**
2. Cliquez sur **"Créer une base de données"**
3. Choisissez **"Démarrer en mode test"** (pour commencer)
4. Sélectionnez une région proche de vous (ex: `europe-west`)
5. Cliquez sur **"Activer"**

### 4️⃣ Récupérer les clés Firebase

1. Dans **"Paramètres du projet"** (⚙️ en haut à gauche)
2. Scrollez jusqu'à **"Vos applications"**
3. Cliquez sur l'icône **Web** `</>`
4. Donnez un nom : `Geek'Zone Web`
5. Copiez le code qui apparaît (firebaseConfig)

**Exemple de ce que vous allez recevoir :**
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "geekzone-launcher.firebaseapp.com",
  projectId: "geekzone-launcher",
  storageBucket: "geekzone-launcher.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdefghijklmnop"
};
```

### 5️⃣ Remplacer dans vos fichiers

**Dans `login.html` ET `profile.html`**, remplacez :
```javascript
const firebaseConfig = {
    apiKey: "VOTRE_API_KEY",  // ← Remplacez ces valeurs
    authDomain: "VOTRE_PROJECT_ID.firebaseapp.com",
    projectId: "VOTRE_PROJECT_ID",
    storageBucket: "VOTRE_PROJECT_ID.appspot.com",
    messagingSenderId: "VOTRE_SENDER_ID",
    appId: "VOTRE_APP_ID"
};
```

Par vos vraies clés copiées depuis Firebase !

---

## 📧 ÉTAPE 2 : CONFIGURER EMAILJS (Envoi d'emails)

### 1️⃣ Créer un compte EmailJS

1. Allez sur : https://www.emailjs.com/
2. Cliquez sur **"Sign Up"** (inscription gratuite)
3. Créez votre compte (email + mot de passe)
4. Confirmez votre email

### 2️⃣ Ajouter un service email

1. Dans le dashboard EmailJS, allez dans **"Email Services"**
2. Cliquez sur **"Add New Service"**
3. Choisissez votre fournisseur email :
   - **Gmail** (recommandé pour commencer)
   - Ou Outlook, Yahoo, etc.
4. Connectez votre compte Gmail
5. Notez le **Service ID** (ex: `service_abc123`)

### 3️⃣ Créer un template d'email

1. Allez dans **"Email Templates"**
2. Cliquez sur **"Create New Template"**
3. Donnez-lui un nom : `Bienvenue Geek'Zone`
4. Template de base :

**Sujet :**
```
Bienvenue sur Geek'Zone Launcher !
```

**Contenu :**
```
Salut {{to_name}} ! 🎮

Ton compte Geek'Zone Launcher a été créé avec succès !

Email : {{to_email}}

{{message}}

À très vite sur Geek'Zone !

---
Geek'Zone Launcher
Le launcher gaming ultime
```

5. Sauvegardez et notez le **Template ID** (ex: `template_xyz789`)

### 4️⃣ Récupérer votre Public Key

1. Allez dans **"Account"** → **"General"**
2. Copiez votre **Public Key** (ex: `AbCdEfGhIjKlMnOp`)

### 5️⃣ Remplacer dans vos fichiers

**Dans `login.html` ET `profile.html`**, remplacez :

```javascript
// EmailJS init
emailjs.init("VOTRE_PUBLIC_KEY_EMAILJS");  // ← Votre Public Key

// Dans la fonction sendWelcomeEmail
await emailjs.send(
    'VOTRE_SERVICE_ID',   // ← Votre Service ID
    'VOTRE_TEMPLATE_ID',  // ← Votre Template ID
    templateParams
);
```

---

## 🚀 ÉTAPE 3 : DÉPLOYER SUR VERCEL

### 1️⃣ Préparer les fichiers

Vous devez avoir ces 3 fichiers :
- `index.html` (page d'accueil)
- `login.html` (connexion/inscription)
- `profile.html` (profil utilisateur)

### 2️⃣ Upload sur GitHub

1. Allez sur votre dépôt GitHub
2. Uploadez les 3 fichiers HTML
3. Commit + Push

### 3️⃣ Vercel déploiera automatiquement !

---

## 📱 ÉTAPE 4 : LIER AVEC VOTRE APP

Pour que votre app utilise les mêmes comptes :

### Option 1 : Firebase SDK dans l'app

Si votre app est en **JavaScript/TypeScript** (Electron, React Native, etc.) :

1. Installez Firebase dans votre app :
```bash
npm install firebase
```

2. Utilisez la **MÊME configuration** Firebase :
```javascript
// Dans votre app
import firebase from 'firebase/app';
import 'firebase/auth';
import 'firebase/firestore';

const firebaseConfig = {
  // LES MÊMES CLÉS que dans vos fichiers HTML !
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  // etc.
};

firebase.initializeApp(firebaseConfig);
```

3. Pour la connexion dans l'app :
```javascript
// Connexion
firebase.auth().signInWithEmailAndPassword(email, password)
  .then((userCredential) => {
    // Utilisateur connecté !
    const user = userCredential.user;
  });

// Récupérer le profil
firebase.firestore().collection('users').doc(user.uid).get()
  .then((doc) => {
    const userData = doc.data();
  });
```

### Option 2 : API REST Firebase

Si votre app est en **autre langage** (Python, C#, etc.) :

Utilisez l'API REST Firebase Auth :
```
https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key=[API_KEY]
```

Documentation : https://firebase.google.com/docs/reference/rest/auth

---

## ✅ CHECKLIST FINALE

Avant de déployer, vérifiez :

### Firebase
- [ ] Projet Firebase créé
- [ ] Authentication Email/Password activée
- [ ] Firestore Database créée
- [ ] Clés Firebase copiées dans `login.html` et `profile.html`

### EmailJS
- [ ] Compte EmailJS créé
- [ ] Service email ajouté (Gmail, etc.)
- [ ] Template email créé
- [ ] Public Key, Service ID et Template ID copiés dans les fichiers

### Fichiers
- [ ] `index.html` mis à jour avec bouton "Mon Compte"
- [ ] `login.html` configuré avec Firebase + EmailJS
- [ ] `profile.html` configuré avec Firebase + EmailJS
- [ ] Les 3 fichiers uploadés sur GitHub

### Test
- [ ] Créer un compte test
- [ ] Vérifier réception email
- [ ] Se connecter
- [ ] Modifier le profil
- [ ] Changer le mot de passe

---

## 🆘 BESOIN D'AIDE ?

Si vous avez des problèmes :

1. **Ouvrez la Console du navigateur** (F12)
2. Regardez les erreurs
3. Vérifiez que toutes les clés sont bien remplacées
4. Rejoignez Discord : https://discord.gg/XUzakkECJF

---

## 🎮 PROCHAINES ÉTAPES

Une fois tout configuré, vous pourrez :
- ✅ Les utilisateurs créent leur compte
- ✅ Email de bienvenue automatique
- ✅ Connexion sur le site ET l'app
- ✅ Modification du profil
- ✅ Synchronisation en temps réel

Bon courage ! 🚀
