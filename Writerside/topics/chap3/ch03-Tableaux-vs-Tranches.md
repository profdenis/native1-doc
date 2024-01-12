# Chapitre 3 : Tableaux et tranches

En Go, un tableau est une collection de valeurs de même type qui sont stockées de manière contiguë en mémoire. La taille
d'un tableau est fixe, c'est-à-dire que vous devez définir la taille d'un tableau au moment de sa déclaration.

Voici comment nous définissons un tableau en Go :

```go
var monTableau [10]int
```

Dans l'exemple ci-dessus, `monTableau` est un tableau de dix entiers. `var` est le mot-clé utilisé pour déclarer une
variable. `monTableau` est le nom du tableau, `[10]` indique la taille du tableau et `int` est le type de données que le
tableau peut stocker.

Pour initialiser un tableau au moment de la déclaration, vous pouvez faire comme suit :

```go
monTableau := [5]int{1, 2, 3, 4, 5}
```

Dans cette déclaration, `monTableau` est un tableau de `5` entiers. Le tableau est initialisé avec les
valeurs `1`, `2`, `3`, `4`, `5`.

Vous pouvez également créer un tableau sans spécifier sa taille et laisser Go déterminer la taille en fonction du nombre
d'éléments que vous fournissez :

```go
monTableau := [...]int{1, 2, 3, 4, 5}
```

Ici, `monTableau` est aussi un tableau de `5` entiers, tout comme dans l'exemple précédent. Cependant, nous avons
utilisé `...` au lieu de spécifier la taille du tableau, et Go a automatiquement déduit la taille à partir du nombre
d'éléments fournis.
