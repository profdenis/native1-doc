# Section 2 : Tranches

En Go, lorsque vous utilisez un tableau statique et passez un pointeur de ce tableau à une fonction, vous devez
spécifier la taille du tableau. C'est parce que les tableaux de différentes tailles sont considérés comme des types
différents en Go. En d'autres mots, la taille du tableau fait partie de son type.

Cependant, si vous voulez une fonction qui puisse accepter une collection d'entiers de n'importe quelle taille, vous
devriez utiliser une **tranche** (_slice_) au lieu d'un tableau. Les tranches sont des abstractions construites autour
des tableaux et sont plus flexibles. Les tranches sont une structure de données de la catégorie des **tableaux
dynamiques**, qui ressemblent aux `ArrayList` en Java.

Voici une modification de l'exemple précédent, mais avec des tranches :

```go
package main

import "fmt"

// La fonction modifierTentative modifie la tranche qu'elle reçoit.
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

Voici la représentation mémoire des variables de l'exemple, mais pour comprendre les détails, il va falloir comprendre
ce qu'est une structure. Plus de détails sur les tranches seront présentés dans un autre chapitre, après la présentation
des structures (`struct`) et des principes de la gestion de la mémoire. Mais en attendant, on peut voir une structure
comme étant un regroupement de plusieurs variables ensemble, un peu comme une classe avec ses attributs dans un langage
orienté-objet.

![ch03s02-Tranche.jpg](ch03s02-Tranche.jpg)

Dans une structure représentant une tranche, on a essentiellement 2 nombres entiers, la longueur et la
capacité de la tranche, suivi d'un pointeur vers un tableau. La capacité est la longueur du tableau, égale à 3 dans
l'exemple, et la longueur est le nombre d'emplacements utilisés dans le tableau, aussi égale à 3 dans l'exemple. Comme
mentionné précédemment, les tranches vont être expliquées plus en détails dans un autre chapitre. Le plus important pour
l'instant, c'est qu'une tranche contient un pointeur vers un tableau, et lorsque l'on passe une tranche en paramètre à
une fonction, on copie la structure, incluant le pointeur vers le tableau, mais on ne copie le tableau lui-même. C'est
un peu comme si on passait un pointeur à un tableau comme dans un exemple précédent, mais accompagné de 2 valeurs
supplémentaires. Donc quand on modifie le contenu de la tranche avec `s[0] = 100`, on modifie réellement le tableau
caché derrière la tranche, qui lui n'est pas copié, mais partagé entre `maTranche` et `s`.

## Taille dynamique

### Avec `append`

Pour ajouter des éléments à une tranche, on doit utiliser la fonction `append`. Cette fonction accepte une tranche
suivie d'un ou plusieurs éléments à ajouter, et retourne la tranche avec les éléments qui ont été ajoutés. Il ne faut
surtout pas ignorer la valeur de retour, sinon les changements seront perdus.

```go
s := []int{1, 2, 3}
fmt.Println(s)       // sortie : [1 2 3]
s = append(s, 7, 4)
fmt.Println(s)       // sortie : [1 2 3 7 4]
```

#### Modification à l'intérieur d'une fonction : exemple 1

Cet exemple sert de référence. C'est un exemple de modification comme déjà fait précédemment. Un élément de la tranche
est modifié à l'intérieur de la fonction, et le changement est visible à l'extérieur.

```go
func modSlice1(s []int) {
	s[0] = 100
}

func section2a() {
	s := []int{1, 2, 3}
	fmt.Println(s)
	modSlice1(s)
	fmt.Println(s)
	fmt.Println()
}
```

Sortie :

```go
[1 2 3]
[100 2 3]

```

#### Modification à l'intérieur d'une fonction : exemple 2

Si on modifie une tranche à l'aide de `append` à l'intérieur d'une fonction, les changements ne seront pas visibles à
l'extérieur parce que la longueur et la capacité sont des copies locales, donc les changements ne seront pas visible à
l'extérieur de la fonction.

```go
func modSlice2(s []int) {
	s = append(s, 100)
}

func section2b() {
	s := []int{1, 2, 3}
	fmt.Println(s)
	modSlice2(s)
	fmt.Println(s)
	fmt.Println()
}
```

Sortie :

```go
[1 2 3]
[1 2 3]
```

#### Modification à l'intérieur d'une fonction : exemple 3

Pour voir l'effet de `append` à l'extérieur de la fonction, il faut passer un pointeur vers la tranche pour être capable
de modifier la structure qui contient la longueur et la capacité de la tranche directement, et non une copie de ces
valeurs.

```go
func modSlice3(s *[]int) {
	*s = append(*s, 100)
}

