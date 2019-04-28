# Chapitre 6 : Closures et Générateurs

> "My spelling is Wobbly. It’s good spelling but it Wobbles, and the letters get in the wrong places." — Winnie-the-Pooh

## En plongée

Ayant grandit en fils de bibliothécaire et d'ès arts spécialisé en anglais, j'ai toujours été fasciné par les langages. Pas les langages de programmation. Enfin si, les langages de programmation, mais aussi les langages naturels. Prenez l'anglais. L'anglais est un langage schizophrénique qui emprunte des mots de l'allemand, du français, de l’espagnol et du latin (pour en citer quelque uns). En fait, "emprunte" n'est pas le bon mot; "pille" est plus approprié. Ou peut être "assimile" — comme les Borgs. Ouais, j'aime ça.

> Nous sommes les Borgs. Abaissez vos boucliers et rendez-vous sans condition. Nous intégrerons vos caractéristiques biologiques et technologiques aux nôtres. Votre culture s’adaptera à nos besoins. Toute résistance serait futile.

Dans ce chapitre, vous allez apprendre les noms pluriels. Mais aussi, les fonctions qui retournent d'autres fonctions, les expressions régulières avancées, et les générateurs. Mais d'abord, discutons comment former les noms pluriels. (Si vous n'avez pas encore lu [le chapitre sur les expressions régulières](), maintenant serait le bon moment. Ce chapitre assume que vous comprenez les bases des expressions régulières, et il en vient rapidement à des usages plus avancés.)

Si vous avez grandit dans un pays anglophone ou apprit l'anglais dans un contexte scolaire, vous êtes probablement familier avec les règles de base:

- Si un mot se termine avec un S, X, ou Z, ajoutez ES. *Bass* devient *basses*, *fax* devient *faxes*, et *waltz* devient *waltzes*.
- Si un mot se termine avec un H sonore, ajoutez ES ; s'il se termine avec un H muet, ajoutez juste un S. Qu'est-ce qu'un H sonore ? C'est un H qui combiné avec d'autres lettres forme un son que vous pouvez entendre. Donc *coach* devient *coaches* et *rash* devient *rashes*, parce que vous pouvez entendre les sons CH et SH quand vous les prononcez. Mais *cheetah* devient *cheetahs*, parce que le H est muet.
- Si un mot se termine avec un Y qui sonne comme in I, remplacez le Y par IES ; si le Y est combiné avec une voyelle pour sonner différemment, ajoutez juste un S. Donc *vacancy* devient *vacancies*, mais *day* devient *days*.
- Si tout échoue, alors ajoutez simplement S et espérez que ça ira. 

(Je sais, il y a plein d'exceptions. *Man* devient *men* et *woman* devient *woman*, mais *human* devient *humans*. *Mouse* devient *mice* and *louse* devient *lice*, mais *house* devient *houses*. *Knife* devient *knives* et *wife* devient *wives*, mais *lowlife* devient *lowlifes*. Et on ne parle même pas des mots qui sont leur propre pluriel comme *sheep*, *deer* et *haiku*.)

Bien évidemment, les autres langues sont complètement différentes.

Développons une librairie Python qui forme automatiquement le pluriel des mots anglais. Nous commencerons seulement avec ces quatre règles, mais gardez à l'esprit que vous allez inévitablement en ajouter d'autres.

## Je sais, utilisons les expression régulières !

Dons vous regardez des mots qui, au moins en anglais, signifie que vous regardez des chaînes de caractères. Vous avez des règles qui disent que vous devez trouver différentes combinaison de caractères, et puis leur faire différentes choses. Cela semble être un boulot pour les expressions régulières !

```python
import re

def plural(noun):          
    if re.search('[sxz]$', noun):             ### (1)
        return re.sub('$', 'es', noun)        ### (2)
    elif re.search('[^aeioudgkprt]h$', noun):
        return re.sub('$', 'es', noun)       
    elif re.search('[^aeiou]y$', noun):      
        return re.sub('y$', 'ies', noun)     
    else:
        return noun + 's'
```

(1) C'est une expression régulière, mais elle utilise une syntaxe que vous n'avez pas vu dans *Expressions Régulières*. Les crochets signifient "correspond exactement à un des caractères". Donc `[sxz]` signifie "`s`, ou `x`, ou `z`", mais seulement un d'entre eux. Le `$` devrait être familier ; il correspond à la fin de chaîne. Combinés, cette expression régulière vérifie si `noun` se termine avec `s`, `x` ou `z`.
(2) Cette fonction `re.sub()` exécute une substitution d'expression régulière. 

Regardons les substitutions d'expression régulière plus en détail.

```python
>>> import re
>>> re.search('[abc]', 'Mark')    ### (1)
<_sre.SRE_Match object at 0x001C1FA8>
>>> re.sub('[abc]', 'o', 'Mark')  ### (2)
'Mork'
>>> re.sub('[abc]', 'o', 'rock')  ### (3)
'rook'
>>> re.sub('[abc]', 'o', 'caps')  ### (4)
'oops'
```

(1) Est-ce que la chaîne `Mark` contient `a`, `b`, ou `c` ? Oui, elle contient `a`.
(2) OK, maintenant trouvons `a`, `b`, ou `c`, et replaçons-le par `o`. `Mark` devient `Mork`.
(3) La même fonction transforme `rock` en `rook`.
(4) Vous devez pensez que cela devrait transformer `caps` en `oaps`, mais en fait non. `re.sub` remplace toutes les correspondances, pas seulement la première. Donc cette expression régulière transforme `caps` en `oops`, parce que le `c` et le `a` sont tous les deux transformés en `o`. 

Et maintenant, revenons à la fonction `plural()`...

```python
def plural(noun):          
    if re.search('[sxz]$', noun):            
        return re.sub('$', 'es', noun)         ### (1)
    elif re.search('[^aeioudgkprt]h$', noun):  ### (2)
        return re.sub('$', 'es', noun)
    elif re.search('[^aeiou]y$', noun):        ### (3)
        return re.sub('y$', 'ies', noun)     
    else:
        return noun + 's'
```

(1) Ici, vous remplacez la fin de chaîne (correspondant à `$`) avec la chaîne `es`. En d'autres mots, ajouter `es` à la chaîne. Vous pouvez obtenir la même chose avec la concaténation de chaîne, par exemple `noun + 'es'`, mais j'ai choisi d'utiliser les expressions régulières pour chaque règle, pour des raisons qui deviendront plus claires plus loin dans ce chapitre.
(2) Regardez attentivement, c'est encore une nouvelle variation. Le `^` en premier caractère entre crochets signifie quelque chose de spécial : négation. `[^abc]` signifie "n'importe quel caractère *sauf* `a`, `b`, ou `c`". Donc `[^aeioudgkprt]` signifie n'importe quel caractère sauf `a`, ` e`, ` i`, `o`, `u`, `d`, `g`, `k`, `p`, `r`, ou `t`. Puis ce caractère doit être suivi par `h`, suivi par la fin de chaîne. Vous cherchez les mots qui finissent avec un H où le H est sonore.
(3) Même motif ici : trouver les mots qui se terminent par Y, où le caractère avant le Y n'est *pas* `a`, `e`, `i`, `o`, ou `u`. Vous cherchez les mots qui finissent avec un Y qui sonne comme un I. 

Regardons la négation d'expression régulière plus en détail.

```python
>>> import re
>>> re.search('[^aeiou]y$', 'vacancy')  ### (1)
<_sre.SRE_Match object at 0x001C1FA8>
>>> re.search('[^aeiou]y$', 'boy')      ### (2)
>>> 
>>> re.search('[^aeiou]y$', 'day')
>>> 
>>> re.search('[^aeiou]y$', 'pita')     ### (3)
>>> 
```

(1) `vacancy `correspond à cette expression régulière, parce qu'il se termine en `cy`, et `c` n'est pas `a`, `e`, `i`, `o`, ou `u`.
(2) `boy` ne correspond pas, parce qu'il se termine en `oy`, et vous avez spécifiquement dit que le caractère avant le `y` ne peut pas être `o`. `day` ne correspond pas, parce qu'il se termine en `ay`.
(3) `pita` ne correspond pas, parce qu'il ne se termine pas en `y`. 

```python
>>> re.sub('y$', 'ies', 'vacancy')               ### (1)
'vacancies'
>>> re.sub('y$', 'ies', 'agency')
'agencies'
>>> re.sub('([^aeiou])y$', r'\1ies', 'vacancy')  ### (2)
'vacancies'
```

(1) Cette expression régulière transforme `vacancy` en `vacancies` et `agency` en `agencies`, ce qui est ce que vous voulez. Notez que cela transforme aussi `boy` en `boies`, mais cela n'arrivera jamais dans la fonction parce que vous avez fait cette `re.search` d'abord pour trouver si vous devez faire ce `re.sub`.
(2) Juste en passant, je voudrais pointer le fait qu'il est possible de combiner ces deux expressions régulières (une pour trouver si la règle s'applique, et une autre pour effectivement l'appliquer) en une unique expression régulière. Cela ressemblerait à ça. Cela doit sembler en grande partie familier : vous utilisez un groupe de capture, que vous avez appris dans l’[Étude Le Cas :  Analyser Les Numéros de Téléphone](). Le groupe est utilisé pour capturer le caractère avant la lettre `y`. Puis dans la chaîne de substitution, vous utilisez une nouvelle syntaxe, `\1`, qui signifie "hé, tu te souviens de ce premier groupe ? met le ici." Dans ce cas, vous capturez le `c` avant le `y` ; quand vous faite la substitution, vous substituez `c` à la place de `c`, et `ies` à la place de `y`. (Si vous avez plus d'un groupe de capture, vous pouvez utiliser `\2` et `\3` et ainsi de suite.) 

Les substitutions d'expression régulière sont extrêmement puissantes, et la syntaxe `\1` les rend encore plus puissante. Mais combiner toute l’opération en une seule expression régulière est aussi plus difficile à lire, et cela ne correspond pas directement à la façon dont vous décrivez les règles du pluriel. Vous avez initialement posé les règles comme "si le mot se termine en S, X, ou Z, alors ajouter ES". Si vous regardez cette fonction, vous avez deux lignes de code qui disent "si le mot se termine en S, X, ou Z, alors ajouter ES". On ne peut pas être plus directe.

## Une liste de fonctions

Maintenant, vous allez ajouter un niveau d'abstraction. Vous avez commencé par définir une liste de règles : si ceci, faire cela, sinon aller a la prochaine règle. Compliquons temporairement une partie du programme, ainsi vous pourrez en simplifier une autre partie.

```python
import re

def match_sxz(noun):
    return re.search('[sxz]$', noun)

def apply_sxz(noun):
    return re.sub('$', 'es', noun)

def match_h(noun):
    return re.search('[^aeioudgkprt]h$', noun)

def apply_h(noun):
    return re.sub('$', 'es', noun)

def match_y(noun):                             ### (1)
    return re.search('[^aeiou]y$', noun)
        
def apply_y(noun):                             ### (2)
    return re.sub('y$', 'ies', noun)

def match_default(noun):
    return True

def apply_default(noun):
    return noun + 's'

rules = ((match_sxz, apply_sxz),               ### (3)
         (match_h, apply_h),
         (match_y, apply_y),
         (match_default, apply_default)
         )

def plural(noun):           
    for matches_rule, apply_rule in rules:     ### (4)
        if matches_rule(noun):
            return apply_rule(noun)
```

(1) Maintenant, chaque règle de correspondance est sa propre fonction qui retourne les résultats de l'appel de la fonction `re.search()`.
(2) Chaque règle d'application est aussi sa propre fonction qui appelle la fonction `re.sub()` pour appliquer la règle de pluriel appropriée.
(3) Au lieu d'avoir une fonction (`plural()`) avec de multiple règles, vous avez la structure de données `rules` qui  est une séquence de paires de fonctions.
(4) Puisque les règles ont été placées dans une structure de données séparée, la nouvelle fonction `plural()` peut être réduite à quelques lignes de code. En utilisant une boucle `for`, vous pouvez extraire les règles de correspondance et les règles d'application deux à deux (une correspondance, une application) de la structure `rules`. A la première itération de la boucle `for`, `matches_rule` recevra `match_sxz`, et `apply_rule` recevra `apply_sxz`. A la seconde itération (assumant que vous arriver jusque là), `matches_rules` sera assignée à `match_h`, et `apply_rule` sera assignée à `apply_h`. La fonction est garantie de retourner quelque chose éventuellement, parce que la dernière règle (`match_default`) retourne simplement `True`, cela signifie que la règle d'application correspondante (`apply_default`) sera toujours appliquée.

La raison pour laquelle cette technique fonctionne est que [tout en Python est un objet](), y compris les fonctions. La structure de données `rules` contient des fonctions — pas des noms de fonctions, mais de vrais objets fonction. Quand ils sont assignés dans la boucle `for`, alors `matches_rule` et `apply_rule` sont de vraies fonctions que vous pouvez appeler. A la première itération de la boucle `for`, c'est équivalent à appeler `matches_sxz(noun)`, et si elle retourne une correspondance, à appeler `apply_sxz(noun)`.

Si cet additionnel niveau d'abstraction prête à confusion, essayez de dérouler la fonction pour voir l’équivalence. La boucle `for` complète est équivalente à :

```python
def plural(noun):
    if match_sxz(noun):
        return apply_sxz(noun)
    if match_h(noun):
        return apply_h(noun)
    if match_y(noun):
        return apply_y(noun)
    if match_default(noun):
        return apply_default(noun)
```

L'avantage ici est que la fonction `plural()` est maintenant simplifiée. Il faut une séquence de règles, définies quelque part, et itérer à travers d'une façon générique. 

1. Trouver une règle de correspondance.
2. Une correspondance ? Alors appeler la règle d'application et retourner le résultat.
3. Pas de correspondance ? Aller a l’étape 1.

Les règles peuvent être définies n'importe où, de n'importe quelle façon. La fonction `plural()` ne s'en préoccupe pas.

Maintenant, est-ce qu'ajouter ce niveau d'abstraction en valait la peine ? Et bien, pas encore. Considérez ce qu'il faudrait pour ajouter une nouvelle règle à la fonction. Dans le premier exemple, cela nécessiterait d'ajouter une instruction `if` à la fonction `plural()`. Dans le second exemple, cela nécessiterait d'ajouter deux fonctions, `match_foo()` et `apply_foo()`, et puis mettre à jour la séquence `rules` pour spécifier où dans la séquence les nouvelles fonctions de correspondance et d'application doivent être appelées par rapport aux autres règles.

Mais ce n'est qu'une étape dans la prochaine section. Allons-y...

## Une liste de motifs

Définir des fonctions nommées séparées pour chaque règle de correspondance et d'application n'est pas vraiment nécessaire. Vous ne les appelez jamais directement ; vous les ajoutez à la séquence `rules` et les appeler depuis-là.  Éliminons les motifs, ainsi la définition de nouvelle règles sera plus simple.

```python
import re

def build_match_and_apply_functions(pattern, search, replace):
    def matches_rule(word):                                     ### (1)
        return re.search(pattern, word)
    def apply_rule(word):                                       ### (2)
        return re.sub(search, replace, word)
    return (matches_rule, apply_rule)                           ### (3)
```

(1) `build_match_and_apply_functions()` est une fonction qui construit d'autres fonction de façon dynamique. Elle prend `pattern`, `search` et `replace`, puis définit une fonction `matches_rule()` qui appelle `re.search()` avec le motif (`pattern`) qui a été passé à la fonction `build_match_and_apply_functions()`, et le mot (`word`) qui a été passé à la fonction `matches_rule()` que vous construisez. Waouh.
(2) Construire la fonction d'application fonctionne de la même manière. La fonction d'application est une fonction qui prend un paramètre, et qui appelle `re.sub()` avec les paramètres `search` et `replace` qui ont été passés à la fonction `build_match_and_apply_functions()`, et le mot (`word`) qui a été passé à la fonction `apply_rule()` que vous construisez. Cette technique utilisant les valeurs de paramètres externes au sein d'une fonction dynamique est appelée *closures*. Vous définissez essentiellement des constantes au sein de la fonction d'application que vous construisez : elle prend un paramètre (`word`), mais elle le traite alors avec deux autres valeurs (`search` et `replace`) qui ont été définies quand vous avez défini la fonction application.
(3) Finalement, la fonction `build_match_and_apply_functions()` retourne un tuple de deux valeurs : les deux fonctions que vous avez juste créées. Les constantes que vous avez définis dans ces fonctions (`pattern` dans la fonction `matches_rule()`, et `search` dans la fonction `apply_rule()`) restent avec ces fonctions, même après que vous retourniez de `build_match_and_apply_functions()`. C'est super cool.

Si cela prête vraiment à confusion (et ça devrait, c'est bizarre), cela devrait devenir plus clair quand vous voyez comment l'utiliser.

```python
patterns = \                                                        ### (1)
  (
    ('[sxz]$',           '$',  'es'),
    ('[^aeioudgkprt]h$', '$',  'es'),
    ('(qu|[^aeiou])y$',  'y$', 'ies'),
    ('$',                '$',  's')                                 ### (2)
  )
rules = [build_match_and_apply_functions(pattern, search, replace)  ### (3)
         for (pattern, search, replace) in patterns]
```

(1) Nos "règles" du pluriel sont maintenant définies comme tuple de tuples de chaînes de caractères (non pas de fonctions). La première chaîne dans chaque groupe est le motif d'expression régulière que vous utiliseriez avec `re.search()` pour voir si la règle correspond. La deuxième et la troisième chaîne dans chaque groupe sont les expressions de recherche et remplacement que vous utiliseriez avec `re.sub()` pour effectivement appliquer la règle pour transformer un nom en son pluriel.
(2) Il y a un léger changement ici, dans la règle par défaut. Dans l'exemple précédent, la fonction `match_default()` retournait simplement `True`, signifiant que si aucune des règles spécifiques ne correspond, le code ajouterait simplement un `s` à la fin du mot donné. Cette exemple fait quelque chose de fonctionnellement équivalent. La dernière expression régulière demande si le mot a une fin (`$` correspond à la fin d'une chaîne). Bien sur, toute chaîne a une fin, même une chaîne vide, donc cette expression correspond toujours. Et donc, elle sert le même objectif que la fonction `matches_default()` qui retournait toujours `True` : elle assure que si aucune règle spécifique ne correspond, le code ajoute un `s` à la fin du mot donné.
(3) Cette ligne est magique. Elle prend la séquence de chaînes dans `patterns` et les transforme en une séquence de fonctions. Comment ? En faisant correspondre les chaînes à la fonction `build_match_and_apply_functions()`. C'est-à-dire, elle prend chaque triplet de chaînes et appelle la fonction `build_match_and_apply_functions()` avec ces trois chaînes comme arguments. La fonction `build_match_and_apply_functions()` retourne un tuple de deux fonctions. Cela signifie que `rules` est donc fonctionnellement équivalemment à l'exemple précédent : une liste de tuples, ou chaque tuple est une paire de fonctions. La première fonction est la fonction de correspondance qui appelle `re.search()`, et la seconde fonction est la fonction d'application qui appelle `re.sub()`.

Cette version du script est complétée par le point d’entrée principal, la fonction `plural()`.

```python
def plural(noun):
    for matches_rule, apply_rule in rules:  ### (1)
        if matches_rule(noun):
            return apply_rule(noun)
```

(1) Puisque la liste `rules` est la même que dans l'exemple précédent (c'est vraiment le cas), cela ne devrait pas être surprenant que la fonction `plural()` n'a pas changé du tout. Elle est complètement générique ; elle prend une liste de fonctions de règle et les appelle dans l'ordre. Elle ne se soucie pas de comment les règles sont définies. Dans l'exemple précédent, elle étaient définies comme des fonctions nommées séparées. Maintenant, elles sont construites dynamiquement en faisant correspondre la sortie de la fonction `build_match_and_apply_functions()` avec une liste brute de chaînes de caractères. Cela n'a aucune d'importance ; la fonction `plural()` fonctionne toujours de la même façon.

