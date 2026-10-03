# Mémos 02 — Infrastructure, état, déploiement et exploitation

## Déclaratif et graphe

Terraform décrit une configuration souhaitée. Les références entre ressources lui donnent un graphe : la Web App dépend du plan, le plan dépend du groupe. Le moteur ordonne les créations puis inverse cet ordre lors de la destruction. Le code ne doit pas être un script qui crée arbitrairement tous les services du compte. Une référence `resource_group.lab.name` exprime un lien plus fiable qu'un texte répété.

| Concept | Ce qu'il contient | Pourquoi il importe |
|---|---|---|
| Configuration .tf | État souhaité en HCL | Relisible, versionné, réutilisable |
| Provider AzureRM | Schéma/API des ressources Azure | Une version majeure change exigences et comportement |
| Variables | Entrées contrôlées | Nom unique et abonnement explicite |
| Outputs | Valeurs calculées utiles | Évite recopies et hostname supposé |
| État tfstate | Correspondance adresses HCL ↔ IDs réels | Nécessaire à plan, mise à jour et destruction |
| Plan enregistré | Proposition concrète d'opérations | Appliquer ce qui a été relu |
| .terraform.lock.hcl | Version et empreintes provider | À committer après init réussi |

Le provider4 reçoit explicitement subscription_id. Le cours désactive son enregistrement automatique de providers : Microsoft.Web est préparé avant atelier. Cela évite de demander une permission supplémentaire au moteur pendant un simple plan.

## Cycle de travail

fmt harmonise l'écriture ; validate vérifie la structure après init et installation du provider. Aucun des deux ne valide coûts, quotas ou politiques Azure. init prépare backend et plugins ; plan compare configuration, état et Azure ; apply réalise les opérations prévues. Avec un plan binaire enregistré, apply utilise ses valeurs : si les entrées changent, recalculer et relire le plan. Un plan contient des données sensibles potentielles et n'est pas un artefact public.

Un plan sans modification après apply montre convergence de ce périmètre à cet instant. Il ne prouve pas absence de modification future ni absence de ressources non suivies. La dérive apparaît lorsqu'une modification du portail entre en conflit avec le code. Dans cet atelier on garde la configuration des logs dans Terraform pour éviter un réglage CLI concurrent.

## État local : simplicité et risque

Backend local est une décision pédagogique pour une seule personne sur un poste. L'état peut contenir des valeurs sensibles même si les variables sont marquées sensitive ; ce marquage masque certains affichages, pas le stockage. Ignorer état, sauvegardes et plans, conserver le dossier de travail jusqu'à nettoyage et ne pas le synchroniser publiquement.

Deux personnes ne doivent pas appliquer concurremment avec deux copies locales indépendantes. Pour une équipe réelle, backend distant avec verrouillage, accès limité, chiffrement et sauvegarde ; configuration hors temps du laboratoire. Perdre l'état ne supprime rien dans Azure. L'import ou la suppression contrôlée exige une analyse ; un nouvel apply ne doit pas être le réflexe.

## App Service : qui paie et qui exécute ?

Le RG organise les ressources et le périmètre. Le plan App Service réserve la capacité de calcul et porte la facturation. La Web App utilise cette capacité, possède URL/configuration et reçoit le JAR. Supprimer ou arrêter seulement l'app peut laisser le plan payant. B1 est un choix pour JavaSE/Always On dans ce cours, soumis à disponibilité et quotas ; aucune promesse de gratuité.

App Service termine HTTPS devant le processus Java. L'application doit écouter l'adresse et le port attendus : 0.0.0.0 et SERVER_PORT fourni par la plateforme. En local on utilise PORT puis 8080. Écouter uniquement localhost empêche le proxy d'atteindre le service. Un startup command explicite évite de dépendre d'une convention implicite de nom du JAR.

## Infrastructure et charge utile

Terraform prépare le runtime JavaSE17 et les paramètres ; CLI dépose le JAR testé. Le code de l'API n'est pas dans l'état Terraform. Le second plan peut donc rester vide après une nouvelle livraison du JAR. Le journal doit relier SHA des sources, test Maven, binaire et résultat HTTP. Une pratique avancée stockerait et signerait les artefacts, puis automatiserait promotion avec identité fédérée et approbation d'environnement.

Aucune identité cloud n'est confiée à une PR. La session Azure CLI de l'élève utilise des droits déjà préparés pour son laboratoire. Un refus d'accès se résout avec l'encadrant au bon périmètre ; attribuer Owner à toute la souscription ne fait pas partie de la solution.

## Observabilité minimale

Les logs route/status/duration répondent « que s'est-il passé pour cette requête ? ». Les métriques agrégées montrent volume, erreurs et consommation sur une fenêtre. La santé /health aide le runtime à vérifier qu'il répond. Ces signaux ne sont pas équivalents : une route unknown à 404 produit un log normal d'erreur client, pas une panne du service. La durée d'une seule requête ne prouve pas un objectif de disponibilité ou un SLA.

Ne pas journaliser corps, query strings, jetons ou adresses de consommateurs dans ce cours. Filesystem et rétention courte sont pratiques pour une séance, pas une politique d'audit. L'atelier n'inclut ni Application Insights, ni alertes durables, ni collecte distribuée : les nommer comme améliorations futures, sans les prétendre réalisées.

## Nettoyage et portée

Préférer plan -destroy puis apply du plan relu. La protection provider sur le RG refuse sa suppression s'il contient des ressources ajoutées hors Terraform ; cette erreur révèle une ressource étrangère qu'il faut examiner. Une commande group delete peut détruire tout le groupe : le runbook ne l'emploie qu'après contrôle des noms/tags et en dernier recours. --no-wait signifie lancement asynchrone, jamais fin de suppression. Vérifier groupe absent et conserver état jusqu'à cette preuve.
