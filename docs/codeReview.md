# Code review

## Branching model

> TODO documenter pleinement le branching model

* 1 feature = 1 branche
* Respecter la convention de nommage des branches
* Pas de push force pendant la phase de relecture
* Push force **APRES** validation et **AVANT** merge pour nettoyer l’historique (enlever notamment les A/R relecture/fixes)
* Merge des pull requests sur github en mode squash (pour éviter les commits de merge)

## Guide du relecteur

* Utiliser le dispositif de review de Github
* Convention / CS (**C**heck**S**tyle/ **C**oding **S**tyle) & Typos
  * à corriger par le relecteur si ce sont les seuls retours (à notifier quand même)
* Robustesse
  * Traquer les sanity checks et type hintings manquants
  * Chercher des cas à la marge non supportés (en cas de doute, posez la question en commentaire, le relecteur a le droit à l’erreur car il doit lire vite, le raisonnement peut se faire en collaboration avec l’auteur de la PR)
* Conception
  * Traquer la surcomplexité (à la ligne, des structures)
  * Méthodes/classes trop longues
  * Copier / Coller ou assimilé
  * Code smells : singletons, héritage abusif, god classes, trop de paramètres, …
  * Couplage (injection manquante, dépendance à une implémentation plutôt qu’une abstraction, …)
* Lisibilité
  * Nommage des fonctions et des variables
  * Commentaires inutiles
* Performances
  * Dégradation du temps de traitement
  * Volumes (requêtes sans limites, écriture fichier sans mode stream, tableau/collection stockés en mémoire...)
* Exploitabilité
  * Présence de logs
  * Valeurs de configuration
  * Pas d'écriture en direct sur le filesystem


## Guide du relu

* Répondre à TOUS les commentaires. En cas d’approbation, répondre “Fixed <hash>” pour faciliter le suivi
