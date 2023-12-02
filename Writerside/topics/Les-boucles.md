# Section 6 : les boucles

## Boucle avec un compteur

À la base, les boucles _pour_ en Go ressemblent aux boucles _pour_ dans d'autres langages comme C et Java, avec quelques
différences :

- il n'y a pas de parenthèses `()` autour des 3 composantes principales de la
  boucle _pour_ : `initialisation ; condition ; incrémentation`
- le compteur de la boucle est normalement défini avec la notation abrégée utilisant le symbole `:=`
- les 3 composantes principales sont optionnelles

```Go
package main

import "fmt"

func loop1() {
	for i := 0; i < 5; i++ {
		fmt.Print(i, " ")
	}
	fmt.Println()
}

```

Sortie :

```
0 1 2 3 4
```

## Les boucles de type _while_

Techniquement, il n'y a pas de boucles _while_ en Go. Le seul type de boucle utilise _for_, comme démontré dans
l'exemple précédent. Puisque les parties _initialisation_ et _incrémentation_ sont optionnelles, on peut écrire une
boucle uniquement avec la condition.

```Go
package main

import "fmt"

func loop2() {
	i := 0
	for i < 5 {
		fmt.Print(i, " ")
		i++
	}
	fmt.Println()
}

```

La sortie est la même que l'exemple précédent.

## Boucle sans condition

Voici une **très mauvaise** manière d'écrire une boucle qui produit le même résultat, qui est techniquement légale, elle
obéit à la syntaxe du langage, mais qui est beaucoup plus difficile à lire, et qui peut causer des problèmes de
maintenance de code plus tard.

```Go
package main

import "fmt"

func loop3() {
	i := 0
	for {
		fmt.Print(i, " ")
		i++
		if i >= 5 {
			break
		}
	}
	fmt.Println()
}

```

À première vue, la boucle ressemble à une boucle infinie parce qu'il n'y a pas de condition après le `for`, ce qui est
légal en Go. C'est la même chose que s'il y avait une condition qui est toujours vraie, comme `for true` (ou comme
`while (true)` en Java). Mais cette boucle n'est pas vraiment infinie parce qu'il y a une condition à l'intérieur qui
devient vraie à un moment donné et qui fait un `break` pour sortir de la boucle.

En général, il faut éviter les boucles infinies ou qui en apparence ont l'air infinie parce qu'elles sont plus
difficiles à lire et à déboguer. Les vraies boucles infinies sont utiles dans certaines situations très particulières,
comme l'implémentation d'un serveur qui doit attendre un nombre indéfini de connexions ou de requêtes, mais ces
situations particulières sont rares.

## Boucles sur les tableaux

Voici comment définir un tableau de nombres entiers avec valeurs initiales, et comment faire une boucle sur le tableau
en utilisant les index, ou positions, des éléments dans le tableau, pour écrire tous les éléments du tableau sur le
_stdout_ avec un format particulier. On utilise la fonction `len` pour obtenir la longueur d'un tableau.

On peut également donner un tableau directement à la fonction `Println` pour en écrire le contenu.

```Go
package main

import "fmt"

func loop4() {
	numbers := [7]int{2, 5, 3, 4, 7, 1, 7}
	for i := 0; i < len(numbers); i++ {
		fmt.Printf("numbers[%d] = %d\n", i, numbers[i])
	}
	fmt.Println(numbers)
}
```

Sortie :

```
numbers[0] = 2
numbers[1] = 5
numbers[2] = 3
numbers[3] = 4
numbers[4] = 7
numbers[5] = 1
numbers[6] = 7
[2 5 3 4 7 1 7]
```

Si on n'initialise pas le tableau immédiatement avec des valeurs spécifiées entre accolades immédiatement après le
type `[7]int`, alors le tableau sera initialisé avec les valeurs zéro du type, dans ce cas-ci `0` à cause du type `int`.
Il est possible de remplacer la longueur `7` par `...` pour que le compilateur calcule la longueur automatiquement. Il
est également possible de ne rien spécifier entre les `[]`, mais dans ce cas, on n'obtiendra pas un tableau, mais une
_slice_ (tranche). Les tableaux et les tranches se ressemblent, mais il y a des différences en termes de leurs
propriétés et de la gestion de la mémoire. Plus de détails viendront à ce sujet dans le prochain chapitre.

Il existe une autre façon de parcourir tous les éléments d'un tableau en Go : en utilisant `range`. C'est la manière la
plus pratique et la plus utilisée de parcourir tous les éléments d'un tableau à partir du début.

```Go
package main

import "fmt"

func loop5() {
	numbers := [...]int{2, 5, 3, 4, 7, 1, 7}
	for i, number := range numbers {
		fmt.Printf("numbers[%d] = %d\n", i, number)
	}
	fmt.Println(numbers)
}
```

La sortie est la même que l'exemple précédent. À chaque tour de boucle, `i` a l'index courant, et `number` a la valeur
de l'élément courant du tableau. Les avantages de ce type de boucle est qu'on n'a pas besoin de gérer l'index
manuellement, il est incrémenté automatiquement, et la fin de boucle est vérifiée aussi automatiquement. Les
désavantages de ce type de boucle sont qu'on ne pas modifier le contenu du tableau dans la boucle, et on ne peut pas
parcourir les éléments du tableau dans un ordre différent (par exemple, de la fin vers le début).

Si on n'a pas besoin des index, mais seulement des valeurs des éléments dans le tableau, on doit alors faire la boucle
de la façon suivante.

```Go
package main

import "fmt"

func loop6() {
	numbers := [...]int{2, 5, 3, 4, 7, 1, 7}
	for _, number := range numbers {
		fmt.Printf("%d ", number)
	}
	fmt.Println()
}
```

Sortie :

```Go
2 5 3 4 7 1 7 
```

Dans le langage Go, toutes les variables déclarées doivent être utilisées au moins une fois. Puisque dans cet exemple,
on ne veut pas utiliser l'index des éléments du tableau, on utilise la barre de soulignement `_` à la place d'un nom de
variable. Si on écrivait `i` à la place de `_` dans cet exemple, on obtiendrait l'erreur `i declared and not used` au moment de la compilation.
