# Opérations sur les tranches

Certaines opérations sur les tranches peuvent être faites **sur place** ou **en place** (_in-place_), ce qui va modifier
le contenu des tranches, ou sinon elles peuvent créer une nouvelle tranche sans toucher à l'originale.

## Trier une tranche

Le code suivant définit une fonction appelée `section4a()`. Cette fonction effectue le tri de tranches de différents
types (entier, et struct) de différentes façons. Ces opérations sont faites _sur place_, donc les tranches sont
modifiées.

```go
func section4a() {
	// trier une tranche de nombres entiers
	numbers1 := []int{2, 5, 1, 7, 3, 8, 4, 7, 6, 4}
	fmt.Println(numbers1)
	fmt.Println("Trié ?", sort.IntsAreSorted(numbers1))

	sort.Ints(numbers1)
	fmt.Println(numbers1)
	fmt.Println("Trié ?", sort.IntsAreSorted(numbers1))

	numbers1 = []int{2, 5, 1, 7, 3, 8, 4, 7, 6, 4}
	fmt.Println(numbers1)
	fmt.Println("Trié ?", sort.IntsAreSorted(numbers1))
	sort.Slice(numbers1, func(i, j int) bool {
		return numbers1[i] < numbers1[j]
	})
	fmt.Println(numbers1)
	fmt.Println("Trié ?", sort.IntsAreSorted(numbers1))

	numbers1 = []int{2, 5, 1, 7, 3, 8, 4, 7, 6, 4}
	fmt.Println(numbers1)
	fmt.Println("Trié ?", sort.IntsAreSorted(numbers1))
	// trier en ordre décroissant
	sort.Slice(numbers1, func(i, j int) bool {
		return numbers1[i] > numbers1[j]
	})
	fmt.Println(numbers1)
	fmt.Println("Trié ?", sort.IntsAreSorted(numbers1))

	// trier une tranche de struct Person
	people := []Person{
		{Name: "John", Age: 30},
		{Name: "Jane", Age: 25},
		{Name: "Alice", Age: 35},
	}
	fmt.Println(people)
	// trier par âge croissant
	sort.Slice(people, func(i, j int) bool {
		return people[i].Age < people[j].Age
	})
	fmt.Println(people)
	// trier par âge décroissant
	sort.Slice(people, func(i, j int) bool {
		return people[i].Age > people[j].Age
	})
	fmt.Println(people)

	// trier par nom croissant
	sort.Slice(people, personNameCompare(people))
	fmt.Println(people)
	// trier par nom décroissant
	sort.Slice(people, personNameCompareReverse(people))
	fmt.Println(people)

}

func personNameCompareReverse(people []Person) func(i int, j int) bool {
	return func(i, j int) bool {
		return people[i].Name > people[j].Name
	}
}

func personNameCompare(people []Person) func(i int, j int) bool {
	return func(i, j int) bool {
		return people[i].Name < people[j].Name
	}
}
```

1. **Tri d'une tranche d'entiers** : D'abord, elle créé une tranche d'entiers appelée `numbers1`, affiche la tranche,
   vérifie si elle est triée (en utilisant `sort.IntsAreSorted()`) et affiche le résultat. Elle trie ensuite la tranche
   en utilisant `sort.Ints()`, affiche de nouveau la tranche et vérifie si elle est triée. Ce processus est répété deux
   fois, la seconde fois en triant la tranche dans l'ordre décroissant avec une fonction lambda et `sort.Slice()`.

2. **Tri d'une tranche de structs** : Ensuite, une tranche de structs `Person` est créée. Chaque `Person` a un nom et un
   âge. Elle affiche la tranche de personnes, la trie par âge en ordre croissant, l'affiche à nouveau, la trie par âge
   en ordre décroissant, et l'affiche une fois de plus. Elle trie ensuite la tranche de personnes par nom en ordre
   croissant, l'affiche, la trie par nom en ordre décroissant, et l'affiche une dernière fois.

Les deux fonctions `personNameCompare()` et `personNameCompareReverse()` sont utilisées pour générer des fonctions de
comparaison pour le tri de slices. La première fonction retourne une fonction qui compare deux personnes en fonction de
leur nom dans l'ordre croissant, tandis que la seconde fonction retourne une fonction de comparaison qui compare deux
personnes en fonction de leur nom dans l'ordre décroissant.

### Fonctions : citoyennes de première classe

En programmation, lorsque nous parlons de **citoyenneté de première classe**, nous faisons référence à des entités qui
peuvent être manipulées de toutes les façons dont les autres entités peuvent l'être. Si nous disons que les fonctions
sont des _citoyennes de première classe_ dans un langage de programmation comme Go, cela signifie que les fonctions
peuvent être utilisées comme n'importe quel autre type de données.

