# Section 1 : Les pointeurs

En Go, un pointeur est une variable qui stocke l'adresse mémoire d'une autre variable. Les pointeurs sont une partie
très fondamentale de Go et sont extrêmement utiles car ils permettent aux fonctions de modifier les données qui leur
sont passées, parce qu'ils ont accès à la même adresse mémoire où réside la variable originale.

Pour créer un pointeur, nous utilisons l'opérateur `&` avant une variable. Cela renvoie l'adresse mémoire de cette
variable. Par exemple :

```go
var x int = 5
ptr := &x
```

Dans ce cas, `ptr` est un pointeur vers `x`, car il contient l'adresse mémoire de `x`.

Le type de `ptr` dans le cas ci-dessus est `*int`, car c'est un pointeur vers un entier. Le symbole `*` avant un type
est utilisé pour désigner un pointeur vers ce type.

Pour obtenir la valeur stockée à l'adresse mémoire indiquée, ou pour _"déréférencer"_ le pointeur, vous pouvez utiliser
l'opérateur `*` avant le pointeur, comme `*ptr`. Par exemple :

```go 
fmt.Println(*ptr) // Cela imprimera : 5
```

![Pointeur vers int](ch02s01-pointeur1-1.jpg)

Vous pouvez également utiliser des pointeurs pour changer la valeur stockée à une certaine adresse mémoire :

```go 
*ptr = 8
fmt.Println(x) // Cela imprimera : 8
```

Dans ce cas, en modifiant `*ptr`, nous changeons en fait la valeur de `x` puisque `ptr` contient l'adresse de `x`.

![Modification de la valeur pointée](ch02s01-pointeur1-2.jpg)

On peut également imprimer la valeur contenue dans la variable `ptr`. Puisque c'est un pointeur, nous obtiendrons une
adresse mémoire, qui dans ce cas-ci est l'adresse (ou l'emplacement mémoire) où est située la variable `x`.

```go 
fmt.Println(ptr) // Cela imprimera une valeur semblable à : 0xc00012e010
```

La valeur imprimée ne sera pas toujours la même. Ça dépendra de l'endroit exact où la variable a été allouée lors de
l'exécution du programme, qui varie selon différents facteurs, qui dépendent en partie du système d'exploitation. Nous
reviendrons sur les modèles d'allocation de mémoire un peu plus loin.

Faites attention, la valeur zéro d'une variable non initialisée de type pointeur est `nil`, et si vous essayez de
déréférencer le pointeur, Go interrompra l'exécution avec une panique au moment de l'exécution. **Assurez-vous toujours
que vos pointeurs sont initialisés avant utilisation.**
