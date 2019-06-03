Structure du Programme
===



> Et mon cœur brille d'une lumière rouge vif sous ma peau translucide et ils doivent m'administrer 10cc de JavaScript pour me faire revenir. (Je réagit bien aux toxine dans le sang) Mec, ce truc ferait bien sauter les pêches sur vos branchies !
>
> _why, *Why's (Poignant) Guide to Ruby*

![Image de tentacules tenant des objets](https://eloquentjavascript.net/img/chapter_picture_2.jpg)

Dans ce chapitre, nous allons commencer à faire des trucs qui peuvent être réellement appeler *programmation*. Nous allons élargir notre maîtrise du langage JavaScript au-delà des noms et fragments de phrases que nous avons vu jusqu'à présent, au point de pouvoir exprimer une prose avec du sens.

Expressions et instructions
---

Dans le [Chapitre 1](), nous avons créé des valeurs et leur avons appliqué des opérateurs pour obtenir de nouvelles valeurs. Créer des valeur de cette façon est la substance principale de n'importe quel programme JavaScript. Mais cette substance doit être encadrée d'une structure plus large pour être utile. Donc c'est ce que nous allons couvrir dans la suite.

Un fragment de code qui produit une valeur est appelé une *expression*. Toute valeur qui est écrite littéralement (telle que `22` ou `"psychanalyse"`) est une expression. Une expression entre parenthèses est aussi une expression, tout l'est un opérateur binaire appliqué à deux expression, ou un opérateur unaire appliqué à une seule.

Cela montre une partie de la beauté d'une interface basée sur le langage. Des expressions peuvent contenir d'autres expressions d'une manière similaire à l'imbrication des sous-phrases dans les humains — une sous-phrase peut contenir elle-même des sous-phrases, et ainsi de suite. Cela nous permet de construire des expressions qui décrivent de manière arbitraire des calculs complexes.

Si une expression correspond à un fragment de phrase, une instruction *JavaScript* correspond a une phrase complète. Un programme est une liste d'instructions.

Le type le plus simple d'instruction est une expression terminée par un point-virgule. Ceci est un programme :

```javascript
1;
!false;
```

Cependant, c'est un programme inutile. Une expression peut se contenter de produire une valeur, qui sera à son tour pourra être utilisée par le code contenant l'expression. Une instruction est autonome, elle ne compte pour quelque chose que si elle affect le monde. Elle peut afficher quelque chose sur l’écran — cela compte comme changer le monde — ou elle peut changer l’état interne de la machine d'une manière qui affectera les instructions qui viennent après elle. Ces changements sont appelés *effets secondaires*. Les instructions de l'exemple précédent produisent juste les valeurs `1` et `true` et s'en débarrassent immédiatement après. Cela ne laisse absolument aucune empreinte sur le monde. Quand vous exécutez ce programme, rien d'observable ne se produit.

Dans certain cas, JavaScript vous permet d'omettre le point-virgule à la fin d'une instruction. Dans les autres cas, il doit être présent, sinon la prochaine ligne sera traitée comme faisant partie de la même instruction. Les règles pour quand il peut être omit sans risque sont quelque peu complexes et sources d'erreurs. Donc, dans ce livre, chaque instruction qui a besoin d'un point-virgule, en aura toujours un. Je vous recommande de faire de même, au moins jusqu'a ce que vous sachiez plus à propos des subtilités des point-virgules absents.

## Bindings

Comment est-ce qu'un programme conserve un état interne ? Comment est-ce qu'il se souvient de quelque chose ? Nous avons vus comment produire de nouvelles valeurs à partir d'anciennes valeurs, mais cela ne change pas les anciennes valeurs, et la nouvelle valeur doit être immédiatement utilisée ou elle va a nouveau disparaître. Pour attraper et retenir des valeurs, JavaScript fournit quelque chose appelle un *binding*, ou une *variable* :

```javascript
let caught = 5 * 5;
```

C'est un second type d'instruction. Le mot spécial (*mot-clef*)  `let` indique que cette phrase va définir un binding. Il est suivis par le nom du binding et, si on veut immédiatement lui donner une valeur, par un opérateur `=` et une expression.

L'instruction précédente crée un binding appelé `caught` et l'utilise pour capturer le nombre qui est produit en multipliant 5 avec 5.

Après avoir définit un binding, son nom peut être utilisé comme une expression. La valeur d'une telle expression est la valeur que le binding contient à ce moment. Voici un exemple :

```javascript
let dix = 10;
console.log(dix * dix);
// → 100
```

Quand un binding pointe vers une valeur, cela ne signifie pas qu'il est lie à cette valeur pour toujours. L’opérateur `=` peut être utilisé à n'importe quel moment sur les bindings existants pour les déconnecter de leur valeur courante et les faire pointer vers une nouvelle. 

```javascript
let ambiance = "lumineuse";
console.log(ambiance);
// → lumineuse
ambiance = "sombre";
console.log(ambiance);
// → sombre
```

Vous pouvez imaginer les bindings comme des tentacules, plutôt que des boites. Ils ne *contiennent* pas de valeurs ; ils s'en *saisissent* — deux bindings peuvent référer à la même valeur. Un programme accède uniquement aux valeurs pour lesquelles il a une référence. Quand vous avez besoin de vous souvenir de quelque chose, vous faites pousser un tentacule pour le saisir ou vous y ré-attachez un de vous tentacules existants.

Regardons un autre exemple. Pour se souvenir du nombre d'euros que Luigi vous doit, vous créez un binding. Et, quand il vous rembourse 35 €, vous donnez une nouvelle valeur à ce binding.

```javascript
let luigiDette = 140;
luigiDette = luigiDette - 35;
console.log(luigiDette);
// → 105
```

Quand vous définissez un binding sans lui donner de valeur, le tentacule n'a rien dont il puisse se saisir donc il finit avec de l'air. Si vous demandez la valeur pour un binding vide, vous obtiendrez la valeur `undefined`.

Une seule instruction `let` peut définir plusieurs bindings. Les définitions doivent être séparées par des virgules.

```javascript
let un = 1, deux = 2;
console.log(un + deux);
// → 3
```

Les mots `var` et `const` peuvent aussi être utilisés pour créer des bindings, d'une manière similaire à `let`.

```javascript
var nom = "Ayda";
const salutation = "Bonjour ";
console.log(salutation + nom);
// → Bonjour Ayda
```

Le premier, `var` (raccourci pour "variable"), est la manière dont les bindings étaient déclarés en JavaScript avant 2015. Je reviendrai sur la façon précise dont il diffère de `let` dans le [prochain chapitre](). Pour le moment, souvenez-vous qu'il fait presque la même chose, mais nous allons rarement l'utiliser dans ce livre car il a quelques propriétés qui prêtent à confusion.

Le mot `const` signifie *constante*. Il définit un binding constant qui pointe vers la même valeur aussi longtemps qu'il est en vie. C'est utile pour les bindings qui donnent un nom à une valeur afin que vous puissiez vous y référer aisément plus tard.

## Noms de bindings

Les noms de binding peuvent être n'importe quel mot. Des chiffres peut faire partie des noms de binding — par exemple, `catch22` est un nom valide — mais le nom ne doit pas commencer par un chiffre. Un nom de binding peut inclure des signes dollars (`$`) ou des tirets bas (`_`) mais pas d'autres caractères de ponctuation ou caractères spéciaux.

Les mots avec une signification particulière, tels que `let`, sont des mots-clefs (*keyword*s), et ils ne doivent pas être utilisés comme nom de binding. Il y a aussi un certain nombre de mots qui sont "réservés pour être utilisés" dans de futures version de JavaScript, qui aussi ne peuvent pas être utilisés comme nom de binding. La liste complète des mots-clefs et des mots réservés est plutôt longue.

```
reak case catch class const continue debugger default
delete do else enum export extends false finally for
function if implements import interface in instanceof let
new package private protected public return static super
switch this throw true try typeof var void while with yield
```

Ne vous souciez pas de mémoriser cette liste. Lorsque la création d'un binding génère une erreur de syntaxe inattendue, vérifiez si vous essayez de définir un mot réservé.

## L'environnement

La collection de bindings et leurs valeurs qui existent à un moment donné est appelé l'*environnement*. Quand un programme démarre, cet environnement n'est pas vide. Il contient toujours des bindings qui fond partie du language en standard, et la plupart du temps, il a aussi des bindings qui fournissent des moyens d'interagir avec le système environnant. Par exemple, dans un navigateur, il y a des fonctions pour interagir avec le site web actuellement chargé et pour lire les entrées de la souris et du claviers.

## Fonctions

Un grand nombre des valeurs fournies dans l'environnement par défaut sont de type *fonction*. Une fonction est un morceau de programme encapsulé dans une valeur. De telles valeurs peuvent être appliquées afin d’exécuter le programme encapsulé. Par exemple, dans un environnement de navigateur, le binding `prompt` contient une fonction qui affiche une petite boîte de dialogue demandant une entrée utilisateur. Il est utilisé ainsi :

```javascript
prompt("Entrer le mot de passe");
```

![Une boîte de dialogue de saisie](https://eloquentjavascript.net/img/prompt.png)

Exécuter une fonction est appelé *invoquer*, *appeler*, ou *appliquer* une fonction. Vous pouvez appeler une fonction en mettant des parenthèses après une expression qui produit une valeur de fonction. Habituellement, vous utiliserez directement le nom du binding qui contient la fonction. Les valeurs entre les parenthèses sont données au programme à l'intérieur de la fonction. Dans l'exemple, la fonction  `prompt` utilise la chaîne que nous lui donnons en tant que texte à afficher. Les valeurs passées à des fonctions sont appelés *arguments*. Différentes fonctions peuvent avoir besoin d'un nombre différent ou de différents types d'arguments.

La fonction `prompt` n'est pas très utilisée dans la programmation web moderne, principalement parce que vous n'avez pas de contrôle sur la manière sur l'apparence des boîtes de dialogue, mais cela peut être un  utile pour des petits programmes ou des expérimentations.

## La fonction console.log

Dans les exemples, j'ai utilisé `console.log` pour afficher des valeurs. La plupart des systèmes JavaScript (incluant tous les navigateurs web modernes et Node.js) fournissent une fonction `console.log` qui retranscrit ses arguments sur un périphérique de sortie texte. Dans les navigateurs, la sortie arrive dans la console JavaScript. Cette partie de l'interface du navigateur est masquée par défaut, mais la plupart des navigateurs l'ouvre lorsque vous pressez `F12` ou, sur un Mac, `COMMAND-OPTION-I`. Si cela ne fonctionne pas, cherchez dans les menus pour un item nommé Developer Tools ou similaire.

Lors de l'exécution des exemples (ou de votre propre code) dans les pages de ce livre, la sortie `console.log` sera affichée après l'exemple, au lieu de la console JavaScript du navigateur.

```javascript
let x = 30;
console.log("la valeur de x est", x);
// → la valeur de x est 30
```

Bien que les noms de bindings ne peuvent pas contenir de caractères point, `console.log` en a un. Ceci est dû au fait que `console.log` n'est pas un simple binding. C'est en fait une expression qui retrouve la propriété `log` de la valeur contenue par le binding `console`. Nous verrons exactement de quoi il en retourne au [Chapitre 4]().

## Valeurs de retour

Afficher une boîte de dialogue ou afficher du texte à l'écran est un *effet secondaire*. Beaucoup de fonctions sont utiles à cause des effets secondaires qu'elles produisent. Des fonctions peuvent aussi produire des valeurs, dans quel cas elle n'ont pas besoin d'avoir un effet secondaire pour être utiles. Par exemple, la fonction `Math.max` prend n'importe quelle quantité de nombres en argument et redonne le plus grand.

```javascript
console.log(Matx.max(2,4));
// → 4
```

Quand une fonction produit une valeur, on dit qu'elle *retourne* cette valeur. N'importe quoi produisant une valeur est une expression en JavaScript, ce qui signifie que les appels aux fonctions peuvent être utilisés dans des expressions plus grandes. Ici un appel à `Math.min`, qui est l'opposé de `Math.max`, est utilisé comme une composante d'une expression plus (+).

```javascript
console.log(Math.min(2, 4) + 100);
// → 102
```

Le [prochain chapitre]() explique comment écrire vos propres fonctions.

## Flux de contrôle

Quand votre programme contient plus d'une instruction, les instructions sont exécutées comme s'il s'agissait d'une histoire, de haut en bas. Cet exemple de programme à deux instructions. La première demande un nombre à l'utilisateur, et la seconde, qui est exécutée après la première, affiche le carré de ce nombre.

```javascript
let leNombre = Number(prompt("Choissez un nombre"));
console.log("Votre nombre est la racine carrée de " +
            leNombre * leNombre);
```

La fonction `Number` convertit une valeur en un nombre. Nous avons besoin de cette conversion car le résultat de `prompt` est une chaîne, et nous voulons un nombre. Il y a des fonctions similaires appelées `String` et `Boolean` qui convertissent dans ces types.

Voici la représentation schématique, plutôt triviale, du flux de contrôle linéaire :

![Contrôle de flux trivial](https://eloquentjavascript.net/img/controlflow-straight.svg)

## Exécution conditionnelle

Tous les programmes ne sont pas des routes droites. Nous pouvons, par exemple, vouloir créer un embranchement, où le programme prends la branche correcte en fonction de la situation. Ceci est appelé *exécution conditionnelle*.

![Flux de contrôle conditionnel](https://eloquentjavascript.net/img/controlflow-if.svg)

Une exécution conditionnelle est crée avec le mot-clef `if` en JavaScript. Dans le cas simple, nous vous que du code soit exécuté if, et seulement si, une certaine condition est satisfaite. Nous pouvons, par exemple, vouloir afficher le carré de l'entrée seulement si l'entrée est effectivement un nombre.

```javascript
let leNombre = Number(prompt("Choissez un nombre"));
if (!Number.isNaN(leNombre)) {
  console.log("Votre nombre est la racine carrée de " +
              leNombre * leNombre);
}
```

Avec cette modification, si vous saisissez "perroquet", rien n'est affiché.

Le mot-clef `if` exécute ou saute une instruction en fonction de la valeur d'une expression booléenne. L'expression décisive est écrite après le mot-clef, entre parenthèse, suivi de l'instruction à exécuter.

La fonction `Number.isNaN` est une fonction JavaScript standard qui retourne `true` uniquement si l'argument qui lui est donné est `NaN`. La fonction `Number` retourne `NaN` quand vous lui donnez une chaîne qui ne représente pas un nombre valide. Et donc, la condition se traduit en "à moins que `leNombre` ne soit pas un nombre, fait ceci".

L'instruction après le `if` est encadrée par des accolades (`{` et `}`) dans cet exemple. Les parenthèses peuvent être utilisées pour grouper n'importe quel nombre d'instructions en une unique instruction, appelé *bloc*. Vous pourriez aussi les avoir omises dans ce cas, puisqu'elles ne contiennent qu'une seule instruction, mais pour éviter d'avoir à penser si elles sont nécessaires, la plupart des programmeurs JavaScript les utilisent pour chaque instruction encapsulée comme celle-ci. Nous suivrons, la plupart du temps, cette convention dans ce livre, excepté pour les occasionnelles unique ligne.

```javascript
if (1 + 1 == 2) console.log("C'est vrai");
// → C'est vrai
```

Souvent, vous n'avez pas seulement le code qui s'exécute quand une condition est vraie, mais aussi le code qui prend en charge l'autre cas. Ce chemin alternatif est représenté par la second flèche dans le diagramme. Vous pouvez utiliser le mot-clef `else`, ensemble avec `if`, pour créer deux chemins d'exécution alternatifs séparés.

```javascript
let leNombre = Number(prompt("Choissez un nombre"));
if (!Number.isNaN(leNombre)) {
  console.log("Votre nombre est la racine carrée de " +
              theNumber * theNumber);
} else {
  console.log("Hé. Pourquoi ne m'avez-vous pas donné un nombre ?");
}
```

Si vous devez choisir parmi plus de deux chemins, vous pouvez chaîner plusieurs paires `if\else` ensemble. Voici un exemple :

```javascript
let nombre = Number(prompt("Choissez un nombre"));

if (nombre < 10) {
  console.log("Petit");
} else if (nombre < 100) {
  console.log("Moyen");
} else {
  console.log("Grand");
}
```

Le programme va d'abord vérifier si `num` est plus petit que 10. Si c'est le cas, il choisit cette branche, affiche "Petit", et c'est fini. Si ce n'est pas le cas, il prend la branche `else`, qui elle même contient un second `if`. Si la seconde condition (`< 100`) est satisfaite, cela signifie que le nombre est compris entre 10 et 100, et "Moyen" est affiché. Sinon, le deuxième et dernière branche `else` est choisie.

Le schéma pour ce programme ressemble à ceci :

![Contrôle de flux avec if imbriqués](https://eloquentjavascript.net/img/controlflow-nested-if.svg)

