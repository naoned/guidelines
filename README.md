# Développer chez Naoned

## Coder avec les standards

- Appliquez PSR-2 (http://www.php-fig.org/psr/psr-2/)
- Utilisez un bon éditeur extensible (Sublime Text 3, phpStorm)
 - Utilisez des extensions et des fonctionnalités qui simplifient la vie : PHPCS pour le PSR-2, Git, Manipulation de fichier, autocompletion, PHPDoc …
- Utilisez les conventions de Naoned :
 - Pas de tabulation, indentation de 4 espaces (2 pour le html et le twig)
 - Classe, méthodes, variables, arguments en camelCase
 - 120 caractères par ligne

## Éprouver son code

- Mettre en place de la revue de code par l’utilisation de pull requests (http://blog.octo.com/revue-de-code-quel-format-choisir/) (ce point est à préciser)
- Écrire les tests en même temps que le code (ce point est à préciser)
- Faire l’expercience du pair-coding : coder à 4 mains et 2 cerveaux (ce point est à préciser)

## Git et le versionning

- Configurez Git : https://help.github.com/articles/set-up-git
- Ajoutez votre clé publique : https://help.github.com/articles/generating-ssh-keys
- Utilisez le modèle de «branchage» décrit ici : http://nvie.com/posts/a-successful-git-branching-model/
- Appliquez une branche de feature à sa branche de départ lors d’une séance de revue de code, sous Github, en utilisant le mécanisme de la «pull request»
- Faites des messages de commit explicite en anglais, sans être trop long, préfixé du numéro d’issue le cas échéant :
 * ~~Fix a bug in discover~~
 * ~~Remove event attached to body so the user can move the blocks as he saves and move them back again in application to issue 1880~~
 * #1880 Fix discover block sort bug, due to crop events on body

## Rester au jus

- Soyez attentif aux bonnes pratiques − http://www.phptherightway.com est une bonne ressource
- Suivez l’actualité de vos domaines favori : twitter, flux rss …
- Ajoutez les informations qui vous semblent nouvelles ou importantes dans https://plus.google.com/u/0/ avec le hashtag #naotech
- Assistez à des conférences, il y en a de bonnes ici : https://www.youtube.com/user/afupPHP Proposez aussi vos découverte sur Google+ !
- Un chan (IRC ?) sera mis en place pour partager des idées, demande de l’aide ou un avis, travailler collaborativement entre développeurs

## Humilité et respect

- Vous ferez du mauvais code
- Acceptez les critiques et les remarques
- Préferez l’**action** à la critique stérile
- Votre meilleure code d’aujourd’hui sera un boulet demain

## Ecrire du code pour les autres

- Utilisez PHPDoc http://fr.wikipedia.org/wiki/PHPDoc
- Un bon code, c’est comme une bonne blague… il n’a pas besoin d’explication.
- En commentaire, précisez «pourquoi» une action est faite, plutôt que «comment»
- Vous lirez plus de code que vous n’en produirez
- Préférez être clair plutôt qu’être brèf
- Exposer uniquement ce qui est vraiment nécessaire et le faire explicitement (méthode privées)
- «La complexité n'a rien à voir avec l'intelligence, la simplicité, si."
- «Code toujours comme si le mec qui fait la maintenance est un psychopathe violent qui sait où tu habites.»
- Pendant la revue de code, essayez de deviner ce que les classes et les méthodes font juste par leurs noms, et vérifiez votre hypothèse. En cas d’erreur, proposez un meilleur nom.

## Communiquer clairement et sans retenue

- Mettez-vous dans la tête de votre interlocuteur
- Préférez les **discussions en chat ouverts** plutôt que privées
- Partagez chaque idée ou nouvelle orientation très tôt

## Soyez concret

- Mettre l'accent sur la prestation **pour le client**
- [YAGNI] (http://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) Écrivez votre code pour votre besoin **actuel et spécifique**, Ne jamais écrire du code pour des raisons de «peut-être demain, vous en aurez besoin".
- [KISS] (http://en.wikipedia.org/wiki/KISS_principle) Ne pas utiliser des fonctions de programmation fantaisiste *juste parce que vous en êtes capable*. Utilisez les fonctions de programmation fantaisiste parce qu'ils ont un **avantage démontrable** spécifique pour le problème que vous essayez de résoudre.
- Toujours fournir un cas d'utilisation concret avec une spécification
- Savoir quand (ne pas) optimiser
- Logguer votre code pour retracer les actions utilisateurs et débusquer les bugs

## Concevoir le changement

- Vous ne pouvez pas savoir *ce qui va changer*, mais vous pouvez être sûr que cela **va changer**.
- Mieux vaut réaliser plusieurs petites choses qu’un grande
- Dépendre des choses qui changent le moins souvent
- DRY − Programmer du code réutilisable
- «La conception est plus l'art de conserver de la flexibilité que celui d'atteindre la perfection."
- Séparer les responsabilités au maximum, une fonction ne fait qu’une seule chose, mais elle la fait bien. Une classe ne traite que d’un objet, mais elle le fait bien.
- Standardiser les échanges. Une méthode ou une fonction doit toujours renvoyer un même type de résultat

## Être responsable

- Estimez et planifiez avec précision, découpez en plus petites tâches si nécessaire.
- Autant que possible, livrez les fonctionnalité finalisés et à temps, prévenir au plus tôt en cas de délai
- Prévoyez l'impact que votre code peut avoir sur l'environement
- Toujours avoir l’**utilisateur final** à l'esprit

## Soyez vous-même

- Vous êtes ici pour une bonne raison :)
- Contribuez à une ambiance agréable

## Contest
````
 _   _    __    __    __      _____  ____    ____  __    ____  __   
( )_( )  /__\  (  )  (  )    (  _  )( ___)  ( ___)/__\  (_  _)(  )  
 ) _ (  /(__)\  )(__  )(__    )(_)(  )__)    )__)/(__)\  _)(_  )(__ 
(_) (_)(__)(__)(____)(____)  (_____)(__)    (__)(__)(__)(____)(____)
````
- Une compétition s’offre à tous : débusquer les pires algorithmes et les plus vilaines tournure de code ! L’occasion de se faire payer un coup par le lauréat, et de aussi de rire un peu, sans tomber dans le lynchage bien sûr ! Modalités à venir.

---
Inspirations et crédits:

- https://signalvnoise.com/posts/3250-clarity-over-brevity-in-variable-and-method-names
- http://blog.codinghorror.com/kiss-and-yagni/
- http://www.articpost.com/best-programming-quotes-that-every-developer-should-know/
- https://github.com/iadvize/guidelines
- http://symfony.com/fr/doc/current/contributing/code/standards.html
