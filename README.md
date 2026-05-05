# 🛡️ Lab 9 : Audit de Sécurité Android Approfondi avec Drozer

## 📋 Présentation du Projet
Ce laboratoire constitue une étude de cas pratique sur l'analyse de la surface d'attaque d'une application Android. Nous utilisons **Drozer**, un framework de test de sécurité robuste, pour auditer l'application **DIVA (Damn Insecure and Vulnerable App)**. 

L'audit se concentre sur l'exploitation des mauvaises configurations des composants inter-processus (IPC) d'Android, qui sont souvent le maillon faible de la sécurité mobile.

---

## 🎯 Objectifs Pédagogiques Détaillés
*   **Compréhension du Modèle de Sécurité Android** : Analyse du système de permissions et de l'isolation des applications (Sandbox).
*   **Maîtrise de Drozer** : Utilisation du modèle Client-Serveur (Console sur PC vs Agent sur Émulateur).
*   **Évaluation des Risques IPC** : Comprendre comment les Intentions (Intents) peuvent être détournées pour accéder à des données privées.
*   **Conformité OWASP** : Appliquer les standards industriels **MASVS** (Mobile Application Security Verification Standard).

---

## 🛠️ Architecture du Lab et Configuration

### Le Fonctionnement de Drozer
Drozer fonctionne via un agent installé sur le terminal Android qui agit comme une application "privilégiée" capable d'interagir avec les autres packages du système. La console sur la machine hôte envoie des commandes à cet agent pour interroger les APIs Android.

![Vérification ADB et Drozer](1.png)
*Figure 1 : Initialisation de l'environnement. ADB est le pont de communication, et Drozer est le moteur d'analyse.*

![Installation Drozer Agent](2.png)
*Figure 2 : Déploiement de l'agent. Cette étape est cruciale car l'agent servira de relais pour toutes nos requêtes d'audit.*

![Activation Drozer Agent](3.png)
*Figure 3 : L'agent doit écouter sur le port 31415. C'est ici que nous activons le serveur embarqué.*

![Port Forwarding](4.png)
*Figure 4 : Le "Port Forwarding" redirige le trafic réseau de votre PC vers l'émulateur, permettant à la console Drozer de "voir" l'agent.*

---

## 🔍 Exploration Technique de l'Application

### Connexion et Reconnaissance
La première étape consiste à établir le canal de communication et à identifier notre cible.

![Connexion Drozer](5.png)
*Figure 5 : Connexion établie. Drozer récupère les informations de base du système Android.*

![Infos Package](9.png)
*Figure 9 : Analyse statique via Drozer. On observe les permissions demandées. Notez la présence de `WRITE_EXTERNAL_STORAGE`, ce qui indique que l'app écrit potentiellement des données sur la carte SD, un emplacement non sécurisé.*

### 📂 Focus sur les Content Providers (Vecteur Critique)
Les Content Providers sont utilisés pour partager des données entre applications. S'ils sont mal configurés, ils deviennent des portes ouvertes.

![Providers Exportés](12.png)
*Figure 12 : On découvre que `NotesProvider` est `exported=true`. Sans permissions définies (`null`), n'importe quelle application malveillante peut agir comme une application "amie" et lire vos notes.*

![Scanner URIs](15.png)
*Figure 15 : Le scanner Drozer tente de deviner les chemins (URIs). Il confirme que `/notes` renvoie des données. C'est une preuve de vulnérabilité directe.*

---

## ⚠️ Analyse des Risques et Scénarios d'Attaque

### 1. Intent Spoofing (Activités Exportées)
L'application expose `APICredsActivity`. Un attaquant pourrait créer une application simple qui envoie un Intent pour lancer cette activité. 
*   **Impact** : L'utilisateur voit soudainement ses clés API à l'écran, ou pire, l'application malveillante récupère ces clés en arrière-plan si l'activité les renvoie dans un résultat.

### 2. Fuite de Données via Content Providers
Le `NotesProvider` ne vérifie pas qui l'appelle.
*   **Scénario d'attaque** : Une application de "lampe torche" malveillante pourrait faire une requête SQL `SELECT * FROM content://jakhar.aseem.diva.provider.notesprovider/notes` et envoyer tout le contenu de vos notes vers un serveur distant.

### 3. Risques du Flag `debuggable="true"`
Ce flag permet à un auditeur (ou un attaquant) d'utiliser `jdb` (Java Debugger) pour s'attacher au processus de l'application.
*   **Risque** : Cela permet d'extraire des variables en mémoire, de modifier le comportement de l'app en temps réel et de contourner les vérifications de sécurité.

---

## 📊 Tableau de Triage et Priorisation

| ID | Composant | Vulnérabilité | Sévérité | Justification Technique |
|:---|:---|:---|:---|:---|
| **V1** | `NotesProvider` | Permission Read/Write absente | **Critique** | Accès total aux données utilisateur par des tiers. |
| **V2** | `APICredsActivity` | Activité sensible exportée | **Critique** | Fuite d'identifiants techniques hardcodés. |
| **V3** | Application | `debuggable="true"` | **Élevée** | Permet l'injection de code et le reverse engineering dynamique. |
| **V4** | Application | `minSdkVersion=15` | **Moyenne** | Supporte des versions d'Android obsolètes avec des failles Kernel connues. |

---

## 🛠️ Remédiations et Bonnes Pratiques (OWASP MASVS)

### Standard MSTG-PLATFORM-1 (Composants)
**Principe** : Ne jamais exporter un composant sauf si c'est strictement nécessaire pour l'interaction avec d'autres apps de confiance.
*   **Action** : Mettre `android:exported="false"` par défaut.

### Standard MSTG-STORAGE-2 (Données)
**Principe** : Les données sensibles doivent être protégées par des permissions de niveau `signature`.
*   **Action** : Si vous devez partager des données, assurez-vous que seule une application signée avec votre même certificat peut y accéder.

---

## 🏁 Conclusion de l'Audit
Ce laboratoire démontre que la sécurité Android ne repose pas uniquement sur le code Java, mais aussi grandement sur la **configuration du Manifeste**. L'utilisation de Drozer permet de simuler le comportement d'une application adverse et de fermer les vecteurs d'attaque avant que l'application ne soit publiée.

---
**Rapport réalisé par :** Laasri Mahmoud  
**Cadre :** Consultant Sécurité Mobile (Audit Défensif)
