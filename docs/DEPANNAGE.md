# Dépannage 02 — diagnostiquer par couche

| Symptôme | Cause plausible | Diagnostic | Résolution |
|---|---|---|---|
| subscription ID required | Provider4 sans entrée explicite | Lire provider et variable, comparer account show | Renseigner UUID du laboratoire |
| AuthorizationFailed | Mauvaise souscription ou droits insuffisants | account show + ID de la ressource de l'erreur | Encadrant vérifie droits existants, aucun rôle large ajouté |
| MissingSubscriptionRegistration | Microsoft.Web non préparé | az provider show --namespace Microsoft.Web --query registrationState | Encadrant effectue prérequis, pas l'élève en séance |
| Web App name unavailable | Nom global déjà pris | Lire erreur de création et vérifier identifiant | Choisir nouvel identifiant avant création ; si partiel relire nouveau plan |
| Quota/SKU/Policy refuse B1 | Abonnement/région limité | Lire code et policy/region de l'erreur | Encadrant choisit région autorisée ou mode local |
| validate provider absent | init non réalisé | Vérifier .terraform et message | init, lock puis validate |
| Plan contient des destroy inattendus | Mauvais état, variables changées | Comparer ID/nom et état, ne pas apply | Retrouver bons fichiers et relire avec encadrant |
| Déploiement 401/403 | CLI obsolète ou identité sans action deploy | az version, account show et erreur Kudu | CLI récente/identité prévue, ne pas activer mot de passe |
| 503 après upload | Port/bind/startup ou JAR erroné | Logs démarrage + manifeste + SERVER_PORT | 0.0.0.0, port plateforme, app.jar et main correct |
| 404 page par défaut | Pas de JAR livré / mauvais chemin | Liste déploiements et startup command | Déployer JAR en app.jar puis vérifier |
| log tail reste ouvert | Fonctionnement normal d'un flux | Observer stdout puis provoquer GET | Ctrl+C après preuve, timeout côté requête |
| métriques vides | Délai d'ingestion ou mauvaise fenêtre | Vérifier ressource, fenêtre UTC et granularité | Attendre quelques minutes ou noter limite |
| destroy refuse RG non vide | Ressource étrangère dans RG | az resource list -g GROUPE -o table | Identifier avec encadrant, ne pas désactiver protection |
| RG existe après delete --no-wait | Suppression asynchrone | group wait --deleted et group exists | Attendre fin, consigner échec/timeout pour suivi |
| Plus de tfstate | État perdu, ressources toujours actives | Vérifier groupe Azure et sauvegarde locale | Récupérer backup ; sinon nettoyage encadré du groupe exact |

Une création partielle peut laisser le plan B1 facturé. Même si l'app échoue, conserver l'état et lancer la destruction du périmètre réellement créé. Ne pas recommencer avec dix identifiants en laissant dix plans actifs.
