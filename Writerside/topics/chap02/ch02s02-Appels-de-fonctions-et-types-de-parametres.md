# Section 2 : Appels de fonctions et types de paramètres

En Go, tous les arguments de fonction sont passés par valeur. Cela signifie que la fonction
reçoit une copie de chaque argument, et toute modification n'est pas reflétée dans la variable originale. Les pointeurs
peuvent offrir une solution à cette situation, puisqu'en passant l'adresse mémoire de la variable à la fonction, elle
peut modifier la variable d'origine.

Reprenons un exemple du chapitre précédent :

```go
package main

import "fmt"

func swap2(x int, y int) {
	temp := x
	x = y
	y = temp
}
```

Cette fonction manipule les valeurs des variables `x` et `y`, qui contiennent une copie des valeurs données en paramètre
lors de l'appel de la fonction. Si, par exemple, la fonction est appelée de cette façon :

```go
a, b := 2, 5
fmt.Println(a, b)
swap2(a, b)
fmt.Println(a, b)
```

![Diagramme de la fonction swap2 - partie 1](ch02s02-swap2-1.jpg)

Alors `x` contient une copie de la valeur de la variable `a`, et `y` contient une copie de la valeur de la variable `b`.
Donc modifier `x` et `y` n'affectent pas `a` et `b` parce qu'elles sont situées à des emplacements mémoire (des
adresses) différents. Les variables `x` et `y` sont locales à la fonction `swap2`, tandis que `a` et `b` ne sont pas du
tout accessibles à l'intérieur de `swap2`.

![Diagramme de la fonction swap2 - partie 2](ch02s02-swap2-2.jpg)

C'est ce qu'on appelle le **passage de paramètres par valeurs**. Les variables `a` et `b` sont passées par valeur à la
fonction `swap2`.

## Passage par pointeurs

La fonction `swap3` est une modification de `swap2`. À la place de passer les paramètres par valeurs, on passe les
adresses des variables `a` et `b` à la fonction, donc on passe `a` et `b` par pointeurs. En réalité, on passe les
adresses de `a` et `b` par valeurs à `swap3`, et on utilise ces valeurs (les addresses), pour accéder aux emplacements
mémoires qui contiennent `a` et `b`.

```go
func swap3(xPtr *int, yPtr *int) {
	temp := *xPtr
	*xPtr = *yPtr
	*yPtr = temp
}
```

Les paramètres ont été renommés `xPtr` et `yPtr` pour faciliter la compréhension, mais il n'est pas nécessaire de
toujours inclure `Ptr` dans le nom des variables qui sont pointeurs.

Si on appelle la fonction comme ceci :

```go
a, b := 2, 5
fmt.Println(a, b)
swap3(&a, &b)
fmt.Println(a, b)
```

La sortie sera :

```
2 5
5 2
```

Dans un premier temps, les variables `xPtr` et `yPtr` sont initialisées pour pointer vers les variables `a` et `b`
respectivement. Donc, `xPtr` contient l'adresse de `a`, et `yPtr` contient l'adresse de `b`. Ensuite, la variable `temp`
est initialisée avec la valeur de `a`, en déréférençant `xPtr` avec l'astérisque `*`. Donc `temp` a la valeur 2.

![Diagramme fonction swap3 - partie 1](ch02s02-swap3-1.jpg)

Ensuite, on prend la valeur de `*yPtr` et on la place dans `*xPtr`, ce qui revient à placer la valeur de `b` dans `a`.
La nouvelle valeur de `a` est 5. Ensuite, la valeur de `temp` est placée dans `*yPtr`, ce qui revient à placer 2
dans `b`.

Quand on déréférence `yPtr` avec l'astérisque `*`, c'est comme si on suivait le pointeur à partir de `yPtr` vers `b`.
Dans ce cas-ci, on a un accès direct à l'emplacement mémoire qui contient la variable `b`, sans pouvoir accéder
directement à cette variable à l'intérieur de la fonction.

![Diagramme fonction swap3 - partie 2](ch02s02-swap3-2.jpg)

En termes de quand vous devriez utiliser des pointeurs, ils sont utiles pour :

1. Lorsque vous devez modifier l'état d'une variable et que vous avez besoin que ce changement soit visible en dehors de
   la portée de la fonction actuelle.
    - La fonction `Scanf`, présentée dans la prochaine section, demande l'utilisation de pointeurs dans ses paramètres
2. Si vous avez une grande quantité de données et que vous devez la passer à différentes fonctions. Il est plus
   efficace d'envoyer un pointeur vers ces données plutôt que de copier toutes les données si on les passe par valeur.
    - Nous verrons des exemples de cette situation plus loin, lors de l'utilisation des tableaux et des tranches.
