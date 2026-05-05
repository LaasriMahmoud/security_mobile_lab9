# Risques liés aux Activities exportées

## V1 — MainActivity exportée sans protection

**Composant** : `jakhar.aseem.diva.MainActivity`  
**Sévérité** : Élevée  
**Confiance** : Élevée

### Description
La MainActivity est accessible via intent explicite ou implicite sans aucune permission requise. Bien que l'activité principale doive être exportée pour le launcher, l'absence de contrôle d'accès la rend accessible depuis n'importe quelle application.

### Impact
Contournement potentiel des flux d'authentification si la MainActivity est conçue pour servir de point de contrôle.

### Référence OWASP MASVS
`MSTG-PLATFORM-1` : L'application ne doit exposer que les composants Android nécessaires.

---

## V2 — APICredsActivity exposée (Critique)

**Composant** : `jakhar.aseem.diva.APICredsActivity`  
**Sévérité** : Critique  
**Confiance** : Élevée

### Description
L'activité répondant à l'intent `VIEW_CREDS` est exportée sans permission. Elle expose des credentials API en clair à toute application tierce pouvant émettre cet intent.

### Impact
Fuite de credentials API confidentiels sans authentification.

### Référence OWASP MASVS
`MSTG-PLATFORM-1`, `MSTG-STORAGE-2`

---

## V3 — APICreds2Activity exposée (Critique)

**Composant** : `jakhar.aseem.diva.APICreds2Activity`  
**Sévérité** : Critique  
**Confiance** : Élevée

### Description
Identique à V2, avec une deuxième activité répondant à l'intent `VIEW_CREDS2`.

### Référence OWASP MASVS
`MSTG-PLATFORM-1`, `MSTG-STORAGE-2`
