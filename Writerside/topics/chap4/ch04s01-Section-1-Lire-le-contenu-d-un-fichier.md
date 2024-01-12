# Section 1 : Lire le contenu d&apos;un fichier

La façon la plus simple, mais pas nécessairement la plus efficace, de lire le contenu d'un fichier est d'utiliser la
fonction `os.ReadFile`. Cette fonction ouvre le fichier, lit son contenu dans une chaîne de caractères, ferme le
fichier, et retourne le contenu. Si le fichier est gros, les performances ne seront pas très bonnes parce que tout le
fichier sera lu et placé dans une seule chaîne.

## Exemple simple avec `os.ReadFile`

```Go
package main

import (
    "fmt"
    "os"
    "log"
)

func section1a() {
	content, err := os.ReadFile("allo.txt") // Lecture du contenu du fichier
	if err != nil {
		log.Fatalf("lecture du contenu du fichier impossible : %s", err)
	}

	fmt.Println("Contenu du fichier :")
	fmt.Println(string(content))
}
```

Si le fichier existe, la sortie sera `Contenu du fichier :` sur la première ligne, suivi du contenu du fichier sur les
lignes suivantes.

Si le fichier n'existe pas, alors il y aura une erreur, `err` ne sera pas `nil`, et le message d'erreur
`lecture du contenu du fichier impossible :` sera affiché précédé de la date et l'heure, et suivi de l'erreur elle-même.
La date et l'heure viennent de la fonction `log.Fatalf`, qui ajoute ces informations automatiquement. Aussi, cette
fonction mettra un terme à l'exécution du programme parce qu'on utilise le niveau de _log_ **Fatal**, qui veut dire
qu'on ne peut pas continuer. Le `f` à la fin de la fonction est similaire au `f` à la fin de `fmt.Printf`, ce qui veut
dire que la fonction accepte les formats pour la sortie.

Voir [Paquet log dans la doc officielle](https://pkg.go.dev/log#Fatalf) pour plus de détails sur le paquet `log`.

L'erreur la plus commune sera une erreur d'ouverture du fichier, similaire
à `open allo.txt: no such file or directory`. Il faut que le fichier soit dans le même dossier que le programme compilé
en fichier exécutable (extension `.exe` sous Windows). On peut également spécifier un chemin relatif ou absolu avec un
ou plusieurs dossiers séparés par des barres obliques.

[//]: # (TODO: dossier par défaut ? le dossier du projet ?)

Il est probablement plus facile de compiler le programme à partir du terminal dans ce cas-ci (avec `go build` quand on
est situé dans le dossier contenant le code à compiler), et d'exécuter le programme
dans le terminal pour être certain de pouvoir trouver le fichier. Par défaut dans GoLand, si on clique sur le triangle
vert pour exécuter le programme, il sera compilé dans un dossier temporaire et exécuté dans ce dossier, donc le
programme ne trouvera le fichier `allo.txt` même s'il est dans le même dossier que les fichiers `chap4.go`
et `section1.go`. Il faut que le fichier `allo.txt` soit dans le même dossier d'où on exécute le programme.

## Même exemple, mais avec ouverture et fermeture de fichier explicites

```Go
func section1b() {
	file, err := os.Open("allo.txt") // Ouverture du fichier en mode lecture
	if err != nil {
		log.Fatalf("ouverture du fichier impossible : %s", err)
	}

	content, err := io.ReadAll(file) // Lecture du contenu du fichier
	if err != nil {
		log.Fatalf("lecture du contenu du fichier impossible : %s", err)
	}

	fmt.Println("Contenu du fichier :")
	fmt.Println(string(content))

	err = file.Close()
	if err != nil {
		log.Printf("erreur lors de la fermeture du fichier : %s", err)
	}
}
```

Cet exemple a le même problème que le précédent si le fichier lu est gros. Le but ici est démontrer comment on peut
ouvrir et fermer un fichier explicitement. Il existe une autre façon de fermer un fichier en Go, avec l'utilisation
de `defer`, mais pour l'instant, nous le faisons de la manière longue.

1. La fonction commence par ouvrir le fichier `allo.txt` en mode lecture avec `os.Open`. Si une erreur se produit (par
   exemple, si le fichier ne peut pas être trouvé), un message d'erreur sera affiché et le programme s'arrêtera
   avec `log.Fatalf`.

2. Ensuite, la fonction lit le contenu du fichier avec `io.ReadAll`. Encore une fois, si une erreur se produit pendant
   la lecture (par exemple, si les données ne peuvent pas être interprétées correctement), un message d'erreur sera
   affiché et le programme s'arrêtera.

3. Le contenu du fichier (qui est maintenant en mémoire sous forme de tableau de bytes) est converti en chaîne de
   caractères avec `string(content)` et est ensuite affiché dans la console avec `fmt.Println`.

4. Enfin, la fonction essaie de fermer le fichier avec `file.Close()`. Si une erreur se produit lors de la fermeture du
   fichier (ce qui est rare, mais peut arriver si par exemple l'espace disque est plein et que le système ne peut pas
   mettre à jour les métadonnées du fichier), un message d'erreur sera affiché, mais contrairement aux erreurs
   précédentes, le programme ne s'arrêtera pas dans ce cas.

## Exemple : Compter le nombre de lignes non-vides dans un fichier

Voici une fonction qui accepte le nom d'un fichier en paramètre, et qui compte le nombre de lignes non-vides dans ce
fichier. Le nom de fichier peut inclure un chemin relatif ou absolu.

```go
func countNonEmptyLines(filename string) {
	file, err := os.Open(filename) // Ouverture du fichier en mode lecture
	if err != nil {
		log.Fatalf("ouverture du fichier impossible : %s", err)
	}

	count := 0
	scanner := bufio.NewScanner(file)
	for scanner.Scan() {
		if len(scanner.Text()) > 0 {
			count++
		}
	}

	err = scanner.Err()
	if err != nil {
		log.Printf("erreur lors de la lecture du fichier : %s", err)
	}

	fmt.Println("Nombre de ligne non-vide :", count)

	err = file.Close()
	if err != nil {
		log.Printf("erreur lors de la fermeture du fichier : %s", err)
	}
}
```

1. La fonction `countNonEmptyLines` est définie, qui prend un nom de fichier en paramètre.
2. La fonction essaie d'abord d'ouvrir le fichier en lecture. S'il y a une erreur (comme le fichier n'existe pas ou
   manque les permissions nécessaires), l'erreur est enregistrée et l'exécution du programme se termine.
3. Une instance de `bufio.Scanner`, `scanner` pour le fichier ouvert est créée pour lire le fichier.
4. Le fichier est lu ligne par ligne avec une boucle `for`. Si une ligne n'est pas vide (elle contient des caractères),
   le compteur est incrémenté.
5. Après que les lignes ont été analysées, il vérifie s'il y a des erreurs qui se sont produites lors de la lecture du
   fichier. Si une erreur s'est produite, elle enregistre le message d'erreur.
6. Le nombre de lignes non vides trouvées dans le fichier est imprimé.
7. Enfin, le fichier est fermé et s'il y a une erreur lors de la fermeture du fichier, il enregistre le message
   d'erreur.

