# Les classes

C'est un plan de fabrication, un moule.  
On les fabrique ainsi:
```php
class Identite {
    public $Nom; // Ceci est un propriété
    public $Prenom; // Ceci est une autre propriété
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
class Indentite {
    // ...
}
 // ... porte le même nom que le fichier dans lequel la classe se situe

>Identite.php
```


# Les instances
Dans une classe, on va pouvoir y couler des objets, on dit alors qu'on va les instanciers.  
On instancie donc, depuis ``index.php`` de cette manière :
```php
$unObjet = new Identite ();
```
On commence par créer une variable qui va contenir la classe.  
Etant donné que nous allons fabriquer une nouvelle identité, nous devons indiquer qu'il s'agit de nouvelles valeurs, d'où le ``new``.
```php
// La classe (le moule) peut est réutilisé 
//à l'infini avec l'aide de nouvelles intances:

$unAutreObjet = new Identite();
$unQuatreVingtDixNeufEmeObjet = new Identite();
```

### Premiere solution d'attribution de valeurs
Nous devons ensuite attribuer des valeurs à notre identité. C'est à dire le nom de la personne, prénom, date de naissance etc etc.  
Pour se faire, nous indiquerons sur le fichier ``index.php`` :
```php
$unObjet->nom = 'DIEU';
$unObjet->prenom = 'John';
// ...
$unObjet->bg = true ; // Il retournera ici 'true' (à la place de 'false') .
```
Il est intéressant à ce stade de faire un ``var_dump($unObjet);`` afin de voir ce que ca donne:  
```php
/var/www/html/trinity/S04/EP02/Resumer red/index.php:8: 

object(Identite)[1]
  public 'nom' => string 'DIEU' (length=4)
  public 'prenom' => string 'John' (length=4)
  public 'bg' => boolean true
```
### Seconde solution d'attribution de valeurs
#### Ecriture d'un constructeur
Nous pouvons également ajouter un *constructeur* afin d'attribuer les valeurs à la suite.  
Pour cela, nous allons devoir faire appel à une fonction. Nous parlerons ici de **METHODE** (fonction = méthode).    
Comment construire notre construsteur :  
```php
public function __construct($nom, $prenom, $bg=false) { 
    // on peut insérer autant de propriétés souhaités. Si elles n'ont pas de valeur par défaut, elles seront alors considérées comme étant 'null'.
    
    $this->nom = $nom ;
    $this->prenom = $prenom ;
    $this->bg = $bg ;
}
```
`$this` représente l'objet en cour de construction (du côté ``index.php``). On va donc alimenter l'identité qui est en cour, qui est demandé avec les nouvelles données.  
Par exemple:  
Si on travail sur cette instance:  
 ``$unObjet = new Identite ();``  
alors c'est sur cette identité que les valeurs seront ajoutées.  
Si on travail sur une autre instance comme par exemple  
``$unAutreObjet = new Identite();``  
alors c'est à cette identité que les valeurs seront ajoutées, et ainsi de suite.  
**!! Les valeurs de ``$unAutreObjet`` n'écraseront pas les valeurs de ``$unObjet`` !!**  

A partir de là, nous pouvons nous amuser a automatiser notre code au travers de fonctions dans notre fichier ``Identite.php``.  
Par exemple:
```php
//On peut concaténé avec des points ...
 public function presentations(){
    echo 'Hello je m\'appelle '. $this->prenom . 'et je suis un '. $this->bg . '<br>';
  }

// ... ou alors avec des {}
 public function presentations(){
    echo "Hello je m'appelle {$this->prenom} et je suis un  {$this->bg}<br>";
  }
```

Sur notre fichier ``index.php``, nous allons désormais pouvoir attribuer de nouvelles valeurs à notre constructeur:
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
le ``var_dump($unObjet);`` donnerait alors :
```
/var/www/html/trinity/S04/EP02/Resumer red/index.php:6:

object(Identite)[1]
  public 'nom' => string 'mon nom de famille' (length=18)
  public 'prenom' => string 'mon prénom' (length=11)
  public 'bg' => boolean true
```
## Lier nos deux fichiers ``index.php`` et ``Indentite.php``

