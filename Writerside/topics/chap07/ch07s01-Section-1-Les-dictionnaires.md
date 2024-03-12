# Section 1 : Les dictionnaires (map)

Dans les sciences de l'informatique, un **dictionnaire**, également connu sous le nom de _map_ et de _tableau
associatif_, est une structure de données qui stocke les valeurs en paires de clés et de valeurs. Chaque clé unique est
associée à une valeur, ce qui permet de récupérer efficacement la valeur à l'aide de la clé correspondante.

L'un des principaux avantages d'un dictionnaire est qu'il permet un accès rapide à ses éléments. Dans de nombreux cas,
l'accès, l'insertion et la suppression d'un élément sont tous effectués en temps constant, indépendamment de la taille
du dictionnaire. Cela contraste avec des structures de données telles que les listes ou les tableaux, où le temps
nécessaire pour trouver un élément peut augmenter à mesure que la taille de la structure de données augmente.

Dans les dictionnaires, la clé joue un rôle significatif, car elle est utilisée pour accéder à la valeur correspondante,
et chaque clé est unique. Cela signifie que si vous essayez d'insérer une nouvelle clé qui existe déjà dans le
dictionnaire, la valeur existante de cette clé sera remplacée par la nouvelle valeur.

Les dictionnaires sont utilisés dans un grand nombre d'applications. Par exemple, ils sont souvent utilisés pour
implémenter la fonctionnalité de recherche rapide dans les bases de données, où la clé serait un identifiant unique et
la valeur serait l'enregistrement associé à cet identifiant. D'autres utilisations courantes des dictionnaires incluent
la détection de duplications, le comptage d'occurrences d'éléments, ou le stockage de configuration et de métadonnées.

Il est important de noter que les dictionnaires, en raison de leur fonctionnement interne, _ne maintiennent pas d'ordre
spécifique des éléments_ stockés, sauf dans des certaines implémentations spécifiques, comme le `TreeMap` de Java. Par
conséquent, si vous avez besoin d'une structure de données ordonnée, d'autres options, comme les listes triées ou les
arbres, pourraient être plus appropriées.

En résumé, les dictionnaires ou les _maps_ sont des structures de données associatives puissantes et flexibles qui
opèrent sur des paires de clés et de valeurs, offrant un accès rapide et des opérations efficaces sur les données.

## Les _maps_ en Go

Les maps sont une structure de données fondamentale dans Go, souvent utilisées lorsque vous souhaitez stocker des
associations clé/valeur, similaires aux dictionnaires en Python, au `HashMap` en Java et aux objets en JavaScript.

Voici comment déclarer une map en Go :

```go
var myMap map[string]int
```

Ceci déclare une map appelée `myMap` qui a des clés de type `string` et des valeurs de type `int`.

Veuillez noter que déclarer une map de cette manière l'initialise à `nil`. Pour assigner une nouvelle map vide
à `myMap`, nous devons utiliser la fonction `make` :

```go
myMap = make(map[string]int)
```

On peut déclarer et initialiser une map en une seule déclaration :

```go
myMap := make(map[string]int)
```

Pour ajouter des valeurs à notre map :

```go
myMap["One"] = 1
myMap["Two"] = 2
myMap["Three"] = 3
```

Pour récupérer une valeur de la map :

```go
value := myMap["Two"]  // value est maintenant égal à 2
```

Si vous essayez d'accéder à une clé qui n'existe pas dans la map, Go retournera la valeur zéro pour le type de valeur de
la map. Pour un int, cela donnerait 0.

Pour vérifier si une clé existe dans la map, vous pouvez utiliser la déclaration `if` :

```go
val, exists := myMap["Four"]
if exists {
    fmt.Println("La valeur existe et est", val)
} else {
    fmt.Println("La valeur n'existe pas")
}
```

Pour supprimer une entrée de la map, utilisez la fonction `delete` :

```go
delete(myMap, "Two")
```

Cela supprime l'entrée avec la clé "Two" de la map. Si la clé n'existe pas dans la map, `delete` ne fait rien et le
programme continue à fonctionner.

Enfin, pour parcourir une map, vous pouvez utiliser une boucle `for` :

```go
for key, value := range myMap {
    fmt.Printf("Clé : %s, Valeur : %d\n", key, value)
}
```

