# Tu publies ou tu supprimes ? — version multijoueur

Application pédagogique en français, inspirée du format des publications Instagram (sans affiliation à Instagram).

## Fonctionnalités
- 8 situations fictives et 3 choix par publication, avec explication pédagogique.
- Participation simultanée via navigateur et pseudonyme.
- Réponses enregistrées sur le serveur, une seule réponse par situation et par participant.
- Tableau de bord administrateur sur `/` puis « Espace admin » (ou `/#admin`) : pourcentages A/B/C, résultats par pseudonyme, export CSV, actualisation toutes les 5 secondes.
- QR code sur `/qr.png`, généré automatiquement avec l'URL du site.

## Lancer localement
Installer Node.js 20 ou supérieur. Dans ce dossier :

```bash
npm install
ADMIN_PASSWORD='Remplacer-Par-Un-Mot-De-Passe-Long' npm start
```

Sur Windows PowerShell :

```powershell
$env:ADMIN_PASSWORD='Remplacer-Par-Un-Mot-De-Passe-Long'; npm start
```

Ouvrir `http://localhost:3000`. **Ce lien local ne fonctionne pas sur les téléphones des élèves hors de cet ordinateur.**

## Mise en ligne (Railway, avec stockage persistant)
1. Créer un dépôt GitHub privé contenant les fichiers de ce dossier (ne pas inclure `node_modules` ni `data`).
2. Sur Railway, créer un projet depuis ce dépôt GitHub et déployer le service Node.js. La commande de démarrage est `npm start`.
3. Ajouter la variable d'environnement `ADMIN_PASSWORD` avec un mot de passe robuste (au moins 12 caractères), conservé secret.
4. Ajouter un **volume persistant** monté sur `/data` et la variable `DATA_DIR=/data`. Sans volume, les réponses peuvent disparaître au redéploiement.
5. Générer un domaine public HTTPS depuis les paramètres réseau du service Railway.
6. Facultatif : définir `PUBLIC_URL=https://votre-domaine-public` pour que le QR code pointe exactement vers ce domaine. Sans cette variable, le serveur utilise l'adresse reçue par la requête.
7. Tester le site sur un autre téléphone en données mobiles, ouvrir `https://votre-domaine/qr.png`, projeter ou imprimer le QR code.
8. Pour consulter les résultats, ouvrir `https://votre-domaine/#admin` et saisir le mot de passe.

**L'hébergement Railway peut être payant.** Vérifier les conditions et les coûts avant de lancer le service. Aucun hébergement ni compte tiers n'est créé automatiquement par ce dossier.

## Précautions pour la classe
- Préférer des pseudonymes anonymes ; éviter les noms complets et toute donnée sensible.
- Les données sont conservées dans le fichier SQLite du volume tant qu'elles ne sont pas effacées ; organiser leur suppression après l'activité.
- Les résultats ne sont accessibles que par mot de passe administrateur, mais le site est public pour les participants qui disposent du lien.
- Pour une nouvelle classe, sauvegarder les résultats CSV, puis repartir d'une base vide avec une nouvelle instance ou supprimer le fichier de base de données **service arrêté**.
- Une connexion Internet est nécessaire sur les téléphones des élèves.
- Ce projet est un prototype pédagogique : pour un déploiement institutionnel, faire valider sécurité, politique de confidentialité et gestion des données par l'établissement.

## Limites
- L'animateur consulte les résultats en direct mais ne déclenche pas les questions à distance ; chaque élève avance à son rythme.
- Les publications sont des simulations textuelles, pas de véritables posts Instagram ni de vidéos hébergées.
- Une personne peut participer plusieurs fois avec différents pseudonymes ; l'application ne vérifie pas l'identité réelle.