## Un fichier de motifs

Vous avez extrait tout le code duplique et ajoutez assez d'abstraction pour les que les règles du pluriel soient définies dans une liste de chaînes de caractères. La prochaine étape logique est de prendre ces chaînes et de les mettre dans un fichier séparé, où elles peuvent être maintenues séparément du code qui les utilise.

D'abord, créons un fichier texte qui contient les règles que vous voulez. Pas de structure de données sophistiquée, juste des chaînes délimitées par des espaces dans trois colonnes. Appelons-le `plural4-rules.txt`. 

```
[sxz]$               $    es
[^aeioudgkprt]h$     $    es
[^aeiou]y$          y$    ies
$                    $    s
```

Maintenant regardons comment nous pouvons utiliser ce fichier de règles.

```python
import re

def build_match_and_apply_functions(pattern, search, replace):     ### (1)
    def matches_rule(word):
        return re.search(pattern, word)
    def apply_rule(word):
        return re.sub(search, replace, word)
    return (matches_rule, apply_rule)

rules = []
with open('plural4-rules.txt', encoding='utf-8') as pattern_file:  ### (2)
    for line in pattern_file:                                      ### (3)
        pattern, search, replace = line.split(None, 3)             ### (4)
        rules.append(build_match_and_apply_functions(              ### (5)
                pattern, search, replace))
```