En Go, les fonctions sont des citoyens de première classe, car elles peuvent être utilisées de manière interchangeable
avec les autres types de données. Elles peuvent être affectées à des variables, passées en paramètre à d'autres
fonctions, retournées par d'autres fonctions, déclarées dans une fonction et même faire partie de structures de données
comme les tranches ou les maps.

Dans le code précédent, nous voyons des exemples de cela. Par exemple, les deux
fonctions `personNameCompare()` et `personNameCompareReverse()` retournent des fonctions. Ces fonctions retournées sont
ensuite utilisées comme arguments pour la fonction `sort.Slice()`. C'est un exemple de traitement des fonctions comme
des citoyens de première classe : dans ce cas, des fonctions sont retournées par d'autres fonctions et passées à
d'autres fonctions. C'est une caractéristique puissante de Go que l'on retrouve dans d'autres langages fonctionnels.

#### Fonctions lambda

Les fonctions lambda (aussi connues sous le nom de fonctions anonymes) en Go sont des fonctions qui ne sont définies
avec aucun nom et peuvent être utilisées pour créer des définitions de fonctions en ligne. Il s'agit de définir une
fonction juste où vous en avez besoin. En d'autres termes, une fonction lambda est une fonction déclarée sans nom
associé.

Les fonctions lambda sont très utiles pour réaliser des opérations simples et cela évite également d'encombrer le code
avec de multiples petites fonctions. Elles sont souvent utilisées comme arguments pour des fonctions qui prennent des
fonctions en paramètre.

Reprenons le code que nous avons discuté précédemment pour illustrer cela. Par exemple, lorsque nous trions la tranche
d'entiers `numbers1` ou la tranche `people` de structures `Person`, nous utilisons des fonctions lambda. Voici l'extrait
de code correspondant :

```go
sort.Slice(numbers1, func(i, j int) bool {
	return numbers1[i] < numbers1[j]
})

sort.Slice(people, func(i, j int) bool {
	return people[i].Age < people[j].Age
})
```

Dans chacun de ces appels à `sort.Slice()`, le deuxième argument est une fonction lambda qui est utilisée pour
déterminer l'ordre de tri. Ces fonctions sont définies sur place, et elles n'ont pas de nom. Ce sont des fonctions
lambda.

## Filtrer une tranche

Le code suivant fourni illustre plusieurs opérations de filtrage sur une tranche de structures `Person`.
Le but est de démontrer différentes manières de filtrer les données dans un tableau de `Person` selon différentes
conditions, notamment l'âge et les caractéristiques du nom.

```go
func section4b() {
	people := []Person{
		{Name: "John", Age: 30},
		{Name: "Jane", Age: 25},
		{Name: "Alice", Age: 35},
	}
	fmt.Println(people)

	// filtrer une tranche de struct Person
	// filtre : plus de 30 ans
	filteredPeople := make([]Person, 0)
	for _, person := range people {
		if person.Age > 30 {
			filteredPeople = append(filteredPeople, person)
		}
	}
	fmt.Println(filteredPeople)

	// filtrer une tranche de struct Person
	// filtre : nom commençant par "J"
	filteredPeople = make([]Person, 0)
	for _, person := range people {
		if person.Name[0] == 'J' {
			filteredPeople = append(filteredPeople, person)
		}
	}
	fmt.Println(filteredPeople)

	// filtrer une tranche de struct Person
	// filtre : nom contenant plus de 5 caractères
	filteredPeople = make([]Person, 0)
	for _, person := range people {
		if len(person.Name) > 5 {
			filteredPeople = append(filteredPeople, person)
		}
	}
	fmt.Println(filteredPeople)
}
```

Tout d'abord, une structure `Person` est déclarée avant la fonction principale :

```go
type Person struct {
	Name string
	Age  int
}
```

Cette structure représente une personne avec un nom (type `string`) et un âge (type `int`).

La fonction `section4b` crée d'abord un tableau de `Person` nommé `people` :

```go
people := []Person{
		{Name: "John", Age: 30},
		{Name: "Jane", Age: 25},
		{Name: "Alice", Age: 35},
}
```

Ensuite, il y a plusieurs sections de code qui filtrent ce tableau `people` en fonction de différentes conditions et
impriment le résultat.

Voici le premier filtre :

```go
filteredPeople := make([]Person, 0)
for _, person := range people {
	if person.Age > 30 {
		filteredPeople = append(filteredPeople, person)
	}
}
fmt.Println(filteredPeople)
```

Cette section filtre toutes les personnes qui ont plus de 30 ans. Le tableau `filteredPeople` est initialement vide.
Pour chaque `Person` dans le tableau `people`, si l'`Age` est supérieur à 30, cette `Person` est ajoutée au
tableau `filteredPeople`. Le résultat est ensuite imprimé.

Les deux sections suivantes suivent la même procédure, mais avec différentes conditions :

