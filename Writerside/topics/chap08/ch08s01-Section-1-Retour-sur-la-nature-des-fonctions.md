# Section 1 : Retour sur la nature des fonctions

## Les fonctions sont des citoyens de première classe

Quand on dit que _les fonctions sont des citoyens de première classe- dans un langage de programmation, cela signifie
que les fonctions peuvent être utilisées de la même manière que n'importe quel autre type de données du langage. Cela
peut inclure :

- La capacité d'affecter des fonctions à des variables ou de les stocker dans des structures de données.
- La possibilité de passer une fonction en tant que paramètre à une autre fonction.
- La possibilité de retourner une fonction en tant que résultat d'une autre fonction.

En bref, une fonction est traitée comme une entité qui peut être manipulée par la syntaxe du langage, tout comme un
entier, une chaîne ou un objet. Ce concept est un élément clé des langages de programmation fonctionnelle, mais il est
également présent dans de nombreux langages de programmation impératifs modernes.

En ce qui concerne le langage Go, il traite également les fonctions comme des citoyens de première classe. Voici un
exemple dans Go :

```go
// Déclaration d'une fonction qui retourne une autre fonction
func createAdder(x int) func(int) int {
    return func(y int) int {
        return x + y
    }
}

// Utilisation de cette fonction
adder := createAdder(10)
fmt.Println(adder(5)) // Affiche 15
```

Dans cet exemple, non seulement une fonction est retournée par une autre fonction, mais la fonction interne est une
fermeture (_closure_ en anglais), elle a également accès à l'argument de la fonction externe.

### Fermeture

Une fermeture, ou _closure_ en anglais, est une fonction qui a accès aux variables de son contexte lexical, c'est-à-dire
à la fonction ou l'environnement de code dans lequel elle a été définie. Une fermeture est capable de "se rappeler" et
de manipuler ces variables, même après que la fonction dans laquelle elle a été définie ait terminé son exécution.

Voyons cela avec un exemple en Go :

```go
package main

import "fmt"

// Cette fonction crée une fermeture
func createAdder(x int) func(int) int {
    // La fonction retournée est une fermeture
    return func(y int) int {
        // Elle "se souvient" de la valeur de x
        return x + y
    }
}

func main() {
    adder := createAdder(10)
    fmt.Println(adder(5)) // Affiche 15
}
```

Dans cet exemple, la fonction `createAdder` retourne une autre fonction. Cette fonction retournée est une fermeture,
parce qu'elle a accès à la variable `x` de `createAdder`. Même après que `createAdder` ait fini de s'exécuter et ait
renvoyé la fermeture, la fermeture peut toujours accéder à `x`.

En d'autres termes, la fermeture capte, ou "emprisonne" son environnement lexical, c'est-à-dire l'ensemble des variables
auxquelles elle peut accéder au moment de sa création. Ce mécanisme est très puissant et est la base de nombreuses
constructions de programmation fonctionnelle.

Cependant, avec une grande puissance vient une grande responsabilité. Il est important d'être attentif lorsque vous
travaillez avec des fermetures, car elles peuvent parfois conduire à des problèmes de performance ou de mémoire si elles
sont utilisées de manière imprudente, par exemple en capturant accidentellement de grosses structures de données.

## Autre exemple

En Go, vous pouvez aussi passer des fonctions en paramètres à d'autres fonctions et les stocker dans des structures de
données. Cela facilite l'écriture de fonctions de haut niveau qui peuvent manipuler les opérations basées sur d'autres
fonctions.

Voici un autre exemple illustrant comment une fonction peut accepter une autre fonction en paramètre en Go.

Supposons que nous ayons une fonction qui parcourt une tranche de nombres et applique une certaine fonction à chaque
nombre :

```go
package main

import "fmt"

// Notre fonction prend une tranche et une fonction en paramètre
func apply(nums []int, fn func(int) int) []int {
    result := make([]int, len(nums))
    for i, num := range nums {
        result[i] = fn(num)
    }
    return result
}

func main() {
    nums := []int{1, 2, 3, 4, 5}
  
    // Nous passons une fonction anonyme qui multiplie chaque nombre par 2
    doubled := apply(nums, func(n int) int {
        return n * 2
    })
  
    fmt.Println(doubled) // Affiche : [2 4 6 8 10]
}
```

Dans cet exemple, la fonction `apply` prend une fonction `fn` comme argument. Cette fonction `fn` est ensuite appliquée
à chaque élément de la liste `nums`. Ceci illustre que les fonctions en Go sont effectivement des citoyens de première
classe - elles peuvent être passées comme arguments à d'autres fonctions.

## Différence entre appeler une fonction et passer une fonction en paramètre

La différence entre appeler une fonction et passer le résultat en paramètre à une fonction, et passer une fonction, sans
l'appeler, à une autre fonction, peut être une source de confusion courante. La différence principale réside dans le
moment où la fonction est exécutée.

Si vous appelez une fonction et passez son résultat en tant que paramètre, cette fonction sera exécutée immédiatement,
avant que l'autre fonction ne soit appelée. Seule la valeur renvoyée par cette fonction est passée en paramètre.

Voici un exemple en Go :

```go
package main

import "fmt"

func multiplyByTwo(n int) int {
    return n * 2
}

func printNumber(n int) {
    fmt.Println(n)
}

func main() {
    // La fonction multiplyByTwo est appelée immédiatement avec 10 comme argument
    // printNumber reçoit uniquement le résultat de cette fonction, soit 20.
    printNumber(multiplyByTwo(10))
}
```

Cependant, si vous passez une fonction en tant que paramètre à une autre fonction sans l'appeler, ce que vous
transmettez est une référence à la fonction elle-même, et non le résultat de son exécution. Cela vous permet d'appeler
cette fonction à un moment ultérieur dans le contexte de la deuxième fonction.

Exemple :

```go
package main

import "fmt"

func multiplyByTwo(n int) int {
    return n * 2
}

func printAfterOperation(n int, fn func(int) int) {
    fmt.Println(fn(n))
}

func main() {
    // Ici, multiplyByTwo est passé en tant que référence de fonction à printAfterOperation
    // Il ne sera exécuté qu'à l'intérieur de printAfterOperation, avec 10 comme argument
    printAfterOperation(10, multiplyByTwo)
}
```

Dans ce deuxième exemple, nous n'évaluons pas immédiatement `multiplyByTwo`, mais nous la passons plutôt comme une
fonction à `printAfterOperation`. Cette deuxième fonction peut ensuite l'appeler avec ses propres arguments.

