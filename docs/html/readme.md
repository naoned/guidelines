# Convention HTML

<!-- MarkdownTOC -->

- [Généralités](#généralités)
- [Règles de style du HTML](#règles-de-style-du-html)
- [Références](#références)

<!-- /MarkdownTOC -->

## Généralités

<details><summary>Ne spécifiez pas le protocole pour les ressources liées</summary><p>
```html
<!-- Bad -->
<script src="https://www.google.com/js/gweb/analytics/autotrack.js"></script>

<!-- Good -->
<script src="//www.google.com/js/gweb/analytics/autotrack.js"></script>
```

```css
/* Bad */
.example {
  background: url(https://www.google.com/images/example);
}

/* Good */
/* Recommended */
.example {
  background: url(//www.google.com/images/example);
}
```
</p></details>

<details><summary>Indentation avec 4 espaces</summary><p>

![Spacebar](spacebar.jpg) * 4
</p></details>

<details><summary>Tout en minuscule</summary><p>
```html
<!-- Bad -->
<A HREF="/">Home</A>

<!-- Good -->
<img src="google.png" alt="Google">
```
</p></details>

<details><summary>Encodage en UTF-8</summary><p>

Configurez votre éditeur pour qu'il travaille en UTF8 et spécifiez le `charset` dans le `<head>` du document.
```html
<meta charset="utf-8">
```
</p></details>

<details><summary>Si possible utiliser des commentaires PHP ou Twig au lieu de HTML</summary><p>

Les commentaires HTML sont envoyés au client et utilisent donc de la bande passante inutilement avec des informations qui ne les intéressent pas.
</p></details>

## Règles de style du HTML

<details><summary>Utilisez du HTML5</summary><p>

La syntaxe HTML5 est préférables pour tout HTML. Ajoutez la `Doctype` html en haut du document `<!DOCTYPE html>`, avant la balise `<html>`.

Bien que correct en HTML5, ne fermez pas les éléments vide.
```html
<!-- Bad -->
<br />

<!-- Good -->
<br>
```
</p></details>

<details><summary>Utilisez du HTML valide</summary><p>

Utilisez le [validateur d'html w3c](https://validator.w3.org/nu/) pour valider votre code
</p></details>

<details><summary>Utilisez le HTML en accordance avec son rôle sémantique</summary><p>

[Html5 Doctor](http://html5doctor.com/) est une lecture intéressante sur ce sujet.
```html
<!-- Bad -->
<div onclick="goToRecommendations();">All recommendations</div>

<!-- Good -->
<a href="recommendations/">All recommendations</a>

<!-- Bad -->
<span onclick="triggerSmth();">All recommendations</div>

<!-- Good -->
<button class="js-trigger-smth">All recommendations</a>
```
</p></details>

<details><summary>Fournissez un contenu alternatif aux contenus multimédias</summary><p>

L'accessibilité de tout média nécessite une alternative texte. Ainsi toute vidéo ou son doit avoir un transcript, toute image doit avoir un texte alternatif, etc...

```html
<!-- Bad -->
<img src="spreadsheet.png">

<!-- Good -->
<img src="spreadsheet.png" alt="Spreadsheet screenshot.">
```
</p></details>

<details><summary>N'utilisez pas le HTML pour styler le contenu</summary><p>

Le style est réservé au CSS.
```html
<!-- Bad -->
<!DOCTYPE html>
<title>HTML sucks</title>
<link rel="stylesheet" href="base.css" media="screen">
<link rel="stylesheet" href="grid.css" media="screen">
<link rel="stylesheet" href="print.css" media="print">
<h1 style="font-size: 1em;">HTML sucks</h1>
<p>I’ve read about this on a few sites but now I’m sure:
  <u>HTML is stupid!!1</u>
<center>I can’t believe there’s no way to control the styling of
  my website without doing everything all over again!</center>
```

```html
<!-- Good -->
<!DOCTYPE html>
<title>My first CSS-only redesign</title>
<link rel="stylesheet" href="default.css">
<h1>My first CSS-only redesign</h1>
<p>I’ve read about this on a few sites but today I’m actually
  doing it: separating concerns and avoiding anything in the HTML of
  my website that is presentational.
<p>It’s awesome!
```
</p></details>

<details><summary>N'utilisez pas d'HTML entities</summary><p>

Il n'y a pas besoin d'utiliser les entités HTML si tout est en UTF8.
```html
<!-- Bad -->
The currency symbol for the Euro is &ldquo;&eur;&rdquo;.

<!-- Good -->
The currency symbol for the Euro is “€”.
```
</p></details>

<details><summary>Ne pas utiliser `type` pour les css et les scripts</summary><p>

```html
<!-- Bad -->
<link rel="stylesheet" href="//www.google.com/css/maia.css"
  type="text/css">
<script src="//www.google.com/js/gweb/analytics/autotrack.js"
  type="text/javascript"></script>

<!-- Good -->
<link rel="stylesheet" href="//www.google.com/css/maia.css">
<script src="//www.google.com/js/gweb/analytics/autotrack.js"></script>
```
</p></details>

<details><summary>Utiliser des doubles guillemets pour les attributs</summary><p>
```html
<!-- Bad -->
<a class='maia-button maia-button-secondary'>Sign in</a>

<!-- Good -->
<a class="maia-button maia-button-secondary">Sign in</a>
```
</p></details>

<details><summary>N'utilisez pas les attributs évènements `onXxx`</summary><p>

On ne fait pas de JS dans le HTML. Ca n'est pas une bonne pratique et çà peut réduire la lisibilité du code en cas d'abus.

`onvlue`, `onchange`, `onclick`, etc...

```html
<!-- Bad -->
<button onclick="doSmth()">Click me</button>

<!-- Good -->
<!-- Bind the event with javascript in a js file -->
<button class="do-smth">Click me</button>
```
</p></details>

<details><summary>Pas de scripts ou de css dans le HTML</summary><p>
```html
<!-- Bad -->
<style>
.btn {
    padding: 3px 6px;
}
</style>
<script>
    document.querySelector('.btn').addEventListener('click', function() {
        /* ... */
    });
</script>

<!-- Good -->
<link href="/css/button.css">
<script src="/js/button.js"></script>
```
</p></details>

## Références

- *[Google HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.xml)*