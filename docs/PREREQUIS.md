# Préparation avant la séance — hors des 4 h 30

Public Bac+4/5, débutant à intermédiaire en cloud. Savoir créer un fichier, lancer une commande, lire une erreur et utiliser les bases Java (classe, méthode, exception). Installer JDK **17**, Maven **3.9.x**, Git, `curl`, un éditeur et Bash (Linux/macOS ou WSL2 sous Windows). La syntaxe des instructions suppose Bash ; utiliser WSL2 plutôt que traduire les commandes en PowerShell pendant la séance.

Disposer d'un compte GitHub avec droit de créer son propre dépôt, Actions autorisé et accès réseau à GitHub et Maven Central. Le dépôt distribué appartient à `pilotcrew-io` ; les preuves et essais sont réalisés dans votre propre copie. Le formateur vérifie les limitations de quota et les autorisations de l'organisation avant la séance.

Contrôles de préparation :

```bash
java -version
javac -version
mvn -version
git --version
curl --version
```

Résultat attendu : Java et javac annoncent 17, Maven utilise ce même JDK. Faire résoudre à l'avance les dépendances de l'atelier via un projet de test fourni par l'encadrant ; le premier téléchargement peut être lent. Ne pas lire le corrigé pour préparer l'exercice.

Une difficulté d'installation n'est pas une difficulté de développement : obtenir un poste prêt avant le démarrage. Préparer un dossier de preuves local expurgées et une fenêtre Bash supplémentaire pour les requêtes HTTP.

## Préparation Azure supplémentaire — 45 à 60 minutes hors séance

Installer Terraform >=1.9,<2 et Azure CLI **2.61 ou ultérieure** ; le formateur actualise CLI avant session. Bash, Maven et Java17 restent requis. Une souscription **active** dédiée à la formation est nécessaire ; l'offre Azure for Students ne garantit ni SKU B1 ni quota suffisant.

L'encadrant prépare : région autorisée (francecentral par défaut), capacité Linux B1, financement, provider Microsoft.Web enregistré et droits existants de l'élève pour créer un nouveau groupe, créer ses ressources, déployer un JAR, configurer les politiques de publication `Microsoft.Web/sites/basicPublishingCredentialsPolicies/write` et supprimer ce groupe. Créer un RG demande une autorisation sur la souscription ; une permission limitée à un autre RG ne suffit pas à cet exercice. Si la politique ne permet qu'un RG précréé, le formateur prépare une variante avec import/data source avant séance ; ne pas improviser une attribution Owner. Aucun exercice ne demande un changement RBAC ou une identité de service.

Avant séance, vérifier :

```bash
terraform version
az version
az login
az account show --query '{name:name,id:id}' -o table
az provider show --namespace Microsoft.Web --query registrationState -o tsv
az webapp list-runtimes --os-type linux -o tsv
```

Attendu : abonnement convenu, provider Registered et Java17/JavaSE disponible. Le navigateur et MFA peuvent être nécessaires au login. Ne pas faire az login dans un workflow GitHub ni mettre un token dans un fichier. Le dossier .azure du poste est privé. L'identifiant unique doit rester simple et ne pas contenir e-mail ou numéro étudiant sensible.

Confirmer estimation de coût **avec les tarifs courants de la région et de l'abonnement** : [App Service pricing](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/). Aucun prix forfaitaire n'est garanti ici. Programmer la fin de séance pour détruire plan et app, même après déploiement partiel. État et plans restent localement jusqu'à destruction vérifiée.
