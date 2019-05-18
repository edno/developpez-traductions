# Valeurs, Types et Opérateurs

> Sous la surface de la machine, le programme bouge. Sans effort, il s’étend et se contracte. En grande harmonie, les électrons se dispersent et se regroupent. Les formes sur l’écran ne sont que des ondulation sur l'eau. L'essence reste invisible en dessous.
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

```
13
```

Utilisez cela dans un programme, et cela entraînera la création du motif de bit pour le nombre 13 dans la mémoire de l’ordinateur.

JavaScript utilise un nombre fixe de bits, 64 d'entre eux, pour stocker une seule valeur de nombre. Il existe seulement un certain nombres de motifs que vous pouvez faire avec 64 bits, ce qui signifie que le nombre de différents nombres qui peut être représenter est limité. Avec *N* nombres décimaux, vous pouvez représenter 10<sup>N</sup> nombres. De façon similaire, avec 64 chiffres binaires, vous pouvez représenter 2<sup>64</sup> nombres différents, ce qui est environ 18 quintillions (un 18 suivi de 18 zéros). C'est beaucoup.

La mémoire d'ordinateur a été beaucoup plus petite, et les gens tendait à utiliser des groupes de 8 ou 16 bits pour représenter leur nombres. Il était facile de déborder (*overflow*) de si petits nombres par accident — pour finir avec un nombre qui ne rentre pas dans le nombre donné de bits.