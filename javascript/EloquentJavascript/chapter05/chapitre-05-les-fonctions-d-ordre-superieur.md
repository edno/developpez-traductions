# Les fonction d'ordre supérieur

> Tzu-li et Tzu-ssu se vantaient de la taille de leurs derniers programmes. 'Deux cents mille lignes', dit Tzu-li, 'sans compter les commentaires !' Tzu-su" Tzu-ssu répondit, ''Pff, le mien à au moins un *million* de lignes déjà.' Maître Yuan-Ma dit 'Mon meilleur programme a cinq cent lignes.' En entendant cela, Tzu-li et Tzu-ssu ont été éclairés.'
>
> — Maître Yuan-Ma, *Le Livre de la Programmation*

> Il y a deux façons de construire un logiciel : une façon est de le faire si simple qu'il n'y pas pas de défauts flagrants, et l'autre façon est de le faire si complexe qu'il n'y a pas de défauts flagrants. "
>
> — C.A.R. Hoare, *1980 ACM Turing Award Lecture*

![Lettres de differents scripts](https://eloquentjavascript.net/img/chapter_picture_5.jpg)

Un long programme est un programme coûteux, et pas seulement à cause du temps qu'il faut pour le construire. Taille implique presque toujours complexité, et complexité rend les programmeurs confus. Des programmeurs confus, à leur tour, introduisent des erreurs (bugs) dans les programmes. Un long programme fournit beaucoup de place à ces bugs pour se cacher, les rendant difficiles a trouver.

Revenons aux deux derniers exemples de l'introduction. Le premier est autonome et long de six lignes.

```javascript
let total = 0, count, 1;
while (count <= 10) {
    total += count;
    count += 1;
}
console.log(total);
```

Le second dépend de deux fonctions externes et d'une ligne de long.

```javascript
console.log(sum(range(1,10)));
```

Lequel contiendra plus probablement un bug?

Si on compte la taille des définitions de `sum` et `range`, le second programme est aussi grand — même plus grand que le premier. Mais je dirais quand même qu’il est plus probable qu'il soit correct.

Il est plus probable qu'il soit correcte car la solution est exprime dans un vocabulaire qui correspond au problème résolu. Faire la somme d'un intervalle de nombres n'a rien à voir avec des boucles et des compteurs. Il s'agit d'intervalles et de sommes.

Les définitions de ces vocabulaires (les fonctions `sum` et `range`) impliqueront bien sur des boucles, compteurs, et autres détails secondaires. Mais parce qu'elles expriment des concepts plus simples que le programme en entier, elles sont plus faciles à faire correctement.

## Abstraction

Dans le contexte de la programmation, ces types de vocabulaires sont habituellement appelés *abstraction*s. Les abstractions masquent les détails et donnent la possibilité de discuter des problèmes à un niveau plus élevé (ou plus abstrait).

Pour analogie, comparez ces deux recettes pour une recette de soupe aux pois cassés. La première est ainsi :

> "Mettre 1 tasse de pois cassés par personne dans un récipient. Ajouter de l'eau jusqu'à ce que les pois soient complément recouverts. Laisser les pois dans l'eau pour au moins 12 heures. Enlever les pois de l'eau et les mettre dans une casserole. Ajouter 4 tasses d'eau par personne. Couvrir la casserole et laisser les pois mijoter pendant deux heures. Prendre un demi-oignon par personne. Le couper en morceaux avec un couteau. L'ajouter aux pois. Prendre une branche de céleri par personne. Le couper en morceaux avec un couteau. L'ajouter aux pois. Prendre une carotte par personne. Le couper en morceaux. Avec un couteau ! L'ajouter aux pois. Cuire pendant 10 minutes supplémentaires."

Et voilà la seconde recette :

> "Par personne: 1 tasse de pois cassés, un demi-oignon, une branche de céleri, et une carotte.
>
> Faire tremper les pois pendant 12 heures. Faire mijoter pendant 2 heures dans 4 tasses d'eau (par personne). Hacher et ajouter les légumes. Cuire pendant 10 minutes supplémentaires."

La seconde est plus courte et plus facile à interpréter. Mais vous devez comprendre quelques mots de cuisine supplémentaires comme *faire tremper*, *mijoter*, *hacher*, et, je suppose, *légumes*.

Lorsque l'on programme, on ne peut pas s'attendre à trouver tous les mots dont on a besoin dans le dictionnaire. Et donc, on peut se retrouver dans le cas de la première recette — identifier précisément les étapes que l'ordinateur doit exécuter, une par une, aveugle aux concepts de haut niveau qu'elles expriment.

C'est une compétence utile, en programmation, que de remarquer quand vous travaillez à un niveau d’abstraction trop bas.

## Abstraire la répétition

Les fonctions simples, comme nous en avons vu jusque là, sont de bonnes façons de construire des abstractions. Mais des fois elles se révèlent insuffisantes.

Il est courant pour un programme de faire quelque chose un certain nombre de fois. Vous pouvez écrire une boucle `for` pour cela, comme :

```javascript
for (let i = 0; i < 10; i++) {
    console.log(i);
}
```

Pouvons-nous abstraire "faire quelque chose *N* fois" en tant que fonction ? Eh bien, c'est facile d’écrire une fonction qui appelle `console.log` *N* fois.

```javascript
function repeatLog(n) {
    for (let i = 0; i < n; i++) {
        console.log(i);
    }
}
```

Mais qu'en est-il si on veut faire quelque chose d'autre qu'écrire les nombres dans des messages de journalisation ? Puisque "faire quelque chose" peut être représente comme une fonction et que les fonctions ne sont que des valeurs, on peut passer nos actions comme une valeur de fonction.

```javascript
function repeat(n, action) {
    for (let i = 0; i < n; i++) {
        action(i);
    }
}

repeat(3, console.log);
// → 0
// → 1
// → 2
```

Il n'est pas nécessaire de passer une fonction prédéfinie a `repeat`. Souvent, il est plus simple de créer une valeur de fonction sur le coup à la place.

```javascript
let labels = [];
repeat(5, i => {
    labels.push(`Unit ${i + 1}`);
});
console.log(labels);
// → ["Unit 1", "Unit 2", "Unit 3", "Unit 4", "Unit 5"]
```

Cette structure est un peu comme un boucle `for` — elle décrit d'abord le type de boucle puis fournit un contenu. Cependant, le contenu est maintenant écrit comme une valeur de fonction, qui est comprise entre les parenthèses de l'appel à `repeat`. C'est pourquoi elle doit être terminée par une accolade fermante *et* une parenthèse fermante. Pour des cas simples comme celui-ci, où le contenu est une unique petite expression, vous pouvez aussi omettre les accolades et écrire la boucle sur une seule ligne.

## Les fonctions d'ordre supérieur

Les fonctions qui opèrent sur d'autres fonctions, soit en les prenant en tant qu'arguments ou en les retournant, sont appelées *fonctions d'ordre supérieur*. Puisque nous avons déjà vue que les fonctions sont des valeurs régulières, il n'y a rien de remarquable dans le fait que de telles fonctions existent. Le terme vient des mathématiques, où la différence entre fonctions et autres valeurs est pris plus au sérieux.

Les fonction d'ordre supérieur nous permettent d'abstraire les *actions*, pas seulement des valeurs. Elles existent sous différentes formes. Par exemple, vous pouvez avoir des fonctions qui créent de nouvelles fonctions.

```javascript
function greaterThan(n) {
    return m => m > n;
}
let greaterThan10 = greaterThan(10);
console.log(greaterThan10(11));
// → true
```

Et nous pouvons avoir des fonction qui modifient d'autres fonctions.

```javascript
function noisy(f) {
    return (...args) => {
        console.log("calling with", args);
        let result = f(...args);
        console.log("called with", args, ", returned", result);
        return result;
    };
}
noisy(Math.min)(3, 2, 1);
// → calling with [3, 2, 1]
```

Nous pouvons même écrire des fonctions qui fournissent de nouveaux type de flux de contrôle.

```javascript
function unless(test, then) {
    if (!test) then();
}

repeat(3, n => {
    unless(n % 2 == 1, () => {
        console.log(n, "is even");
    });
});
// → 0 is even
// → 2 is even
```

Il y a une méthode standard pour les tableaux, `forEach`, qui fournit l’équivalent d'une boucle `for/of` comme une fonction d'ordre supérieur. 

```javascript
["A", "B"].forEach(l => console.log(l));
// → A
// → B
```

## Jeu de données de scripts

Un domaine où les fonctions d'ordre supérieur excellent est le traitement de données. Pour traiter des données, nous allons avoir besoin de données réelles. Ce chapitre utilisera un jeu de données relatif aux scripts — systèmes d’écriture comme le latin, le cyrillique, ou l'arabe.

Vous souvenez-vous d'Unicode vu au [Chapitre 1](#), le système assigne un nombre à chaque caractère d'une langue écrite ? La plupart de ces caractères sont associés à un script particulier. La norme contient 140 scripts différents — 81 sont toujours utilisés aujourd'hui, 59 sont historiques.

Bien que je ne sache que lire les caractères latins, j’apprécie le fait que des gens écrivent des textes dans au moins 80 autres systèmes d’écriture, beaucoup d'entre eux que je ne pourrait pas reconnaître. Par exemple, voici un échantillon d’écriture manuscrite tamile. 

![Écriture manuscrite tamile](https://eloquentjavascript.net/img/tamil.png)

Le jeu de données d'exemple contient des morceaux d'information relatifs aux 140 scripts définis dans Unicode. Il est disponible dans le bac-à-sable de codage de ce chapitre en que `SCRIPTS` binding. Le binding contient un tableau d'objets, chacun d'entre-eux décrivant un script.

```javascript
{
  name: "Coptic",
  ranges: [[994, 1008], [11392, 11508], [11513, 11520]],
  direction: "ltr",
  year: -200,
  living: false,
  link: "https://en.wikipedia.org/wiki/Coptic_alphabet"
}
```

Un tel objet nous fournit le nom du script, les intervalles Unicode qui lui sont assignés, la direction dans laquelle il est écrit, la date (approximative) d'origine, s'il est toujours utilisé, et un lien vers plus d'informations. La direction peut être "`ltr`" pour de gauche à droite (*left to right*), "`rtl`" pour de droite à gauche (*right to left*) (la façon dont les textes arabes et hébreux sont écrits), ou "`ttb`" pour de haut en bas (*top to bottom*) (comme dans l’écriture mongole).

La propriété `ranges` contient un tableau d'intervalles de caractères Unicode, chacun d'entre-eux étant un tableau a deux éléments contenant une limite inférieure et une limite supérieure. N'importe quel code caractère compris dans ces intervalles sont assignés au script. La limite inférieure est inclusive (code 994 est un caractère copte), et la limite supérieure est non inclusive (le code 1008 n'est pas un caractère copte).

## Filtrer les tableaux

Pour trouver les scripts, dans le jeu de données, qui sont toujours utilises, la fonction suivante peut être utile. Elle retire les éléments d'un tableau, qui ne satisfont pas une condition.

```javascript
function filter(array, test) {
    let passed = [];
    for (let element of array) {
        if (test(element)) {
            passed.push(element);
        }
    }
    return passed;
}

console.log(filter(SCRIPTS, script => script.living));
// → [{name: "Adlam", …}, …]
```

La fonction utilise l'argument appelé `test`, une valeur de fonction, pour compléter un "manque" dans le traitement — le processus pour décider quels éléments collecter.

Notez comment la fonction `filter`, plutôt que de supprimer des éléments du tableau existant, construit un nouveau tableau avec seulement les éléments qui satisfont la condition `test`. Cette fonction est *pure*. Elle ne modifie pas le tableau qui lui est donné.

Comme `forEach`, `filter` est une méthode standard pour les tableaux. L'exemple définit la fonction seulement pour montrer ce qui se passe à l’intérieur. A partir de maintenant, nous l'utiliserons plutôt de comme cela :

```javascript
console.log(SCRIPTS.filter(s => s.direction == "ttb"));
// → [{name: "Mongolian", …}, …]
```

## Transformer avec *map*

Assumons que nous avons un tableau d'objet représentant des scripts, produit en filtrant le tableau `SCRIPT` de quelque façon. Mais nous voulons un tableau de noms, qui est plus simple à parcourir.

La méthode `map` transforme un tableau en appliquant une fonction à tous ses éléments et construit un nouveau tableau avec les valeurs résultantes. Le nouveau tableau aura la même longueur que le tableau d’entrée, mais son contenu sera *associé* (*mapped*) à une nouvelle structure par la fonction.

``` javascript
function map(array, transform) {
    let mapped = [];
    for (let element of array) {
        mapped.pushed(transform(element));
    }
    returned mapped;
}

let rtlScripts = SCRIPTS.filter(s => s.direction == "rtl");
console.log(map(rtlScripts, s => s.name));
// → ["Adlam", "Arabic", "Imperial Aramaic", …]
```

Comme `forEach` et `filter`, `map` est une méthode standard pour les tableaux.

## Résumer avec *reduce*

Une autre chose commune à faire avec des tableaux est d'en calculer une valeur unique. Notre exemple récurrent, sommer une collection de nombre, en est un exemple. Un autre exemple est de trouver le script avec le plus de caractères.

L’opération d'ordre supérieur qui représente ce modèle est appelée *reduce* (quelquefois aussi appelée *fold*). Elle construit une valeur en prenant de manière répétitive un unique élément du tableau et en le combinant avec la valeur actuelle. En sommant des nombres, vous commencez avec le nombre zéro et, pour chaque élément, l'ajouter à la somme.

Les paramètres de `reduce` sont, à part le tableau, une fonction de combinaison et une valeur de départ. Cette fonction est un peu moins simple que `filter` et `map`, donc regardez-la de près :

```javascript
function reduce(array, combine, start) {
    let current = start;
    for (let element of array) {
        current = combine(current, element);
    }
    return current;
}

console.log(reduce([1, 2, 3, 4], (a, b) => a + b, 0));
// → 10
```

La méthode standard pour les tableaux `reduce`, qui bien sur correspond a cette fonction a un avantage supplémentaire. Si votre tableau contient au moins un élément, vous pouvez omettre l'argument `start`. Cette méthode prendra le premier élément du tableau comment sa valeur de départ et commencera la réduction à partir du second élément.

```javascript
console.log([1, 2, 3, 4].reduce((a, b) => a + b));
// → 10
```

Pour utiliser `reduce` (deux fois) pour utiliser le script avec le plus de caractères, on peut écrire quelque chose comme :

```javascript
function characterCount(script) {
    return script.ranges.reduce((count, [from, to]) =>{
        return count + (to - from);
    }, 0);
}

console.log(SCRIPTS.reduce((a, b) => {
  return characterCount(a) < characterCount(b) ? b : a;
}));
// → {name: "Han", …}
```

La fonction `characterCount` réduit les intervalles assignés à un script en sommant leurs tailles. Notez l'utilisation de la déstructuration dans la liste de paramètres de la fonction de réduction. Le deuxième appel à `reduce` utilise alors ça pour trouver le script le plus grand en comparant de manière répétitive deux scripts et retournant le plus grand.

Le script han a plus de 89 000 caractères qui lui sont assignés dans la norme Unicode, en faisant de loin le plus grand système d’écriture dans le jeu de données. Han est un script (quelques fois) utilisé pour les textes chinois, japonais, et coréens. Ces langues partagent beaucoup de caractères, cependant elles tendent à les écrire différemment. Le consortium Unicode (basé aux États-Unis) a décidé de les traiter comme un unique système d’écriture pour sauver des codes caractères. On l'appelle unification han et cela continue de faire des mécontents.

## Composabilité

