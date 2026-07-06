
#  README — Lancer l’application Flask (Windows / PowerShell)

##  Prérequis
- **Python ** installé (vérifie avec `python --version`)
- **PowerShell** (ouvrir en tant qu’utilisateur normal)

  Si tu n’as pas `pip`, installe-le avec : `python -m ensurepip --upgrade`.



##  Étapes de démarrage

### 1) Ouvrir PowerShell
- `Win + X` → **Windows PowerShell**
- Navigue vers le projet si besoin :
 powershell
cd "chemin\\versle\\projet"


### 2) Aller dans le dossier backend
 powershell
cd backend


### 3) (Optionnel mais recommandé) Créer un environnement virtuel
 Si tu as déjà un dossier `venv/`(comme le cas dans le dossier ), passe à l’étape 4.
 powershell
python -m venv venv


### 4) Autoriser l’exécution (temporaire, pour cette session)
 Réponds **O** pour “Oui” si PowerShell te le demande.
powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass


### 5) Activer l’environnement virtuel
powershell
.\venv\Scripts\Activate.ps1


### 6) Installer les dépendances nécessaires
  : « pip install flask werkzeug (si pas encore installées) ». :
powershell
pip install flask werkzeug


powershell
pip install flask-cors


### 7) Lancer l’application
powershell
python app.py

Tu devrais voir dans la console quelque chose comme :

 * Running on http://127.0.0.1:5000 (Press CTRL+C to quit)


### 8) Ouvrir l’application
- Va à l’adresse affichée (en général) : **http://127.0.0.1:5000/**
- Pages utiles :
  - `/` → Connexion
  - `/register.html` → Créer un compte
  - `/utilisateur.html` → Espace utilisateur
  - `/reserver.html` → Réserver une salle
  - `/admin.html` → Panneau admin

---

##  Comptes par défaut
Un **admin** est créé automatiquement lors de l’initialisation de la base :

- **Email** : `admin@mail.com`  
- **Mot de passe** : `admin123`



##  Structure du projet 
```
partie logicielle/
└── backend/
    ├── app.py
    ├── modele.py
    ├── basededonnes.py
    ├── routes/
    │   ├── admin_routes.py
    │   ├── auth__routes.py
    │   └── reservation_routes.py
    └── services/
        ├── service_reservation.py
        └── service_salle.py
└── frontend/
    ├── css/style.css
    ├── js/
    │   ├── auth.js
    │   ├── admin.js
    │   └── utilisateur.js
    ├── index.html
    ├── register.html
    ├── utilisateur.html
    ├── reserver.html
    └── admin.html




##  Tests rapides

### 1) Connexion
- Compte admin : `admin@mail.com / admin123`
- Créer un compte utilisateur via `/register.html`

### 2) Admin
- Ajouter une salle : `/admin.html` → *ID, Nom, Capacité*
- Lister les salles et les réservations

### 3) Utilisateur
- Faire une réservation : `/reserver.html`
- Voir / annuler : `/utilisateur.html`

---

##  Dépannage 

### « Je n’arrive pas à activer le venv / script bloqué »
Exécute ceci **avant** `./venv/Scripts/Activate.ps1` :
 powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass


### « Après une réservation, rien n’apparaît dans mes tableaux »
Les services stockent des données **en mémoire**. En mode debug, le **reloader** de Flask peut lancer **deux processus** et “perdre” l’état.  
**Solution** : lance Flask **sans reloader** dans `app.py` :
python
if __name__ == "__main__":
    app.run(debug=True, use_reloader=False)


### « Déconnexion → 404 »
Redirige vers `/` (pas `/index.html`), car `index.html` est servi sur la racine `/`.

Dans `frontend/js/auth.js` :
javascript
window.location.href = "/";


### « 403 Accès interdit sur /admin/* »
Reconnecte‑toi comme **admin** et vérifie que les requêtes `fetch` ont bien `credentials: "include"` (déjà présent dans nos JS).  
Assure‑toi d’utiliser **l’app servie par Flask** (http://127.0.0.1:5000), pas un `file:///`.

---

##  Arrêter le serveur
Dans PowerShell : `Ctrl + C`  
Désactiver le venv :
 powershell
deactivate

