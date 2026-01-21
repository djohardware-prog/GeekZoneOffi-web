# 🔧 MODIFICATION INDEX.HTML - Ajout Avatar Utilisateur

## 📝 INSTRUCTIONS

### **ÉTAPE 1 : Ajouter le CSS pour l'avatar**

**Trouve dans index.html** la section `/* Navigation */` (vers ligne 86) et **AJOUTE CE CSS APRÈS** :

```css
/* Avatar utilisateur */
.user-menu {
    position: relative;
    cursor: pointer;
}

.user-avatar {
    width: 45px;
    height: 45px;
    background: linear-gradient(135deg, var(--primary-yellow), #FFA500);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Orbitron', sans-serif;
    font-size: 18px;
    font-weight: 900;
    color: var(--dark-bg);
    border: 2px solid var(--primary-yellow);
    transition: all 0.3s ease;
}

.user-avatar:hover {
    transform: scale(1.1);
    box-shadow: 0 0 20px rgba(255, 215, 0, 0.5);
}

.dropdown {
    position: absolute;
    top: 60px;
    right: 0;
    background: var(--darker-bg);
    border: 2px solid var(--primary-yellow);
    border-radius: 10px;
    min-width: 220px;
    display: none;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
    z-index: 1000;
}

.dropdown.active {
    display: block;
    animation: slideDown 0.3s ease;
}

@keyframes slideDown {
    from {
        opacity: 0;
        transform: translateY(-10px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.dropdown a {
    display: block;
    padding: 15px 20px;
    color: white;
    text-decoration: none;
    transition: all 0.3s ease;
    border-bottom: 1px solid rgba(255, 215, 0, 0.1);
    font-size: 16px;
}

.dropdown a:last-child {
    border-bottom: none;
}

.dropdown a:hover {
    background: rgba(255, 215, 0, 0.1);
    color: var(--primary-yellow);
    padding-left: 30px;
}

.btn-account {
    display: none; /* Caché par défaut, montré si pas connecté */
}
```

---

### **ÉTAPE 2 : Modifier le HTML de la navigation**

**Trouve dans index.html** la ligne avec `<a href="login.html" class="nav-link" style="background: var(--primary-yellow);...">Mon Compte</a>`

**REMPLACE TOUTE CETTE LIGNE PAR** :

```html
<!-- Bouton Mon Compte (si pas connecté) -->
<a href="login.html" id="btnAccount" class="nav-link btn-account" style="background: var(--primary-yellow); color: var(--dark-bg); padding: 12px 30px; border-radius: 5px; font-weight: 700;">
    Mon Compte
</a>

<!-- Avatar utilisateur (si connecté) -->
<div class="user-menu" id="userMenu" style="display: none;">
    <div class="user-avatar" id="userAvatar" onclick="toggleDropdown()">G</div>
    <div class="dropdown" id="dropdown">
        <a href="dashboard.html">📊 Dashboard</a>
        <a href="profile.html">⚙️ Mon Profil</a>
        <a href="#" onclick="logout(); return false;">🚪 Déconnexion</a>
    </div>
</div>
```

---

### **ÉTAPE 3 : Ajouter le SDK Firebase**

**AVANT la balise `</body>` (tout à la fin du fichier)**, **AJOUTE** :

```html
<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>

<script>
    // Configuration Firebase
    const firebaseConfig = {
        apiKey: "AIzaSyDZe7Pz24bvL_K0KVzaDg5zTCKSUilfqeE",
        authDomain: "geekzone-launcher.firebaseapp.com",
        projectId: "geekzone-launcher",
        storageBucket: "geekzone-launcher.firebasestorage.app",
        messagingSenderId: "724224317677",
        appId: "1:724224317677:web:e8fada43d4519498a94e4"
    };

    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();

    // Vérifier l'état de connexion
    auth.onAuthStateChanged((user) => {
        const btnAccount = document.getElementById('btnAccount');
        const userMenu = document.getElementById('userMenu');
        const userAvatar = document.getElementById('userAvatar');

        if (user) {
            // Utilisateur connecté
            btnAccount.style.display = 'none';
            userMenu.style.display = 'block';
            
            // Afficher la première lettre du nom
            const displayName = user.displayName || user.email || 'U';
            userAvatar.textContent = displayName.charAt(0).toUpperCase();
        } else {
            // Utilisateur non connecté
            btnAccount.style.display = 'inline-block';
            userMenu.style.display = 'none';
        }
    });

    // Toggle dropdown
    function toggleDropdown() {
        const dropdown = document.getElementById('dropdown');
        dropdown.classList.toggle('active');
    }

    // Fermer dropdown si clic ailleurs
    window.onclick = function(event) {
        if (!event.target.matches('.user-avatar')) {
            const dropdown = document.getElementById('dropdown');
            if (dropdown && dropdown.classList.contains('active')) {
                dropdown.classList.remove('active');
            }
        }
    }

    // Déconnexion
    function logout() {
        if (confirm('Voulez-vous vraiment vous déconnecter ?')) {
            auth.signOut().then(() => {
                window.location.reload();
            });
        }
    }
</script>
```

---

## ✅ RÉSULTAT

**Avant connexion :**
- Navigation affiche : "Mon Compte" (bouton jaune)

**Après connexion :**
- Navigation affiche : Avatar avec première lettre du nom
- Clic sur avatar → Menu déroulant :
  - 📊 Dashboard
  - ⚙️ Mon Profil
  - 🚪 Déconnexion

---

## 🎯 TEST

1. Ouvre index.html dans un navigateur
2. **Si pas connecté** → Tu vois "Mon Compte"
3. Connecte-toi via "Mon Compte"
4. Retourne sur index.html
5. **Maintenant** → Tu vois ton avatar !
6. Clique dessus → Menu avec Dashboard, Mon Profil, Déconnexion

---

**C'est tout ! Simple et efficace !** 🚀