func section2c() {
	s := []int{1, 2, 3}
	fmt.Println(s)
	modSlice3(&s)
	fmt.Println(s)
	fmt.Println()
}
```

Sortie :

```go
[1 2 3]
[1 2 3 100]
```

### Créer une tranche avec la fonction `make`

Il est possible d'utiliser la fonction `make` pour créer une tranche de longueur et de capacité variables. La fonction
accepte le type à créer, suivi de la longueur et de la capacité : `make([]int, length, capacity)`.

#### Modification à l'intérieur d'une fonction : exemple 4

Voici un exemple semblable aux précédents, mais avec la fonction `make` pour la création de la tranche. Dans cet
exemple, après l'appel à `make`, la tranche contient 3 éléments, tous égaux à 0, la valeur par défaut pour un `int`.
Mais le tableau a de l'espace pour 2 éléments de plus parce que la capacité est 5. Plus de détails sur les algorithmes
utilisés pour la gestion de la mémoire par une tranche seront présentés dans un autre chapitre.

```go
func modSlice4(s []int) {
	s = append(s, 100)
}

func section2d() {
	s := make([]int, 3, 5)
	s[0] = 4
	s[1] = 1
	s[2] = -5
	fmt.Printf("%d %d %v\n", len(s), cap(s), s)
	modSlice4(s)
	fmt.Printf("%d %d %v\n\n", len(s), cap(s), s)
}
```

Sortie :

```go
3 5 [4 1 -5]
3 5 [4 1 -5]
```

#### Modification à l'intérieur d'une fonction : exemple 5

Un autre exemple, semblable au précédent, mais utilisant un pointeur vers une tranche comme type du paramètre de la
fonction. Notez que la capacité de la tranche augmente automatiquement si nécessaire.

```go
func modSlice5(s *[]int) {
	*s = append(*s, 100)
}

func section2e() {
	s := make([]int, 3, 5)
	s[0] = 4
	s[1] = 1
	s[2] = -5
	fmt.Printf("%d %d %v\n", len(s), cap(s), s)
	modSlice5(&s)
	fmt.Printf("%d %d %v\n", len(s), cap(s), s)
	modSlice5(&s)
	modSlice5(&s)
	fmt.Printf("%d %d %v\n\n", len(s), cap(s), s)
}
```

Sortie :

```go
3 5 [4 1 -5]
4 5 [4 1 -5 100]
6 10 [4 1 -5 100 100 100]
```

## Obtenir une sous-tranche d'une tranche

Une sous-tranche est une tranche dérivée d'une autre tranche. On doit spécifier un intervalle avec 2 index, l'index de
départ et celui d'arrivée. Voici quelques exemples :

```go
func section2f() {
	numbers := []int{0, 1, 2, 3, 4, 5}
	fmt.Println(numbers)
	fmt.Println(numbers[1:6])
	fmt.Println(numbers[1:5])
	sub := numbers[1:]
	fmt.Println(sub)
	numbers[2] = -1
	fmt.Println(numbers)
	fmt.Println(sub)
	fmt.Println(sub[:len(sub)-1])
	fmt.Println(sub[1:2])
	fmt.Println(sub[1])
}
```

Sortie :

```go
[0 1 2 3 4 5]
[1 2 3 4 5]
[1 2 3 4]
[1 2 3 4 5]
[0 1 -1 3 4 5]
[1 -1 3 4 5]
[1 -1 3 4]
[-1]
-1
```

Il est important de noter que les sous-tranches partagent le même tableau sous-jacent avec la tranche originale, toute
modification de la sous-tranche affectera la tranche originale. Ce comportement est dû à la façon dont les tranches en
Go sont conçues, où une tranche se compose d'un pointeur vers le tableau, la longueur de la tranche, et sa capacité.

Lorsque vous traitez de grands ensembles de données, la création de sous-tranches peut économiser de la mémoire, car
vous ne dupliquez pas l'ensemble des données. Gardez à l'esprit la caractéristique de partage de tableau des tranches
lors de la modification de celles-ci, surtout lors de l'exécution concurrente.

N'oubliez pas de vérifier les limites du tableau lors du découpage, pour éviter une panique d'exécution. Les limites
d'une tranche sont inclusives pour l'indice de début et exclusives pour l'indice de fin,
c'est-à-dire `slice[start:end]`.

Notez aussi, dans l'exemple précédent, la différence entre les 2 dernières sorties. La dernière sortie est créée en
spécifiant seulement une valeur entre les crochets `[]`, donc aucune sous-tranche n'est créée ici, on obtient uniquement
l'élément à l'index donné. Tandis que pour l'avant-dernière sortie, on utilise 2 index séparés par `:`, donc on obtient
une tranche avec uniquement un élément.