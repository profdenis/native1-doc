# Section 2 : Goroutines

Les **goroutines** sont une des caractéristiques du langage Go, qui facilitent la programmation concurrente. Une
goroutine est une sorte de _thread_ léger, mais géré par le _runtime_ de Go et non par le système d'exploitation. Vous
pouvez démarrer une goroutine simplement en utilisant le mot clé `go` devant un appel de fonction.

## Exemple : avec time.Sleep

Voici un exemple simple qui illustre l'utilisation de goroutines :

```go
package main

import (
	"fmt"
	"time"
)

func countdown(n int, id int) {
	for i := n; i >= 0; i-- {
		fmt.Printf("goroutine %d : %d\n", id, i)
		time.Sleep(1 * time.Second)
	}
}

func main() {
	for i := 1; i <= 4; i++ {
		go countdown(5, i) // Lancer la goroutine
	}
	time.Sleep(6 * time.Second) // Laisser du temps pour l'exécution des goroutines
}
```

Dans cet exemple, `countdown` est une fonction qui affiche le décompte de `n` à `0`. En préfixant l'appel de
fonction `countdown(5, i)` par le mot clé `go` dans la boucle de la fonction `main`, Go créera une nouvelle goroutine
pour chaque appel de fonction et commencera son exécution de manière concurrente.

Le `time.Sleep` à la fin de la fonction `main` est nécessaire pour empêcher que le programme principal ne se termine
avant que les goroutines aient fini leur décompte. En vrai code, vous devriez utiliser des mécanismes de synchronisation
comme des groupes d'attentes (_WaitGroups_) ou des canaux (_channels_) pour contrôler l'exécution de vos goroutines,
mais pour un exemple simple comme celui-ci, un simple `time.Sleep` suffit pour illustrer le principe.

À noter que l'ordre d'exécution précis des goroutines n'est pas garanti et peut varier à chaque exécution. C'est l'un
des défis de la programmation concurrente : la nécessité de bien synchroniser les différentes tâches pour éviter des
conflits ou des comportements imprévisibles.

## Exemple : avec canal

```go
package main

import (
	"fmt"
	"time"
)

// Nous ajoutons un canal done de type bool
func countdown(n int, id int, done chan<- bool) {
	for i := n; i >= 0; i-- {
		fmt.Printf("goroutine %d : %d\n", id, i)
		time.Sleep(1 * time.Second)
	}
	done <- true // Envoie true sur le canal quand la goroutine a terminé
}

func main() {
	done := make(chan bool, 4) // Crée un canal de booléens avec une capacité de 4

	for i := 1; i <= 4; i++ {
		go countdown(5, i, done) // démarre une goroutine et lui passe le canal done
	}

	for i := 1; i <= 4; i++ {
		<-done // Attend un vrai de chaque goroutine
	}
}
```

Dans cet exemple, nous avons modifié la déclaration du canal `done` pour qu'il soit un canal de `bool`. Nous avons aussi
ajouté une capacité à la création du canal pour éviter un blocage si les goroutines finissent leur décompte avant
que `main` commence à recevoir sur le canal. Ensuite, nous envoyons `true` sur le canal `done` dans chaque goroutine
après qu'elle a fini son décompte. Enfin, à la fin de `main`, nous recevons de chaque goroutine sur le canal `done`
pour attendre qu'elle ait fini.

## Exemple avec WaitGroup

Le package `sync` de Go fournit une structure `WaitGroup` qui peut être utilisée pour synchroniser plusieurs goroutines.
Voici comment on pourrait modifier notre exemple pour utiliser un `WaitGroup` :

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func countdown(n int, id int, wg *sync.WaitGroup) {
	defer wg.Done() // Signale au WaitGroup que cette goroutine est terminée lorsque la fonction se termine
	for i := n; i >= 0; i-- {
		fmt.Printf("goroutine %d : %d\n", id, i)
		time.Sleep(1 * time.Second)
	}
}

func main() {
	var wg sync.WaitGroup // Crée un WaitGroup

	for i := 1; i <= 4; i++ {
		wg.Add(1) // Augmente le compteur du WaitGroup de 1
		go countdown(5, i, &wg) // Démarre une goroutine avec le WaitGroup
	}

	wg.Wait() // Attend que toutes les goroutines signalent qu'elles sont terminées
}
```

La méthode `Add` est utilisée pour augmenter le compteur du `WaitGroup`. Chaque fois que nous lançons une goroutine,
nous augmentons le compteur de 1. La méthode `Done` est appelée pour diminuer le compteur lorsque la goroutine a fini
son travail. Enfin, la méthode `Wait` est utilisée pour bloquer la goroutine actuelle (dans ce cas, la goroutine
principale) jusqu'à ce que le compteur du `WaitGroup` atteigne zéro, c'est-à-dire que toutes les goroutines qu'il attend
ont signalé qu'elles sont terminées. Cela nous permet d'attendre de manière sûre et flexible l'achèvement de toutes les
goroutines.