(1) La fonction `build_match_and_apply_functions()` n'a pas changé. Vous utilisez toujours des closures pour construire deux fonctions dynamiquement qui utilisent les variables définies dans la fonction externe.
(2) La fonction globale `open()` ouvre un fichier et retourne un objet fichier. Dans ce cas, le fichier que nous ouvrons contient les motifs de chaînes pour mettre au pluriel des noms. L'instruction `with` crée ce qui est appelé un *contexte* : quand le bloc `with` se termine, Python va automatiquement fermer le fichier, même si une exception se produit a l’intérieur du bloc `with`. Vous apprendrez plus à propos des blocs `with` et des objets fichier dans le chapitre [Fichiers]().
(3) L'idiome `for line in <fileobject>` lit les données depuis le fichier ouvert, une ligne à la fois, et assigne le texte à la variable `line`. Vous apprendrez plus à propos de la lecture de fichier dans le chapitre  [Fichiers]().
(4) Chaque ligne du fichier a en fait trois valeurs, mais elle sont séparées par des espaces blancs (tabulation ou espace, cela n'a pas d'importance). Pour les séparer, utilisez la  méthode de chaîne de caractères `split()`. Le premier argument de la méthode `split()` est `None`, ce qui signifie "découpe à chaque espace blanc (tabulation ou espace, cela n'a pas d'importance)." Le second argument est `3`, ce qui signifie "découpe sur les espaces blancs 3 fois, puis laisse en l’état le reste de la ligne." Une ligne comme `[sxz]$ $ es` sera découpée en la liste `['[sxz]$', '$', 'es']`, ce qui signifie que `pattern` aura '`[sxz]$`', `search` aura '`$`', et `replace` aura '`es`'. C'est beaucoup de puissance dans une seule petite ligne de code.
(5) Finalement, vous passez `pattern`, `search`, et `replace` à la fonction `build_match_and_apply_functions()`, qui retourne un tuple de fonctions. Vous ajouter ce tuple à la liste `rules`, et `rules` finit par contenir la liste des fonctions de correspondance et d'application que la fonction `plural()` attend.