Pour cela, nous devons indiquer au fichier ``index.php`` d'aller chercher la classe dans notre fichier ``Identite.php`` :
```php
include __DIR__ . '/inc/Identite.php'; // include est permissif, demandé avec un "s'il te plait" (#MikaBuche)
require __DIR__ . '/inc/Identite.php'; // require est un ordre et ne passera pas s'il ne trouve pas le fichier. Il va die(); et s'arrêter net, il ne poursuivra donc pas la lecture du code.
// Attention a la Majuscule du 'I' et attention au '.' 
```
``__DIR__`` va donner le chemin absolu depuis la racine jusqu'au dossier dans lequel le fichier ``index.php`` se situe.   
Par exemple, notre fichier ``index.php`` se situe, selon ``__DIR__`` ici :
```php
__DIR__ = /var/www/html/trinity/S04/EP02/Resumer red/
```
Par exemple, pour accéder à notre fichier ``Identite.php``, nous avons du lui indiquer le chemin ``/inc/Identite.php``, comme ci-dessus.
## Visibilité

Ici, nous devons déterminer s'il est possible de lire et d'écrire dans la propriété d'un objet via un code externe à la classe.
- ``public`` = open bar (*profitez c'est pas tous les jours*), tout le monde peut faire ce qu'il veut des propriétés.
- ``private`` = sécurisé, seul le code à l'intérieur de la classe pourra lire/écrire dans la propriété.
- c'est exactement pareil avec les ``method``.
Par exemple:

```php
// Cette classe est publique, donc accéssible à tous en lecture ET en écriture.
 class Identite {
   public $nom = 'DIEU';
   public $prenom = 'John' ;
   public $bg = true ;
   public $tel = '06 54 16 49 69';

}
// Dans notre instance, nous serons facilement en mesure de faire appel au numéro de téléphone, c'est à dire non seulement le lire, mais également le changer à volonté, sans aucune restriction, de même pour sa valeur et son type (int, string, booleen, etc).
$unObjet->tel = '06 08 30 50 69';

// Ici, la classe 'Identite' est 'public' pour toutes les propriétés, SAUF pour la propriété '$tel' qui est 'private'. Elle ne peut donc être NI lu, NI modifiée. Il faudrait pour cela établir une méthode afin de pouvoir de lui apporter des modifications et la lire.

 class Identite{
   public $nom = 'DIEU';
   public $prenom = 'John' ;
   public $bg = true ;
   private $tel = '06 54 84 64 30';

}
```

## Getters / Setters
### Getters

Si nous souhaitons rendre une propriété **uniquement** lisible, nous pouvons utiliser la methode ``getter``.  
Il s'agit d'une méthode à insérer dans notre ``class Identite {}`` située dans notre fichier ``Identite.php`` de la manière suivante:
```php
    // Dans notre exemple, la classe Identite se présentera ainsi:
     class Identite{
        public $nom = 'Le plus beau';
        public $prenom = 'John' ;
        public $bg = true ;
        private $tel = $unNumero; // On modifie l'écriture en dur par une variable
    
        public function getTel(){
            // si on a bien le droit d'acceder a tel ALORS on le return
            return $this->unNumero;
        }
}
```

### Setters

Si nous souhaitons rendre une propriété **modifiable**, nous utiliserons la méthode ``setter``.  
Il s'agit d'une méthode à insérer dans notre ``class Identite {}`` située dans notre fichier ``Identite.php`` de la manière suivante:

```php
    // Dans notre exemple, la classe Identite se présentera ainsi:
     class Identite{
        public $nom = 'Le plus beau';
        public $prenom = 'John' ;
        public $bg = true ;
        private $tel = $unNumero; // On modifie l'écriture en dur par une variable
    
        public function setTel($unNumero){
            // plus tard on mettra tout un tas de conditions 
            // dans mes setters pour etre SUR qu'on peut modifier la propriété
            $this->numero = $unNumero;
        }
}
```
