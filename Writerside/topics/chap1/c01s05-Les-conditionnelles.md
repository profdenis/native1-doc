# Section 5 : les conditionnelles

## `if ... else`

La particularité des conditionnelles dans le langage Go est qu'on ne place pas la condition entre parenthèses, et que
les accolades sont obligatoires autour du code à exécuter dans la partie `if` et dans la partie `else`, même s'il y a
seulement une ligne de code à exécuter. De plus, l'accolade d'ouverture `{` doit être sur la même ligne que le `if` ou
le `else`.

Utilisez les opérateurs `&&` pour la conjonction (et), `||` pour la disjonction (ou) et `!` pour la négation. Ajouter
des parenthèses au besoin pour s'assurer que les opérateurs sont appliqués dans le bon ordre.

```go
package main

import "fmt"

func conditional1(x int) {
	if x > 0 {
		fmt.Printf("x = %d est plus grand que 0\n", x)
	} else {
		fmt.Printf("x = %d est plus petit ou égal à 0\n", x)
	}
}
```

Sortie pour des appels avec les valeurs 5, 0, et -5 :

```
x = 5 est plus grand que 0
x = 0 est plus petit ou égal à 0
x = -5 est plus petit ou égal à 0
```

Il n'y a pas de syntaxe particulière pour les séquences de `if ... else`, il faut simplement démarrer un autre
`if ... else` tout de suite après le premier `else`.

```go
package main

import "fmt"

func conditional2(x int) {
	if x > 0 {
		fmt.Printf("x = %d est plus grand que 0\n", x)
	} else if x == 0 {
		fmt.Println("x est égal à 0")
	} else {
		fmt.Printf("x = %d est plus petit que 0\n", x)
	}
}
```

Sortie pour des appels avec les valeurs 5, 0, et -5 :

```
x = 5 est plus grand que 0
x est égal à 0
x = -5 est plus petit que 0
```

## `switch`

Un _switch_ (_aiguillage_) est une bonne façon de simplifier la syntaxe quand on a besoin d'une série de `if ... else`.
Il existe 2 principales formes de `switch` en Go : un `switch` sur une variable avec des cas sur différentes valeurs
possibles de la variable, et un `switch` sans variable avec des cas sur des expressions booléennes.

### `switch` sans variable

```go
package main

import "fmt"

func conditional3(x int) {
	switch {
	case x > 0:
		fmt.Printf("x = %d est plus grand que 0\n", x)
	case x == 0:
		fmt.Println("x est égal à 0")
	case x < 0:
		fmt.Printf("x = %d est plus petit que 0\n", x)
	}
}
```

Sortie pour des appels avec les valeurs 5, 0, et -5 : même que l'exemple précédent.

Cet exemple correspond presque exactement à l'exemple précédent écrit avec des `if ... else`, sauf pour le dernier cas
qui pourrait être remplacé par le cas spécial `default` qui correspond au dernier `else` d'une série de `if ... else`.
On obtiendra la même sortie avec ce changement. Dans ce cas-ci, remplacer `case x < 0` par `default` est approprié parce
que la condition `x < 0` couvre toutes les possibilités restantes. Notez qu'il n'est pas nécessaire d'utiliser
des `break` pour terminer chaque cas.

```go
package main

import "fmt"

func conditional4(x int) {
	switch {
	case x > 0:
		fmt.Printf("x = %d est plus grand que 0\n", x)
	case x == 0:
		fmt.Println("x est égal à 0")
	default:
		fmt.Printf("x = %d est plus petit que 0\n", x)
	}
}
```

### `switch` avec variable

```go
package main

import "fmt"

func conditional5(name string) {
	switch name {
	case "Denis":
		fmt.Println("Bonjour Prof. Denis !")
	case "":
		fmt.Println("Bonjour Inconnu !")
	default:
		fmt.Printf("Allo %s !\n", name)
	}
}
```

Sortie pour des appels avec les valeurs `"Denis"`, `""`, et `"Alice"` :

```
Bonjour Prof. Denis !
Bonjour Inconnu !
Allo Alice !
```

Lorsqu'on _switch_ sur une variable, les cas doivent être des valeurs spécifiques de la variable. Si on veut grouper
plusieurs valeurs dans un même cas, on peut les séparer par des virgules, comme dans l'exemple suivant.

```go
package main

import "fmt"

func conditional6(name string) {
	switch name {
	case "Denis", "Benoit":
		fmt.Printf("Bonjour Prof. %s !\n", name)
	case "":
		fmt.Println("Bonjour Inconnu !")
	default:
		fmt.Printf("Allo %s \n!", name)
	}
}
}
```

Sortie pour des appels avec les valeurs `"Denis"` et `"Benoit"` :

```
Bonjour Prof. Denis !
Bonjour Prof. Benoit !
```