L’amélioration ici est que vous avez complètement séparé les règles du pluriel dans une fichier externe, ainsi il peut être maintenu séparément du code qui 'utilise. Le code avec le code, les données avec  les données, et tout va bien.

## Générateurs

Est-ce que ce ne serait pas grandiose d'avoir une fonction générique `plural()` qui analyse le fichier de règles ? Récupère les règles, cherche une correspondance, applique la transformation appropriée, va à la règle suivante. C'est tout ce que la fonction `plural()` a à faire, et c'est tout ce que la fonction `plural()` devrait faire.

```python
def rules(rules_filename):
    with open(rules_filename, encoding='utf-8') as pattern_file:
        for line in pattern_file:
            pattern, search, replace = line.split(None, 3)
            yield build_match_and_apply_functions(pattern, search, replace)

def plural(noun, rules_filename='plural5-rules.txt'):
    for matches_rule, apply_rule in rules(rules_filename):
        if matches_rule(noun):
            return apply_rule(noun)
    raise ValueError('no matching rule for {0}'.format(noun))
```

Mais comment est-ce que *ça* fonctionne ? Regardons d'abord un exemple interactif.

```python
>>> def make_counter(x):
...     print('entering make_counter')
...     while True:
...         yield x                    ### (1)
...         print('incrementing x')
...         x = x + 1
... 
>>> counter = make_counter(2)          ### (2)
>>> counter                            ### (3)
<generator object at 0x001C9C10>
>>> next(counter)                      ### (4)
entering make_counter
2
>>> next(counter)                      ### (5)
incrementing x
3
>>> next(counter)                      ### (6)
incrementing x
4
```

