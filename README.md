# POO

= Programmation Orientée Objet

## Les classes

C'est un plan de fabrication, un moule.  
On les fabrique ainsi:

```php
class Identite {
    public $nom; // Ceci est un propriété (ou attribut)
    public $prenom; // Ceci est une autre propriété (ou attribut)
    // etc etc

    public $bg = false; 
    // Ici, 'false' sera renvoyé comme valeur par défaut si aucune autre valeur ne lui est attribuée.
}
```

Attention, UNE classe doit être dans UN fichier.  
Ce fichier a le même nom que la classe elle même.  
Il est important également de veiller à ce que la classe et le fichier commencent par une majuscule.  
Par exemple :

```php
// Ma classe ...
class Identite {
    // ...
}
// ... porte le même nom que le fichier dans lequel la classe se situe
// Identite.php
```


## Les instances (=objet)

Dans une classe, on va pouvoir y couler des objets, on dit alors qu'on va les instanciers.  
On instancie donc, depuis `index.php` de cette manière :

```php
$unObjet = new Identite ();
```

On commence par créer une variable qui va contenir la classe.  
Etant donné que nous allons fabriquer une nouvelle identité, nous devons indiquer qu'il s'agit de nouvelles valeurs, d'où le `new`.

```php
// La classe (le moule) peut est réutilisé 
// à l'infini avec l'aide de nouvelles intances:

$unAutreObjet = new Identite();
$unQuatreVingtDixNeufiemeObjet = new Identite();
```

## Propriétés / attributs

### Premiere solution d'attribution de valeurs

Nous devons ensuite attribuer des valeurs à notre identité. C'est à dire le nom de la personne, prénom, date de naissance etc.  
Pour se faire, nous indiquerons sur le fichier `index.php` :

```php
$unObjet->nom = 'DIEU';
$unObjet->prenom = 'John';
// ...
$unObjet->bg = true ; // Il retournera ici 'true' (à la place de 'false') .
```

Il est intéressant à ce stade de faire un `var_dump($unObjet);` afin de voir ce que ca donne :

```php
/var/www/html/trinity/S04/EP02/Resumer red/index.php:8: 

object(Identite)[1]
  public 'nom' => string 'DIEU' (length=4)
  public 'prenom' => string 'John' (length=4)
  public 'bg' => boolean true
```

### Seconde solution d'attribution de valeurs

Nous pouvons également ajouter un *constructeur* afin d'attribuer les valeurs à la suite.  
Pour cela, nous allons devoir faire appel à une fonction. Nous parlerons ici de **METHODE** (fonction dans une class = méthode).   
**ATTENTION : il ne peut y avoir qu'UN SEUL constructeur par class !**
Comment construire notre constructeur :

```php
public function __construct($nom, $prenom, $bg=false) { 
    // on peut insérer autant de propriétés souhaités. Si elles n'ont pas de valeur par défaut, elles seront alors considérées comme étant 'null'.
    
    $this->nom = $nom ;
    $this->prenom = $prenom ;
    $this->bg = $bg ;
}
```

`$this` représente l'objet en cours de construction (du côté `index.php`). On va donc alimenter l'identité qui est en cours, qui est demandé avec les nouvelles données.  
Par exemple:  
Si on travail sur cette instance:  
 `$unObjet = new Identite ();`  
alors c'est sur cette identité que les valeurs seront ajoutées.  
Si on travail sur une autre instance comme par exemple  
`$unAutreObjet = new Identite();`  
alors c'est à cette identité que les valeurs seront ajoutées, et ainsi de suite.  
**!! Les valeurs de `$unAutreObjet` n'écraseront pas les valeurs de `$unObjet` !!**  

A partir de là, nous pouvons nous amuser à automatiser notre code au travers de méthodes dans notre fichier `Identite.php`.  
Par exemple:

```php
//On peut concaténer avec des points .
 public function presentations(){
    echo 'Hello je m\'appelle '. $this->prenom . 'et je suis un '. $this->bg . '<br>';
  }

// ... ou alors avec des {}
 public function presentations(){
    echo "Hello je m'appelle {$this->prenom} et je suis un  {$this->bg}<br>";
  }
```

Sur notre fichier `index.php`, nous allons désormais pouvoir attribuer de nouvelles valeurs via notre constructeur :

```php
// Une premiere façon d'écrire
$unObjet = new Identite ('mon nom de famille', 'mon prénom', true);

// Une seconde façon d'écrire
$unObjet = new Identite (
    'mon nom de famille', 
    'mon prénom', 
    true
);
```

le `var_dump($unObjet);` donnerait alors :

```
/var/www/html/trinity/S04/EP02/Resumer red/index.php:6:

object(Identite)[1]
  public 'nom' => string 'mon nom de famille' (length=18)
  public 'prenom' => string 'mon prénom' (length=11)
  public 'bg' => boolean true
```

## Lier nos deux fichiers `index.php` et `Indentite.php`

Pour cela, nous devons indiquer au fichier `index.php` d'aller chercher la classe dans notre fichier `Identite.php` :

```php
include __DIR__ . '/inc/Identite.php'; // include est permissif, demandé avec un "s'il te plait" (#MikaBuche)
require __DIR__ . '/inc/Identite.php'; // require est un ordre et ne passera pas s'il ne trouve pas le fichier. Il va die(); et s'arrêter net, il ne poursuivra donc pas la lecture du code.
// Attention a la Majuscule du 'I' et attention au '.' 
```

