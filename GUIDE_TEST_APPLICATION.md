# Guide de Test de l'Application REST Client - Comptes Bancaires

## 📋 Prérequis avant de lancer l'application

### 1. Vérifier que votre serveur backend est démarré
Votre application Android se connecte à : `http://10.0.2.2:8082/banque/comptes`

**Note**: `10.0.2.2` est l'adresse localhost pour l'émulateur Android (correspond à `localhost` sur votre Mac)

**Démarrez votre serveur Spring Boot** sur le port 8082 avant de lancer l'app Android.

### 2. Créer et lancer un émulateur Android

Dans Android Studio :
1. Cliquez sur **Device Manager** (icône téléphone en haut à droite)
2. Si aucun émulateur n'existe, cliquez sur **Create Device**
   - Choisissez **Pixel 5** ou **Pixel 6**
   - Sélectionnez API Level 28 ou supérieur (Android 9.0+)
   - Si vous avez un Mac Apple Silicon (M1/M2/M3), choisissez une image **arm64-v8a**
   - Cliquez sur **Finish**
3. Cliquez sur le bouton **Play** ▶️ pour lancer l'émulateur
4. Attendez que l'émulateur démarre complètement (2-5 minutes la première fois)

### 3. Lancer l'application

Dans Android Studio :
1. Assurez-vous que l'émulateur est sélectionné en haut (à côté du bouton Run)
2. Cliquez sur le bouton **Run** ▶️ (ou appuyez sur **Ctrl+R**)
3. L'application va se compiler et s'installer sur l'émulateur

---

## 🧪 Tests à effectuer

### Test 1 : Affichage des comptes ✅

**Objectif**: Vérifier que la liste des comptes s'affiche correctement

**Étapes**:
1. L'application démarre et affiche l'écran principal
2. Par défaut, le format **JSON** est sélectionné
3. La liste des comptes devrait s'afficher automatiquement

**Résultat attendu**:
- Les comptes s'affichent avec : ID, Solde, Type, Date de création
- Chaque compte a deux boutons : "Modifier" et "Supprimer"

**Si la liste est vide**:
- Vérifiez que votre serveur Spring Boot est démarré
- Vérifiez l'URL : `http://localhost:8082/banque/comptes` dans votre navigateur
- Ajoutez des comptes via l'application ou directement dans votre base de données

---

### Test 2 : Basculer entre JSON et XML 🔄

**Objectif**: Tester le changement de format de données

**Étapes**:
1. Cliquez sur le bouton radio **XML** en haut de l'écran
2. La liste devrait se recharger

**Résultat attendu**:
- Les mêmes données s'affichent
- L'application récupère les données en format XML du serveur
- Aucune erreur n'apparaît

**Testez aussi**:
- Revenez au format **JSON**
- Les données se rechargent correctement

---

### Test 3 : Ajouter un nouveau compte ➕

**Objectif**: Créer un nouveau compte bancaire

