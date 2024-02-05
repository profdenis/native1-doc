# Chapitre 5 : Les structures

## Exemple de base

En Go, une `struct` (structure) est une collection personnalisée de types de données. Elle permet de combiner des
éléments de différents types dans une seule structure de données. L'exemple qui suit démontre comment les structs
peuvent être utilisées pour créer et gérer des instances d'une personne.

Voici un exemple minimal d'utilisation d'une `struct` appelée `Person` qui contient les champs `Name` et `Age`:

```go
package main

import "fmt"

type Person struct {
	Name string
	Age  int
}

func main() {
    // Initialiser une nouvelle instance de Person
	person1 := Person{Name: "John Doe", Age: 30}

	// Accéder aux champs de la struct
	fmt.Println(person1.Name) // John Doe
	fmt.Println(person1.Age)  // 30

	// Modifier les champs de la struct
	person1.Name = "Jane Doe"
	person1.Age = 28

	fmt.Println(person1.Name) // Jane Doe
	fmt.Println(person1.Age)  // 28
	
	fmt.Println(person1)      // {Jane Doe 28}
}
```

1. Nous définissons un nouveau type appelé `Person` qui est une struct contenant deux champs : `Name` de type `string`
   et `Age` de type `int`.
2. Dans la fonction `main`, nous créons une nouvelle instance de `Person` et l'initialisons avec des valeurs pour `Name`
   et `Age`.
3. Nous utilisons l'opérateur `.` pour accéder aux champs de notre instance de `Person`, puis nous imprimons ces valeurs
   avec `fmt.Println`.
4. Enfin, nous utilisons l'opérateur '.' pour modifier les valeurs des champs `Name` et `Age` dans notre instance
   de `Person`.
5. Nous pouvons aussi imprimer une structure directement dans un `fmt.Println`, mais la sortie peut être ambiguë, comme
   le montre la dernière sortie dans l'exemple : à partir d'uniquement du résultat de la sortie, il n'est pas clair s'il
   y a 2 ou 3 champs dans la structure.

Puisque les champs `Name` et `Age` commencent par des majuscules, ils sont publics. S'ils commençaient par une
minuscule, ils seraient privés, donc accessibles seulement au code situé dans le même paquet (`package`) que sa
déclaration. En Go, l'accessibilité (public vs. privé) est toujours définie au niveau du paquet.

## Méthodes

## Tranches