(1) La présence du mot-clef `yield` dans `make_counter` signifie que ce n'est pas une fonction normale. C'est un type spécial de fonction qui génère des valeurs l'une après l'autre. Vous pouvez l'imaginez comme une fonction pouvant être interrompue et reprise. L'appeler retournera un *générateur* qui peut être utiliser pour générer des valeurs successives de `x`.
(2) Pour créer une instance du générateur `make_counter`, appeler-le juste comme une autre fonction. Notez qu'en fait cela n’exécute pas le code de la fonction. Vous pouvez le voir parce que la première ligne de la fonction `make_counter` appelle `print()`, mais rien n'est encore affiché.
(3) La fonction `make_counter` retourne un objet générateur.
(4) La fonction `next()` prend un objet générateur et retourne sa prochaine valeur. La première fois que vous appeler `next()`, elle exécute le code dans `make_counter` jusqu'à la première instruction `yield`, puis retourne la valeur qui a été générée. Dans ce cas, ce sera `2`, car vous avez initialement appelle le générateur en appelant `make_counter(2)`.
(5) Appeler de façon répétitive `next()` avec le mème objet générateur reprend exactement où on en était reste et continue jusqu'à ce que l'on rencontre la prochaine instruction `yield`. Toutes les variables, état local, etc. sont sauvegardées à `yield` et rétablies à `next()`. La prochaine ligne de code attendant d’être exécutée appelle `print()`, qui affiche `incrementing x`. Après cela, l'instruction `x = x + 1`. Puis on boucle à nouveau au travers de la boucle `while`, et la première chose que l'on rencontre est l'instruction `yield x`, qui sauvegarde l’état de toute chose et retourne la valeur courante de `x` (maintenant `3`).
(6) La deuxième fois que vous appeler `next()`, vous faites à nouveau les même choses, mais cette fois `x` vaut `4`.

