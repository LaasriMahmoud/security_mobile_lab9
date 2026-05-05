# Risques liés aux Services — DIVA

## Analyse

Aucun service exporté n'a été détecté via Drozer pour le package `jakhar.aseem.diva`.

**Commande exécutée** :
```
dz> run app.service.info -a jakhar.aseem.diva
```

**Résultat** : `No exported services.`

## Conclusion

Ce vecteur d'attaque n'est **pas applicable** à DIVA dans sa version analysée. Le risque lié aux services exposés est **nul** pour cette application.

> **Note** : Bien que DIVA ne présente pas de services exportés, dans un audit réel, les services doivent être systématiquement vérifiés car ils peuvent permettre l'exécution de fonctions privilégiées à distance.
