# Sources officielles et décisions techniques

Vérification documentaire : **3 octobre 2026**. Les versions ci-dessous sont des choix reproductibles du laboratoire, pas une promesse d'être les dernières. Vérifier les références avant une nouvelle promotion. Pages consultées :

| Source primaire | Utilisation dans le laboratoire |
|---|---|
| [Oracle — HttpServer Java 17](https://docs.oracle.com/en/java/javase/17/docs/api/jdk.httpserver/com/sun/net/httpserver/HttpServer.html) | serveur HTTP embarqué, contexte et arrêt |
| [Maven — cycle de vie](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html) | test, package, verify et clean |
| [Maven — Surefire](https://maven.apache.org/surefire/maven-surefire-plugin/) | découverte des tests et rapports XML/TXT |
| [JUnit — guide](https://docs.junit.org/5.12.2/user-guide/) | assertions, before/after et tests Jupiter |
| [GitHub — Java avec Maven](https://docs.github.com/en/actions/tutorials/build-and-test-code/java-with-maven) | runner, installation JDK et cache Maven |
| [actions/setup-java](https://github.com/actions/setup-java) | major v6 documentée, Java17 et permissions contents:read |
| [actions/checkout](https://github.com/actions/checkout) | major v7 documentée pour runner hébergé |
| [actions/upload-artifact](https://github.com/actions/upload-artifact) | major v7 documentée, rapports et durée de rétention |
| [GitHub — règles de branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches) | statut CI requis, disponibilité selon offre et dépôt |

Le projet cible Java17, JUnit5.12.2, compiler3.14.0, surefire3.5.3 et jar3.4.2. Les majors d'Actions simplifient l'entretien ; un dépôt professionnel fixe des SHA complets vérifiés puis organise leurs mises à jour. Aucun SHA fictif. Les commandes sont prévues pour GitHub.com et runner hébergé ubuntu-24.04 ; GitHub Enterprise peut supporter d'autres majors.

## Sources Azure et Terraform

| Source primaire | Décision vérifiée |
|---|---|
| [Azure CLI webapp deploy](https://learn.microsoft.com/en-us/cli/azure/webapp?view=azure-cli-latest#az-webapp-deploy) | src-path, type jar, target-path, restart et track-status |
| [App Service — Java Linux](https://learn.microsoft.com/en-us/azure/app-service/configure-language-java-deploy-run?pivots=platform-linux) | JavaSE pour un JAR exécutable et runtime Java17 |
| [App Service — variables](https://learn.microsoft.com/en-us/azure/app-service/reference-app-settings) | SERVER_PORT port à écouter, warmup et hostname réel |
| [App Service — désactiver auth basique](https://learn.microsoft.com/en-us/azure/app-service/configure-basic-auth-disable) | Déploiement CLI via Microsoft Entra, CLI récente |
| [AzureRM — Linux Web App](https://registry.terraform.io/providers/hashicorp/azurerm/4.50.0/docs/resources/linux_web_app) | Schéma provider4 : Java stack, logs, TLS et outputs |
| [AzureRM — provider4](https://registry.terraform.io/providers/hashicorp/azurerm/4.50.0/docs) | subscription_id explicite et resource_provider_registrations |
| [Terraform — state](https://developer.hashicorp.com/terraform/language/state) | État nécessaire, backend et relation aux ressources |
| [Terraform — plan](https://developer.hashicorp.com/terraform/cli/commands/plan) | -out et -destroy, plan binaire privé |
| [Azure CLI — group](https://learn.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest) | exists et wait --deleted pour finir nettoyage |
| [Azure App Service — tarification Linux](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/) | Coût à vérifier, aucun montant inventé |

Terraform1.9.8 peut servir de version de référence ; contrainte >=1.9,<2. Provider **4.50.0 exact** limite les variations de schéma ; committer .terraform.lock.hcl généré par init. Les pages Registry ne sont pas toutes extractibles par l'outil de recherche ; le provider a été installé avec signature vérifiée et fmt contrôlé. La commande validate a été tentée, mais son plugin est bloqué ici par une restriction de socket Unix de cet environnement ; elle doit être exécutée sur le poste de cours. La validation locale ne prouve pas un déploiement cloud et ne remplace pas les tests de terrain de l'encadrant.