Puisque `make_counter` définit une boucle infinie, vous pouvez théoriquement faire cela pour toujours, et ça continuerait juste d’incrémenter `x` et de cracher des valeurs. Mais regardons plutôt des utilisations productives des générateurs.

> ***“yield” met en pause une fonction. “next()” la reprend où elle s’était interrompue.***

### Un générateur Fibonacci

``` python
def fib(max):
    a, b = 0, 1          ### (1)
    while a < max:
        yield a          ### (2)
        a, b = b, a + b  ### (3)
```

(1) La séquence de Fibonacci est une séquence de nombres où chaque nombre est la somme des deux nombres qui le précèdent. Elle commence avec `0` et `1`, puis progresse doucement d'abord, puis de plus en plus rapidement. Pour commencer la séquence, vous avez besoin de deux variables : `a` commence à `0`, et `b` commence à `1`.
(2) `a` est le nombre courant dans la séquence, donc on le génère.
(3) `b` est le prochain nombre dans la séquence, donc on l'assigne à `a`, mais on calcule aussi la prochaine valeur `(a + b)` et on l'assigne à `b` pour l'utiliser plus tard. Notez que cela se produit en parallèle ; si `a` vaut `3` et `b` vaut `5`, alors `a, b = b, a + b` assignera `5` à `a` (la précédente valeur de `b`) et `8` à `b` (la somme des précédentes valeurs de `a` et `b`). 

