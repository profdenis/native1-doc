# Section 1 : La pile d'exécution

En Go, chaque goroutine se voit attribuer sa propre pile, (_stack_) qui contient l'espace mémoire pour les variables
locales et
les arguments de fonction. Lorsqu'une fonction est appelée, un _frame_ (aussi appelé _cadre d'exécution_) est créé sur
la pile de la goroutine. Ce frame contient les variables locales et les arguments de la fonction.

Voici une vue simplifiée de ce à quoi ressemble la mécanique de la pile et des frames :

![Pile et cadres d'exécution](PileAppelsFonctions1.jpg)

Prenons par exemple deux fonctions A et B. En supposant que la fonction A appelle la fonction B, le frame pour A sera
d'abord empilé, suivi par le frame pour B. Chaque frame contient les variables locales et les arguments de fonction pour
la fonction correspondante. Le sommet de la pile correspond toujours à la fonction en cours d'exécution (B dans ce
scénario).

Concernant l'allocation de mémoire, en Go, les variables locales et les arguments de fonction sont alloués sur la pile.
C'est l'une des fonctionnalités clés de Go qui permet des performances élevées, car l'allocation et la désallocation sur
la pile sont généralement plus rapides que sur le tas. De plus, la mémoire allouée sur la pile ne nécessite pas de
_garbage collection_, ce qui peut également améliorer les performances.

Il est cependant important de noter que ce n'est pas toujours le cas. Si une variable locale est référencée en dehors de
la fonction (c'est-à-dire, elle "échappe" la fonction), alors elle sera allouée sur le tas. C'est ce qu'on appelle
_l'analyse d'échappement_, que le compilateur Go utilise pour déterminer où allouer de la mémoire pour une variable.

## Exemple 1

**Question** : qu'est-ce que la fonction suivante calcule ?

```go
func f(i int) int {
	if i <= 0 {
		return 0
	}
	return i + f(i-1)
}
```

Si on appelle la fonction de cette façon : `f(4)`, nous aurons une pile d'exécution qui ressemblera à ceci à un certain
moment, si la fonction est appelée `f` est appelée à partir de la fonction `main` :

![Pile et cadres d'exécution](PileAppelsFonctions1.jpg)

Chaque rectangle est un cadre d'exécution pour chaque appel de fonction. Dans ce cadre, il y aura, entre autres, la
mémoire nécessaire aux paramètres et aux variables locales (dans ce cas-ci, il y a seulement le paramètre `i`). À chaque
appel de fonction, un cadre d'exécution est ajouté sur la pile d'exécution.

## Exemple 2

Si la fonction `main` appelle la fonction `section1`, qui elle-même appelle la fonction `ReadInt`, qui elle-même appelle
la fonction `ReadLine`, nous aurons à un moment donné, lors de l'exécution de `ReadLine`, une pile d'appels de fonctions
qui aura les cadres suivants :

![Pile et cadres d'exécution](PileAppelsFonctions2.jpg)

Chaque cadre contiendra de l'espace mémoire pour ses variables locales et ses paramètres.

## Exercice

Exécutez les fonctions des 2 exemples avec le débugueur, ligne par ligne, en étudiant la composition de la pile
d'exécution.
