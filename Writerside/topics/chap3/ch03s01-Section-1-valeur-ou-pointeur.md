# Section 1 : Valeur ou pointeur ?

En Go, les tableaux sont des types valeur, pas des pointeurs, contrairement à d'autres langages de programmation.

Cela signifie que lorsqu'un tableau est assigné à une nouvelle variable, une copie du tableau original est créée. De la
même manière, passer un tableau à une fonction entraînera la création d'une copie de ce tableau dans la fonction. Les
modifications apportées à la copie ne modifieront pas le tableau original.

Voici un exemple pour illustrer ceci :

```go
package main

import "fmt"

// La fonction modifierTentative modifie le tableau qu'elle reçoit.
func modifierTentative(tab [3]int) {
	tab[0] = 100
}

func main() {
	monTableau := [3]int{1, 2, 3}

	modifierTentative(monTableau)

	fmt.Println(monTableau) // Sortie : [1 2 3]

	autreTableau := monTableau
	autreTableau[1] = 100

	fmt.Println(monTableau) // Sortie : [1 2 3]
}
```

[//]: # (TODO: diagramme )

Dans l'exemple ci-dessus, même si la fonction `modifierTentative` a tenté de modifier le premier élément du
tableau `tab`, cela n'a pas eu d'impact sur `monTableau` dans la fonction `main`. C'est pourquoi lorsque nous
imprimons `monTableau` après l'appel à `modifierTentative`, les valeurs restent inchangées.

[//]: # (TODO: diagramme )

Le même principe s'applique si on crée un autre tableau à partir de `monTableau`. Quand on fait
`autreTableau := monTableau`, on crée un autre espace mémoire pour `autreTableau` du même type et de même longueur que
`monTableau`, et le contenu de `monTableau` est copié dans `autreTableau`.

Si vous voulez que les modifications se répercutent sur le tableau d'origine, vous devriez utiliser des
tranches (`slice`) ou passer un pointeur vers le tableau d'origine. Les tranches seront présentées un peu plus loin. Un
exemple avec un pointeur vers un tableau suit.

## Pointeur vers un tableau

L'exemple suivant est une modification de l'exemple suivant, sauf que la fonction accepte un pointeur vers le tableau
plutôt qu'une copie du tableau.

```go
package main

import "fmt"

// La fonction modifierTentative modifie le tableau qu'elle reçoit.
func modifierTentative(tab *[3]int) {
    (*tab)[0] = 100
}

func main() {
    monTableau := [3]int{1, 2, 3}
    
    modifierTentative(&monTableau)
    
    fmt.Println(monTableau)  // Output : [100 2 3]
}
```

[//]: # (TODO: diagramme )

Dans cet exemple, `modifierTentative` accepte un pointeur vers un tableau de 3 entiers (`*[3]int`). À l'intérieur de
la fonction, nous utilisons `(*tab)[0]` pour accéder au premier élément du tableau pointé par `tab` et pour le modifier.
Les parenthèses sont nécessaires ici parce que les crochets `[]` ont préséance sur l'astérisque `*`, donc il faut
déréférencer le pointeur vers un tableau avant d'essayer d'accéder à ses éléments.

Dans la fonction `main`, nous passons l'adresse du tableau (`&monTableau`) à la fonction `modifierTentative`.

Maintenant, les modifications apportées à l'intérieur de `modifierTentative` sont répercutées sur le
tableau `monTableau` dans `main`. Ainsi, lorsque nous imprimons `monTableau` après l'appel à `modifierTentative`, il
affiche le tableau modifié :

```[100 2 3]```
