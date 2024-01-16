# Section 4 : Validation des entrées

Étant donné que `Scanf` est un peu capricieuse, plusieurs personnes préfèrent utiliser des alternatives pour l'entrée de
valeurs. Les exemples de la section précédente utilisaient le _stdin_, mais les exemples seraient presque identiques si
on utilisait `Sscanf` ou `Fscanf`. Cette section inclut quelques alternatives. Le chapitre sur la gestion des fichiers
contiendra plus d'exemples.

## Lire une ligne complète jusqu'à la fin

Il est préférable d'utiliser un `Reader` sur le _stdin_ que d'utiliser `Scanf` pour lire une ligne complète, comme
démontré dans l'exemple suivant.

On commence par définir une fonction nommée `ReadLine`, qui lit une ligne de texte sur le _stdin_ et la retourne sans
inclure le `'\n'` à la fin de la ligne. La fonction retourne aussi une erreur s'il y en a une, ou sinon `nil` s'il n'y
en a pas. La fonction `section4a` plus loin démontre comment utiliser `ReadLine` en copiant l'exemple de la section
précédente.

```go
func ReadLine() (string, error) {
	scanner := bufio.NewScanner(os.Stdin)
	if scanner.Scan() {
		return scanner.Text(), nil
	} else {
		err := scanner.Err()
		if err != nil {
			return "", err
		} else {
			return "", io.EOF
		}
	}
}
```

La fonction `ReadLine` est une fonction utilitaire qui lit une ligne d'entrée de l'utilisateur depuis le terminal.
La fonction `bufio.NewScanner(os.Stdin)` initialise un nouveau scanner sur le `os.Stdin`, le flot d'entrée standard.
`scanner.Scan()` avance le scanner jusqu'à la prochaine ligne. Si `Scan` renvoie `false`, il a été impossible de lire
une line de texte, soit parce qu'il y a eu une erreur, soit parce qu'on a atteint la fin du fichier. Nous vérifions
alors si une erreur est survenue. Si c'est le cas, nous renvoyons l'erreur. Sinon, nous renvoyons le symbole
représentant la fin du fichier `io.EOF` (_End-Of-File_). Si tout s'est bien passé, `Scan` retourne `true`, et on
retourne la ligne de texte qui a été lue avec `scanner.Text()`.

`os.Stdin` est de type `*os.File`, donc `os.Stdin` est un pointeur vers un fichier. Un fichier très spécial, en lecture
seule, qui correspond à ce que l'utilisateur tape sur le terminal, mais du point de vue du système d'exploitation, c'est
tout de même un fichier. Si jamais le `os.Stdin` est fermé, ce qui ne devrait normalement pas arriver, le scanner
recevra un `io.EOF`, et il faut le gérer correctement. Le chapitre 4 sur la gestion des fichiers décrira le `io.EOF`
plus en détails.

Ensuite, la fonction `section4a` :

```go
func section4a() {
    fmt.Print("Veuillez entrer votre nom : ")
    name, err := ReadLine()

    if err != nil {
        fmt.Println(err)
    }

    fmt.Print("Veuillez entrer votre âge : ")
    ageStr, err := ReadLine()

    if err != nil {
        fmt.Println(err)
    }

    age, err := strconv.Atoi(ageStr)

    if err != nil {
        fmt.Println(err)
    }

    fmt.Printf("Bonjour %s, vous avez %d ans.\n", name, age)
}
```

Cette fonction fait les choses suivantes :

1. Demande à l'utilisateur de saisir son nom en utilisant `fmt.Print`.
2. Lit le nom entré en utilisant `ReadLine`.
3. Si une erreur se produit lors de la lecture, imprime l'erreur.
4. Demande à l'utilisateur d'entrer son âge.
5. Lit l'âge entré, à nouveau en utilisant `ReadLine`.
6. Si une erreur se produit pendant la lecture, imprime l'erreur.
7. Convertit l'âge entré (qui est une chaîne de caractères) en un nombre entier à l'aide de `strconv.Atoi`.
8. Si la conversion provoque une erreur, imprime cette erreur.
9. Enfin, salue l'utilisateur en utilisant `fmt.Printf`, en insérant le nom et l'âge qu'ils ont entrés dans la chaîne.

**Note** : Gérez toujours correctement les erreurs dans votre code. Dans ce cas, après une erreur, la fonction continue
l'exécution même si les données sont peut-être incorrectes ou absentes. Il est généralement conseillé de renvoyer ou de
traiter les erreurs de manière à ce qu'elles ne causent pas de problèmes imprévus plus tard dans l'exécution du
programme.

