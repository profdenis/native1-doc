# Section 3 : Sérialisation des structures

La sérialisation est le processus de conversion des structures de données ou des objets en un format qui peut
être stocké (par exemple, dans un fichier ou une mémoire tampon), ou transmis (par exemple, à travers un réseau) et
reconstruit plus tard (peut-être dans un environnement différent).

Pour une structure Go, la sérialisation signifie la conversion de l'instance de la struct en un format qui peut être
facilement stocké ou transmis. Le format cible peut être JSON, XML, texte brut ou tout autre format de données
utilisable.

Reprenons l'exemple de la structure `Person` :

```go
type Person struct{
    Name string
    Age int
}
```

Et une instance de cette struct :

```go
p := Person{Name: "Alice", Age:25}
```

La sérialisation de cette instance de struct (dans le format JSON) transforme la struct en une chaîne JSON :

```json
{
  "Name": "Alice",
  "Age": 25
}
```

On peut ensuite enregistrer cette chaîne JSON dans un fichier, ou l'envoyer à travers un réseau, etc. Lorsque nous avons
besoin d'utiliser ces données à nouveau, nous désérialisons la chaîne JSON en une instance de la struct `Person`.
Cela permet de sauvegarder l'état de l'objet et de le restaurer plus tard, ou de le transmettre à un autre emplacement
ou un autre système.

Dans certaines situations, selon les conventions utilisées, il peut être préférable de nommer les clés en JSON, qui
correspondent aux noms des champs en Go, en utilisant le _camelCase_. Pour ce faire, on peut placer des étiquettes
(_tags_) dans la déclaration des structures, qui seront utilisées dans le processus de conversion (voir les exemples
plus bas). La déclaration de la structure `Person` deviendra :

```go
type Person struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}
```

La création des instances ne sera pas affectée, mais le résultat de la sérialisation donnera du JSON avec les noms
spécifiés dans les étiquettes.

```json
{
  "name": "Alice",
  "age": 25
}
```

## Exemple complet

Voici un exemple complet de sérialisation et désérialisation d'une structure vers/à partir une chaîne JSON.

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Person struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func section3a() {
	// Créer une instance de Person
	p := Person{Name: "John", Age: 30}

	// Séraliser la struct en JSON
	jsonData, err := json.Marshal(p)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(string(jsonData)) // Affiche : {"name":"John","age":30}

	// Désérialiser le JSON en struct
	jsonStr := `{"name":"James","age":40}`
	var p2 Person
	err = json.Unmarshal([]byte(jsonStr), &p2)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(p2) // Affiche : {James 40}
}
```

Sortie :

```go
{"name":"John","age":30}
{James 40}
```

1. Une instance de la structure `Person` est créée avec le nom "John" et l'âge 30.

   ```go
   p := Person{Name: "John", Age: 30}
   ```

2. Cette instance est ensuite sérialisée au format JSON à l'aide de la fonction `json.Marshal`. S'il y a une erreur lors
   de ce processus, elle est imprimée et la fonction retourne immédiatement.

    ```go
    jsonData, err := json.Marshal(p)
    if err != nil {
		fmt.Println(err)
		return
	}
    fmt.Println(string(jsonData)) // Imprime: {"name":"John","age":30}
    ```

3. Ensuite, une chaîne JSON représentant une autre personne (_nom : "James", âge : 40_) est désérialisée en une instance
   de la structure `Person` en utilisant la fonction `json.Unmarshal`. Là encore, s'il y a une erreur lors de ce
   processus, elle est imprimée et la fonction retourne immédiatement.

    ```go
    jsonStr := `{"name":"James","age":40}`
	var p2 Person
	err = json.Unmarshal([]byte(jsonStr), &p2)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(p2) // Imprime: {James 40}
    ```

La structure `Person` et ces opérations (sérialisation et désérialisation en JSON) sont prédominantes dans les scénarios
où les données doivent être stockées, transférées ou traitées. JSON (JavaScript Object Notation) est un format populaire
à ces fins en raison de sa simplicité et de sa compatibilité avec de nombreux langages, y compris Go.

## Exemple d'écriture d'une structure dans un fichier

```go
import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"os"
)

