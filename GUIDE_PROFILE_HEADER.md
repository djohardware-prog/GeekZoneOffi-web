# 🔧 MODIFICATION PROFILE.HTML - Ajout Header avec Navigation

## 📝 INSTRUCTIONS

### **ÉTAPE 1 : Ajouter le CSS pour le nouveau header**

**Dans profile.html**, trouve la section `.logout-btn` (vers ligne 84) et **REMPLACE TOUT LE CSS de `.logout-btn` PAR** :

```css
/* Header navigation */
.header-right {
    display: flex;
    gap: 20px;
    align-items: center;
}

.btn-home {
    padding: 12px 25px;
    background: transparent;
    color: var(--primary-yellow);
    border: 2px solid var(--primary-yellow);
    font-family: 'Orbitron', sans-serif;
    font-size: 14px;
    font-weight: 700;
    text-transform: uppercase;
    cursor: pointer;
    transition: all 0.3s ease;
    text-decoration: none;
    display: flex;
    align-items: center;
    gap: 10px;
}

.btn-home:hover {
    background: var(--primary-yellow);
    color: var(--dark-bg);
    transform: translateY(-3px);
    box-shadow: 0 10px 30px rgba(255, 215, 0, 0.3);
}

.user-menu {
    position: relative;
    cursor: pointer;
}

.user-avatar {
    width: 50px;
    height: 50px;
    background: linear-gradient(135deg, var(--primary-yellow), #FFA500);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Orbitron', sans-serif;
    font-size: 20px;
    font-weight: 900;
    color: var(--dark-bg);
    border: 3px solid var(--primary-yellow);
    transition: all 0.3s ease;
}

.user-avatar:hover {
    transform: scale(1.1);
    box-shadow: 0 0 30px rgba(255, 215, 0, 0.5);
}

.dropdown {
    position: absolute;
    top: 70px;
    right: 0;
    background: var(--darker-bg);
    border: 2px solid var(--primary-yellow);
    border-radius: 10px;
    min-width: 200px;
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
}

.dropdown a:last-child {
    border-bottom: none;
}

.dropdown a:hover {
    background: rgba(255, 215, 0, 0.1);
    color: var(--primary-yellow);
    padding-left: 30px;
}
```

---

### **ÉTAPE 2 : Modifier le HTML du header**

**Dans profile.html**, trouve cette ligne (vers ligne 474) :

```html
<div class="header">
    <div class="logo">Geek'Zone</div>
    <button class="logout-btn" onclick="logout()">Déconnexion</button>
</div>
```

**REMPLACE PAR** :

```html
<div class="header">
    <div class="logo" onclick="window.location.href='index.html'" style="cursor: pointer;">Geek'Zone</div>
    
    <div class="header-right">
        <a href="index.html" class="btn-home">
            <span>←</span>
            Page d'accueil
        </a>
        
        <div class="user-menu" onclick="toggleDropdown()">
            <div class="user-avatar" id="headerAvatar">G</div>
            <div class="dropdown" id="headerDropdown">
                <a href="dashboard.html">📊 Dashboard</a>
                <a href="profile.html">⚙️ Mon Profil</a>
                <a href="#" onclick="logout(); return false;">🚪 Déconnexion</a>
            </div>
        </div>
    </div>
</div>
```

---

### **ÉTAPE 3 : Ajouter les fonctions JavaScript**

**Dans profile.html**, trouve la fonction `logout()` (vers la fin du fichier, ligne 670) et **AJOUTE JUSTE APRÈS** :

```javascript
// ==========================================
// TOGGLE DROPDOWN HEADER
// ==========================================
function toggleDropdown() {
    const dropdown = document.getElementById('headerDropdown');
    dropdown.classList.toggle('active');
}

// Fermer dropdown si clic ailleurs
window.onclick = function(event) {
    if (!event.target.matches('.user-avatar')) {
        const dropdown = document.getElementById('headerDropdown');
        if (dropdown && dropdown.classList.contains('active')) {
            dropdown.classList.remove('active');
        }
    }
}

// Mettre à jour l'avatar du header
function updateHeaderAvatar() {
    if (currentUser) {
        const displayName = currentUser.displayName || currentUser.email || 'U';
        const headerAvatar = document.getElementById('headerAvatar');
        if (headerAvatar) {
            headerAvatar.textContent = displayName.charAt(0).toUpperCase();
        }
    }
}
```

---

### **ÉTAPE 4 : Appeler updateHeaderAvatar()**

**Dans profile.html**, trouve la ligne `loadUserProfile(user);` (vers ligne 700) et **AJOUTE JUSTE APRÈS** :

```javascript
loadUserProfile(user);
updateHeaderAvatar(); // ← AJOUTE CETTE LIGNE
```

---

## ✅ RÉSULTAT

La page **profile.html** aura maintenant :

**Header avec :**
- Logo Geek'Zone (cliquable → retour index.html)
- Bouton "← Page d'accueil"
- Avatar utilisateur avec menu déroulant :
  - 📊 Dashboard
  - ⚙️ Mon Profil
  - 🚪 Déconnexion

**Plus de bouton "Déconnexion" isolé**, tout est dans le menu !

---

## 🎯 TEST

1. Connecte-toi sur le site
2. Va sur profile.html
3. Tu vois le **nouveau header** avec l'avatar
4. Clique sur l'avatar → Menu déroulant
5. Teste tous les liens !

---

**C'est tout ! Design unifié sur toutes les pages !** 🚀
