# Évaluation 02 — 20 points

| Critère | Points | Preuve attendue |
|---|---:|---|
| Java autonome et tests utiles | 3 | clean verify, 8 cas, JAR local et contrat |
| HCL lisible, variables validées et provider4 explicite | 4 | .tf, fmt/validate ; aucun état suivi |
| Périmètre trois ressources, sécurité et outputs réels | 3 | Plan expurgé et explication RG/plan/app |
| Plan enregistré relu puis apply manuel | 2 | 3 créations, abonnement/région vérifiés |
| Déploiement JAR et HTTPS opérationnel | 3 | Trace CLI expurgée, URL et réponses contractuelles |
| Logs, métrique et interprétation mesurée | 2 | Une corrélation requête/log, fenêtre métrique ou limite documentée |
| Nettoyage complet vérifié | 2 | destroy et group exists=false ; aucun plan payant laissé |
| Journal et limites des preuves | 1 | SHA, versions et bilan d'idempotence |
| **Total** | **20** | |

Aucun point de nettoyage si seule une commande lancée est fournie sans vérification de fin. Un résultat local ne vaut pas preuve cloud. Si la plateforme est officiellement indisponible, l'encadrant peut redistribuer les 9 points apply/deploy/observabilité/nettoyage sur plan, runbook de diagnostic et justification des contrôles ; mentionner « Azure non exécuté » et conserver le barème alternatif choisi avant notation.

Critères d'acceptation : Maven vert ; format/validation Terraform ; plan sans opérations hors périmètre ; URL issue d'output ; contrat HTTPS ; différence infra/code expliquée ; état et plans privés ; groupe absent à la fin. Les captures ne doivent exposer ni jeton, ni e-mail, ni numéro d'abonnement.