Donc vous avez une fonction qui crache les nombres Fibonacci successifs. Bien sur, vous pouvez faire cela avec la récursion, mais cette manière est plus facile à lire. Aussi, cela fonctionne bien avec les boucles `for`.

```python
>>> from fibonacci import fib
>>> for n in fib(1000):      ### (1)
...     print(n, end=' ')    ### (2)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
>>> list(fib(1000))          ### (3)
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987]
```

(1) Vous pouvez utiliser un générateur comme `fib()` directement dans une boucle `for`. La boucle `for` appellera automatiquement la fonction `next()`, récupère les valeurs de `fib()` et les assigne à la variable d'index (`n`) de la boucle `for`.
(2) A chaque passage dans la boucle `for`, `n` reçoit une nouvelle valeur de l'instruction `yield` dans `fib()`,  tout ce que vous avez à faire arrivé là c'est de l'afficher. Une fois que `fib()` est à court de nombres (`a` devient plus grand que `max`, qui dans ce cas vaut `1000`), alors la boucle `for` se termine normalement.
(3) C'est un idiome utile : passez un générateur à la fonction `list()`, et il itérera le générateur tout entier (exactement comme la boucle `for` de l'exemple précédent) et retournera une liste de toutes les valeurs. 

### Un générateur de règle du pluriel

