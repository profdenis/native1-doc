# Section 3 : Les différentes façons d'écrire des valeurs

En plus des fonctions `Print`, `Println` et `Printf` qui sont utilisées pour écrire sur le _stdout_, le paquet `fmt`
contient des fonctions pour écrire dans une chaîne de caractères (`Sprint`, `Sprintln`, `Sprintf`) ou dans un
fichier (`Fprint`, `Fprintln`, `Fprintf`).

- Les fonctions se terminant par *ln* ajoutent un changement de ligne (`\n`) à la fin de la chaîne de caractères à
  écrire.
- Les fonctions se terminant par un *f* utilisent une syntaxe spéciale pour **formatter** les chaînes de caractères.
- Les fonctions commençant avec un *S* écrivent dans une chaîne de caractères (*S* pour *string*)
- Les fonctions commençant avec un *F* écrivent dans un fichier (*F* pour *file*)
    - Les fonctions sur les fichiers seront présentées plus loin, dans le chapitre consacré à la gestion des fichiers.

## Écrire dans une chaîne de caractères

```go
package main

import "fmt"

func print1() {
	year := 2024
	output := fmt.Sprintln("année =", year)
	fmt.Println(output)
}
```

Dans cet exemple simplifié, il serait préférable d'utiliser `Println` directement sur les deux valeurs à
écrire (`fmt.Pprintln("année =", year)`), mais dans un exemple plus complexe, lorsqu'il est nécessaire d'écrire la même
chaîne de caractères à plusieurs reprises, possiblement à des endroits différents (_stdout_, fichier, ...), écrire dans
une chaîne en premier peut simplifier le code plus loin dans le programme, surtout si on a besoin de formatter les
valeurs de façon particulière.

## Écrire selon un format

Le même principe s'applique aux fonctions `Printf`, `Sprintf` et `Fprintf` : une chaîne de caractères décrivant le
format doit être donnée comme premier argument aux fonctions, suivie d'une ou plusieurs valeurs à formatter.

### Nombres entiers

```go
package main

import "fmt"

func print2() {
	year := 2024
	month := 1
	day := 5
	
	fmt.Printf("année = %d\n", year)
	fmt.Printf("%d-%d-%d\n", year, month, day)
	fmt.Printf("%4d-%2d-%2d\n", year, month, day)
	fmt.Printf("%4d-%02d-%02d\n", year, month, day)
}
```

Sortie :

```
année = 2024
2024-1-5
2024- 1- 5
2024-01-05
```

La chaîne de caractères décrivant le format (_chaîne de format_, ou _format string_ en anglais) doit inclure au moins
une entrée débutant par le caractère `%`, suivi par la description du format. Dans l'exemple précédent, il y a 4
sorties.
Dans la première, la sortie sera identique à l'exemple précédent. Le `%d` va être remplacé par la valeur de la
variable `year`. Le `d` indique que la valeur sera un nombre entier en base 10 (_décimal_). On pourrait utiliser

- `%x` pour un nombre entier en base 16 (_hexadécimal_)
- `%o` pour un nombre entier en base 8 (_octal_)
- `%b` pour un nombre entier en base 2 (_binaire_)

Dans la deuxième sortie, il y a 3 formats à l'intérieur de la chaîne de format, et ils sont séparés par des tirets qui
ne font pas partie des formats, mais qui feront partie de la sortie. Le problème est que la sortie n'est pas très belle
parce que les valeurs pour le mois et la journée sont plus petites que 10, alors qu'habituellement, on réserve 2
emplacements (ou 2 colonnes) pour ces valeurs même si elles n'en n'ont pas besoin.

On peut remplacer le deuxième format par celui-ci : `"%4d-%2d-%2d\n"`. La sortie sera alors `2024- 1- 5`, ce qui n'est
pas vraiment mieux. On spécifie que l'on veut avoir 4 colonnes pour l'année, 2 colonnes pour le mois et aussi 2 colonnes
pour le jour, mais par défaut, les colonnes libres sont remplies avec des espaces.

Si on remplace le deuxième format par celui-ci : `"%4d-%02d-%02d"`, alors les colonnes libres seront remplies par des
zéros, ce qui correspond au format habituel `2024-01-05`.

Cet exemple montre comment formatter une date dans un format précis, mais il existe plusieurs autres formats possibles.
Formatter des dates est un besoin commun, qui doit supporter des formats différents selon les paramètres
de localisation de l'application. Le paquet `time` peut être utilisé pour, entre autres, formatter les dates et
les heures. Pour plus de détails :
[Date and time format in Go cheatsheet](https://gosamples.dev/date-time-format-cheatsheet/)

### Formatter d'autres types

Référence : [Paquet `fmt`](https://pkg.go.dev/fmt)

Les formats les plus courants :

| Formats          | Détails                                     |
|------------------|---------------------------------------------|
| `%v`             | format par défaut                           |
| `%d`, `%x`, `%b` | nombres entiers en bases 10, 16 et 2        |
| `%f`, `%F`       | nombres réels, avec un point, sans exposant |
| `%e`, `%E`       | nombres réels, notation scientifique        |
| `%s`             | chaînes de caractères                       |
| `%t`             | booléen                                     |
| `%T`             | type de la valeur                           |

Pour les nombres réels, on peut spécifier la précision voulue.

```go
package main

import "fmt"

func print3() {
	x := 1234.5678
	fmt.Printf("%f\n%.2f\n%.f\n", x, x, x)
}
```

Sortie :

```
1234.567800
1234.57
1235
```