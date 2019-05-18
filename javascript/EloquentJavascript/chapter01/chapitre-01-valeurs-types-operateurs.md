# Valeurs, Types et Opérateurs

> Sous la surface de la machine, le programme bouge. Sans effort, il s’étend et se contracte. En grande harmonie, les électrons se dispersent et se regroupent. Les formes sur l’écran ne sont que des ondulations sur l'eau. L'essence reste invisible en dessous.
>
> — Maître Yuan-Ma, *Le Livre de la Programmation*

![Image d'une mer de bits](https://eloquentjavascript.net/img/chapter_picture_1.jpg)

A l’intérieur du monde de l'ordinateur, ce ne sont que des données. Vous pouvez lire des données, modifier des données, créer de nouvelles données — mais ce qui n'est pas données ne peut pas être mentionné. Toutes ces données sont stockées comme de long séquences de bits et sont donc fondamentalement identiques.

Bits sont n'importe quoi ayant deux valeurs, habituellement décris comme zéros et uns. A l’intérieur de l'ordinateur, ils prennent des formes telles qu'une charge électrique haute ou basse, un signal fort ou faible, ou un point brillant ou terne sur la surface d'un CD. N'importe quelle information discrète peut être réduite à une séquence de zéros et de uns, et donc représentés en bits.

Par exemple, nous pouvons exprimer le nombre 13 en bits. Cela fonctionne de la même façon qu'un nombre décimal, mais au lieu de 10 différents chiffres, vous n'en avez que 2, et le poids de chacun accroît par un facteur 2 de droite à gauche. Voici les bits qui font le nombre 13, avec le poids des chiffres affiché en dessous d'eux :

```
   0   0   0   0   1   1   0   1
 128  64  32  16   8   4   2   1
```

C'est donc le nombre binaire 000011001. Ses chiffres non nuls représentent 8, 4 et 1, et totalisent 13. 

## Valeurs

Imaginez une mer de bits — un océan de bits. Un ordinateur moderne typique a plus de 30 milliards de bits dans son stockage de données volatiles (mémoire de travail). Le stockage non-volatile (le disque dur ou équivalent) tend à en avoir plus par plusieurs ordres de magnitude.

Pour être capable de travailler avec de telles quantités de bits sans être perdu, nous devons les séparer en morceaux qui représentent des éléments d’information. Dans un environnement JavaScript, ces morceaux sont appelés *valeurs*. Bien que toutes ces valeurs soient faites de bits, elles jouent des rôles différents. Toute valeur a un type qui détermine son rôle. Certaines valeurs sont des nombres, certaines sont des morceaux de texte, certaines sont des fonctions, et ainsi de suite.

Pour créer une valeur, vous devez juste invoquer son nom. C'est pratique. Vous n'avez pas besoin de collecter des matériaux de construction pour vos valeurs ou payer pour elles. Vous en appelez une, et tada, vous l'avez. Bien sûr, elles ne sont pas vraiment créées à partir de rien. Chaque valeur doit être stockée quelque part, et si vous voulez utiliser une gigantesque quantité d'entre elles au même moment, vous pouvez être à court de mémoire. Heureusement, cela n'est un problème que si vous avez besoin de toutes simultanément. Des que vous n'utilisez plus une valeur, elle disparaîtra, laissant derrière elle ses bits pour être recycler comme matériel de construction pour la prochaine génération de valeurs.

Ce chapitre introduit les éléments atomique des programmes JavaScript, qui sont les types simples de valeurs et les opérateurs qui peuvent agir sur de telles valeurs.

## Nombres

Les valeurs du type nombre sont, sans surprise, des valeurs numériques. Dans un programme JavaScript, elles sont écrites ainsi :

```javascript
13
```

Utilisez cela dans un programme, et cela entraînera la création du motif de bit pour le nombre 13 dans la mémoire de l’ordinateur.

JavaScript utilise un nombre fixe de bits, 64 d'entre eux, pour stocker une seule valeur de nombre. Il existe seulement un certain nombres de motifs que vous pouvez faire avec 64 bits, ce qui signifie que le nombre de différents nombres qui peut être représenter est limité. Avec *N* nombres décimaux, vous pouvez représenter 10<sup>N</sup> nombres. De façon similaire, avec 64 chiffres binaires, vous pouvez représenter 2<sup>64</sup> nombres différents, ce qui est environ 18 quintillions (un 18 suivi de 18 zéros). C'est beaucoup.

La mémoire d'ordinateur a été beaucoup plus petite, et les gens tendait à utiliser des groupes de 8 ou 16 bits pour représenter leur nombres. Il était facile de déborder (*overflow*) de si petits nombres par accident — pour finir avec un nombre qui ne rentre pas dans le nombre donné de bits. Aujourd'hui, même les ordinateurs qui rentrent dans votre poche ont plein de mémoire, donc vous êtes libres d'utiliser des morceaux de 64 bits, et vous devez seulement vous inquiéter quand vous traiter avec des nombres réellement astronomiques.

Cependant, Tous les nombres moins de 18 quintillions ne peuvent pas tous être contenu dans un nombre JavaScript. Ces bits stockent aussi des nombres négatifs, donc un bit indique le signe du nombre. Un problème plus important est que les nombre non-entiers doivent aussi être représentés. Pour faire cela, certains bits sont utilises pour stocker la position du séparateur décimal. Le vrai nombre entier maximal qui peut être stocké est plus dans l'intervalle de 9 quadrillions (15 zéros) — ce qui est toujours plaisamment énorme.

Les nombres fractionnaires sont écrits avec un point.

```javascript
9.81
```

Pour de très grands ou très petites nombres, vous pouvez aussi utiliser la notation scientifique en ajoutant un *e* (pour *exposant*), suivi par l'exposant du nombre.

```javascript
2.998e8
```

C'est 2.998 × 108 = 299 800 000.

Les calculs avec les nombres entiers (aussi appeler *entiers*) plus petit que le précédemment mentionne 9 quadrillions sont garantis d’être toujours précis. Malheureusement, les calculs avec les nombres fractionnaires ne le sont généralement pas. Tout comme π (pi) ne peut pas être exprimé précisément par un nombre fini de chiffres décimaux, beaucoup de nombres perdent en précision quand seulement 64 bits sont disponibles pour les stocker. C'est honteux, mais cela ne cause des problèmes pratiques que dans des situations spécifiques. Ce qui est important c'est de le savoir et de traiter les nombres numériques fractionnaires comme approximations, et non pas comme des valeurs précises.

## Arithmétique

La principale chose à faire avec les nombres c'est de l’arithmétique. Les opérations arithmétiques comme l'addition ou la multiplication prennent deux valeurs de nombre et produisent un nouveau nombre a partir d'eux. Voici à quoi cela ressemble en JavaScript :

```javascript
100 + 4 * 11
```

Les symboles `+` et `*` sont appelés *opérateurs*. Le premier correspond à l'addition, et le second correspond à la multiplication. Mettre un opérateur entre deux valeurs l'appliquera à ces valeurs et produira une nouvelle valeur.

Mais est-ce que l'exemple signifie "ajouter 4 et 100, et multiplier le résultat par 11", ou est-ce que la multiplication est réalisée avant l'addition ? Comme vous avez du le deviner, la multiplication est faite en premier. Mais comme en arithmétique, vous pouvez changer cela en encadrant l'addition avec des parenthèses.

```javascript
(100 + 4) * 11
```

Pour la soustraction, il y a l’opérateur `-`, et la division peut être faite avec l’opérateur `/`.

Quand les opérateurs apparaissent ensemble sans parenthèses, l'ordre dans lequel ils sont appliqués est déterminé par la *précédence* des opérateurs. L'exemple montre que la multiplication vient avant l'addition. L’opérateur `/` a la même précédence que `*`. De même pour `+` et `-`. Quand plusieurs opérateurs avec la même précédence apparaissent les uns après les autres, comme dans `1 - 2 + 1`, ils sont appliqués de gauche à droite : `(1 - 2) + 1`.

Ces règles de précédence ne sont pas quelque chose dont vous devez vous inquiéter. En cas de doute, ajouter simplement des parenthèses.

Il y a un autre opérateur arithmétique, que vous ne reconnaîtrez peut être pas immédiatement. Le symbole `%` est utilise pour représenter l’opération de *reste*. `X % Y` est le reste de la division de `X` par `Y`. Par exemple, `314 % 100` produit `14`, et `144 % 12` retourne `0`. La précédence de l’opérateur de reste est le même que la multiplication et la division. Vous verrez aussi souvent cet opérateur être appelé *modulo*.

## Nombres speciaux

Il y a trois valeurs spéciales en JavaScript qui sont considérées comme des nombres mais ne se comportent pas comme des nombres normaux.

Les deux premiers sont `Infinity` et `-Infinity` qui représentent les infinis positif et négatif. `Infinity - 1` est toujours `Infinity`, et ainsi de suite. Cependant, ne faites pas trop confiance aux calculs basés sur l’infini. Cela n'est pas mathématiquement correct, et cela mènera rapidement au prochain nombre spécial : `NaN`.

`NaN` signifie "*not a number*" (pas un nombre), mais cependant *c'est* une valeur de type nombre. Vous obtiendrez ce résultat quand vous, par exemple, essayez de calculer `0 / 0` (zéro divise par zéro), `Infinity - Infinity`, ou n'importe quel nombre d'autre opération numérique qui ne retourne pas de résultat significatif.

## Chaînes

Le type de donne de base suivant est la chaîne (*string*). Les chaînes sont utilisées pour représenter du texte. Elle sont écrites en encadrant leur contenu avec des guillemets.

```javascript
`Down on the sea`
"Lie on the ocean"
'Float on the ocean'
```

Vous pouvez utiliser des guillemets simples, des guillemets doubles, ou des guillemets obliques pour marquer les chaînes, du moment que le début et la fin de la chaîne concordent.

Presque tout peut être mis entre guillemets, et JavaScript en fera une chaîne. Mais quelques caractères sont plus difficiles.  Vous pouvez imaginer comment mettre des guillemets entre guillemets peut être compliqué. Les *sauts de ligne* (les caractères que vous avez quand vous pressez `ENTER`) peuvent être inclus sans échappement seulement si la chaîne est entre guillemets obliques (`` `).

Pour rendre possible d'inclure de tels caractères dans une chaîne, la notation suivante est utilisée : des qu'un anti-slash (`\`) est trouvé dans un texte entre guillemets, il indique que le caractères suivant a une signification spéciale. Ceci est appelé échapper le caractère. Un guillemet qui est précédé par un anti-slash ne terminera pas la chaîne mais en fera parti. Quand un caractère `n` apparaît après un anti-slash, il est interprété comme un saut de ligne. De façon similaire, un `t` après un anti-slash signifie un caractère tabulation. Prenez la chaîne suivante :

```javascript
"This is the first line\nAnd this is the second"
```

Le texte réel contenu est :

```
This is the first line
And this is the second
```

Les chaînes, aussi, doivent être modelées comme des séries de bits pour pouvoir exister dans l'ordinateur. La façon dont JavaScript le fait est basé sur la norme *Unicode*. Cette norme virtuellement assigne un nombre à chaque caractère dont vous pourrez avoir besoin, incluant les caractères du grec, arabe, japonais, arménien, et ainsi de suite. Si nous avons un nombre pour chaque caractère, une chaîne peut être décrite comme une séquence de nombres.

Et c'est ce que JavaScript fait. Mais il y une complication : la représentation JavaScript utilise 16 bits par chaîne par élément de chaîne, ce qui décrit jusqu'a 2<sup>16</sup> caractères différents. Mais Unicode définis plus de caractères que cela — à peu près deux fois plus, a ce stade. Donc certains caractères, tels que beaucoup d’émoticônes, prennent jusqu'à deux "positions de caractère" dans les chaînes JavaScript. Nous y reviendrons au [Chapitre 5]().

Les chaînes ne peuvent pas être divisées, multipliées, ou soustraites, mais l’opérateur `+` *peut* être utilisé sur elles. Il n'additionne pas, mais il concatène — il colle deux chaînes ensemble. La ligne suivante produira le chaîne "`concatène`" :

```javascript
"con" + "cat" + "è" + "ne"
```

Les valeurs de chaîne on un nombre de fonctions (méthodes) qui leur sont associées et qui peuvent être utilisées pour réaliser des opération sur elles. J'en dirai plus à propos de celles-ci dans le [Chapitre 4]().

Les chaînes écrites avec des guillemets simples ou doubles se comportent de façon très similaire — la seule différence est le type de guillemet que vous avez besoin d’échapper à l’intérieur. Les chaînes avec guillemets obliques, habituellement appelées *littéraux de gabarits*, peuvent faire quelques truc supplémentaires. En dehors de pouvoir étendre des lignes, elles peuvent aussi intégrer d'autres valeurs.

```javascript
`la moitié de 100 est ${100 / 2}`
```

Quand vous écrivez quelque chose à l’intérieur de `${}` dans un littéral de gabarit, son résultat sera calculé, converti en chaîne, et inclus à cette position. Cet exemple produit "*la moitié de 100 est 50*".

## Opérateurs unaires



