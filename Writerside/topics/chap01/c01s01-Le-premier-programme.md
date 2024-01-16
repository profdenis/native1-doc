# Section 1 : Le premier programme

Ce n'est pas original, mais le premier programme dans un nouveau langage de programmation est presque toujours le fameux
*Hello World !*.

```go
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

## Compilation et exécution

La compilation avec Go peut être décrite en plusieurs étapes :

1. **Analyse Lexicale et Syntaxique** : Le compilateur Go lit le texte du programme source dans des fichiers `.go`
   caractère par caractère. En parcourant le texte, il identifie les mots-clés, les identificateurs, les littéraux, etc.
   C'est l'expression du programme sous forme d'arborescence de syntaxe abstraite (AST).

2. **Analyse Sémantique** : La prochaine étape est l'analyse sémantique où le compilateur vérifie les déclarations et
   les affectations de types pour s'assurer qu'elles sont logiquement correctes dans le contexte du langage de
   programmation Go. Par exemple, affecter une valeur de chaîne à une variable déclarée comme un entier serait détecté
   comme une erreur de type lors de cette étape.

3. **Génération du code intermédiaire** : Go génère un code intermédiaire appelé _SSA (Single Static Assignment)_, qui
   simplifie l'optimisation et la traduction ultérieure en code machine.

4. **Optimisation** : Go effectue plusieurs optimisations pour améliorer la performance du programme final, y compris
   l'élimination de code mort (code qui ne peut jamais être exécuté), l'_inlining_ (remplacer une invocation de fonction
   par le corps de la fonction), etc.

5. **Génération du code machine** : Enfin, Go convertit le code SSA en code binaire pour la machine cible. Cela inclut
   également le regroupement de toutes les dépendances (autres packages Go) nécessaires au programme. Le code machine
   est écrit dans un fichier (par exemple, un fichier exécutable `.exe` dans Windows).

Le code binaire peut alors être exécuté par l'ordinateur. Dans ce processus, **il est important de comprendre que le
GoLang est un langage compilé vers une architecture spécifique** (x86, ARM, etc.) **et un système d'exploitation
spécifique** (Linux, Windows, OSX, etc.).

Par conséquent, pour exécuter un programme Go sur une plate-forme différente, vous devrez le recompiler pour cette
plate-forme.

Supposons que l'exemple précédent a été enregistré dans le fichier `main.go` Pour compiler ce programme, vous utiliserez
la commande `go build` pour le compiler, ce qui produira un fichier binaire exécutable.

```shell
go build main.go
```

Ensuite, vous pouvez exécuter le programme compilé :

```shell
./main
```

Il imprimera `"Hello, world!"` à la sortie.

Notez que vous pouvez également utiliser la commande `go run` pour compiler et exécuter le programme en une seule étape,
bien que cela ne laisse pas d'exécutable persistant.

C'est un aperçu de haut niveau, et en réalité le processus est assez complexe. Le but ici n'est pas de comprendre les
détails du processus de compilation, mais de comprendre l'idée générale.