Une façon différente de gérer les erreurs lors de l'entrée d'un nombre entier suit après les exemples d'exécution.

### Exécution 1

Avec des entrées sans erreurs, la sortie sera la même.

```
Veuillez entrer votre nom : Denis
Veuillez entrer votre âge : 22
Bonjour Denis, vous avez 22 ans.
```

### Exécution 2

Avec des entrées vides, il n'y aura pas d'erreur pour le nom parce que la chaîne de caractères vide est une chaîne
valide. Par contre, une chaîne vide ne peut pas être convertie en nombre entier, donc il y a une erreur pour l'âge.

```
Veuillez entrer votre nom : 
Veuillez entrer votre âge : 
strconv.Atoi: parsing "": invalid syntax
Bonjour , vous avez 0 ans.
```

On pourrait définir une autre version de `ReadLine`, qui n'accepterait pas les chaînes vides, qu'on pourrait nommer par
exemple `ReadLineNonEmpty` ou `ReadNonEmptyLine`.

## Lire un entier

La fonction `ReadInt` utilise la fonction `ReadLine` de la section précédente et s'assure que l'entrée est vraiment un
nombre entier. Elle retourne une erreur seulement si le nombre d'essais maximum a été atteint. Il serait possible de
faire à peu près la même chose, mais en utilisant `Scanf` à la place. Le comportement ne serait pas nécessairement le
même dans tous les cas parce que `Scanf` ne lit pas automatiquement jusqu'à la fin de la ligne, comme `ReadLine` le
fait.

```go
func ReadInt(nAttempts int) (int, error) {
	for i := 0; i < nAttempts; i++ {
		inputStr, err := ReadLine()
		if err != nil {
			fmt.Println("Erreur lors de la lecture. Essayez de nouveau.")
		} else {
			inputNum, err := strconv.Atoi(inputStr)
			if err != nil {
				fmt.Println("Ceci n'est pas un nombre entier. Essayez de nouveau.")
			} else {
				return inputNum, nil
			}
		}
	}
	return 0, fmt.Errorf("échec : nombre limite de %d essai(s) atteint", nAttempts)
}
```

Cette fonction tente de lire un entier depuis l'entrée standard (`stdin`). Le nombre de tentatives permises pour que
l'utilisateur fournisse un nombre entier valide est défini par le paramètre `nAttempts`.

Voici une interprétation pas à pas :

1. La fonction initialise une boucle `for` qui continue pendant le nombre spécifié de tentatives (`nAttempts`).

2. À l'intérieur de la boucle, elle appelle la fonction `ReadLine()` pour lire une ligne à partir de l'entrée standard.
   ```go
   inputStr, err := ReadLine()
   ```
   `ReadLine` est la même fonction que dans l'exemple précédent.

3. Si une erreur s'est produite lors de la lecture de la ligne (par exemple, si une erreur d'E/S s'est produite), la
   fonction affiche un message d'erreur et passe à la prochaine itération.
    ```
    fmt.Println("Erreur lors de la lecture. Essayez de nouveau.")
    ```

4. Si aucune erreur ne s'est produite lors de la lecture, elle tente de convertir la ligne en entier à l'aide
   de `strconv.Atoi`.
    ```go
    inputNum, err := strconv.Atoi(inputStr)
    ```

5. Si une erreur s'est produite lors de la conversion (par exemple, si la ligne n'est pas un entier valide), la fonction
   imprime un message d'erreur informatif et passe à la prochaine itération.
    ```
    fmt.Println("Ceci n'est pas un nombre entier. Essayez de nouveau.")
    ```

6. Si la conversion a réussi, la fonction retourne l'entier et une erreur `nil`.
    ```go
    return inputNum, nil
    ```

7. Si la fonction n'a pas pu lire un entier valide après `nAttempts` tentatives, elle renvoie `0` et un message d'erreur
   indiquant que la limite des tentatives a été atteinte.
    ```go
    return 0, fmt.Errorf("échec : nombre limite de %d essai(s) atteint", nAttempts)
    ```

Donc, cette fonction essaie de lire un ensemble de caractères depuis l'entrée standard, convertit cet ensemble de
caractères en un entier, et le renvoie. Si elle n'est pas en mesure de lire l'entrée correctement, ou si les caractères
lus ne représentent pas un entier, elle renvoie une erreur dès que le nombre limite d'essais a été atteint.


