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

## Boucles while et do

Prenez un programme qui affiche tout les nombres pairs de 0 à 12. Une façon de l'écrire est comme cela :

```javascript
console.log(0);
console.log(2);
console.log(4);
console.log(6);
console.log(8);
console.log(10);
console.log(12);
```

Ça fonctionne, mais l'idée d'écrire un programme est de faire quelque chose avec *moins* de travail, et non pas plus. Si nous avons besoin de tous les nombres inférieurs à 1000, cette approche serait impraticable. Ce dont nous avons besoin c'est une façon d'exécuter un morceau de code plusieurs fois. Cette forme de flux de contrôle est appelée une *boucle*.

![Boucle de contrôle de flux](https://eloquentjavascript.net/img/controlflow-loop.svg)

La mise en boucle du flux de contrôle nous permet de revenir à un point donné dans le programme où nous étions avant et le répéter avec l'état courant de notre programme. Si nous combinons cela avec un binding qui compte, nous pouvons faire quelque chose comme :

```javascript
let nombre = 0;
while (nombre <= 12) {
  console.log(nombre);
  nombre = nombre + 2;
}
// → 0
// → 2
//   … etcetera
```

Une instruction avec le mot-clef `while` créer une boucle. Le mot `while` est suivi d'une expression entre parenthèses et puis d'une instruction, un peu comme `if`. La boucle continue d'entrer dans cette instruction aussi longtemps que l'expression produit une valeur qui vaut `true` quand elle est convertie en booléen.

Le binding `nombre` démontre la manière dont un binding peut suivre la progression d'un programme. À chaque fois que la boucle se répète, `nombre` reçoit une valeur qui est 2 de plus que sa précédente valeur. Au début de chaque répétition, il est comparé avec le nombre 12 pour décider si oui ou non le travail du programme est terminé.

En tant qu'exemple faisant effectivement quelque chose d'utile, nous pouvons maintenant écrire un programme qui calcule et affiche la valeur de 2<sup>10</sup> (2 à la puissance de 10). Nous utilisons deux bindings : un pour garder une trace de notre résultat et un pour compter combien de fois nous avons multiplié ce résultat par 2. La boucle vérifie si le second binding a déjà atteint 10, si non, elle met à jour les deux bindings.

```javascript
let resultat = 1;
let compteur = 0;
while (compteur < 10) {
  resultat = resultat * 2;
  compteur = compteur + 1;
}
console.log(result);
// → 1024

```

Le compteur aurait aussi put commencer à `1` et vérifier `<=10`, mais pour des raisons qui deviendront plus apparentes au [Chapitre 4](), c'est une bonne idée de s'habituer à compter à partir de `0`.

Une boucle `do` est une structure de contrôle similaire à une boucle `while`. Elle diffère seulement sur un point : une boucle `do` exécute toujours son contenu au moins une fois, et elle commencer à vérifier si elle doit s'arrêter seulement après la première exécution. Pour refléter cela, la vérification apparaît après le contenu de la boucle.

```
let votreNom;
do {
  votreNom = prompt("Qui êtes-vous ?");
} while (!votreNom);
console.log(votreNom);
```

Ce programme vous force à saisir votre nom. Il va demander encore et encore jusqu'à ce qu'il reçoive quelque chose qui n'est pas une chaîne vide. Appliquer l'opérateur `!` à une valeur la convertira en type booléen avant de l'opposer, et toute chaîne sauf `""` est convertie en `true`. Cela signifie que la loupe continue de tourner en rond jusqu'à ce que vous fournissiez un nom non-vide.

## Indenter le code

 

Dans les exemples, j'ai ajouté des espaces devant les instructions qui font partie d'instruction plus large. Ces espaces ne sont pas nécessaires ­— l'ordinateur acceptera le programme très bien sans eux. En fait, même les sauts de lignes sont optionnels. Vous pouvez écrire un programme sur une unique longue ligne si vous le sentez ainsi.

Le rôle de cette indentation dans les blocs et de faire apparaître la structure du code. Dans le code, où de nouveaux blocs sont ouverts dans d'autres blocs, il peut devenir difficile de voir où un bloc se termine et un autre commence. Avec un indentation correcte, la forme visuelle d'un programme correspond à la forme des blocs à l'intérieur. J'aime utiliser deux espaces pour chaque bloc ouvert, mais les goûts varient — certaines personnes utilisent quatre espaces, et d'autres personnes utilisent des caractères tabulation. Ce qui est important est que chaque nouveau bloc ajoute le même nombre d'espaces.

```javascript
if (false != true) {
  console.log("Cela fait sens.");
  if (1 < 2) {
    console.log("Pas de surprise ici.");
  }
}
```

La plupart des éditeurs de code (incluant celui de ce livre) aideront à indenter automatiquement les nouvelles lignes avec le nombre correct d'espaces.

## Boucles for

Beaucoup de boucles suivent le modèle présenter dans les exemples `while`. D'abord un binding "compteur" est créé pour suivre la progression de la boucle. Puis vient une boucle `while`, habituellement avec une expression de test qui vérifie si le compteur à atteint sa valeur de fin. À la fin du contenu de la boucle, le compteur est mis à jour pour suivre la progression.

Parce que que ce modèle est si commun, JavaScript et les langages similaires fournissent un format légèrement plus courte et plus compréhensible, la boucle `for`.

```javascript
for (let nombre = 0; nombre <= 12; nombre = nombre + 2) {
  console.log(nombre);
}
// → 0
// → 2
//   … etcetera
```

Ce programme est un équivalent exact à l'exemple [précédent](#boucles-while-et-do) d'affichage des nombres pairs. Le seul changement est que toutes les instructions liées à l'état de la boucle sont groupés ensemble après `for`.

Les parenthèses après un mot-clef `for` doivent contenir deux points-virgules. La partie avant le premier point-virgule *initialise* la boucle, habituellement en définissant un binding. La second partie est l'expression qui *vérifie* si la boucle doit continuer. La dernière partie *met à jour* l'état de la boucle après chaque itération. Dans la plupart des cas, c'est plus court et plus clair qu'une construction `while`.

Voici le code qui calcule 2<sup>10</sup> utilisant `for` à la place de `while` :

```javascript
let resultat = 1;
for (let compteur = 0; compteur < 10; compteur = compteur + 1) {
  resultat = resultat * 2;
}
console.log(resultat);
// → 1024
```

## Sortir d'une boucle

Avoir la condition de bouclage produisant `false` n'est pas la seule manière dont une boucle peut se terminer. Il y a une instruction spéciale appelé `break` qui a pour effet d'immédiatement sortir de la boucle.

Ce programme illustre l'instruction `break`. Il cherche le premier nombre qui est à la fois plus grand ou égale à 20 et divisible par 7.

```javascript
for (let courant = 20; ; courant = courant + 1) {
  if (courant % 7 == 0) {
    console.log(courant);
    break;
  }
}
// → 21
```

Utiliser l'opérateur de reste (`%`) est une façon simple de tester si un nombre est divisible par un autre nombre. Si tel est le cas, le reste de la division vaut zéro.

La construction `for` dans l'exemple n'a pas la partie qui vérifie la fin de la boucle est atteinte. Cela signifie que la boucle ne s'arrêtera jamais à moins que l'instruction `break`, à l'intérieur, ne soit exécutée.

Si vous deviez enlever cette instruction `break`, ou accidentellement écrire une condition de fin qui produit toujours `true`, votre programme serait bloqué dans une *boucle infinie*. Un programme coincé dans une boucle infinie ne finira jamais sont exécution,  ce qui est généralement une mauvaise chose.

Si vous créez une boucle infinie dans l'un des exemples de ces pages, on vous demandera généralement si vous voulez arrêter le programme après quelques secondes. Si cela échoue, vous devrez fermer l'onglet dans lequel vous travaillez, ou sur certain navigateur fermer le navigateur complet, pour reprendre.

Le mot-clef `continue` est similaire à `break`, dans le fait qu'il influence la progression d'une boucle. Quand `continue` est rencontré dans le contenu d'une boucle, le contrôle saute hors du contenu et continue avec l'itération suivante de la boucle.

## Mettre à jour succinctement des bindings

En particulier lors de la mise en boucle, un programme doit souvent "mettre à jour" un binding pour conserver une valeur basée sur la valeur précédente de ce binding.

```javascript
compteur = compteur + 1;
```

JavaScript fournit un raccourcit pour cela.

```javascript
compteur += 1;
```

Des raccourcis similaires fonctionnent pour de nombreux autres opérateurs, tels que `resultat =* 2` pour doubler `resultat` ou `compteur =-1` pour décrémenter.

Cela nous permet de raccourcir un peu plus notre exemple de comptage.

```javascript
for (let nombre = 0; nombre <= 12; nombre += 2) {
  console.log(nombre);
}
```

Pour `compteur += 1` et `compteur -=1`, il y a même des équivalents plus courts : `compteur++` et `compteur--`.

## Répartir sur la base d'une valeur avec switch

Il n'est pas rare pour du code de ressembler à cela :

 ```javascript
if (x == "valeur1") action1();
else if (x == "valeur2") action2();
else if (x == "valeur3") action3();
else actionDefaut();
 ```

Il y a une construction appelé `switch` qui est prévue pour exprimer une telle "répartition" d'une façon plus directe. Malheureusement, la syntaxe JavaScript utilisée pour cela (qui est héritée de la lignée des langages de programmation C/Java) n'est pas très pratique — une chaîne d'instruction `if` peut sembler meilleure. En voici un exemple :

```javascript
switch (prompt("Quel temps fait-il?")) {
  case "pluvieux":
    console.log("Pensez à prendre un parapluie.");
    break;
  case "ensoleillé":
    console.log("Habillez-vous légèrement.");
  case "nuageux":
    console.log("Allez dehors.");
    break;
  default:
    console.log("Type de temps inconnu !");
    break;
}
```

Vous pouvez mettre n'import quel nombre de labels `case` dans le bloc ouvert part `switch`. Le programme commencera l'exécution à partir du label correspondant à la valeur qui a été passée à `switch`, ou à partir de `default` si aucune valeur correspondante n'a été trouvée. Il continuera l'exécution, même au travers d'autres labels, jusqu'à ce qu'il atteigne une instruction `break`. Dans certains cas, tel que le cas "ensoleillé" de cet exemple, cela peut être utilisé pour partager du code entre les options (il recommande d'aller dehors pour le temps ensoleillé et le temps nuageux). Mais faites attention — il est facile d'oublier un tel `break`, ce qui obligera le programme à exécuter du code que vous ne voulez pas être exécuté.

