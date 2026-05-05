# Risques liés aux Content Providers — DIVA

## V4 — NotesProvider exposé sans permission (Critique)

**Composant** : `jakhar.aseem.diva.NotesProvider`  
**Authority** : `jakhar.aseem.diva.provider.notesprovider`  
**Sévérité** : Critique  
**Confiance** : Élevée

### Description
Le Content Provider `NotesProvider` est exporté sans aucune permission de lecture ou d'écriture (`Read Permission: null`, `Write Permission: null`). Les URIs `/notes` et `/notes/` sont confirmées accessibles par le scanner Drozer sans aucune authentification.

### URIs vulnérables confirmées
```
content://jakhar.aseem.diva.provider.notesprovider/notes
content://jakhar.aseem.diva.provider.notesprovider/notes/
```

### Scénario d'abus
Une application malveillante installée sur le même appareil peut :
1. **Lire** toutes les notes privées de l'utilisateur (requête SELECT)
2. **Modifier** des notes existantes (UPDATE)
3. **Injecter** de fausses données (INSERT)
4. **Supprimer** des données utilisateur (DELETE)

### Impact
- Fuite de données privées de l'utilisateur
- Corruption de données
- Atteinte à la vie privée (RGPD)

### Référence OWASP MASVS
`MSTG-STORAGE-2` : Les données sensibles ne doivent pas être stockées sans protection adéquate.  
`MSTG-PLATFORM-2` : Les entrées des sources externes doivent être validées.

### Remédiation
```xml
<!-- AndroidManifest.xml -->
<provider
    android:name=".NotesProvider"
    android:authorities="jakhar.aseem.diva.provider.notesprovider"
    android:exported="false" />

<!-- Si export nécessaire -->
<provider
    android:name=".NotesProvider"
    android:authorities="jakhar.aseem.diva.provider.notesprovider"
    android:exported="true"
    android:readPermission="jakhar.aseem.diva.permission.READ_NOTES"
    android:writePermission="jakhar.aseem.diva.permission.WRITE_NOTES" />
```