Ici, `range` sur une map retourne deux valeurs à chaque itération, la clé et la valeur.

## Exemple 1

```go
var menuStrings = make(map[string]string)

func ShowMenu(name string) {

	if _, ok := menuStrings[name]; !ok {
		menuBytes, err := os.ReadFile("menu/" + name + ".txt")
		if err != nil {
			log.Fatalf("lecture du contenu du menu impossible : %s", err)
		}
		menuStrings[name] = string(menuBytes)
	}
	fmt.Println(menuStrings[name])
}
```

- Tout d'abord, une variable globale `menuStrings` est déclarée qui est une map avec des clés `string` et des
  valeurs `string`. Cette variable est initialisée avec une map vide à l'aide de la fonction `make`.

- Ensuite, une fonction `ShowMenu` est définie, qui prend un paramètre `name` de type `string`.

- Dans cette fonction, il y a une vérification pour voir si une clé correspondant à `name` existe déjà dans la
  map `menuStrings`. La syntaxe `_, ok := menuStrings[name]` effectue une recherche de la clé dans la map et renvoie
  deux valeurs : la valeur du dictionnaire à la clé, et un booléen indiquant si la clé a été trouvée dans la map. Le
  symbole `_` est un identifiant vide en Go, qui est utilisé lorsque vous voulez ignorer une valeur retournée. Ici, nous
  ignorons la valeur renvoyée par la map et nous nous intéressons seulement à la valeur booléenne `ok`.

- Si la clé n'existe pas dans `menuStrings` (c'est-à-dire que `ok` est `false`), alors le programme tente de lire un
  fichier texte du répertoire `menu/` ayant le nom dans la variable`name` avec l'extension `.txt`. L'opération de
  lecture de fichier peut échouer pour diverses raisons (par exemple si le fichier n'existe pas), et dans ce cas une
  erreur `err` est renvoyée.

- Si une erreur est produite lors de la lecture du fichier, un message d'erreur est affiché et le programme se termine
  avec `log.Fatalf`. Sinon, le contenu du fichier, qui est un tableau de bytes (`menuBytes`), est converti en `string`
  et stocké dans la map `menuStrings` avec la clé étant `name`.

- Enfin, que la valeur ait été lue à partir du fichier ou qu'elle existait déjà dans la map, la fonction `fmt.Println`
  est utilisée pour afficher la chaîne correspondante à la clé `name` de la map `menuStrings`.

En somme, ce petit programme effectue une opération de mise en cache des menus basée sur les noms de fichier. Chaque
fois que vous demandez un menu par son nom, le programme vérifie d'abord s'il a déjà lu le fichier du disque. Si c'est
le cas, il utilise la version en cache. Sinon, il lit le fichier du disque, l'ajoute au cache, et l'utilise.

## Exemple 2

```go
package main

import "fmt"

type Person struct {
	Id   int
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("ID: %d, Nom : %s, Âge : %d\n", p.Id, p.Name, p.Age)
}

func main() {
	// Création des personnes
	person1 := &Person{Id: 1, Name: "John Doe", Age: 30}
	person2 := &Person{Id: 2, Name: "Jane Doe", Age: 28}
	person3 := &Person{Id: 3, Name: "Richard Roe", Age: 33}
	person4 := &Person{Id: 4, Name: "Maggie Roe", Age: 32}
	
	// Création de la map pour stocker les personnes par leur ID
	persons := make(map[int]*Person)

	// Ajout des personnes dans la map
	persons[person1.Id] = person1
	persons[person2.Id] = person2
	persons[person3.Id] = person3
	persons[person4.Id] = person4
	
	// Affichage des personnes de la map
	for _, person := range persons {
		fmt.Print(person)
	}
}
```

Dans cet exemple, quatre instances de `Person` sont créées (elles sont allouées avec `&` pour retourner un pointeur vers
l'instance). Ensuite une map appelée `persons` est créée avec `make(map[int]*Person)`. Cette map associe des entiers
(les identifiants des personnes) à des pointeurs de `Person`.

Chaque personne est ajoutée à la map avec son `ID` comme clé et elle-même comme
valeur (`persons[person1.Id] = person1`).

Enfin, une boucle `for` est utilisée pour parcourir la map `persons` et chaque personne est affichée avec son ID et ses
détails. 