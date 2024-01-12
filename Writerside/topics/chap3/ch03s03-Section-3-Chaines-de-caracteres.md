# Section 3 : Chaînes de caractères

En Go, une chaîne de caractères est essentiellement une tranche d'octets en lecture seule. Cela permet aux chaînes Go
d'avoir des tailles variables, comme les tranches de n'importe quel type. Si les chaînes étaient construites directement
avec des tableaux, on ne pourrait pas mélanger les chaînes de différentes longueurs.

Les chaînes de caractères sont **immuables**. Cela signifie qu'une fois qu'une chaîne est créée, elle ne peut pas être
modifiée. Si vous essayez de modifier une chaîne, Go créera une nouvelle chaîne avec les modifications, au lieu de
modifier la chaîne originale. Ou si vous essayez de modifier directement un caractère spécifique d'une chaîne, il y aura
une erreur.

Voici un exemple démontrant l'immutabilité des chaînes :

```go
package main

import "fmt"

func main() {
    s := "bonjour"
    s += " le monde"  // Crée une nouvelle chaîne
    fmt.Println(s) // Affiche : "bonjour le monde"
    
    s[0] = 'B' // ne compilera pas
}
```

Dans l'exemple ci-dessus, `s += " le monde"` n'ajoute pas _"le monde"_ à la chaîne originale _"bonjour"_. Au lieu de
cela, il crée une nouvelle chaîne "bonjour le monde" et l'assigne à `s`.

Si vous avez besoin d'un type semblable à une chaîne que vous pouvez modifier, envisagez d'utiliser
une tranche d'octets (`[]byte`) ou un `strings.Builder` (voir l'exemple suivant).

Les chaînes Go sont encodées en UTF-8, un seul caractère dans une chaîne Go pourrait être
représenté par un ou plusieurs octets. Donc le nombre de caractères dans une chaîne n'est pas égal, en général, au
nombre d'octets dans une tranche représentant la même chaîne. C'est en partie pourquoi les chaînes sont immuables en Go.

[//]: # (TODO: exemple avec strings.Builder)

## Utilisation d'un `strings.Builder`

Voici un exemple très simple d'utilisation d'un `strings.Builder`.

```Go
package main

import (
	"fmt"
	"strings"
)

func main() {
	var builder strings.Builder

	builder.WriteString("Hello")
	builder.WriteString(" ")
	builder.WriteString("World")
	builder.WriteString("!")

	result := builder.String()

	fmt.Println(result) 
}
```

Dans cet exemple, `strings.Builder` est utilisé pour faire une concaténation efficace de plusieurs chaînes de
caractères. C'est très utile lorsque vous avez besoin de concaténer un grand nombre de chaînes, car il est plus
efficace en termes de performance que l'opérateur `+` ou que `fmt.Sprintf`.

Plus d'exemples sur l'utilisation des `strings.Builder` seront présentés dans le chapitre sur la gestion des fichiers.