## Majuscules

Les noms de bindings ne peuvent pas contenir d'espace, cependant il est souvent pratique d'utiliser plusieurs mots pour décrire clairement ce que le binding représente. Voici à peu près vos choix pour écrire un nom de binding avec plusieurs mots :

```
petitetortuefloue
petite_tortue_floue
PetiteTortueFloue
petiteTortueFloue
```

Le premier style peut être difficile à lire. Je préfère plutôt l'aspect des tirets-bas, bien que ce style un peu pénible à taper. Les fonctions JavaScript standard, et la plupart des développeurs JavaScript, suivent le style du bas — ils mettent en majuscules tous les mots sauf le premier. Il n'est pas difficile de s'habituer à de petite chose comme ça, et du code avec des style de nommage différents peut être perturbant à lire, donc nous suivons cette convention.

Dans de rares cas, tels que la fonction `Number`, la première lettre d'un binding est aussi en majuscule. Cela a été fait pour marquer cette fonction en tant que constructeur. Ce qu'est un constructeur deviendra plus clair dans le [Chapitre 6](). Pour le moment, l'important est de ne pas être dérangé par le manque apparent de cohérence.

## Commentaires

Souvent, le code brut n'exprime pas toute l'information que vous voulez qu'un programme transmette aux lecteurs humains, ou bien il l'exprime d'une manière tellement cryptique que les gens ne pourront pas comprendre. À d'autres moments, vous voudrez peut-être juste inclure des idées connexes dans votre programme. C'est ce pourquoi les *commentaires* sont faits.

