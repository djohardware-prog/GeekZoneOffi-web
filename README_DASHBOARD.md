# 🎮 SYSTÈME DASHBOARD - GEEK'ZONE LAUNCHER

## 📋 FICHIERS MODIFIÉS/CRÉÉS

### ✅ **NOUVEAU :**
1. **dashboard.html** - Page d'accueil pour utilisateurs connectés

### ✅ **MODIFIÉ :**
1. **login.html** - Redirige vers dashboard.html au lieu de profile.html

---

## 🎯 FONCTIONNEMENT

### **AVANT connexion :**
```
index.html (page d'accueil) 
    ↓ clic sur "Mon Compte"
login.html (connexion/inscription)
```

### **APRÈS connexion :**
```
login.html (connexion réussie)
    ↓ redirection automatique
dashboard.html (page personnalisée)
    ├─ Bouton "← Page d'accueil" → retour vers index.html
    └─ Avatar en haut à droite → menu déroulant
        ├─ ⚙️ Mon Profil → profile.html
        └─ 🚪 Déconnexion → retour index.html
```

---

## 🎨 FEATURES DU DASHBOARD

### **Section Bienvenue**
- Affiche "Bienvenue, [NOM] !"
- Récupère le nom depuis Firebase

### **Statistiques (4 cartes)**
1. **🎮 Jeux installés** - Nombre total de jeux
2. **⏱️ Temps de jeu** - Heures totales jouées
3. **🔥 Jours avec nous** - Calculé depuis la date de création du compte
4. **⭐ Favoris** - Nombre de jeux favoris

### **Section "Vos jeux"**
- Affiche les jeux synchronisés depuis le launcher
- Chaque jeu montre :
  - Icône du jeu
  - Nom
  - Temps de jeu
  - Nombre de lancements
  - Bouton "▶ Lancer"

**Si aucun jeu :**
- Message : "Aucun jeu détecté"
- Bouton "Télécharger le launcher"

### **Section "Activité récente"**
- Liste des dernières actions
- Exemple : "Compte créé avec succès !"

### **Header**
- Logo Geek'Zone (cliquable → retour index.html)
- Bouton "← Page d'accueil" (retour index.html)
- Avatar utilisateur (menu déroulant)
  - ⚙️ Mon Profil
  - 🚪 Déconnexion

---

## 🔐 SÉCURITÉ

Le dashboard est **protégé** :
- Vérifie l'authentification Firebase au chargement
- Si pas connecté → redirection automatique vers login.html
- Impossible d'accéder sans être authentifié

---

## 🔄 SYNCHRONISATION LAUNCHER ↔ SITE

### **Comment ça marche ?**

1. **L'utilisateur lance un jeu dans le LAUNCHER**
2. **Le launcher enregistre les données dans Firestore** :
   ```javascript
   db.collection('users').doc(userId).collection('games').doc(gameId).set({
       name: "Rocket League",
       icon: "🚗",
       playtime: 25,        // heures
       launches: 42,        // nombre de fois lancé
       isFavorite: true,
       lastPlayed: timestamp
   });
   ```

3. **Le SITE WEB lit ces données** et les affiche dans le dashboard

---

## 🚀 INTÉGRATION DANS LE LAUNCHER (À FAIRE)

Pour que le launcher envoie les données vers Firebase, ajoute ce code dans ton `GameService.cs` :

### **Après avoir lancé un jeu :**

```csharp
using GeekZoneLauncherv2.Services;

public async Task RecordGameLaunch(string gameId, string gameName, string icon)
{
    var authService = AuthenticationService.Instance;
    var firebaseService = new FirebaseService();
    
    if (!authService.IsLoggedIn()) return;
    
    var userId = authService.GetCurrentUserId();
    
    // Enregistrer dans Firestore
    await firebaseService.UpdateGameStatsAsync(userId, gameId, new {
        name = gameName,
        icon = icon,
        launches = increment(1),
        lastPlayed = DateTime.UtcNow
    });
}
```

### **Après avoir joué (fermeture du jeu) :**

```csharp
public async Task RecordPlaytime(string gameId, int minutesPlayed)
{
    var userId = authService.GetCurrentUserId();
    
    await firebaseService.UpdateGameStatsAsync(userId, gameId, new {
        playtime = increment(minutesPlayed / 60.0) // convertir en heures
    });
}
```

---

## 📊 STRUCTURE FIRESTORE

```
users/
  └─ {userId}/
      ├─ name: "John Doe"
      ├─ email: "john@example.com"
      ├─ createdAt: timestamp
      ├─ lastLogin: timestamp
      └─ games/
          ├─ rocket-league/
          │   ├─ name: "Rocket League"
          │   ├─ icon: "🚗"
          │   ├─ playtime: 25 (heures)
          │   ├─ launches: 42
          │   ├─ isFavorite: true
          │   └─ lastPlayed: timestamp
          │
          ├─ minecraft/
          │   ├─ name: "Minecraft"
          │   ├─ icon: "⛏️"
          │   ├─ playtime: 100
          │   ├─ launches: 150
          │   ├─ isFavorite: true
          │   └─ lastPlayed: timestamp
          │
          └─ farming-simulator/
              ├─ name: "Farming Simulator 25"
              ├─ icon: "🚜"
              ├─ playtime: 50
              ├─ launches: 30
              ├─ isFavorite: false
              └─ lastPlayed: timestamp
```

---

## 🎨 DESIGN

Le dashboard utilise un design **cyberpunk/gaming** avec :
- Fond noir avec grille animée jaune
- Typographie Orbitron (futuriste) + Rajdhani
- Animations au survol
- Cartes avec bordures jaunes
- Dégradés et effets de glow

---

## 📝 UPLOAD SUR GITHUB

Ajoute ces fichiers à ton dépôt :

```
ton-repo/
├─ index.html (déjà existant)
├─ login.html (MODIFIÉ - redirige vers dashboard)
├─ profile.html (déjà existant)
└─ dashboard.html (NOUVEAU)
```

---

## ✅ CHECKLIST FINALE

- [ ] dashboard.html uploadé sur GitHub
- [ ] login.html modifié uploadé sur GitHub
- [ ] Vercel redéploie automatiquement
- [ ] Tester : créer un compte → redirige vers dashboard ✅
- [ ] Tester : bouton "Page d'accueil" fonctionne ✅
- [ ] Tester : menu avatar fonctionne ✅
- [ ] Tester : déconnexion fonctionne ✅

**PLUS TARD (quand le launcher sera prêt) :**
- [ ] Ajouter le code de synchronisation dans GameService.cs
- [ ] Tester : lancer un jeu dans le launcher → apparaît sur le site
- [ ] Tester : statistiques mises à jour en temps réel

---

## 🎉 RÉSULTAT FINAL

**Flux utilisateur complet :**

1. **Visiteur** arrive sur index.html
2. Clique "Mon Compte" → login.html
3. Crée un compte ou se connecte
4. **Redirection automatique** → dashboard.html
5. Voit ses stats et jeux (synchronisés avec le launcher)
6. Peut aller sur "Mon Profil" (profile.html) pour changer mot de passe
7. Peut revenir à "Page d'accueil" (index.html) pour télécharger le launcher
8. Peut se déconnecter

---

**🚀 Tout est prêt ! Télécharge le ZIP et upload sur GitHub !**
