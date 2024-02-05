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

![ch03s01-Tableau 1a.jpg](ch03s01-Tableau-1a.jpg)

Dans l'exemple ci-dessus, même si la fonction `modifierTentative` a tenté de modifier le premier élément du
tableau `tab`, cela n'a pas eu d'impact sur `monTableau` dans la fonction `main`. C'est pourquoi lorsque nous
imprimons `monTableau` après l'appel à `modifierTentative`, les valeurs restent inchangées.

![ch03s01-Tableau-1b.jpg](ch03s01-Tableau-1b.jpg)

Le même principe s'applique si on crée un autre tableau à partir de `monTableau`. Quand on fait
`autreTableau := monTableau`, on crée un autre espace mémoire pour `autreTableau` du même type et de même longueur que
`monTableau`, et le contenu de `monTableau` est copié dans `autreTableau`.

Si vous voulez que les modifications se répercutent sur le tableau d'origine, vous devriez utiliser des
tranches (`slice`) ou passer un pointeur vers le tableau d'origine. Les tranches seront présentées un peu plus loin. Un
exemple avec un pointeur vers un tableau suit.

## La longueur du tableau fait partie du type

Le fait qu'en Go, un tableau est de type valeur, implique que, en plus du type des éléments du tableau, la longueur
du tableau fait partie du type du tableau. Il n'est pas possible, par exemple, de passer en argument un tableau de type
`[4]int` à une fonction qui demande un tableau de type `[3]int`.

Modifiez l'exemple précédent en modifiant soit la taille du tableau dans le `main`, soit la longueur du tableau dans la
déclaration du paramètre de la fonction, et vous obtiendrez une erreur du
genre `cannot use monTableau (variable of type [4]int) as [3]int value in argument to modifierTentative`. Le programme
ne compilera pas parce que les types ne correspondent pas. Puisque les valeurs du tableau donné en argument doivent être
copiées dans le tableau local à la fonction, non seulement les valeurs doivent être du même type, mais il doit y avoir
le même nombre de valeurs (c.-à-d. la même longueur de tableau) dans les deux tableaux.

Pour passer un tableau à une fonction, on utilise soit un pointeur vers un tableau, ou plus couramment une tranche, qui
contient un pointeur vers un tableau.

## Pointeur vers un tableau

L'exemple suivant est une modification de l'exemple précédent, sauf que la fonction accepte un pointeur vers le tableau
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

![ch03s01-Tableau-2.jpg](ch03s01-Tableau-2.jpg)

Dans cet exemple, `modifierTentative` accepte un pointeur vers un tableau de 3 entiers (type `*[3]int`). À l'intérieur
de la fonction, nous utilisons `(*tab)[0]` pour accéder au premier élément du tableau pointé par `tab` et pour le
modifier. Les parenthèses sont nécessaires ici parce que les crochets `[]` ont préséance sur l'astérisque `*`, donc il
faut déréférencer le pointeur vers un tableau avant d'essayer d'accéder à ses éléments.

Dans la fonction `main`, nous passons l'adresse du tableau (`&monTableau`) à la fonction `modifierTentative`.

Maintenant, les modifications apportées à l'intérieur de `modifierTentative` sont répercutées sur le
tableau `monTableau` dans `main`. Ainsi, lorsque nous imprimons `monTableau` après l'appel à `modifierTentative`, il
affiche le tableau modifié :

```[100 2 3]```
