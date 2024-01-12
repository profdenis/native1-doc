# Section 2 : Tranches

En Go, lorsque vous utilisez un tableau statique et passez un pointeur de ce tableau à une fonction, vous devez
spécifier la taille du tableau. C'est parce que les tableaux de différentes tailles sont considérés comme des types
différents en Go. En d'autres mots, la taille du tableau fait partie de son type.

Cependant, si vous voulez une fonction qui puisse accepter une collection d'entiers de n'importe quelle taille, vous
devriez utiliser une **tranche** (_slice_) au lieu d'un tableau. Les tranches sont des abstractions construites autour
des tableaux et sont plus flexibles. Les tranches sont une structure de données de la catégorie des tableaux dynamiques,
qui ressemblent aux `ArrayList` en Java.

Voici une modification de l'exemple précédent, mais avec des tranches :

```go
package main

import "fmt"

// La fonction modifierTentative modifie le slice qu'elle reçoit.
func modifierTentative(s []int) {
    s[0] = 100
}

func main() {
    maTranche := []int{1, 2, 3}
    
    modifierTentative(maTranche)
    
    fmt.Println(maTranche)  // Output : [100 2 3]
}
```

Dans cet exemple, `modifierTentative` accepte une tranche d'entiers (`[]int`). À l'intérieur de la fonction, nous
utilisons `s[0]` pour accéder au premier élément de la tranche `s` et pour la modifier.

Et ici, contrairement aux tableaux, le passage d'une tranche à une fonction ne crée pas de nouvelle copie des données du
tableau. Au lieu de cela, la fonction modifie la tranche originale. C'est pourquoi lorsque nous imprimons `maTranche`
après l'appel à `modifierTentative`, ça affiche la tranche modifiée : `[100 2 3]`.

Non seulement la taille d'une tranche est dynamique (c'est pour ça qu'on appelle souvent les structures de ce type des
tableaux dynamiques), mais on peut également modifier leur contenu sans avoir besoin d'utiliser un pointeur vers la
tranche, comme dans l'exemple précédent.

Plus de détails sur les tranches seront présentés dans un autre chapitre, après la présentation des
structures (`struct`) et des principes de la gestion de la mémoire. Voici la représentation mémoire des variables de
l'exemple, mais pour comprendre les détails, il faut d'abord comprendre ce qu'est une structure.