- La deuxième section de filtrage ajoute à `filteredPeople` toutes les `Person` dont le nom commence par la lettre 'J'.
- La troisième section de filtrage ajoute les personnes dont le nom comporte plus de 5 caractères.

### Version améliorée

En examinant le code précédent, il y a un patron répété de création d'une liste filtrée de personnes en fonction de
différentes conditions. C'est une bonne occasion d'appliquer la refonte **Extraire la fonction**. En extrayant la
logique répétée dans une fonction distincte qui prend une fonction de filtrage comme l'un de ses paramètres, vous pouvez
éliminer la répétition du code tout en rendant le code plus facile à comprendre et à modifier.

Le code qui suit est une réécriture de l'exemple précédent qui réduit la répétition.

```go
func section4c() {
    people := []Person{
        {Name: "John", Age: 30},
        {Name: "Jane", Age: 25},
        {Name: "Alice", Age: 35},
    }
    fmt.Println(people)

    filteredPeople := filterPeople(people, func(person Person) bool {
        return person.Age > 30
    })
    fmt.Println(filteredPeople)

    filteredPeople = filterPeople(people, func(person Person) bool {
        return person.Name[0] == 'J'
    })
    fmt.Println(filteredPeople)

    filteredPeople = filterPeople(people, func(person Person) bool {
        return len(person.Name) > 5
    })
    fmt.Println(filteredPeople)
}

type FilterFunc func(person Person) bool

func filterPeople(people []Person, filter FilterFunc) []Person {
    filteredPeople := make([]Person, 0)
    for _, person := range people {
        if filter(person) {
            filteredPeople = append(filteredPeople, person)
        }
    }
    return filteredPeople
}
```

`FilterFunc` est un nouveau type pour une fonction qui prend une `Person` et renvoie un `bool`, définissant si cette
personne doit être incluse dans les résultats filtrés.

Donc, dans chacun des appels à `filterPeople`, une nouvelle fonction anonyme est créée qui spécifie la logique de
filtrage pour cette liste particulière.

Cette refactorisation élimine la redondance dans votre code, et rend également le code plus facile à comprendre : au
lieu de devoir lire les spécificités de chaque boucle pour comprendre ce qui se passe, vous pouvez maintenant voir
facilement que chaque séquence filtre la liste `people` selon un critère distinct. De plus, si vous souhaitez ajouter
une nouvelle condition pour filtrer les personnes, vous avez simplement besoin d'ajouter un nouvel appel à
`filterPeople` avec la fonction de filtrage appropriée.

### Autre amélioration

Si désiré, à la place d'utiliser des fonctions de filtrage anonymes, les fonctions de filtrage pourraient être définies
un peu à la manière de la fonction `personNameCompareReverse` de l'exemple de tri précédent.

```go
func section4d() {
	people := []Person{
		{Name: "John", Age: 30},
		{Name: "Jane", Age: 25},
		{Name: "Alice", Age: 35},
	}
	fmt.Println(people)

	filteredPeople := filterPeople(people, peopleFilterMinAge(31))
	fmt.Println(filteredPeople)

}

func peopleFilterMinAge(minAge int) FilterFunc {
	return func(person Person) bool {
		return person.Age >= minAge
	}
}
```

Ce code poursuit la tendance de refactorisation et d'amélioration de la lisibilité en utilisant le filtrage des
personnes.

Commençons par examiner la nouvelle fonction `peopleFilterMinAge`.

Elle définit une fonction `peopleFilterMinAge` qui prend un âge minimum `minAge` comme argument et renvoie une fonction
de type `FilterFunc` qui, à son tour, accepte une personne et renvoie si l'âge de cette personne est supérieur ou égal
à `minAge`.

Nous pouvons voir le rendement de cette fonction utilisée dans un filtre dans la fonction `section4d` :

```go
filteredPeople := filterPeople(people, peopleFilterMinAge(31))
```

Ici, `peopleFilterMinAge(31)` renvoie une fonction qui sera utilisée comme critère de filtre par `filterPeople`. Elle
vérifiera si l'âge de chaque personne dans le tableau `people` est supérieur ou égale à 31. Si c'est le cas, cette
personne sera incluse dans la liste `filteredPeople`.

En résumé, `section4d` utilise une nouvelle fonction `peopleFilterMinAge` pour créer une fonction de filtre dynamique
basée sur un âge minimum, puis utilise cette fonction de filtre pour obtenir une liste de personnes dont l'âge est
supérieur ou égal à l'âge minimum. Ceci donne encore plus de flexibilité au code, car on peut maintenant facilement
créer différents filtres d'âge en appelant simplement `peopleFilterMinAge` avec l'âge minimum souhaité en paramètre. On
pourrait poursuivre avec des définitions similaires pour d'autres critères.



