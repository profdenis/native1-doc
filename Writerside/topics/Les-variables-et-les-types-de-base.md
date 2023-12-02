# Section 2 : les variables et les types de base

Référence : [A tour of Go](https://go.dev/tour/basics/11)

| catégorie                             | types                                                    |
|---------------------------------------|----------------------------------------------------------|
| logique                               | `bool`                                                   |
| caractères                            | `string`, `rune` (alias pour `int32`)                    |
| nombres entiers (signés)              | `int`, `int8`, `int16`, `int32`, `int64`                 |
| nombres entiers positifs (non signés) | `uint`, `uint8`, `uint16`, `uint32`, `uint64`, `uintptr` |
| données brutes                        | `byte` (alias for `uint8`)                               |
| nombres réels                         | `float32`, `float64`                                     |
| nombres complexes                     | `complex64`, `complex128`                                |

Les types `int`, `uint`, et `uintptr` ont une largeur de 32 bits sur un système 32-bit et une largeur de 64 bits sur un
système 64-bit. Quand vous avez besoin d'un nombre entier, vous devriez utiliser `int` à moins d'avoir une raison
spécifique d'utiliser une taille précise ou un nombre non-signé.

## Définition avec `var` et sans valeur initiale

Une déclaration de type `var` commence avec le mot-clé `var`, suivi par le nom de la variable ou une liste de noms de
variables séparées par des virgules, et suivi par le type de la ou les variable(s).

```Go
package main

import "fmt"

func variables2a() {
	var i int
	var finished bool
	var name string
	var x, y float32

	fmt.Println(i)
	fmt.Println(finished)
	fmt.Println(name)
	fmt.Println(x, y)
}
```

Les variables vont être automatiquement initialisées avec leur **valeur zéro** (ou leur valeur par défaut), qui dépend
du type de la variable. Pour les nombres, la valeur par défaut est `0`, pour les booléens la valeur est `false`, et pour
les chaînes de caractères, la chaîne vide `""` est utilisée comme valeur zéro.

## Définition avec `var` et valeur initiale

On peut aussi préciser une valeur pour initialiser une variable au moment de la déclaration.

```Go
package main

import "fmt"

func variables2b() {
	var i int = 5
	var finished bool = true
	var name string = "Denis"
	var x, y float32 = 2.5, -1.4

	fmt.Println(i)
	fmt.Println(finished)
	fmt.Println(name)
	fmt.Println(x, y)
}
```

Si une valeur initiale est spécifiée, alors il n'est pas nécessaire de spécifier le type. La variable prendra
automatiquement le type de la valeur utilisée. L'exemple suivant est équivalent à l'exemple précédent, à une différence
près : les variables `x` et `y` sont maintenant de type `float64`, et non `float32`. Le type pour les nombres réels
est `float64` par défaut.

```Go
package main

import "fmt"

func variables2c() {
	var i = 5
	var finished = true
	var name = "Denis"
	var x, y = 2.5, -1.4

	fmt.Println(i)
	fmt.Println(finished)
	fmt.Println(name)
	fmt.Println(x, y)
	fmt.Printf("%T\n", x)
}
```

Sortie :

```
5
true
Denis
2.5 -1.4
float64

```

La dernière ligne de code écrit le type de la variable `x` sur la sortie standard (le _stdout_). Plus de détails sur les
sorties seront donnés dans la section suivante.

### Constantes

On peut définir une constante en remplaçant `var` par `const`. Essayer de modifier la valeur d'une constante va créer
une erreur de compilation.

## Définition avec le raccourci `:=`

Il est possible d'utiliser un raccourci pour les définitions de variables avec valeurs initiales. L'exemple suivant est
équivalent à l'exemple précédent, sauf que `x` et `y` sont définies séparément.

```Go
package main

import "fmt"

func variables2d() {
	i := 5
	finished := true
	name := "Denis"
	x := 2.5
	y := -1.4

	fmt.Println(i)
	fmt.Println(finished)
	fmt.Println(name)
	fmt.Println(x, y)
	fmt.Printf("%T\n", x)
}
```

Il est aussi possible de définir 2 variables ou plus à la fois sur une même ligne, dans la même déclaration comme ceci
`x, y := 2.5, -1.4`, mais il est souvent préférable en général de définir seulement une variable à la fois pour
faciliter la lecture et la compréhension du code. Dans des situations simples, comme dans l'exemple qui vient d'être
donné, ça peut aller, mais avec des initialisations plus complexes, c'est préférable de séparer les déclarations. Un
endroit courant où plus d'une variable est définie dans une seule déclaration est lorsqu'une fonction retourne plus
d'une valeur, ou en dans une boucle sur un intervalle, comme un `range` sur un tableau par exemple. Plus de détails
viendront dans les prochaines sections.