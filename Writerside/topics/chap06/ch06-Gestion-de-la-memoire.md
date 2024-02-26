# Chapitre 6 : Gestion de la mémoire

Le langage de programmation Go gère la mémoire de manière automatique à travers un ramasse-miettes (_garbage collector_
en anglais). Voici des détails importants à noter :

- **Allocation de mémoire** : En Go, lorsque vous déclarez une variable, Go alloue automatiquement de la mémoire pour
  cette variable. L'espace mémoire est alloué sur le **tas** si la variable est créée à l'aide du mot-clé `new` ou si
  elle est un composite (slice, map, channel) ou si vous utilisez seulement son adresse pour y accéder, sinon, elle est
  allouée sur la **pile**. Vous n'avez pas besoin de spécifier la quantité de mémoire nécessaire.

- **Garbage Collection** : Go utilise un ramasse-miettes pour libérer la mémoire. Cela signifie qu'au lieu de devoir
  libérer explicitement la mémoire que vous avez allouée (comme vous le feriez en C ou en C++), le ramasse-miettes de Go
  identifiera les portions de mémoire qui ne sont plus référencées par votre programme et libérera automatiquement cette
  mémoire. Cela contribue à prévenir les fuites de mémoire.

- **Concurrence** : Go est conçu pour être efficace avec le multiprocesseur et les systèmes concurrents. Le _garbage
  collector_ de Go est conçu pour être rapide et ne pas bloquer l'exécution de votre programme pendant qu'il libère la
  mémoire. Cela facilite la création de programmes Go très concurrents.

Un des plus grands avantages de la gestion de la mémoire en Go est qu'elle diminue la complexité de la programmation en
éliminant la nécessité de gérer manuellement la mémoire, prévenant ainsi de fuites de mémoire et d'erreurs d'accès à la
mémoire.

```go
package main

import "fmt"

func main() {
    // La mémoire est allouée automatiquement lors de la déclaration d'une variable
    var num *int
    num = new(int) // Le mot-clé "new" alloue de la mémoire pour stocker un int sur le tas

    // Le ramasse-miettes libèrera automatiquement la mémoire allouée à "num"
    // lorsque "num" ne sera plus utilisé
    fmt.Println(*num) 
}
```

Le code ci-dessus est un exemple de comment la gestion de la mémoire est effectuée en Go.
