# Sécurité du laboratoire

Cette API est une démonstration publique sans comptes, données personnelles, base de données ou vraie cotation. Les prix sont fictifs. Ne pas la réutiliser comme service de production : absence d'authentification, de quotas, de supervision durable et de protection contre les abus.

Ne jamais committer mot de passe, jeton, profil de publication, clé privée, fichiers `.env`, état Terraform ou plan binaire. Les preuves doivent être expurgées : supprimer identifiants d'abonnement, e-mails et jetons. Un fichier ignoré déjà suivi par Git reste suivi ; contrôler l'index avant chaque commit. Si un secret fuit, le révoquer avant de nettoyer l'historique et prévenir l'encadrant par son canal habituel.

Les Actions du laboratoire ne demandent que `contents: read`. Ne pas employer `pull_request_target` pour exécuter le code d'un contributeur. Aucun déploiement cloud depuis une pull request ou un fork. Les mises en production et autorisations RBAC ne font pas partie de ce laboratoire.

Un incident pédagogique se signale en privé à l'encadrant ; une issue publique décrit uniquement le symptôme sans secret ni donnée personnelle.

## Périmètre Azure

Utiliser une souscription pédagogique et un unique RG réservé à cet atelier. Les droits sont préparés par l'encadrant ; aucun rôle Owner, identité persistante ou profil de publication créé. L'app publique ne contient que données fictives. HTTPS est exigé, FTP et authentifications basiques de publication sont désactivés. La session CLI authentifiée réalise manuellement apply et deploy après revue.

Les états/plans Terraform peuvent contenir des données sensibles : ne pas les committer, uploader ou inclure dans le journal. Ne pas supprimer l'état avant la destruction. Consulter les coûts B1 avant création, puis supprimer les trois ressources même après échec partiel. Un arrêt de l'app ou une alerte budget ne met pas fin à la facturation du plan.
