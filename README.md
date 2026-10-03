# Atelier 02 · Terraform et Azure App Service — élève

**4 h 30 · Bac+4/5 · Java17, Terraform et Azure CLI.** Déclarer un environnement reproductible, déployer un JAR fictif, observer les requêtes et vérifier sa destruction. Atelier autonome : l'atelier 01 n'est pas un prérequis.

Ce dépôt fournit mission, concepts et modèles de réponses seulement. Source Java, tests, HCL et commandes de création restent à réaliser dans votre copie de travail.

## Commencer

1. [Préparer outils, droits, abonnement et budget](docs/PREREQUIS.md) **avant** séance.
2. [Suivre le parcours 270 minutes](docs/ATELIER.md).
3. [Lire les concepts et raisons](docs/MEMOS.md).
4. [Diagnostiquer par couche](docs/DEPANNAGE.md).
5. [Rendre les preuves du barème](docs/EVALUATION.md) dans [le journal](travail/REPONSES.md).

Le corrigé complet est un dépôt séparé, communiqué après restitution.

## Coût et périmètre

Un seul RG, un plan Linux **B1 payant** et une Web App JavaSE17. Financement/région/quotas confirmés avant apply ; aucun prix fixe annoncé. Prévoir la destruction du plan, même si le déploiement échoue. Ne jamais committer état ou plan Terraform. La vérification HTTPS intervient après déploiement du JAR, pas seulement après création du runtime.

[Sécurité pédagogique](SECURITY.md) · [Sources officielles](docs/SOURCES.md) · MIT, Curtys ACCIPE 2026.

Dépôt de référence : https://github.com/pilotcrew-io/azdevops-02-terraform-azure-eleve

## Parcours GitHub et remise

Lire [CONTRIBUTING.md](CONTRIBUTING.md) pour le fork, la branche, la pull request et les preuves attendues.
