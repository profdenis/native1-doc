# Section 1 : Le premier programme

Ce n'est pas original, mais le premier programme dans un nouveau langage de programmation est presque toujours le fameux
*Hello World !*.

```Go
package main

import "fmt"

func main() {
	fmt.Println("Hello World !")
}
```

## Explications

1. `package main` : tous les fichiers `.go` doivent débuter par une déclaration de paquet (*package*), et lorsqu'on veut
   un *exécutable*, le paquet doit être `main`, et il doit y avoir une fonction appelée `main`.
2. `import "fmt"` : on doit importer le paquet `fmt` pour pouvoir utiliser la fonction `Println` pour écrire du texte
   sur la sortie standard (*standard output*, ou *stdout*).
3. `func main() {` : déclaration de la fonction `main`, qui sera le point de départ de notre programme exécutable.
4. `fmt.Println("Hello World !")` : le contenu de la fonction est évidemment très simple, on écrit simplement une chaîne
   de caractères sur le *stdout*
    1. `Print` --> imprime (ou écrit), `ln` --> ajoute un changement de ligne (un `\n`) à la fin
    2. les fonctions et les variables **publiques** commencent toujours avec une lettre majuscule en Go : si la fonction
       était définie avec une lettre minuscule au début, soit `println` à la place, la fonction serait **privée** et on
       ne pourrait pas l'appeler
    3. il existe plusieurs autres fonctions dans le paquet `fmt`, pour écrire sur le *stdout* ou ailleurs
5. `}` : les blocs de code sont délimités par des accolades (comme en Java), mais il n'y a pas de point-virgules `;` à
   la fin des lignes qui contiennent des énoncés qui ne nécessitent pas de définition de bloc

