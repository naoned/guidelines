# Convention SASS - CSS

<!-- MarkdownTOC -->

- [Généralités](#généralités)
- [Exemple](#exemple)
- [Syntaxe & formattage](#syntaxe--formattage)
    - [Css](#css)
    - [Sass](#sass)
- [Design patterns](#design-patterns)
- [Références](#références)

<!-- /MarkdownTOC -->

## Généralités

- Encodage UTF-8
- Indentation avec des espaces (4) - Tabulations proscrites
- Si possible faites du SASS et pas du CSS. [Learn SASS](http://sass-lang.com/guide)

## Exemple

Un exemple valant mieux qu'un long discours :

<details><summary>**./index.scss**</summary><p>
```sass
// One main entry file that imports everything
@charset 'utf-8'; // Force utf8 in the entry file

@import 'utils/variables';
@import 'utils/functions';

@import 'components/record-media-list';

@import 'layouts/pages/record';
```
<hr></p></details>

<details><summary>**./utils/_variables.scss**</summary><p>
```sass
// A color palette for the project
$palette: (
    // Name a color by its role not by its true color name.
    // Try to limit the number of colors in your palette.
    primary: (
        base: #5dc0d2,
        dark: darken(#5dc0d2, 10%)
    ),
    info: (
        base: #44b27b
    ),
    success: (
        base: #0dff1d
    ),
    text-primary: (
        base: #fff
    ),
    background: (
        base: #ccc,
        light: #ececec
    )
);

// Declare global variables in the variables file

// Always set font size using rem or em
$font-size-base: 1rem;

// Declare media queries as variables
$screen-xs: 30em;
$screen-sm: 48em;
$screen-md: 62em;
$screen-lg: 75em;

// Use relative units, preferably em or rem. Avoid px.
$spacer: 1rem;
```
<hr></p></details>

<details open><summary>**./components/_record-media.scss**</summary><p>
```sass
// Declare any variable at the top of the file
// Prefix the variable according to your component name
$record-media-spacing: $spacer / 2;

.record-media {
    height: 100%;
    text-align: center;
    overflow-y: scroll;
    overflow-x: hidden;
    // Repeated related values are placed in a variable
    padding-top: $record-media-spacing;
    padding-bottom: $record-media-spacing;
    position: relative;
    border: 1px solid transparent;
    // Nest pseudo classes and selectors using the & selector
    &.selected,
    &:selected {
        border-color: color('alert');
    }
    &:hover {
        // Use the Palette tints for slight color shifts.
        border-color: color('alert', 'light');
    }
}

.record-media-img {
    // Use the project color Palette
    background: color('background', 'light');
    display: inline-block;
    box-shadow: 0 0 1rem rgba(#000, 0.6);
    max-width: 100%;
}

.record-media-caption {
    background: #000;
    // Mobile first
    @media(min-width: $screen-md) {
        position: absolute;
        bottom: 0;
        left: 0;
        right: 0;
        display: none;
        &:hover {
            display: block;
        }
    }
    @media(min-width: $screen-lg) {
        display: block;
    }
}
```
<hr></p></details>

<details><summary>**./layouts/pages/_record.scss**</summary><p>
```sass
// A layout composes components
.record-page {
    display: flex;
    flex-direction: column;
    .record-media {
        // A Layout may override a component if it needs to
        border-color: color('info');
    }
    .viewer {
        flex-grow: 1;
    }
}
// A Layout may apply custom styles if needed
.record-page p:first-child {
    font-weight: bold;
}
```
<hr></p></details>

<details><summary>**./utils/_functions.scss**</summary><p>
```sass
// Color function that enables the use of the palette
@function color($color, $variant:'base'){
    @return map-get(map-get($palette, $color), $variant);
}
```
<hr></p></details>

## Syntaxe & formattage

### Css

<details><summary>Ne spécifiez pas le protocole pour les ressources liées</summary><p>
```css
/* Bad */
.example {
  background: url(https://www.google.com/images/example);
}

/* Good */
.example {
  background: url(//www.google.com/images/example);
}
```
<hr></p></details>

<details><summary>Ecrivez vos sélecteurs en **minuscules avec des tirets comme séparateurs**</summary><p>
```sass
// Bad
.my_class {}
.component__elem--modifier {} // By the way, don't use BEM

// Good
.my-class {}
.component-elem-modifier {}
```
<hr></p></details>

<details><summary>Limitez la profondeurs des sélecteurs, gardez vos sélecteurs les moins spécialisés possibles</summary><p>

Ecrire des sélecteurs qui prennent en compte plusieurs niveaux d'enfants augmente la dépendance entre le CSS et le HTML. Le CSS se spécialise à du HTML très particulier et réduit par conséquent la portée de son application.

Comme dans l'exemple ci-dessous, des sélecteurs trop spécialisés créent de la répétition car nous devons dupliquer le sélecteur pour l'adapter à un autre contexte.

La duplication des sélecteurs est un bon signe qu'une classe peut être créée pour uniformiser un style commun.

```sass
/* Bad */
.sidebar div, .footer div {
    border: 1px solid #333;
}

.sidebar div h3, .footer div h3 {
    margin-top: 5px;
}

.sidebar div ul, .footer div ul {
    margin-bottom: 5px;
}

/* Good */
.pod div {
    border: 1px solid #333;
}
.pod-title {
    margin-top: 5px;
}
.pod-list {
    margin-bottom: 5px;
}
```

En réduisant la [spécificité](https://developer.mozilla.org/en/docs/Web/CSS/Specificity) au strict minimum nous évitons aussi des problèmes liés à la personnalisation de ces règles dans d'autres contextes.
<hr></p></details>

<details><summary>Evitez les sélecteurs typés</summary><p>
```sass
// Bad
span.message {...}
// Good
.message {...}
```
<hr></p></details>

<details><summary>Ne jamais utiliser `!important`, gérez correctement la [spécificité des sélecteurs](https://developer.mozilla.org/en/docs/Web/CSS/Specificity)</summary><p>

`!important` rend la maintenabilité difficile en rendant les overrides complexes et imprévisibles.
<hr></p></details>

<details><summary>Utilisez des noms de classes qui ont du sens, qui sont génériques et qui sont aussi courts que possible</summary><p>
```sass
// Bad, meaningless
.yee-1901 {}
// Bad, green is too specific
.button-green {}

// Good, component name
.gallery {}
// Good, generic name
.button-success {}
```
<hr></p></details>

<details><summary>Evitez les #ids, gardez les sélecteurs [les moins spécifiques possibles](https://developer.mozilla.org/en/docs/Web/CSS/Specificity)</summary><p>

Les IDs augmentent la spécificité des sélecteurs et limitent la réutilisabilité du code. Vous écrivez peut-être votre code en pensant qu'il ne sera utilisé qu'une fois et qu'il faut donc utiliser une ID mais est-ce que çà restera vrai tout le temps ?
<hr></p></details>

<details><summary>Utilisez des commentaires pour améliorer la lisibilité de votre CSS</summary><p>

Le CSS n'est pas un language lisible. Si nécessaire utilisez les commentaires pour augmenter la lisibilité.
<hr></p></details>

<details><summary>Toutes les tailles de polices doivent être en `rem` ou `em`</summary><p>

Pour rendre un site web accessible un site web doit s'adapter à la préférence de l'utilisateur. En utilisant `em` et `rem` pour **toutes** les tailles de police, le site sera tributaire soit de la configuration du système de l'utilisateur ou de la configuration du navigateur. Dans les 2 cas cela induit qui si un utilisateur mal voyant qui a augmenté la taille de police de son système, votre site sera automatiquement adapté à son besoin.

Cela implique un design de site avec une certaine flexibilité prévue vis-à-vis de la taille des contenus.
<hr></p></details>

<details><summary>Privilégiez toujours des unités relatives `%`, `rem`, `em`, `vh`, `vw`,... à des pixels</summary><p>

Similairement aux tailles de polices, pour être accessible, votre site doit s'adapter aux préférences de l'utilisateur qui le visite. Il faut donc utiliser des tailles relatives plus tôt que des tailles en pixels.

Il y a cependant quelques cas où çà n'est pas possible :
- Des tailles fines de ~1px risqueraient de disparaître en utilisant des em/rem/etc... si une fois affichées elles font moins de 1px.
- Si un design de site n'est pas capable de s'adapter en largeur à des tailles variables il peut nécessiter des media queries en pixel.
- *D'autres exemples ?*

Par défaut les navigateurs utilisent une taille de police de 16px. On peut donc convertir des pixels en partant du principe que 1em = 16px.
<hr></p></details>

<details><summary>Si possible utilisez les propriétés raccourcies</summary><p>
```sass
/* Bad */
border-top-style: none;
font-family: palatino, georgia, serif;
font-size: 100%;
line-height: 1.6;
padding-bottom: 2em;
padding-left: 1em;
padding-right: 1em;
padding-top: 0;

/* Good */
border-top: 0;
font: 100%/1.6 palatino, georgia, serif;
padding: 0 1em 2em;
```
<hr></p></details>

<details><summary>N'utilisez pas d'unité avec la valeur 0</summary><p>
```sass
margin: 0;
padding: 0;
```
<hr></p></details>

<details><summary>Ecrivez les couleurs hexadécimales en minuscule</summary><p>
```sass
/* Bad */
color: #EEBBCC;

/* Good */
color: #eebbcc;

/* Better */
color: #ebc;
```
<hr></p></details>

<details><summary>N'ajoutez pas de prefix spécifiques aux navigateurs</summary><p>

Utilisez un transpiler pour préfixer automatiquement le CSS.
```sass
/* Bad */
.box_shadow {
    -webkit-box-shadow: 0px 0px 4px 0px #ffffff;
    box-shadow: 0px 0px 4px 0px #ffffff;
}

/* Good */
.box_shadow {
    box-shadow: 0px 0px 4px 0px #ffffff;
}
```
<hr></p></details>

<details><summary>Utilisez des simples cotes plus tôt que des doubles et aucune pour les valeur de url()</summary><p>
```sass
/* Bad */
@import url("//www.google.com/css/maia.css");

html {
  font-family: "open sans", arial, sans-serif;
}

/* Good */
@import url(//www.google.com/css/maia.css);

html {
  font-family: 'open sans', arial, sans-serif;
}
```
<hr></p></details>

### Sass

<details><summary>Structurez votre projet `Layouts`, `Components`, `Vendors`, etc...</summary><p>

Séparez les layouts et les composants. Un layout permet l'assemblage et la personnalisation de composants dans un contexte particulier.

Exemple :

```
sass/
|
|– utils/
|   |– _variables.scss    # Sass Variables
|   |– _functions.scss    # Sass Functions
|   |– _mixins.scss       # Sass Mixins
|   |– _placeholders.scss # Sass Placeholders
|
|– base/
|   |– _reset.scss        # Reset/normalize
|   |– _typography.scss   # Typography rules
|
|– components/
|   |– _buttons.scss      # Buttons
|   |– _carousel.scss     # Carousel
|   |– _cover.scss        # Cover
|   |– _dropdown.scss     # Dropdown
|
|– layouts/
|   |– _header.scss       # Header
|   |– _footer.scss       # Footer
|   |– _sidebar.scss      # Sidebar
|   |– pages/
|   |   |– _home.scss         # Home specific styles
|   |   |– _contact.scss      # Contact specific styles
|
|– vendors/
|   |– _bootstrap.scss    # Bootstrap import & overrides
|   |– _fontawesome.scss  # Fontawesome import & overrides
|
`– index.scss             # Main Sass file
```
<hr></p></details>

<details><summary>Préfixez de `_` les fichiers qui ne créeront pas de fichier css du même nom (partial)</summary><p>

Le SASS définit les `partials` comme des fichiers qui sont nécessairement importés dans d'autres fichiers et qui ne seront pas compilés vers un fichier css du même nom.

Bien que beaucoup de compilateurs de SASS forcent maintenant à déclarer précisément quels fichiers seront transformés en fichier css sans utiliser l'absence de `_` pour le deviner nous continuons à respecter la nomenclature.
<hr></p></details>

<details><summary>Toutes les variables, fonctions et mixins sont écrites en **minuscules avec des tirets comme séparateurs**</summary><p>
```sass
// Bad
$MyVariable: 1em;
@function GETWIDTH() { ... }
// etc...
```
<hr></p></details>

<details><summary>Préfixez vos noms de variables du nom de votre composant</summary><p>

- Les variables sont valables dans le scope dans lequel elles sont déclarées. Si une variable est déclarée hors d'un sélecteur elle est donc dans le scope global et peut donc rentrer en conflit avec d'autres variables. Préfixez la variable avec le nom du composant.
- Les variables peuvent être exposées dans un outil de configuration global. Soit simplement dans un fichier global ou dans un outil externe. Préfixez les variables de votre composant pour que même hors du contexte de votre fichier la variable garde son sens.

<hr></p></details>

<details><summary>Déclarez vos variables soit dans le fichier de variables globales de l'application ou en haut de votre fichier</summary><p>

La gestion des variables en SASS est complexe. D'un côté avoir des variables globales toutes déclarées dans un fichier permet d'avoir un fichier unique par lequel on peut rapidement reprogrammer le style d'un site. De l'autre côté trop de variables bloque la lisibilité.

- Soyez parcimonieux dans la création de variables et essayez de les créer uniquement pour exposer une configuration de votre composant ou layout.
- Déclarez vos variables soit en haut de votre composant/layout ou dans un fichier de variables globales pour que d'autres développeurs sachent où regarder pour reconfigurer votre composant/layout.

<hr></p></details>

<details><summary>Créez et uniformisez votre projet avec une palette de couleur</summary><p>

Il est important que le designer pense ses maquettes avec une palette de couleurs bien définie.

- Utilisez les couleurs de la palette du projet pour vos intégrations :<br>
En utilisant toujours la palette on peut facilement changer les couleurs de toute l'application.

- Nommez vos couleurs avec leur rôle le moins spécialisé possible :
```sass
$palette: (
    // Bad
    'media-viewer-alert-background': (
        ...
    ),
    // Good
    'alert': (
        ...
    )
)
```

- Les teintes de couleurs doivent être de simples adjectifs renforçant/orientant le rôle ou la couleur principale. Ne spécialisez pas la teinte à un contexte précis. En renforçant la couleur par un adjectif elle reste globale et peut donc être utilisée peu importe le contexte.

```sass
$palette: (
    'alert': (
        // Bad, specialized for text, can't be reused
        'text': red,
        // Good, not specialized works anywhere
        'strong': orange
    )
)
```
<hr></p></details>

<details><summary>Limitez le nombre de couleurs et de teintes dans votre palette</summary><p>

Trop de teintes ou trop de couleurs est une bonne indication que la palette est mal définie et nécessite une uniformisation.
<hr></p></details>

<details><summary>Uniformisez vos `Breakpoints` via des variables</summary><p>
```sass
// Boostrap 4
$grid-breakpoints: (
  xs: 0,
  sm: 576px,
  md: 768px,
  lg: 992px,
  xl: 1200px
);

// Bootstrap 3
$screen-xs:                  480px !default;
$screen-sm:                  768px !default;
$screen-md:                  992px !default;
$screen-lg:                  1200px !default;
$screen-xs-max:              ($screen-sm-min - 1) !default;
$screen-sm-max:              ($screen-md-min - 1) !default;
$screen-md-max:              ($screen-lg-min - 1) !default;
```
<hr></p></details>

<details><summary>Evitez les imbriquation de plus de 3 niveaux</summary><p>

Il y a 3 raisons à ne pas abuser de l'imbriquation des sélecteurs :
- L'imbriquation augmente la complexité des sélecteurs et les rend plus difficile à overrider si nécessaire. Elle augmente les chances d'apparition de l'horrible `!important`.
- Le sélecteur étant divisé sur plusieurs niveaux une personne cherchant à debugger en partant du sélecteur complet ne le trouvera pas avec une simple recherche. C'est encore pire si le sass joue sur des placement étrange du sélecteur parent `&`.
- L'imbriquation réduit la lisibilité du code vu qu'il faut en permanence remonter l'arborescence de l'imbriquation pour comprendre le rôle d'un sélecteur. Par exemple dans cet exemple, lire le code n'aide pas à le comprendre, il faut absolument le voir en action :
```sass
.root {
  width: 400px;
  margin: 0 auto;

  .links {
    .link {
      display: inline-block;

      & ~ .link {
        margin-left: 10px;
      }

      a {
        padding: 10px 40px;
        cursor: pointer;
        background: gray;

        &:hover {
          background: blue;
          color: white;
          font-size: 700;
        }

        .icon {
          margin-right: 5px;
          @include selector-modifier(-2 ':hover', 1 suffix '.zh') {
            color: red;
            background: green;
          }
        }
      }
    }
  }
}
```

Préférez extraire des sous classes avec des noms clairs :
```sass
.root {
    width: 400px;
    margin: 0 auto;
}

.root-link {
    display: inline-block;
    a {
        padding: 10px 40px;
        cursor: pointer;
        background: gray;

        &:hover {
          background: blue;
          color: white;
          font-size: 700;
        }
    }
}

.root-link ~ .root-link {
    margin-left: 10px;
}

.root-link-icon {
    margin-right: 5px;
    @include selector-modifier(-2 ':hover', 1 suffix '.zh') {
        color: red;
        background: green;
    }
}
```
<hr></p></details>

## Design patterns

<details><summary>Faites de l'intégration orientée composant / objet</summary><p>

Pensez les éléments de vos pages comme des composants réutilisables et séparez les du layout des pages.

Voyez vos Layouts comme des `controllers` et vos composants comme des `services`. Le Controller/Layout doit contenir le moins de logique possible, il orchestre les différentes briques/components à sa disposition pour en faire la vue.

C'est autant un travail de design que d'intégration.
Il est important que le designer pense aussi ses maquettes avec des éléments réutilisables.

Une bonne convention à suivre est [SMACSS](https://smacss.com/). Notamment son approche simpliste du module et des sous-classes.

```sass
.module { ... }
.module-subclass { ... }
```
<hr></p></details>

<details><summary>Intégration Mobile First</summary><p>

Développez en **Mobile First**. Vous intégrez depuis le plus petit écran vers le plus grand.

- Le choix de l'amélioration continue.<br>
Vous développez la version de base de votre produit en premier et en fonction du contexte vous l'améliorez. Vous avez une base commune stable.

- Une meilleure organisation des Media Queries :<br>
Chaque Media Query est nécessairement une amélioration vers une taille supérieure d'écran. On sait à quoi s'attendre. Avec du Responsive classique une Media Query peut faire tout et n'importe quoi dans le cadre d'une résolution minimum et/ou maximum aléatoire. Le Mobile First est plus lisible.
<hr></p></details>

<details><summary>Préférez [Flexbox](https://developer.mozilla.org/fr/docs/Web/CSS/Disposition_des_bo%C3%AEtes_flexibles_CSS/Utilisation_des_flexbox_en_CSS) à Float</summary><p>

Le CSS3 est maintenant bien implémenté, le [Flexbox](https://developer.mozilla.org/fr/docs/Web/CSS/Disposition_des_bo%C3%AEtes_flexibles_CSS/Utilisation_des_flexbox_en_CSS) est le remplaçant idéal à toutes les techniques précédentes pour la mise en forme du contenu.
<hr></p></details>

## Références

- [Google HTML/CSS Style guide](https://google.github.io/styleguide/htmlcssguide.xml#CSS_Style_Rules)
- [SASS Guidelines](https://sass-guidelin.es/)
- [CSS Guidelines](http://cssguidelin.es/)
- [SMACSS](https://smacss.com/)
- [OOCSS](https://github.com/stubbornella/oocss/wiki)
