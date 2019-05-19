# Valeurs, Types et Opérateurs

> Sous la surface de la machine, le programme bouge. Sans effort, il s’étend et se contracte. En grande harmonie, les électrons se dispersent et se regroupent. Les formes sur l’écran ne sont que des ondulations sur l'eau. L'essence reste invisible en dessous.
>
> — Maître Yuan-Ma, *Le Livre de la Programmation*

![Image d'une mer de bits](https://eloquentjavascript.net/img/chapter_picture_1.jpg)

À l’intérieur du monde de l'ordinateur, ce ne sont que des données. Vous pouvez lire des données, modifier des données, créer de nouvelles données — mais ce qui n'est pas une donnée ne peut pas être mentionné. Toutes ces données sont stockées comme de long séquences de bits et sont donc fondamentalement identiques.

Bits sont n'importe quoi ayant deux valeurs, habituellement décris comme zéros et uns. À l’intérieur de l'ordinateur, ils prennent des formes telles qu'une charge électrique haute ou basse, un signal fort ou faible, ou un point brillant ou terne sur la surface d'un CD. N'importe quelle information discrète peut être réduite à une séquence de zéros et de uns, et donc représentés en bits.

Par exemple, nous pouvons exprimer le nombre 13 en bits. Cela fonctionne de la même façon qu'un nombre décimal, mais au lieu de 10 différents chiffres, vous n'en avez que 2, et le poids de chacun accroît par un facteur 2 de droite à gauche. Voici les bits qui font le nombre 13, avec le poids des chiffres affiché en dessous d'eux :

```
   0   0   0   0   1   1   0   1
 128  64  32  16   8   4   2   1
```

C'est donc le nombre binaire 00001101. Ses chiffres non nuls représentent 8, 4 et 1, et totalisent 13. 

## Valeurs

Imaginez une mer de bits — un océan de bits. Un ordinateur moderne typique a plus de 30 milliards de bits dans son stockage de données volatiles (mémoire de travail). Le stockage non-volatile (le disque dur ou équivalent) tend à en avoir plus par plusieurs ordres de magnitude.

Pour être capable de travailler avec de telles quantités de bits sans être perdu, nous devons les séparer en morceaux qui représentent des éléments d’information. Dans un environnement JavaScript, ces morceaux sont appelés *valeurs*. Bien que toutes ces valeurs soient faites de bits, elles jouent des rôles différents. Toute valeur a un type qui détermine son rôle. Certaines valeurs sont des nombres, certaines sont des morceaux de texte, certaines sont des fonctions, et ainsi de suite.

Pour créer une valeur, vous devez juste invoquer son nom. C'est pratique. Vous n'avez pas besoin de collecter des matériaux de construction pour vos valeurs ou payer pour elles. Vous en appelez une, et tada, vous l'avez. Bien sûr, elles ne sont pas vraiment créées à partir de rien. Chaque valeur doit être stockée quelque part, et si vous voulez utiliser une gigantesque quantité d'entre elles au même moment, vous pouvez être à court de mémoire. Heureusement, cela n'est un problème que si vous avez besoin de toutes simultanément. Dès que vous n'utilisez plus une valeur, elle disparaîtra, laissant derrière elle ses bits pour être recyclés comme matériel de construction pour la prochaine génération de valeurs.

Ce chapitre introduit les éléments atomiques des programmes JavaScript, qui sont les types simples de valeurs et les opérateurs qui peuvent agir sur de telles valeurs.

## Nombres

Les valeurs du type nombre sont, sans surprise, des valeurs numériques. Dans un programme JavaScript, elles sont écrites ainsi :

```javascript
13
```

Utilisez cela dans un programme, et cela entraînera la création du motif de bit pour le nombre 13 dans la mémoire de l’ordinateur.

JavaScript utilise un nombre fixe de bits, 64 d'entre eux, pour stocker une seule valeur nombre. Il existe seulement un certain nombres de motifs que vous pouvez faire avec 64 bits, ce qui signifie que le nombre de différents nombres pouvant être représentés est limité. Avec *N* nombres décimaux, vous pouvez représenter 10<sup>N</sup> nombres. De façon similaire, avec 64 chiffres binaires, vous pouvez représenter 2<sup>64</sup> nombres différents, ce qui vaut environ 18 quintillions (un 18 suivi de 18 zéros). C'est beaucoup.

La mémoire d'ordinateur a été beaucoup plus petite, et les gens tendaient à utiliser des groupes de 8 ou 16 bits pour représenter leurs nombres. Il était facile de déborder (*overflow*) de si petits nombres par accident — pour finir avec un nombre qui ne peut pas être contenu dans le nombre donné de bits. Aujourd'hui, même les ordinateurs qui rentrent dans votre poche ont plein de mémoire, donc vous êtes libres d'utiliser des morceaux de 64 bits, et vous devez seulement vous inquiéter quand vous traiter avec des nombres réellement astronomiques.

Cependant, Les nombres de moins de 18 quintillions ne peuvent pas tous être contenus dans un nombre JavaScript. Ces bits stockent aussi des nombres négatifs, donc un bit indique le signe du nombre. Un problème plus important est que les nombres non-entiers doivent aussi être représentés. Pour faire cela, certains bits sont utilisés pour stocker la position du séparateur décimal. Le vrai nombre entier maximal qui peut être stocké est plus autour de 9 quadrillions (15 zéros) — ce qui est toujours plaisamment énorme.

Les nombres fractionnaires sont écrits avec un point.

```javascript
9.81
```

Pour de très grands ou très petites nombres, vous pouvez aussi utiliser la notation scientifique en ajoutant un *e* (pour *exposant*), suivi par l'exposant du nombre.

```javascript
2.998e8
```

C'est 2.998 × 108 = 299 800 000.

Les calculs avec les nombres entiers (aussi appelés *entiers*) plus petits que le susmentionné 9 quadrillions sont garantis d’être toujours précis. Malheureusement, les calculs avec les nombres fractionnaires ne le sont généralement pas. Tout comme π (pi) ne peut pas être exprimé précisément par un nombre fini de chiffres décimaux, beaucoup de nombres perdent en précision quand seulement 64 bits sont disponibles pour les stocker. C'est une honte, mais cela ne cause des problèmes pratiques que dans des situations spécifiques. Ce qui est important c'est de le savoir et de traiter les nombres numériques fractionnaires comme approximations, et non pas comme des valeurs précises.

### Arithmétique

La principale chose à faire avec les nombres c'est de l’arithmétique. Les opérations arithmétiques comme l'addition ou la multiplication prennent deux valeurs nombre et produisent un nouveau nombre à partir d'eux. Voici à quoi cela ressemble en JavaScript :

```javascript
100 + 4 * 11
```

Les symboles `+` et `*` sont appelés *opérateurs*. Le premier correspond à l'addition, et le second correspond à la multiplication. Mettre un opérateur entre deux valeurs l'appliquera à ces valeurs et produira une nouvelle valeur.

Mais est-ce que l'exemple signifie “ajouter 4 et 100, et multiplier le résultat par 11“, ou est-ce que la multiplication est réalisée avant l'addition ? Comme vous avez dû le deviner, la multiplication est faite en premier. Mais comme en arithmétique, vous pouvez changer cela en encadrant l'addition avec des parenthèses.

```javascript
(100 + 4) * 11
```

Pour la soustraction, il y a l’opérateur `-`, et la division peut être faite avec l’opérateur `/`.

Quand les opérateurs apparaissent ensemble sans parenthèses, l'ordre dans lequel ils sont appliqués est déterminé par la *précédence* des opérateurs. L'exemple montre que la multiplication vient avant l'addition. L’opérateur `/` à la même précédence que `*`. De même pour `+` et `-`. Quand plusieurs opérateurs avec la même précédence apparaissent les uns après les autres, comme dans `1 - 2 + 1`, ils sont appliqués de gauche à droite : `(1 - 2) + 1`.

Ces règles de précédence ne sont pas quelque chose dont vous devez vous inquiéter. En cas de doute, ajouter simplement des parenthèses.

Il y a un autre opérateur arithmétique, que vous ne reconnaîtrez peut-être pas immédiatement. Le symbole `%` est utilisé pour représenter l’opération de *reste*. `X % Y` est le reste de la division de `X` par `Y`. Par exemple, `314 % 100` produit `14`, et `144 % 12` retourne `0`. La précédence de l’opérateur de reste est le même que pour la multiplication et la division. Vous verrez aussi souvent cet opérateur être appelé *modulo*.

### Nombres spéciaux

Il y a trois valeurs spéciales en JavaScript qui sont considérées comme des nombres mais ne se comportent pas comme des nombres normaux.

Les deux premières sont `Infinity` et `-Infinity` qui représentent les infinis positif et négatif. `Infinity - 1` est toujours `Infinity`, et ainsi de suite. Cependant, ne faites pas trop confiance aux calculs basés sur l’infini. Cela n'est pas mathématiquement correct, et cela mènera rapidement au prochain nombre spécial : `NaN`.

`NaN` signifie "*not a number*" (pas un nombre), cependant *c'est* une valeur de type nombre. Vous obtiendrez ce résultat quand, par exemple, vous essayez de calculer `0 / 0` (zéro divise par zéro), `Infinity - Infinity`, ou n'importe quelle autre opération numérique qui ne retourne pas de résultat significatif.

## Chaînes

Le type de donnée de base suivant est la chaîne (*string*). Les chaînes sont utilisées pour représenter du texte. Elles sont écrites en encadrant leur contenu avec des guillemets.

```javascript
`Down on the sea`
"Lie on the ocean"
'Float on the ocean'
```

Vous pouvez utiliser des guillemets simples, des guillemets doubles, ou des guillemets obliques pour marquer les chaînes, du moment que le début et la fin de la chaîne concordent.

Presque tout peut être mis entre guillemets, et JavaScript en fera une chaîne. Mais quelques caractères sont plus difficiles.  Vous pouvez imaginer comment mettre des guillemets entre guillemets peut être compliqué. Les *sauts de ligne* (les caractères que vous avez quand vous pressez `ENTER`) peuvent être inclus sans échappement seulement si la chaîne est entre guillemets obliques (`` `).

Pour rendre possible d'inclure de tels caractères dans une chaîne, la notation suivante est utilisée : dès qu'un anti-slash (`\`) est trouvé dans un texte entre guillemets, il indique que le caractère suivant a une signification spéciale. Ceci est appelé *échapper* le caractère. Un guillemet qui est précédé par un anti-slash ne terminera pas la chaîne mais en fera partie. Quand un caractère `n` apparaît après un anti-slash, il est interprété comme un saut de ligne. De façon similaire, un `t` après un anti-slash signifie un caractère tabulation. Prenez la chaîne suivante :

```javascript
"This is the first line\nAnd this is the second"
```

Le texte réel contenu est :

```
This is the first line
And this is the second
```

Les chaînes, aussi, doivent être modelées comme des séries de bits pour pouvoir exister dans l'ordinateur. La façon dont JavaScript le fait est basé sur la norme *Unicode*. Cette norme assigne virtuellement un nombre à chaque caractère dont vous pourrez avoir besoin, incluant les caractères du grec, arabe, japonais, arménien, et ainsi de suite. Si nous avons un nombre pour chaque caractère, une chaîne peut être décrite comme une séquence de nombres.

Et c'est ce que JavaScript fait. Mais il y a une complication : la représentation JavaScript utilise 16 bits par  élément de chaîne, ce qui décrit jusqu'a 2<sup>16</sup> caractères différents. Mais Unicode définit plus de caractères que cela — à peu près deux fois plus, à ce stade. Donc certains caractères, tels que beaucoup d’émoticônes, prennent jusqu'à deux "positions de caractère" dans les chaînes JavaScript. Nous y reviendrons au [Chapitre 5]().

Les chaînes ne peuvent pas être divisées, multipliées, ou soustraites, mais l’opérateur `+` *peut* être utilisé sur elles. Il n'additionne pas, mais il *concatène* — il colle deux chaînes ensemble. La ligne suivante produira le chaîne "`concatène`" :

```javascript
"con" + "cat" + "è" + "ne"
```

Les valeurs chaîne ont un nombre de fonctions (méthodes) qui leur sont associées et qui peuvent être utilisées pour réaliser des opérations sur elles. J'en dirai plus à propos de celles-ci dans le [Chapitre 4]().

Les chaînes écrites avec des guillemets simples ou doubles se comportent de façon très similaire — la seule différence est le type de guillemet que vous avez besoin d’échapper à l’intérieur. Les chaînes avec guillemets obliques, habituellement appelées *littéraux de gabarits*, peuvent faire quelques trucs supplémentaires. En dehors de pouvoir étendre des lignes, elles peuvent aussi intégrer d'autres valeurs.

```javascript
`la moitié de 100 est ${100 / 2}`
```

Quand vous écrivez quelque chose à l’intérieur de `${}` dans un littéral de gabarit, son résultat sera calculé, converti en chaîne, et inclus à cette position. Cet exemple produit "*la moitié de 100 est 50*".

## Opérateurs unaires

Tous les opérateurs unaires ne sont pas des symboles. Certains sont écrits avec des mots. Un exemple est l’opérateur `typeof` qui produit une valeur chaîne nommant le type de la valeur que vous lui avez donnée.

```javascript
console.log(typeof 4.5)
// → number
console.log(typeof "x")
// → string
```

Nous utiliserons `console.log` dans le code de l'exemple pour indiquer que nous voulons voir le résultat de quelque chose qui a été évalué. Plus d'information dans le  [chapitre suivant]().

Les opérateurs qui ont été présentés opèrent tous avec deux valeurs, mais `typeof` n'en prend qu'une seule. Les opérateurs qui utilisent deux valeurs sont appelés opérateurs *binaires*, alors que ceux qui en prennent une sont appelés opérateurs *unaires*. L’opérateur moins peut être utilisé à la fois comme un opérateur binaire et comme un opérateur unaire.

```javascript
console.log(- (10 - 2))
// → -8
```

## Valeurs booléennes

Il est souvent utile d'avoir une valeur qui distingue entre deux possibilités telles que "oui" et "non", ou "on" et "off". Pour ce faire, JavaScript a un type *booléen* qui a seulement deux valeurs, *true* (vrai) et *false* (faux) qui sont écrites telles quelles.

### Comparaison

Voici une façon de produire des valeurs booléennes :

``` javascript
console.log(3 > 2)
// → true
console.log(3 < 2)
// → false
```

Les signes `>` et `<` sont respectivement les symboles traditionnels pour "plus grand que" et "plus petit que". Ce sont des opérateurs binaires. Leur application résulte une valeur booléenne indiquant s'ils sont vrais ou non.

Les chaînes peuvent être comparées de la même façon.

```javascript
console.log("Aardvark" < "Zoroaster")
// → true
```

La manière dont les chaînes sont ordonnées est grosso-modo alphabétique, mais pas vraiment ce que vous vous attendriez à voir dans un dictionnaire : les lettres majuscules sont toujours "moins" que les minuscules, donc `"Z" < "a"`, et les caractères non alphabétiques (!,-, et autres) sont aussi inclus dans l'ordre. Lors de la comparaison de chaînes, JavaScript parcourt les caractères de gauche à droite, comparant les codes Unicode un par un.

Les autres opérateurs similaires sont `>=` (plus grand ou égal à), `<=` (plus petit ou égal à), `==` (égal à), et `!=` (pas égal à).

```javascript
console.log("Itchy" != "Scratchy")
// → true
console.log("Pomme" == "Orange")
// → false
```

Il y a seulement une valeur en JavaScript qui n'est pas égale à elle-même, et c'est `NaN` ("not a number").

```javascript
console.log(NaN == NaN)
// → false
```

`NaN` est supposé représenter le résultat d'un calcul absurde, et en tant que tel, il n'est pas égal au résultat de quelqu'autre calcul absurde.

### Opérateurs logiques

Il y a aussi quelques opérations qui peuvent être appliquées aux valeurs booléennes elles-mêmes. JavaScript supporte trois opérateurs logiques: *et*, *ou* et *non*. Ils peuvent être utilisés pour "raisonner" à propos des booléens.

L’opérateur `&&` représente la logique *et*. C'est un opérateur binaire, et son résultat est vrai (*true*) seulement si les deux valeurs qui lui sont données sont vraies.

```javascript
console.log(true && false)
// → false
console.log(true && true)
// → true
```

L’opérateur `||` représente la logique *ou*. Il retourne vrai (*true*) si l'une ou l'autre des valeurs qui lui sont données est vraie.

```javascript
console.log(false || true)
// → true
console.log(false || false)
// → false
```

*Non* est écrit avec un point d'exclamation (`!`). C'est un opérateur unaire qui inverse la valeur qui lui est passée —`!true` retourne `false` et `!false` retourne `true`.

Lorsque l'on mélange ces opérateurs booléens avec les opérateurs arithmétiques et les autres, il n'est pas toujours évident de savoir quand les parenthèses sont nécessaires. En pratique, vous pour normalement le savoir en connaissant les opérateurs que nous avons vu jusque-là, `||` a la précédence la plus basse, puis vient `&&`, et puis les opérateurs de comparaison (`>`, `==`, ainsi de suite), et puis le reste. Cet ordre a été choisi de telle manière que, dans une expression typique comme la suivante, aussi peu de parenthèses que possible sont nécessaires : 

```javascript
1 + 1 == 2 && 10 * 10 > 50
```

Le dernier opérateur logique dont je vais discuter n'est ni unaire, ni binaire, mais ternaire, opérant sur trois valeurs. Il est écrit avec un point d'interrogation et un deux-points, comme suit :

```javascript
console.log(true ? 1 : 2);
// → 1
console.log(false ? 1 : 2);
// → 2
```

Celui-ci est appelé opérateur conditionnel (ou quelquefois juste opérateur ternaire puisque c'est le seul opérateur de ce type dans le langage). La valeur à gauche du point d'interrogation "choisit" laquelle des deux autres valeurs sera retournée. Quand elle est vraie, elle choisit la valeur du milieu, et quand elle est fausse, elle choisit la valeur de droite.

## Valeurs vides

Il y a deux valeurs spéciales, écrites `null` et `undefined`, qui sont utilisées pour dénoter l'absence de valeur *significative*. Elles sont elles-mêmes des valeurs, mais elles ne comportent pas d'information.

Beaucoup d’opérations du langage qui ne produisent pas de valeur significative (vous verrez cela plus tard) retournent `undefined` simplement parce qu'elles doivent retourner *une* valeur.

La différence de sens entre `undefined` et `null` est un accident de conception de JavaScript, et cela n'a pas d'important la plupart du temps. Dans les cas où vous êtes effectivement concernés par ces valeurs, je recommande de les considérer comme interchangeables.

## Conversion de type automatique

Dans l'[Introduction](), j'ai mentionné que JavaScript se met en quatre pour accepter presque tous les programmes que vous lui donnez, même ceux qui font des choses bizarres. Ceci est bien démontré avec les expressions suivantes :

```javascript
console.log(8 * null)
// → 0
console.log("5" - 1)
// → 4
console.log("5" + 1)
// → 51
console.log("five" * 2)
// → NaN
console.log(false == 0)
// → true
```

Quand un opérateur est appliqué sur le "mauvais" type de valeur, JavaScript va discrètement convertir cette valeur dans le type dont il a besoin, utilisant un jeu de règles qui souvent ne sont pas celles que vous voulez ou attendez C'est appelé "*conversion de type*". Le `null` de la première expression devient `0`, et le `"5"` de la seconde expression devient `5` (d'une chaîne à un nombre). Mais dans la troisième expression, `+` essaye de concaténer avant de faire l’opération numérique, donc le `1` est converti en `"1"` (d'un nombre à une chaîne).

Quand quelque chose qui ne correspond pas à un nombre de façon évidente (tel que `"five"` ou `undefined`) est converti en un nombre, vous obtenez la valeur `NaN`. De plus, les opérations arithmétiques sur `NaN` continuent de produire un `NaN`, donc si vous vous retrouvez à obtenir un de ceux-ci à un endroit inattendu, regardez pour les conversions de type accidentelles.

Lors de la comparaison de valeurs de même type utilisant `==`, le résultat est facile à prédire : vous devriez obtenir `true` quand les deux valeurs sont les même, sauf dans le cas de `NaN`. Mais si les types diffèrent, JavaScript utilise un jeu de règles compliqué et confus pour déterminer quoi faire. Dans la plupart des cas, il essaie juste de convertir une des valeurs dans le type de l'autre valeur. Cependant, quand `null` ou `undefined` apparaissent d'un côté ou de l'autre de l’opérateur, il produit `true` seulement si les deux côtés sont soit `null` ou `undefined`.

```javascript
console.log(null == undefined);
// → true
console.log(null == 0);
// → false
```

Ce comportement est souvent utile. Quand vous voulez tester si une valeur a une vraie valeur plutôt que `null` ou `undefined`, vous pouvez la comparer à `null` avec l’opérateur `==` (ou `!=`).

Mais que faire si vous voulez tester si quelque chose réfère précisément à la valeur `false` ? Des expressions comme `0 == false` et `"" == false` sont aussi vraies à cause de la conversion de type automatique. Quand vous ne voulez *aucune* conversion de type se produire, il y a deux opérateurs additionnels : `===` et `!===`. Le premier teste si une valeur est précisément égale à l'autre, et le second teste si elle est précisément non égale. Donc `"" === false` est faux comme attendu.

Je recommande d'utiliser les opérateurs de comparaison à trois caractères de manière défensive pour éviter d’être piéger par les conversions de type inattendues. Mais quand vous êtes certains que les types de chaque côté sont les même, il n'y a pas de problème à utiliser les opérateurs plus courts.

### Court-cicuiter les opérateurs logiques

Les opérateurs `&&` et `||` gèrent les valeurs de types différents d'une façon particulière. Ils convertiront la valeur à leur gauche en type booléen afin de décider quoi faire, mais en fonction de l’opérateur et du résultat de cette conversion, ils retourneront soit la valeur *originale* de gauche ou la valeur de droite. 

Par exemple, l’opérateur `||` retournera la valeur à sa gauche quand elle peut être convertie en `true` et, sinon, il retournera la valeur sur sa droite. Cela a l'effet attendu quand les valeurs sont booléennes et fait quelque chose d'analogue pour les valeurs d'autres types.

```javascript
console.log(null || "user")
// → user
console.log("Agnes" || "user")
// → Agnes
```

Nous pouvons utiliser cette fonctionnalité comme un moyen de revenir à une valeur par défaut. Si vous avez une valeur qui peut être vide, vous pouvez mettre `||` après avec sa valeur de remplacement. Si la valeur initiale peut être convertie en `false`, vous aurez la valeur de remplacement. Les règles pour convertir les chaînes et les nombres en booléens définissent que `0`, `NaN`, et la chaîne vide (`""`) comptent comme `false`, alors que toutes les autres valeurs comptent comme `true`. Donc `0 || -1` produit `-1`, et `"" || "!?"` retourne `"!?"`.

L’opérateur `&&` fonctionne de la même façon, mais dans l'autre sens. Quand la valeur à sa gauche est quelque chose qui se convertit en `false`, il retourne cette valeur, sinon il retourne la valeur à sa droite.

Une autre propriété importante de ces deux opérateurs est que la partie à leur droite est évaluée seulement quand cela est nécessaire. Dans le cas de `true || X`, quel que soit `X` — même si c'est une partie de programme qui fait quelque chose de *terrible* — le résultat sera vrai, et donc `X` n'est jamais évalué. De même pour `false && X` qui est faux et qui ignore `X`. Ceci est appelé *évaluation en court-circuit*.

L’opérateur conditionnel fonctionne d'une manière similaire. Parmi la seconde et la troisième valeur, seule celle qui est sélectionnée est évaluée.

## Résumé

Nous avons vu quatre types de valeur JavaScript dans ce chapitre : nombres, chaînes, booléens, et valeurs non définies.

De telles valeurs sont créées en tapant leur nom (`true`, `null`) ou valeur (`13`, `"abc"`). Vous pouvez combiner et transformer des valeurs avec des opérateurs. Nous avons vu les opérateurs binaires pour l’arithmétique (`+`, `-`, `*`, `/` et `%`), la concaténation de chaîne (`+`), la comparaison (`==`, `!=`, `===`, `!==`, `<`, `>`, `<=`, `>=`), et la logique (`&&`, `||`), de même que plusieurs opérateurs unaires (`-` pour opposer un nombre, `!` pour opposer logiquement, et `typeof` pour trouver le type d'une valeur) et un opérateur ternaire (`?:`) pour choisir une valeur parmi deux à partir d'une d'une troisième valeur.

Cela vous donne assez d'information pour utiliser JavaScript comme une calculatrice de poche mais pas beaucoup plus. Le [prochain chapitre]() commencera à intégrer ces expressions ensemble pour faire des programmes de base.