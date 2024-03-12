# Section 3 : Canaux

En informatique, un **canal** (ou _channel_ en anglais) est un mécanisme de communication utilisé pour transférer des
données entre des _threads_ concurrents, ou plus généralement entre des blocs de code concurrents. Les canaux sont
particulièrement utiles dans les systèmes de programmation concurrente et parallèle pour aider à synchroniser
l'exécution de différents threads et éviter les conditions de concurrence.

Les canaux fonctionnent généralement sur le modèle de la communication point à point, ce qui signifie que les données
sont envoyées depuis un point (généralement un thread ou une goroutine) et reçues à un autre point. Cela permet de
synchroniser les deux points de la communication : l'émetteur ne continue pas jusqu'à ce que les données aient été
reçues, et le récepteur ne continue pas jusqu'à ce qu'il ait reçu des données.

Dans le contexte de Go, les canaux sont un mécanisme de communication de première classe, intégré au langage lui-même.
Go supporte des canaux typés qui peuvent être utilisés pour transmettre des données de tout type entre des goroutines.

En Go, les canaux sont utilisés en conjonction avec le mot-clé `go` pour démarrer des goroutines et le mot-clé `chan`
pour déclarer des canaux. Les opérations d'envoi et de réception sur les canaux sont réalisées en utilisant les
opérateurs `<-` et `->`. Les canaux en Go sont également assortis de mécanismes de synchronisation : une opération
d'envoi sur un canal non tamponné bloquera jusqu'à ce qu'une goroutine ailleurs dans le programme exécute une opération
de réception correspondante, et vice versa.

Par conséquent, en Go, les canaux sont un moyen élégant et sûr de gérer la synchronisation et la communication entre
goroutines, leur permettant de fonctionner ensemble de manière fluide et sans conflits.

## Exemple : somme des éléments d'une tranche (sans goroutines)

```go
package main

import "fmt"

func sumSlice(ints []int) int {
	sum := 0
	for _, num := range ints {
		sum += num
	}
	return sum
}

func main() {
	nums := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	fmt.Printf("La somme des numéros est : %d\n", sumSlice(nums))
}
```

## Exemple : somme des éléments d'une tranche (avec goroutines)

```go
package main

import (
	"fmt"
)

func sumSlice(ints []int, results chan<- int) {
	sum := 0
	for _, num := range ints {
		sum += num
	}
	results <- sum
}

func parallelSum(nums []int) int {
	// Création du canal
	results := make(chan int)

	// Divise la slice et lance les goroutines.
	half := len(nums) / 2
	go sumSlice(nums[:half], results)
	go sumSlice(nums[half:], results)

	// Recevoir les résultats des goroutines et calcule la somme totale.
	sum1, sum2 := <-results, <-results

	return sum1 + sum2
}

func main() {
	nums := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	totalSum := parallelSum(nums)
	fmt.Printf("La somme des numéros est : %d\n", totalSum)
}
```

Dans cet exemple, j'ai créé une nouvelle fonction `parallelSum`. Au début de `parallelSum`, nous créons un canal de
type `int` appelé `results`. Ensuite, la slice de nombres est divisée en deux, avec chaque moitié étant passée à une
nouvelle goroutine qui exécute la fonction `sumSlice`. Chaque goroutine calcule sa somme partiellement et envoie le
résultat sur le canal `results`.

Dans `parallelSum`, nous recevons les résultats des deux goroutines directement dans les variables `sum1` et `sum2` en
utilisant l'opération de réception de canal (`<-chan`). Cela bloque la goroutine en cours (qui est la goroutine
principale) jusqu'à ce que chaque goroutine ait envoyé son résultat, ce qui remplace effectivement le besoin
d'un `WaitGroup`.

Enfin, dans la fonction `main`, nous appelons `parallelSum` directement avec notre slice de nombres, et nous imprimons
la valeur retournée qui représente la somme totale de tous les nombres dans la slice.