Retournons à `plural5.py` et voyons comment cette version de `plural()` fonctionne.

```python
def rules(rules_filename):
    with open(rules_filename, encoding='utf-8') as pattern_file:
        for line in pattern_file:
            pattern, search, replace = line.split(None, 3)                   ### (1)
            yield build_match_and_apply_functions(pattern, search, replace)  ### (2)

def plural(noun, rules_filename='plural5-rules.txt'):
    for matches_rule, apply_rule in rules(rules_filename):                   ### (3)
        if matches_rule(noun):
            return apply_rule(noun)
    raise ValueError('no matching rule for {0}'.format(noun))
```

(1) Pas de magie ici. Souvenez-vous que les lignes du fichier de règles ont trois valeurs séparées par des espaces, donc vous utilisez `line.split(None, 3)` pour récupérer les trois "colonnes" et les assigner à trois variables locales.
(2) *Et puis vous générez*. Qu'est-ce que vous générez ? Deux fonctions, construites dynamiquement avec notre vieille amie, `build_match_and_apply_functions()`, qui est identique à celle des exemples précédents. En d'autres mots, `rules()` est un générateur qui crache des fonctions de correspondance et d'application *à la demande*.
(3) Puisque `rules()` est un générateur, vous pouvez l'utiliser directement dans une boucle `for`. Au premier passage dans la boucle `for`, vous appellerez la fonction `rules()`, qui ouvrira le fichier de motifs, lira la première ligne, construira dynamiquement une fonction de correspondance et une fonction d'application à partir des motifs de cette ligne, et retournera les fonctions construites dynamiquement. Au second passage dans la boucle `for`, vous reprendrez exactement là où vous avez laissé `rules()` (qui était au milieu de la boucle `for line in pattern_file`). La première chose qu'elle fera sera de lire la prochaine ligne du fichier (qui est toujours ouvert), construire dynamiquement une autre fonction de correspondance et une autre fonction d'application sur la base des motifs de cette ligne, et retournera les deux fonctions.

Qu'avez-vous gagné par rapport à l’étape 4 ? Temps de démarrage. A l’étape 4, quand vous importez le module `plural4`, il lit tout le fichier de motifs et construit une liste de toutes les règles possibles, avant mème que ne vous pensiez à appeler la fonction `plural()`. Avec les générateurs, vous pouvez tout faire de façon paresseuse : vous lisez la première règle et créez les fonctions et les testez, et si ça fonctionne vous ne lisez même pas le reste du fichier ou créez d'autres fonctions.

Qu'avez-vous perdu ? Performance ! Chaque fois que vous appelez la fonction `plural()`, le générateur `rules` recommence depuis le début — ce qui signifie rouvrir le fichier de motifs et lire depuis le début, une ligne à la fois.

Et si vous pouviez avoir le meilleur des deux mondes : temps de démarrage minimal (ne pas exécuter de code à `import`), et *performance* maximale (ne pas construire les même fonctions encore et encore). Oh, et vous voulez toujours garder les règles dans un fichier séparé (parce que le code avec le code et les données avec les données), tant que vous n'avez jamais besoin de lire deux fois la même ligne.

Pour faire cela, vous allez devoir construire un nouveau générateur. Mais avant que vous fassiez *cela*, vous devez apprendre les classes Python.

## Pour aller plus loin (en anglais)

* [PEP 255: Simple Generators](https://www.python.org/dev/peps/pep-0255/)

* [Understanding Python’s “with” statement](http://effbot.org/zone/python-with-statement.htm)

* [Closures in Python](http://ynniv.com/blog/2007/08/closures-in-python.html)

* [Fibonacci numbers](https://en.wikipedia.org/wiki/Fibonacci_number)

  