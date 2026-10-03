# Atelier 02 — Déclarer puis déployer une application Java sur Azure

**4 h 30, autonome.** Aucun résultat de l'atelier 01 requis. Mission : une équipe doit recréer le même environnement sans clics oubliés, puis livrer un JAR identifiable et supprimer les ressources en fin de séance. Vous construisez une petite API Java17 et **trois** ressources Azure : un groupe dédié, un App Service Plan Linux B1, une Web App Java SE. Le JAR est déployé manuellement après validation du plan. Pas de base de données, réseau privé, service financier ou identité CI.

## Contrat et limites

GET /health répond 200 avec status UP ; GET /api/quotes renvoie le tableau fictif DEMO/42.0/EUR/fictional true ; chemins inconnus 404 et méthodes non GET 405 avec Allow: GET. Content-Type JSON UTF-8. Java lit SERVER_PORT fourni par App Service, puis PORT pour le poste, puis 8080. Le cloud exige une écoute sur 0.0.0.0. Les prix fictifs ne proviennent d'aucun marché.

Le plan **B1 est payant** même si l'application ne reçoit aucun trafic. Le coût dépend de la région, de l'abonnement, de la devise et de la durée. L'encadrant confirme le financement et la région avant apply. L'arrêt d'une Web App ne supprime pas le coût de son plan. Une alerte budget ne bloque pas automatiquement la dépense. Réserver 25 minutes à la destruction ; aucun bonus ne justifie de l'omettre.

## Déroulé — 270 minutes

| Temps | Travail | Preuve / décision |
|---|---|---|
| 00:00–00:15 · 15 min | Lire scénario, droits et budget ; choisir identifiant unique | Abonnement, région et périmètre notés hors dépôt |
| 00:15–01:25 · 70 min | Construire ou compléter API Java autonome et tests | clean verify, 8 cas et JAR local |
| 01:25–02:20 · 55 min | Écrire HCL : provider4, variables, RG, plan, app, logs, outputs | fmt/validate et schéma expliqués |
| 02:20–02:45 · 25 min | Login CLI, init, plan enregistré, revue du périmètre | Exactement 3 créations ; aucune destruction |
| 02:45–03:15 · 30 min | Apply après revue, déployer JAR avec CLI | outputs, déploiement accepté |
| 03:15–03:40 · 25 min | Vérifier HTTPS/contrat, lire logs et métriques | Requêtes et un événement d'exploitation corrélés |
| 03:40–04:05 · 25 min | Vérifier idempotence ; préparer plan de destruction | plan sans modification ; 3 suppressions prévues |
| 04:05–04:30 · 25 min | Détruire et attendre disparition ; restituer preuves | Groupe absent, bilan et limites |

Les attentes Azure sont incluses. Si le provisioning dépasse sa plage, réduire la collecte de métriques puis procéder au nettoyage. Si Azure indisponible, arrêt avant apply avec preuves locales et plan si possible ; ne pas inventer résultat cloud. Une suppression encore en cours après séance reste à la charge du binôme et de l'encadrant jusqu'à vérification.

## Étape 1 — Dessiner et limiter

Représenter les trois ressources, la relation plan/app, la machine locale et le flux de déploiement. Définir un identifiant court unique 6–16 caractères a-z0-9 commençant par lettre, sans donnée personnelle sensible. Noms demandés : rg-azdo02-ID, plan-azdo02-ID, app-azdo02-ID. Un nom de Web App peut déjà exister mondialement ; ne pas réutiliser une ressource d'un autre élève.

Avant séance, l'encadrant prépare l'abonnement, enregistrement Microsoft.Web, droits existants pour créer et supprimer ces ressources et quotas B1. Ne pas créer rôle ou identité pour résoudre un refus d'accès.

## Étape 2 — Construire la charge utile

Écrire pom et source Java17 avec manifeste main. Installer JUnit en scope test ; réaliser tests de port, /health, /api/quotes, 404 et 405. Les tests HTTP utilisent un port éphémère et ferment le serveur. Un JAR sans dépendance d'exécution suffit pour ce serveur JDK. Faire passer tests puis lancer JAR hors IDE. L'atelier est cloud : limiter cette phase au contrat simple et ne pas ajouter fonctionnalité.

## Étape 3 — Écrire l'infrastructure

Créer infra/versions.tf, variables.tf, main.tf et outputs.tf. Terraform >=1.9,<2 ; provider AzureRM4, abonnement explicite, authentification CLI et enregistrement provider désactivé car préparé par encadrant. Backend local pour comprendre état ; aucun tfstate ou plan binaire ne va dans Git.

Créer RG et plan Linux B1, puis Linux Web App HTTPS JavaSE17. Le démarrage exécute le JAR déposé comme app.jar. Désactiver FTP et authentifications basiques de publication ; déploiement via identité Entra de la session Azure CLI. Configurer health /health, Always On et logs filesystem de courte rétention. Déclarer outputs nom RG/app, URL à partir du default_hostname et ID app/plan. Les hostnames Azure peuvent évoluer : ne pas fabriquer app.azurewebsites.net.

## Étape 4 — Valider puis prévoir

Commandes locales de validation :

```bash
mvn --batch-mode --no-transfer-progress clean verify
terraform -chdir=infra fmt -check
terraform -chdir=infra validate
az account show --query '{name:name,id:id}' -o table
az webapp list-runtimes --os-type linux -o tsv
```

validate suppose init déjà effectué dans votre réalisation ; il ne prouve pas droit d'accès, quota ou disponibilité Azure. Comparer l'abonnement CLI et subscription_id. Enregistrer un plan de création puis le lire avec l'encadrant. Le plan doit annoncer 3 add, 0 change, 0 destroy. Vérifier noms, tags education, région, SKU et HTTPS. Ne pas appliquer une configuration qui propose des ressources hors groupe dédié.

## Étape 5 — Créer et livrer, deux actes distincts

Après revue, appliquer le plan enregistré. Retenir noms/URL depuis outputs. Déployer le JAR validé via `az webapp deploy`, type jar, destination app.jar. Le programme compilé est prêt ; ne pas activer un build distant. Ni Terraform apply ni un déploiement « accepté » ne prouvent que les routes fonctionnent : attendre la santé puis tester HTTPS.

## Étape 6 — Observer sans données sensibles

Dans un terminal, ouvrir la trace des logs. Dans l'autre, envoyer successivement health, quotes et unknown. Relier route/status/duration aux requêtes et capturer une ligne expurgée. Voir Metrics du portail : Requests, Http4xx, CPU Time et Memory Working Set selon les métriques exposées par votre runtime. Un pic est un indice, pas une cause. Les métriques peuvent apparaître avec quelques minutes de retard ; relever intervalle et granularité, ne pas inventer un graphique.

## Étape 7 — Idempotence et destruction

Refaire plan pour démontrer absence de changement après déploiement du code : Terraform gère infrastructure et configuration, pas le contenu du JAR dans cet atelier. Expliquer pourquoi un changement de code ne figure pas dans ce plan. Préparer plan destroy, vérifier exactement les 3 ressources, appliquer et attendre. Garder l'état jusqu'à la fin pour permettre réparation ou destruction ; ne jamais supprimer tfstate pour « nettoyer » le cloud.

## Livrable

HCL, API et tests, journal avec plan expurgé, SHA du code, preuve HTTP et logs, statut final du RG absent. Aucun état, secret ou plan binaire suivi. Extension hors temps : backend distant Azure Storage avec RBAC restreint, étudié séparément ; ne pas l'ajouter pendant cette séance.
