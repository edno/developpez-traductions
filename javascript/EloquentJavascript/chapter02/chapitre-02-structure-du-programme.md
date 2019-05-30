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