`__DIR__` va donner le chemin absolu depuis la racine jusqu'au dossier dans lequel le fichier `index.php` se situe.   
Par exemple, notre fichier `index.php` se situe, selon `__DIR__` ici :

```php
var_dump(__DIR__);
// résultat : /var/www/html/trinity/S04/EP02/Resumer red/
```

Par exemple, pour accéder à notre fichier `Identite.php`, nous avons du lui indiquer le chemin `/inc/Identite.php`, comme ci-dessus.

## Visibilité

Ici, nous devons déterminer s'il est possible de lire et d'écrire dans la propriété d'un objet via un code externe à la classe.
- `public` = open bar (*profitez c'est pas tous les jours*), tout le monde peut faire ce qu'il veut des propriétés.
- `private` = sécurisé, seul le code à l'intérieur de la classe pourra lire/écrire dans la propriété. C'est comme si la ou les données mises en `private` étaient dans un coffre-fort *(seul les getter et setter auront la clef pour ouvrir ou se servir dans le coffre)*
- c'est exactement pareil avec les `method`.
Par exemple:

```php
// Les propriétés de cette classe sont publiques, donc accessibles à tous en lecture ET en écriture.
 class Identite {
   public $nom = 'DIEU';
   public $prenom = 'John' ;
   public $bg = true ;
   public $tel = '06 54 16 49 69';

}
// Dans notre instance, nous serons facilement en mesure de faire appel au numéro de téléphone, c'est à dire non seulement le lire, mais également le changer à volonté, sans aucune restriction, de même pour sa valeur et son type (int, string, booleen, etc).
$unObjet->tel = '06 08 30 50 69';

// Ici, les propriétés de la classe 'Identite' sont en 'public', SAUF la propriété '$tel' qui est 'private'. Elle ne peut donc être NI lue, NI modifiée. Il faudrait pour cela établir une méthode afin de pouvoir de lui apporter des modifications et la lire.

 class Identite{
   public $nom = 'DIEU';
   public $prenom = 'John' ;
   public $bg = true ;
   private $tel = '06 54 84 64 30';

}
```

## Getters / Setters

### Getters

Si nous souhaitons rendre une propriété **uniquement** lisible, nous pouvons utiliser la methode `getter`.  
Il s'agit d'une méthode à insérer dans notre `class Identite {}` située dans notre fichier `Identite.php` de la manière suivante :

```php
// Dans notre exemple, la classe Identite se présentera ainsi:
class Identite{
    public $nom = 'Le plus beau';
    public $prenom = 'John' ;
    public $bg = true ;
    private $tel;

    public function getTel() {
        // on retourne la valeur de la propriété $unNumero de l'objet
        return $this->unNumero;
    }
}
```

### Setters

Si nous souhaitons rendre une propriété **modifiable**, nous utiliserons la méthode `setter`.  
Il s'agit d'une méthode à insérer dans notre `class Identite {}` située dans notre fichier `Identite.php` de la manière suivante :

```php
// Dans notre exemple, la classe Identite se présentera ainsi:
class Identite{
    public $nom = 'Le plus beau';
    public $prenom = 'John' ;
    public $bg = true ;
    private $tel;

    public function setTel($unNumero){
        // plus tard on pourra mettre des conditions 
        // dans mes setters pour etre SûR qu'on peut modifier la propriété
        $this->numero = $unNumero;
    }
}
```

Nous pourrions résumer tout celà ainsi :  

Si on reprend l'exemple du numéro de téléphone :  
`private` permettra à ce que personne ne puisse ni le voir ni le modifier.  
Cependant nous pourrions avoir besoin que quelques personnes puissent y avoir accès, comme notre médecin ou la nounou par exemple. Alors, nous utiliserons la méthode `getter` afin de pouvoir rester joignable, uniquement pour ces personnes.  
Enfin, il serait sans doute judicieux qu'au moins une personne puisse modifier le numéro, comme l'opérateur téléphonique. Dans ce cas, nous lui attribuerons la methode `setter`.

Autre exemple, ta date d'anniversaire, sujet parfois sensible chez certain(e)s :  
Il s'agit d'une donnée sensible, qu'on ne souhaite pas exposer à n'importe qui. Dans ce cas, nous indiquerons que la propriété est `private`.  
Par contre, il peut être utile que quelques personnes puissent la connaitre, comme ton mari/conjoint/chat (ne jamais cracher sur un potentiel cadeau). Dans ce cas, nous pourrons leur associer une methode `getter` afin qu'ils puissent lire cette donnée sans pour autant pouvoir la modifier.  
Enfin, une seule personne pourrait être en mesure de modifier l'âge (c'est toi-même, et oui désolé mais tu vieillis chaque secondes qui passent). Tu es donc la seule détentrice des droits à modifier ton âge. Tu as donc une methode `setter`.  

Grosso modo:  

`private` = données ultra-confidentiel. On ne souhaite pas que n'importe qui puisse y avoir accès.  
`getter` = on donne accès à ces données, uniquement en lecture, à une poignée de personnes.  
`setter` = on donne un accès supplémentaire à d'autres personnes afin qu'ils puissent modifier les données.
