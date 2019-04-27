# Closures et Générateurs

> "My spelling is Wobbly. It’s good spelling but it Wobbles, and the letters get in the wrong places." — Winnie-the-Pooh

## En Plongée

Ayant grandit en fils de bibliothécaire et d'ès arts spécialisé en anglais, j'ai toujours été fasciné par les langages. Pas les langages de programmation. Enfin si, les langages de programmation, mais aussi les langages naturels. Prenez l'anglais. L'anglais est un langage schizophrénique qui emprunte des mots de l'allemand, du français, de l’espagnol et du latin (pour en citer quelque uns). En fait, "emprunte" n'est pas le bon mot; "pille" est plus approprie. Ou peut être "assimile" — comme les Borgs, Ouais, j'aime ça.

> Nous sommes les Borgs. Abaissez vos boucliers et rendez-vous sans condition. Nous intégrerons vos caractéristiques biologiques et technologiques aux nôtres. Votre culture s’adaptera à nos besoins. Toute résistance serait futile.

Dans ce chapitre, vous allez apprendre les noms pluriels. Mais aussi, les fonctions qui retournent d'autres fonctions, les expressions régulières avancées, et les générateurs. Mais d'abord, discutons comment former les noms pluriels. (Si vous n'avez pas encore lu [le chapitre sur les expressions régulières](), maintenant serait le bon moment. Ce chapitre assume que vous comprenez les bases des expressions régulières, et il en vient rapidement à des usages plus avancés.)

Si vous avez grandit dans un pays anglophone ou apprit l'anglais dans un contexte scolaire, vous êtes probablement familier avec les règles de base:

- Si un mot se termine avec un S, X, ou Z, ajoutez ES. *Bass* devient *basses*, *fax* devient *faxes*, et *waltz* devient *waltzes*.
- Si un mot se termine avec un H sonore, ajoutez ES ; s'il se termine avec un H muet, ajoutez juste un S. Qu'est-ce qu'un H sonore ? C'est un qui combiné avec d'autres lettres forme un son que vous pouvez entendre. Donc *coach* devient *coaches* et *rash* devient *rashes*, parce que vous pouvez entendre les sons CH et SH quand vous les prononcez. Mais *cheetah* devient *cheetahs*, parce que le H est muet.
- Si un mot se termine avec un Y qui sonne comme in I, remplacez le Y par IES; si le Y est combine avec une voyelle pour sonner différemment, ajoutez juste un S. Donc *vacancy* devient *vacancies*, mais *day* devient *days*.
- Si tout échoue, alors ajouter simplement S et espérez que ça ira. 

(Je sais, il y a pleins d'exception. *Man* devient *men* et *woman* devient *woman*, mais *human* devient *humans*. *Mouse* devient *mice* and *louse* devient *lice*, mais *house* devient *houses*. *Knife* devient *knives* et *wife* devient *wives*, mais *lowlife* devient *lowlifes*. Et on ne parle même pas des mots qui sont leur propre pluriel comme *sheep*, *deer* et *haiku*.)

Bien évidemment, les autres langues sont complètement différentes.

Développons une librairie Python qui forme automatiquement le pluriel des mots anglais. Nous commencerons seulement avec ces quatre règles, mais gardez à l'esprit que vous allez inévitablement en ajouter d'autres.

## Je Sais, Utilisons Les Expression Régulières !

Dons vous regardez des mots qui, au moins en anglais, signifie que vous regardez des chaînes de caractères. Vous avez des règles qui disent que vous devez trouver différentes combinaison de caractères, et puis leur faire différentes choses. Cela semble être un job pour les expressions régulières !

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

(1) C'est une expression régulière, mais elle utilise une syntaxe que vous n'avez pas vu dans *Expressions Régulières*. Les crochets signifient "correspond exactement à un des caractères". Donc `[sxz]` signifie "`s`, ou `x`, ou `z`", mais seulement un d'entre eux. Le `$` devrait être familier; il correspond à la fin de chaîne. Combines, cette expression régulière vérifie si `noun` se termine avec `s`, `x` ou `z`.
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
(2) OK, maintenant trouvons `a`, `b`, ou `c`, et replaçons le par `o`. `Mark` devient `Mork`.
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
(2) Regardez attentivement, c'est encore une nouvelle variation. Le `^` en premier caractère dans les crochets signifie quelque chose de spécial : négation. `[^abc]` signifie "n'importe quel caractère *sauf* `a`, `b`, ou `c`". Donc `[^aeioudgkprt]` signifie n'importe quel caractère sauf `a`, ` e`, ` i`, `o`, `u`, `d`, `g`, `k`, `p`, `r`, ou `t`. Puis ce caractère doit être suivi par `h`, suivi par la fin de chaîne. Vous cherchez les mots qui finissent avec un H où le H est sonore.
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
(2) Juste en passant, je voudrais pointer le fait qu'il est possible de combiner ces deux expressions régulières (une pour trouver si la règle s'applique, et une autre pour effectivement l'appliquer) en une unique expression régulière. C'est à cela que ça ressemblerait. En grande partie familier : vous utilisez un groupe de capture, que vous avez appris dans l’[Étude Le Cas :  Analyser Les Numéros de Téléphone](). Le groupe est utilisé pour capturer le caractère avant la lettre `y`. Puis dans la chaîne de substitution, vous utiliser une nouvelle syntaxe, `\1`, qui signifie "hé, tu te souviens de ce premier groupe ? met le ici." Dans ce cas, vous capturez le `c` avant le `y` ; quand vous faite la substitution, vous substituez `c` à la place de `c`, et `ies` à la place de `y`. (Si vous avez plut d'un groupe de capture, vous pouvez utiliser `\2` et `\3` et ainsi de suite.) 

Les substitutions d'expression régulière sont extrêmement puissante, et la syntaxe `\1` les rend encore plus puissante. Mais combiner toute l’opération en une seule expression régulière est aussi plus difficile a lire, et cela ne correspond pas directement à la façon dont vous décrivez les règles du pluriel. Vous avez initialement posé les règles comme "si le mot se termine en S, X, ou Z, alors ajouter ES". Si vous regardez cette fonction, vous avez deux lignes de code qui disent "si le mot se termine en S, X, ou Z, alors ajouter ES". On ne peut pas être plus directe.

## Une Liste De Fonctions

Maintenant, vous allez ajouter un niveau d'abstraction. Vous avez commencé par définir une liste de règles : si ceci, faire cela, sinon aller a la prochaine règle. Compliquons temporairement une partie du programme, ainsi vous pouvez simplifier une autre partie.

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
(2) Chaque règle d'application est aussi sa propre fonction qui appelle la fonction `re.sub()` pour applique la règle de pluriel appropriée.
(3) Au lieu d'avoir une fonction (`plural()`) avec de multiple règles, vous avez la structure de données `rules` qui  est une séquence de paire de fonctions.
(4) Puisque les règles ont été placées dans une structure de données séparée, la nouvelle fonction `plural()` peut être réduite a quelques lignes de code. En utilisant une boucle `for`, vous pouvez extraire les règles de correspondance et les règles d'application deux à la fois (une correspondance, une application) de la structure `rules`. A la première itération de la boucle `for`, `matches_rule` recevra `match_sxz`, et `apply_rule` recevra `apply_sxz`. A la seconde itération (assumant que vous arriver jusque là), `matches_rules` sera assignée à `match_h`, et `apply_rule` sera assignée à `apply_h`. La fonction est garantie de retourner quelque chose éventuellement, parce que la dernière règle (`match_default`) retourne simplement `True`, cela signifie que la règle d'application correspondante (`apply_default`) sera toujours appliquée.

La raison pour laquelle cette technique fonctionne est que [tout en Python est un objet](), y compris les fonctions. La structure de données `rules` contient des fonctions — pas des noms de fonctions, mais de vrais objets fonction. Quand ils sont assignés dans la boucle `for`, alors `matches_rule` et `apply_rule` sont de vrai fonctions que vous pouvez appeler. A la première itération de la boucle `for`, c'est équivalent à appeler `matches_sxz(noun)`, et si elle retourne une correspondance, à appeler `apply_sxz(noun)`.

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

Les règles peuvent être définies n'importe ou, de n'importe quelle façon. La fonction `plural()` ne s'en préoccupe pas.

Maintenant, est-ce qu'ajouter ce niveau d'abstraction valait la peine ? Et bien, pas encore. Considérez ce qu'il faudrait pour ajouter une nouvelle règle a la fonction. Dans le premier exemple, cela nécessiterait d'ajouter une instruction `if` à la fonction `plural()`. Dans le second exemple. cela nécessiterait d'ajouter deux fonctions, `match_foo()` et `apply_foo()`, et puis mettre a jour la séquence `rules` pour spécifier ou dans la séquence les nouvelles fonctions de correspondance et d'application doivent être appelées par rapport aux autres règles.

Mais ce n'est qu'une étape dans la prochaine section. Allons-y...

## Une Liste De Motifs

http://web.archive.org/web/20180309013153/http://www.diveintopython3.net/generators.html#a-list-of-patterns