type Person struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func section3b() {
	// Créer une instance de Person
	p := Person{Name: "John", Age: 30}

	// Séraliser la struct en JSON
	jsonData, err := json.Marshal(p)
	if err != nil {
		fmt.Println(err)
		return
	}

	// Ouvrir le fichier en mode écriture
	file, err := os.OpenFile("person.json", os.O_CREATE|os.O_WRONLY, 0644)
	if err != nil {
		fmt.Println(err)
		return
	}
	defer file.Close()

	// Écrire les données JSON dans le fichier
	_, err = file.Write(jsonData)
	if err != nil {
		fmt.Println(err)
		return
	}
	file.Sync()

	// Lire les données du fichier
	data, err := os.ReadFile("person.json")
	if err != nil {
		fmt.Println(err)
		return
	}

	// Désérialiser les données JSON pour recréer une instance de Person
	var p2 Person
	err = json.Unmarshal(data, &p2)
	if err != nil {
		fmt.Println(err)
		return
	}

	// Afficher l'instance de Person
	fmt.Println(p2) // Devrait afficher: {John 30}
}
```

Dans la fonction `section3b` :

1. On commence par créer une instance `Person` avec le nom "John" et l'âge 30, que l'on sérialise en JSON.
2. Ensuite, on ouvre un fichier appelé "person.json" en mode écriture. Si le fichier n'existe pas, il sera
   créé (`os.O_CREATE`). Si le fichier existe déjà, son contenu sera écrasé (`os.O_WRONLY`). Le chiffre `0644` est une
   valeur octale représentant les permissions du fichier.
3. On écrit ensuite les données JSON dans le fichier.
4. Après cela, on lit les données du fichier dans un `byte` slice.
5. Enfin, on désérialise les données JSON pour recréer une instance `Person` et on l'affiche.

**Note** : Assurez-vous de gérer vos erreurs correctement dans un véritable environnement de production. Ici, nous nous
contentons d'imprimer les erreurs, mais en réalité, vous devriez les gérer d'une manière qui convient à votre
application. Aussi, n'oubliez pas de fermer le fichier après l'avoir utilisé pour libérer les ressources système.

## Exemple d'écriture d'une tranche de structures dans un fichier

D'accord, pour écrire et lire une tranche de Person, vous pouvez suivre un schéma similaire. Voici comment :

```go
import (
	"encoding/json"
	"fmt"
	"os"
)

type Person struct {
	Name string `json:"name"`
	Age  int	   `json:"age"`
}

func section3c() {
	// Créer une tranche de Person
	people := []Person{
		{Name: "John", Age: 30},
		{Name: "Jane", Age: 25},
		{Name: "Alice", Age: 35},
	}

	// Séraliser la slice en JSON
	jsonData, err := json.MarshalIndent(people, "", "\t")
	if err != nil {
		fmt.Println(err)
		return
	}

	// Ouvrir le fichier en mode écriture
	file, err := os.OpenFile("people.json", os.O_CREATE|os.O_TRUNC|os.O_WRONLY, 0644)
	if err != nil {
		fmt.Println(err)
		return
	}
	defer file.Close()

	// Écrire les données JSON dans le fichier
	_, err = file.Write(jsonData)
	if err != nil {
		fmt.Println(err)
		return
	}
	file.Sync()

	// Lire les données du fichier
	data, err := os.ReadFile("people.json")
	if err != nil {
		fmt.Println(err)
		return
	}

	// Désérialiser les données JSON pour recréer une slice de Person
	var people2 []Person
	err = json.Unmarshal(data, &people2)
	if err != nil {
		fmt.Println(err)
		return
	}

	// Afficher la slice de Person
	fmt.Println(people2) // Devrait afficher : [{John 30} {Jane 25} {Alice 35}]
}
```

Dans la fonction `section3c`:

1. On commence par créer une tranche de `Person` avec trois personnes, que l'on sérialise en JSON.
    - Dans ce cas-ci, on utilise `json.MarshalIndent` pour faciliter la lecture de la chaîne produite par un humain.
    - On donne la chaîne vide come préfixe, et le caractère de tabulation pour le caractère d'indentation. On pourrait
      utiliser des espaces à la place, comme par exemple `"    "`.
2. On ouvre ensuite un fichier appelé "people.json" en mode écriture. Si le fichier n'existe pas, il est
   créé (`os.O_CREATE`). Si le fichier existe déjà, son contenu est écrasé (`os.O_WRONLY`).
3. On y écrit ensuite les données JSON.
4. On lit ensuite les données du fichier dans une tranche de `byte`.
5. Enfin, on désérialise ces données JSON pour créer une nouvelle tranche de `Person` et on l'affiche.

S'il y a une erreur à chaque étape, celle-ci est affichée et la fonction retourne immédiatement.

Contenu du fichier `people.json` :

```json
[
  {
    "name": "John",
    "age": 30
  },
  {
    "name": "Jane",
    "age": 25
  },
  {
    "name": "Alice",
    "age": 35
  }
]
```
