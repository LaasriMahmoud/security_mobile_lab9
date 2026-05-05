# Checklist de fin d'audit — Lab 9 Drozer
**Application** : DIVA (Damn Insecure and Vulnerable Android App)  
**Package** : `jakhar.aseem.diva`  
**Auditeur** : Laasri Mahmoud  
**Date** : 05/05/2026

---

## ✅ Conformité de l'audit

- [x] Toutes les étapes du lab ont été suivies
- [x] Tous les composants Android ont été analysés (activities, services, receivers, providers)
- [x] Le tableau de triage est complet (8 vulnérabilités identifiées)
- [x] Les remédiations proposées sont spécifiques et applicables
- [x] Le mapping OWASP MASVS est correct pour chaque vulnérabilité

## ✅ Absence de données sensibles

- [x] Aucune donnée utilisateur réelle n'est présente dans le rapport
- [x] Aucun mot de passe ou clé n'est inclus dans le rapport
- [x] Les captures d'écran ne contiennent pas d'informations sensibles réelles
- [x] Les chemins système complets ont été anonymisés dans le rapport
- [x] Les identifiants personnels ont été supprimés

## ✅ Qualité du rapport

- [x] Le rapport est bien structuré avec sections exécutives et techniques
- [x] Les vulnérabilités sont clairement expliquées avec scénarios d'abus
- [x] Les recommandations sont précises et actionnables avec exemples de code
- [x] La documentation est complète avec captures d'écran référencées
- [x] Le format des livrables est conforme aux attentes (README.md, triage.csv, checklist_fin.md)

## ✅ Captures d'écran documentées

| Capture | Étape | Description |
|---------|-------|-------------|
| 1.png | Étape 1 | Vérification environnement — `drozer`, `adb version`, `adb devices` |
| 2.png | Étape 1 | Installation Drozer Agent via `adb install drozer-agent.apk` |
| 3.png | Étape 1 | Émulateur avec Drozer Agent activé (Embedded Server ON, port 31415) |
| 4.png | Étape 1 | Port forwarding `adb forward tcp:31415 tcp:31415` |
| 5.png | Étape 2 | Connexion console Drozer — `drozer console connect` |
| 6.png | Étape 2 | Liste des modules Drozer disponibles — `dz> list` |
| 7.png | Étape 3 | Liste des packages installés — `run app.package.list` |
| 8.png | Étape 3 | Filtrage app vulnérable dans la liste |
| 9.png | Étape 3 | Informations détaillées DIVA — `run app.package.info -a jakhar.aseem.diva` |
| 10.png | Étape 3 | Activities exportées — `run app.activity.info -a jakhar.aseem.diva` |
| 11.png | Étape 3 | Services exportés — `run app.service.info -a jakhar.aseem.diva` |
| 12.png | Étape 3 | Content Providers — `run app.provider.info -a jakhar.aseem.diva` |
| 13.png | Étape 4 | Manifeste complet — `run app.package.manifest jakhar.aseem.diva` |
| 14.png | Étape 4 | Intent-filters des activités — `run app.activity.info -a jakhar.aseem.diva -i` |
| 15.png | Étape 4 | URIs accessibles — `run scanner.provider.finduris -a jakhar.aseem.diva` |
| 16.png | Étape 4 | URIs référencées — `run app.provider.finduri jakhar.aseem.diva` |

---

> **Résultat global** : Audit complété avec succès. 8 vulnérabilités identifiées, toutes documentées avec remédiations adaptées et mapping OWASP MASVS.
