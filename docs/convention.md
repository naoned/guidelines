# Convention de code

## Généralités

 * Encodage UTF-8
 * Saut de ligne UNIX
 * Indentation avec des espaces (4) - Tabulations proscrites

## Exemple

Un exemple valant mieux qu'un long discours :

```php
<?php

namespace Naoned\Ponies;

use Naoned\User;
use Naoned\Breeeder;
use Naoned\Breeders\NullBreeder;
use Naoned\Accessories\Saddle;
use Naoned\Exceptions\BadKnownRoadsFormat;
use Psr\Log\LoggerTrait;

// No Interface suffix on interfaces
class Unicorn extends Pony implements Ridable
{
    // No Trait suffix on traits ... except for PSR ones :-/
    use LoggerTrait;

    const
        // order = public, protected, private
        public DEFAULT_CORN_SIZE = 42,
        private SOME_CONST = 'rainbow';

    public
        $somePublicVar; // should almost never happen ...

    private
        $owner,
        $breeder,
        $rider,
        $saddle,
        $options;

    public function __construct(User $owner, array $options = array())
    {
        $this->owner = $owner;

        // Never initialized properties in declaration, use constructor for it
        $this->breeder = new NullBreeder();
        $this->rider = null;
        $this->saddle = null;
        $this->options = $options;
    }

    // Always typehint everything you can
    public function setBreeder(Breeder $breeder): self
    {
        $this->breeder = $breeder;

        // Fluid interface on option setters
        return $this;
    }

    // Visibility is mandatory, even for public methods
    public function ride(User $braveman): ?int
    {
        // whitelist sanity check
        if($this->saddle instanceof Saddle)
        {
            $this->changeRider($braveman);

            try
            {
                return $this->startRiding();  
            }
            catch(BadKnownRoadsFormat $e)
            {
                // avoid empty catches
                $this->logger->warning("None road are known. It's quite weird !");
            }
        }

        // Always leave a blank line before return or throw
        return null;
    }

    private function changeRider(?User $rider): void
    {
        $this->rider = $rider;
    }

    private function startRiding(): int
    {
        $distances = 0;

        $knownRoads = retrieveKnowRoads();

        if(! $knownRoads instanceof \iterable)
        {
            $this->methodWithAGreatName();

            // Always leave a blank line before return
            // No Exception suffix, use namespace for it
            throw new BadKnownRoadsFormat();
        }

        foreach($knownRoads as $road)
        {
            $distances += $road->getDistance();
        }

        // Always leave a blank line before return or throw
        return $distances;
    }

    public function prepare(): void
    {
        $hands = $this->owner->wearGloves()
            ->motivate()
            ->getHands();

        // Anonymous function
        $how = function() use($hands) {
            $hands->take($this);
        };

        $this->ownerAction($how);
    }

    private function ownerAction(\Closure $action)
    {
        return $this->owner->execute($action);
    }

    public static function sometimesAStaticMethodCanBeCorrect()
    {
    }
}
```

## Mise en forme
 * camelCase pour les variables et les méthodes, studlyCaps pour les classes, interfaces et traits
 * Accolades sur la ligne suivante (à l'exception des fonctions anonymes)
 * Espaces autour des opérateurs (y compris l'opérateur de concaténation .)
 * Ecrire true, false, null et autres mots clé en minuscules
 * Code en anglais **à l'exception du vocabulaire du métier (et uniquement lui)**
 * Utiliser le [type hinting](http://www.php.net/manual/fr/language.oop5.typehinting.php) à chaque fois que c'est possible
 * Utiliser le type hinting pour [les retours de méthode](http://php.net/manual/fr/functions.returning-values.php#functions.returning-values.type-declaration) pour les projets en PHP >= 7.0
 * Utiliser [les types nullables](http://php.net/manual/fr/migration71.new-features.php#migration71.new-features.nullable-types) pour les projets en PHP >= 7.1
 * Proscrire l'utilisation des opérateurs ternaires

## Usage général
 * Programmation orientée objet (voir chapitre dédié plus bas)
 * Pas de magie (_ _get, _ _call, new $class, …)
 * Pas d'usage des effets de bord de PHP
   - Pas de StdClass
   - Déclarer tout explicitement : variables, propriétés de classe, visibilité, …
   - @ proscrit
 * Les erreurs/warnings/notice doivent toutes être corrigées (et donc **toutes** affichées en dev)
 * Pas de variable globale (sous peine d'arrachage de doigts)
   - pas d'exception pour le conteneur d'injection
 * Tests de robustesse, pas de code optimiste
   - explicites
   - en mode “whitelist”
   - inutiles dans un scope privé

```php
<?php

// Tests de robustesse : exemple

$log = someFct();

// Pas bien du tout
$log->info($msg);

// Pas bien
if($log)
{
   $log->info($msg);
}

// A peine mieux
if($log !== null)
{
   $log->info($msg);
}

// BIEN !
if($log instanceof Logger)
{
   $log->info($msg);
}
```
## Programmation orientée objet
 * Utilisation d'interfaces
 * Injection de dépendances, découplage
 * Pas d'utilisation systématique et abusive de l'héritage
 * Privé par défaut (on passe en protégé uniquement quand le cas se présente)
 * Pas de getter/setter “au cas où” ⇒ uniquement les rares qui seront nécessaires
 * Ne jamais injecter le container d'injection

## Namespaces
 * Ne pas utiliser les alias (use X as Y)
 * Utilisation de [PSR-4](http://www.php-fig.org/psr/psr-4/)

## Code smells
Eléments qui doivent vous interpeller et vous inciter à trouver une meilleure solution (vrai à 90%)

 * else (de surcroît si on en enchaîne plusieurs)
 * if en cascade
 * switch
 * propriétés privées avec un getter ET un setter
 * classes 100% (ou presque) publiques
 * classes 100% (ou presque) statiques

## Test unitaire : exemple

```php
<?php
 
class UnicornTest extends PHPUnit_Framework_TestCase
{
    private
        $unicorn;
 
    protected function setUp()
    {
        $this->unicorn();
    }
 
    /**
     * @dataProvider providerTestIsRainbowCompatible()
     */
    public function testIsRainbowCompatible(string $unicornType, bool $expected)
    {
        $result = $this->burger->isRainbowCompatible($unicornType);
 
        $this->assertSame($expected, $result);
    }
 
    // provider always follows the test method
    // name is always provider<TestMethodName>
    public function providerTestIsRainbowCompatible()
    {
        return array(
            // Explicit names is better than numerical index based names
            'colored type #1' => array('Shrek', true),    
            'colored type #2' => array('The Flash', true),    
            'non existing type' => array('Bouffifre', false),    
            'approximating type' => array('Cetailem', false),    
            'mixed type' => array('Smurf', true),    
        );
    }
}
```