Un commentaire est un morceau de texte qui fait partie d'un programme mais est complètement ignoré par l'ordinateur. JavaScript a deux manières pour écrire les commentaires. Pour écrire un commentaire en une seule ligne, vous pouvez utiliser deux caractères barre oblique (`//`) et puis, après, le texte de commentaire.

```javascript
let balanceCompte = calculerBalance(compte);
// C'est un trou de verdure où chante une rivière
balanceCompte.ajuster();
// Accrochant follement aux herbes des haillons d'argent
let rapport = new Rapport();
// Où le soleil, de la montagne fière, luit
ajouterAuRapport(balanceCompte, rapport);
// C'est un petit val qui mousse de rayons.
```

Un commentaire `//` va uniquement jusqu'à la fin de la ligne. Une section de texte comprise entre `/*` et `*/` sera ignorée complètement, quand bien même elle contiendrait des sauts de ligne. C'est utile pour ajouter des blocs d'information à propos d'un fichier ou d'un bout de programme.

```javascript
/*
  J'ai d'abord trouvé ce numéro griffoné au dos d'un vieux carnet de notes.
  Depuis lors, il est souvent revenu, apparaissant parmis les numéros de téléphones
  et les numéros de série des produits que j'ai acheté. Il est évident qu'il m'aime,
  donc j'ai décidé de le garder.
*/
const monNumero = 11213;
```

