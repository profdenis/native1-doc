# Section 4 : Les fonctions

Une déclaration de fonction a la structure suivante en Go :

```go
func nomFonction(param1 type1, param2 type2, ...) typeRetour {}
```

Une déclaration commence toujours par `func`, suivi du nom de la fonction, qui doit être écrite en `camelCase` ou
en `PascalCase`. Comme pour les variables, si le nom de la fonction commence par une **minuscule**, la fonction sera
**privée** et accessible seulement dans le paquet qui la contient, tandis que si le nom commence par une **majuscule**,
la fonction sera **publique**, donc accessible de l'extérieur du paquet. Donc, pour une fonction privée, on utilise le
_camelCase_, et pour une fonction publique, on utilise le _PascalCase_.

Si la fonction ne retourne rien, alors on doit omettre le type de retour. On ne doit **pas** utiliser `void` comme dans
d'autres langages.

```go
package main

import "fmt"

func allo(name string) {
	fmt.Printf("Allo %s !\n", name)
}
```

Sortie :

```
Allo Alice !
```

Voici une fonction qui fait un calcul simple et retourne le résultat :

```go
package main

import "fmt"

func square(x int) int {
	return x * x
}
```

Sortie avec l'appel `fmt.Println(square(4))` : `16`

Il est possible pour une fonction de retourner plus qu'une valeur. Les types de retours doivent être séparés par des
virgules, et des parenthèses doivent être utilisées autour des types de retour.

```go
func nomFonction(param1 type1, param2 type2, ...) (typeRetour1, typeRetour2, ...) {}
```

Voici une fonction qui retourne les 2 valeurs données en paramètre, mais en ordre inverse.

```go
package main

import "fmt"

func swap(x int, y int) (int, int) {
	return y, x
}
```

Appel de la fonction :

```go
a, b := 2, 5
fmt.Println(swap(a, b))
```

Sortie : `5 2`

Voici une autre fonction pour échanger des valeurs, mais elle ne fonctionne pas :

```go
package main

import "fmt"

func swap2(x int, y int) {
	temp := x
	x = y
	y = temp
}
```

Appel de la fonction :

```go
a, b := 2, 5
fmt.Println(a, b)
swap2(a, b)
fmt.Println(a, b)
```

Sortie :

```
2 5
2 5
```

Plus de détails vont être donnés dans le chapitre suivant, mais la raison pourquoi cette fonction n'a aucun effet sur
les variables `a` et `b` est que la fonction travaille sur des copies des valeurs de `a` et `b`. Les variables `x`
et `y` sont initialisés avec les valeurs de `a` et `b`, mais sont indépendantes de `a` et `b`.

La meilleure façon d'échanger les valeurs de deux variables en Go est celle-ci :

`b, a = a, b`.

`b` va obtenir la valeur de `a`, et `a` va obtenir la valeur de `b`. Ça fonctionne parce que les changements de valeurs
vont se faire en parallèle.
