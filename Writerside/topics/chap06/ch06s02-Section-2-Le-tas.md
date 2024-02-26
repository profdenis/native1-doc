# Section 2 : Le tas

Le problème avec la mémoire allouée sur la pile d'exécution est qu'à la fin de l'exécution de la fonction, la mémoire
est libérée avec le cadre d'exécution de la fonction. Donc une fonction peut partager de la mémoire avec les fonctions
qu'elle appelle, en passant des pointeurs vers les variables à partager aux autres fonctions, et une fonction peut
retourner une ou des valeurs à la fonction qui l'a appelée, mais ces valeurs retournées seront copiées lors du retour de
fonction (donc, pas vraiment partagées).

Pour partager de la mémoire plus librement, sans avoir besoin de la copier à plusieurs reprises, et sans avoir besoin de
se préoccuper de son allocation sur la pile, on peut allouer de la mémoire sur le tas. La mémoire allouée sur le tas est
gérée différemment de la mémoire allouée sur la pile, qui est directement liée à la durée d'exécution d'une fonction.

L'allocation de mémoire sur le tas (_heap_) est généralement gérée par le compilateur. Cela se fait principalement par
l'intermédiaire du mot-clé `new` et l'opérateur `&` lors de l'initialisation d'une variable.

## Avec le mot-clé `new`

En Go, `new(T)` alloue de la mémoire sur le tas pour une nouvelle item de type `T`, initialise à zéro et renvoie un
pointeur vers cet item. Voici un exemple :

```go
i := new(int)
*i = 3
fmt.Println(*i)  // Affiche "3"
```

Dans cet exemple, `new(int)` alloue de la mémoire pour un nouvel `int` sur le tas, initialisé à zéro, et renvoie un
pointeur vers cet `int`. Ensuite, nous utilisons le `*` pour déréférencer le pointeur et assigner la valeur `3` à
l'`int` sur le tas.

## Avec l'opérateur `&` lors de l'initialisation de variables

Cet opérateur est généralement utilisé pour obtenir l'adresse d'une variable existante. Cependant, lorsqu'il est utilisé
pendant l'initialisation d'une variable (par exemple, lors de l'initialisation d'une `struct`), cela provoquera aussi
l'allocation de mémoire sur le tas.

```go
type Person struct {
   Name string
   Age  int
}
   
p := &Person{"Alice", 30}
fmt.Println(p)  // Affiche l'adresse mémoire de la struct Person sur le tas
```

Dans cet exemple, `&Person{"Alice", 30}` crée une nouvelle struct `Person` avec les valeurs fournies, alloue de la
mémoire pour celle-ci sur le tas, et renvoie un pointeur vers cette struct.

Il est à noter que les détails de l'allocation de mémoire peuvent être assez compliqués en pratique, car le compilateur
Go utilise une technique appelée "analyse d'échappement" pour décider où une variable devrait être allouée. Il existe
des cas où une variable pourrait être allouée sur la pile même si vous avez utilisé `new` ou `&` lors de
l'initialisation.

## Analyse d'échappement

**L'analyse d'échappement** (ou _Escape Analysis_ en anglais) est un processus effectué par le compilateur pour
déterminer si une certaine variable peut être allouée sur la pile au lieu du tas.

La logique principale derrière l'analyse d'échappement est la suivante : si la durée de vie d'une variable est limitée à
l'exécution d'une seule fonction, alors le compilateur peut décider d'allouer cette variable sur la pile plutôt que sur
le tas. Cela peut offrir un avantage de performance significatif, car l'accès à la pile est généralement plus rapide que
l'accès à la mémoire du tas, et les objets de la pile ne nécessitent pas de garbage collection.

Néanmoins, si une variable est accessible (ou « s'échappe ») en dehors de la fonction où elle a été définie (par
exemple, si elle est renvoyée par une fonction ou capturée à l'intérieur d'une fermeture), alors elle doit être allouée
sur le tas.

Par exemple, dans le code Go suivant:

```go
func foo() *Bar {
    b := Bar{}
    return &b
}
```

La variable `b` est allouée sur le tas parce qu'elle est renvoyée par la fonction `foo` et pourrait donc être utilisée
ailleurs, en dehors de la portée de la fonction `foo`. Si `b` était allouée sur la pile, elle serait détruite dès
que `foo` aurait terminé l'exécution, ce qui ne serait pas correct si une autre partie du code a besoin d'y accéder plus
tard.

Le compilateur Go exécute l'analyse d'échappement pour déterminer si les variables doivent être allouées sur le tas ou
sur la pile.

## Adresse sur la pile vs. sur le tas

Voici un exemple de code qui montre comment allouer une struct `Person` sur le tas et une sur la pile, puis imprime leur
adresse pour comparaison :

```go
package main

import (
	"fmt"
)

type Person struct {
	Name string
	Age  int
}

func main() {
	// Allocation sur le tas
	pHeap := &Person{"Alice", 30}
	fmt.Printf("Address of person on heap: %p\n", pHeap)

	// Allocation sur la pile
	pStack := Person{"Bob", 35}
	fmt.Printf("Address of person on stack: %p\n", &pStack)
}
```

Dans ce code, `pHeap` est un pointeur vers une instance de `Person` qui est allouée sur le tas, et `pStack` est une
instance de `Person` qui est allouée sur la pile. Les adresses de ces deux variables sont ensuite imprimées pour
comparaison. Vous remarquerez probablement que l'adresse de la variable sur le tas est significativement plus grande que
celle de la variable sur la pile.
Les adresses précises varieront à chaque exécution du programme.