**Étapes**:
1. Cliquez sur le bouton **FAB** (bouton flottant violet en bas à droite avec l'icône +)
2. Une boîte de dialogue "Ajouter un compte" s'ouvre
3. Saisissez un solde : **15000.50**
4. Sélectionnez le type : **COURANT** ou **EPARGNE**
5. Cliquez sur **Ajouter**

**Résultat attendu**:
- Un message Toast "Compte ajouté" apparaît
- La liste se recharge automatiquement
- Le nouveau compte apparaît dans la liste avec la date du jour (2025-10-29)

**Testez aussi**:
- Ajoutez plusieurs comptes avec différents types
- Testez avec des valeurs décimales (ex: 1234.56)
- Cliquez sur "Annuler" pour fermer la boîte de dialogue sans ajouter

---

### Test 4 : Modifier un compte existant ✏️

**Objectif**: Mettre à jour les informations d'un compte

**Étapes**:
1. Dans la liste, cliquez sur le bouton **Modifier** d'un compte
2. Une boîte de dialogue "Modifier un compte" s'ouvre
3. Le solde actuel est pré-rempli
4. Le type actuel est pré-sélectionné
5. Modifiez le solde : **25000.00**
6. Changez le type si nécessaire
7. Cliquez sur **Modifier**

**Résultat attendu**:
- Un message Toast "Compte modifié" apparaît
- La liste se recharge
- Les nouvelles informations s'affichent correctement
- La date de création reste inchangée

**Testez aussi**:
- Modifiez uniquement le solde
- Modifiez uniquement le type
- Cliquez sur "Annuler" pour ne pas sauvegarder les modifications

---

### Test 5 : Supprimer un compte ❌

**Objectif**: Supprimer un compte bancaire

**Étapes**:
1. Cliquez sur le bouton **Supprimer** d'un compte
2. Une boîte de dialogue de confirmation apparaît : "Voulez-vous vraiment supprimer ce compte ?"
3. Cliquez sur **Oui** pour confirmer

**Résultat attendu**:
- Un message Toast "Compte supprimé" apparaît
- La liste se recharge
- Le compte n'apparaît plus dans la liste

**Testez aussi**:
- Cliquez sur **Non** pour annuler la suppression
- Le compte reste dans la liste

---

### Test 6 : Gestion des erreurs 🚨

**Objectif**: Vérifier que l'application gère correctement les erreurs

#### Test 6.1 : Serveur arrêté
**Étapes**:
1. Arrêtez votre serveur Spring Boot
2. Dans l'app, changez de format (JSON ↔ XML) pour forcer un rechargement
3. Ou essayez d'ajouter un compte

**Résultat attendu**:
- Un message Toast d'erreur apparaît
- L'application ne plante pas
- La liste reste vide ou affiche les anciennes données

#### Test 6.2 : Données invalides
**Étapes**:
1. Essayez d'ajouter un compte avec un solde vide
2. Essayez d'ajouter un compte avec du texte dans le champ solde

**Résultat attendu**:
- L'application devrait gérer l'erreur
- Un message d'erreur apparaît

#### Test 6.3 : Connexion réseau
**Étapes**:
1. Redémarrez le serveur Spring Boot
2. Rechargez les données dans l'app
3. Tout devrait refonctionner

---

## 🔍 Vérifications dans Logcat (Android Studio)

Pour voir les logs de l'application :

1. Ouvrez l'onglet **Logcat** en bas d'Android Studio
2. Filtrez par : `ma.projet.restclient`
3. Vous verrez les requêtes HTTP et les réponses

**Exemple de logs attendus**:
```
GET http://10.0.2.2:8082/banque/comptes
Response: 200 OK
Body: [{"id":1,"solde":1000.0,"type":"COURANT","dateCreation":"2025-10-29"}]
```

---

## ✅ Checklist complète des tests

- [ ] L'application démarre sans erreur
- [ ] La liste des comptes s'affiche (format JSON)
- [ ] Basculer vers le format XML fonctionne
- [ ] Ajouter un nouveau compte fonctionne
- [ ] Le nouveau compte apparaît dans la liste
- [ ] Modifier un compte existant fonctionne
- [ ] Les modifications sont visibles
- [ ] Supprimer un compte avec confirmation fonctionne
- [ ] Le compte disparaît de la liste
- [ ] Les messages Toast s'affichent correctement
- [ ] L'application gère les erreurs réseau
- [ ] Pas de crash lors des tests

---

## 🐛 Problèmes courants et solutions

### Problème : "No connected devices"
**Solution**: Lancez l'émulateur via Device Manager

### Problème : "Cannot resolve symbol 'R'"
**Solution**: 
```bash
# Dans le terminal Android Studio
./gradlew clean build
# Puis : Build > Rebuild Project
```

### Problème : L'app se connecte mais aucune donnée
**Solution**: 
- Vérifiez que le serveur est sur le port 8082
- Testez dans votre navigateur : `http://localhost:8082/banque/comptes`
- Vérifiez que des comptes existent dans la base de données

### Problème : "Network error" ou timeout
**Solution**:
- Vérifiez le fichier `network_security_config.xml` (déjà configuré)
- Vérifiez que l'URL dans `RetrofitClient.java` est bien `http://10.0.2.2:8082/`
- Pour un vrai appareil Android, changez l'IP vers l'IP de votre Mac sur le réseau local

### Problème : L'émulateur est très lent
**Solution**:
- Allouez plus de RAM (4 GB recommandé)
- Sur Mac Apple Silicon, utilisez une image ARM64
- Fermez les autres applications

---

## 📸 Captures d'écran à vérifier

Assurez-vous que votre interface ressemble à ceci :

### Écran principal
- En haut : Carte avec boutons radio JSON/XML
- Au milieu : RecyclerView avec la liste des comptes
- En bas à droite : Bouton flottant violet (+)

### Élément de liste (item_compte)
- Ligne 1 : ID: X (en gras)
- Ligne 2 : Solde: XXXX.XX
- Ligne 3 : Type: COURANT ou EPARGNE
- Ligne 4 : Date: 2025-10-29
- À droite : Boutons "Modifier" et "Supprimer"

### Boîte de dialogue Ajouter/Modifier
- Champ de saisie pour le solde (fond gris clair)
- Carte avec 2 boutons radio : COURANT et EPARGNE
- Boutons : Annuler / Ajouter (ou Modifier)

---

## 🎉 Test final

Effectuez ce scénario complet :

1. Démarrez avec une liste de 3 comptes
2. Ajoutez un nouveau compte (ID: 4)
3. Modifiez le compte ID: 2
4. Supprimez le compte ID: 1
5. Basculez vers XML
6. Basculez vers JSON
7. Ajoutez encore un compte
8. Vérifiez que tout est cohérent

**Si tous ces tests passent, votre application fonctionne parfaitement !** ✅

---

## 📝 Notes importantes

- L'application utilise l'adresse `http://10.0.2.2:8082/` qui pointe vers `localhost:8082` de votre Mac
- Les permissions Internet sont configurées dans AndroidManifest.xml
- Le fichier network_security_config.xml autorise le trafic HTTP (cleartext) vers 10.0.2.2
- Tous les appels API sont asynchrones (pas de blocage de l'UI)
- Les callbacks gèrent les succès et les erreurs

Bon test ! 🚀