## Résumé

Vous savez maintenant qu'un programme est fait à partir d'instructions, qui elles-même contiennent quelque fois d'autres instructions. Des instructions tendent à contenir des expressions, qui elles-même peuvent être construites à partir d'expression plus petites.

Mettre des instructions les unes derrière les autres vous donne un programme qui est exécuté de haut en bas. Vous pouvez introduire des perturbations dans le flux de contrôle en utilisant des instructions conditionnelles (`if`, `else`, et `switch`) et des instructions de boucle (`while`, `do`, et `for`).

Des bindings peuvent être utilisés pour mettre un nom sur des morceaux de données, et ils sont utiles pour suivre l'état de votre programme. L'environnement est l'ensemble des bindings qui sont définis. Les systèmes JavaScript ajoutent toujours un certain nombre de binding standard utiles dans votre environnement.

Les fonctions sont des valeurs spéciales qui encapsulent un morceau de programme. Vous pouvez les invoquer en écrivant `nomFonction(argument1, argument2)`. Un tel appel de fonction est une expression et peut produire une valeur.

## Exercices

Si vous n'êtes par sûr de comment tester vos solutions aux exercices, référez-vous à l'[Introduction]().

Chaque exercice commence avec une description du problème. Lisez cette description et essayez de résoudre l'exercice. Si vous rencontrez des problèmes,  envisagez de lire les astuces après l’exercice. Les solutions complètes aux exercices ne sont pas incluses dans ce livre, mais vous pouvez les trouver en ligne à l'adresse [https://eloquentjavascript.net/code](https://eloquentjavascript.net/code#2). Si vous voulez apprendre quelque chose des exercices, je recommande de regarder les solutions seulement après que vous ayez résolu l'exercice, ou au moins après que vous l'ayez attaqué assez longtemps et avec assez d'insistance pour avoir un léger mal de tête.

### Mettre un triangle en boucle

Écrivez un programme qui effectue sept appels à `console.log` pour afficher le triangle suivant :

```
#
##
###
####
#####
######
#######
```

Il sera utile de savoir que vous pouvez trouver la longueur d'une chaîne en écrivant `.length` après elle.

```javascript
let abc = "abc";
console.log(abc.length);
// → 3
```

La plupart des exercices contiennent un morceau de code que vous pouvez modifier pour résoudre l'exercice. Souvenez-vous que vous pouvez cliquer sur les blocs de code pour les éditer.

```javascript
// Votre code ici
```

> Vous pouvez commencer avec un programme qui affiche les nombres 1 à 7, que vous pouvez obtenir en faisant quelques modifications à l'exemple d'affichage des nombres pairs donné plutôt dans ce chapitre, là où la boucle `for` a été introduite.
>
> Maintenant considérez l'équivalence entre nombres et chaîne de caractères dièse. Vous pouvez aller de 1 à 2 en ajoutant 1 (`+= 1`). Vous pouvez aller de "#" à "##" en ajoutant un caractère (`+= "#"`) . Et donc, votre solution peut suivre de près le programme d'affichage de nombres.

### FizzBuzz

Écrivez un programme qui utilise `console.log` pour afficher tous les nombres de 1 à 100, avec deux exceptions. Pour les nombres divisibles par 3, affichez "`Fizz`" au lieu du nombre, et pour les nombres divisible par 5 (et pas par 3), affichez "`Buzz`" à la place.

Quand avez fait fonctionner ça, modifiez votre programme pour afficher "`FizzBuzz`" pour les nombres qui sont à la fois divisible par 3 et 5 (et toujours afficher "`Fizz`" ou "`Buzz`" pour les nombres divisible uniquement par un de ces nombres).

(C'est en fait un question d'entretient censée éliminer un large pourcentage de candidats programmeurs. Donc, si vous l'avez résolu, votre valeur sur le marché du travail vient juste d'augmenter.)

```javascript
// Votre code ici
```

> Passer en revue les nombres est clairement un travail de boucle, et sélectionner quoi afficher est une question d'exécution conditionnelle. Souvenez-vous du truc utilisant l'opérateur de reste (`%`) pour vérifier si un nombre est divisible par un autre (il a un reste à zéro).
>
> Dans la première version, il y trois possibilités pour chaque nombre, donc vous allez devoir créer une chaîne de `if/else if else`.
>
> La seconde version du programme à une solution simple et une intelligente. La solution simple c'est d'ajouter une autre "branche" conditionnelle pour précisément tester la condition donnée. Pour la solution intelligente, construisez une chaîne contenant le ou les mots à afficher et affichez soit ce mot ou le nombre s'il n'y a pas de mot, potentiellement en faisant bon usage de l'opérateur `||`.

### Plateau d'échecs

Écrivez un programme qui créer un chaîne qui représente une grille de 8×8, utilisant des caractères nouvelle ligne pour séparer les lignes. À chaque position de la grille, il y a soit un espace ou soit un caractère "#". Les caractères doivent former un plateau d'échecs.

Passer cette chaîne à `console.log` devrait afficher quelque chose comme :

```
 # # # #
# # # # 
 # # # #
# # # # 
 # # # #
# # # # 
 # # # #
# # # #
```

Quand vous avez un programme qui génère ce motif, définissez un binding `taille = 8` et modifiez le programme pour qu'il fonctionne avec n'importe quelle `taille`, affichant une grille de largeur et hauteur données.

```
// Votre code ici
```

> Vous pouvez construire la chaîne en commençant avec une chaîne vide (`""`) et ajoutant des caractères de façon répétitive. Un caractère nouvelle ligne s'écrit "`\n`".
>
> Pour travailler avec deux dimensions, vous allez avoir besoin d'une boucle dans une boucle. Mettez des accolades autour des corps de chaque boucle pour facilement voir où elles commencent et se terminent. Essayez d'indenter correctement le contenu des boucles. L'ordre des boucles doit suivre l'ordre dans lequel nous allons construire la chaîne (ligne par ligne, gauche à droite, haut en bas). Donc la boucle extérieure se charge des lignes, et la boucle intérieure se charge des caractères sur une ligne.
>
> Vous allez avoir besoin de deux bindings pour suivre votre progression. Pour savoir s'il faut mettre un espace ou un dièse à une position donnée, vous pouvez tester si la somme des deux compteur est paire (`% 2`).
>
> Terminer une ligne en ajoutant un caractère nouvelle ligne doit être fait après que la ligne ait été construite, donc le faire après la boucle intérieure mais dans la boucle extérieure.