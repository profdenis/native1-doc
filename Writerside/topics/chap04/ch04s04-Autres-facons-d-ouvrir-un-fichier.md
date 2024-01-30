# Section 4 : Autres façons d&apos;ouvrir un fichier

La fonction `os.OpenFile` est un outil très puissant qui vous permet d'ouvrir un fichier avec différents niveaux
d'accès et permissions. La signature de la fonction est :

`func OpenFile(name string, flag int, perm FileMode) (*File, error)`

Où :

* `name` est le chemin vers le fichier.
* `flag` est une valeur int qui représente les modes d'accès aux fichiers (lecture, écriture, ajout, etc.).
* `perm` sont les permissions de fichiers de style Unix que vous souhaitez définir pour le fichier.

## flag

Dans la documentation du [paquet `os`](https://pkg.go.dev/os#pkg-constants), on a les définitions suivantes :

```Go
const (
	// Exactly one of O_RDONLY, O_WRONLY, or O_RDWR must be specified.
	O_RDONLY int = syscall.O_RDONLY // open the file read-only.
	O_WRONLY int = syscall.O_WRONLY // open the file write-only.
	O_RDWR   int = syscall.O_RDWR   // open the file read-write.
	// The remaining values may be or'ed in to control behavior.
	O_APPEND int = syscall.O_APPEND // append data to the file when writing.
	O_CREATE int = syscall.O_CREAT  // create a new file if none exists.
	O_EXCL   int = syscall.O_EXCL   // used with O_CREATE, file must not exist.
	O_SYNC   int = syscall.O_SYNC   // open for synchronous I/O.
	O_TRUNC  int = syscall.O_TRUNC  // truncate regular writable file when opened.
)
```

Le paramètre `flag` spécifie le mode d'accès au fichier. Ce paramètre est généralement défini en effectuant un _OU_
binaire entre deux ou plusieurs constantes du package `os`. Voici un résumé de ce que fait chaque flag :

1. **os.O_RDONLY** : Ce flag ouvre le fichier en mode lecture seule. Sa valeur est `0`.
2. **os.O_WRONLY** : Ce flag ouvre le fichier en mode écriture seule. Si le fichier existe déjà, et que ce flag est
   utilisé seul, le fichier ne sera pas tronqué.
3. **os.O_RDWR** : Ce flag ouvre le fichier en mode lecture-écriture.
4. **os.O_APPEND** : Ce flag ouvre le fichier en mode ajout, de sorte que les données sont toujours écrites à la fin du
   fichier.
5. **os.O_CREATE** : Ce flag crée le fichier s'il n'existe pas.
6. **os.O_TRUNC** : Ce flag vide le fichier (le tronque) s'il existe.
7. **os.O_EXCL** : Ce flag, utilisé uniquement avec O_CREATE, renvoie une erreur si le fichier existe déjà.
8. **os.O_SYNC** : Ce flag ouvre le fichier pour l'I/O synchrone, rendant les opérations d'E/S immédiates et s'assurant
   que les données sont écrites sur le matériel sous-jacent avant de considérer l'opération d'écriture comme terminée.

La combinaison de ces flags vous permet de configurer le comportement de `os.OpenFile` pour répondre à vos besoins
spécifiques. Ces flags sont indépendants de la plateforme, vous pouvez donc les utiliser de manière fiable sur tous les
systèmes d'exploitation pris en charge.

## perm

Le troisième argument de la fonction `os.OpenFile`, `perm`, est utilisé pour définir les permissions du fichier dans le
style Unix. L'argument `perm` signifie "permission".

Dans les systèmes basés sur Unix, cette permission contrôle qui peut lire, écrire ou exécuter le fichier. Ces
permissions sont représentées par un ensemble de 9 bits. Elles sont généralement représentées en octal (base 8) pour
plus de commodité.

Décortiquons cela:

- Le premier chiffre représente les permissions du propriétaire.
- Le deuxième chiffre représente les permissions du groupe.
- Le troisième chiffre représente les permissions de tous les autres.

Chaque chiffre est la somme de :

- 4 pour lire (r),
- 2 pour écrire (w),
- 1 pour exécuter (x).

Par exemple, prenons une permission octale : `0644`.

- Le `0` au début est là parce que c'est un nombre octal.
- `6` est `4+2`, ce qui signifie des permissions de lecture et d'écriture pour le propriétaire du fichier.
- `4` signifie seulement la permission de lecture pour les membres du groupe du fichier.
- Le dernier `4` signifie seulement la permission de lecture pour tous les autres.

Voici un autre exemple : `0755`.

- `7` est `4+2+1`, ce qui signifie des permissions de lecture, d'écriture et d'exécution pour le propriétaire.
- Le premier `5` est `4+0+1`, ce qui signifie des permissions de lecture et d'exécution pour le groupe.
- Le dernier `5` signifie des permissions de lecture et d'exécution pour tous les autres.

En conclusion, le paramètre `perm` de la fonction `os.OpenFile` vous permet de contrôler l'accès à votre fichier lorsque
vous le créez. Par exemple :

```go
file, err := os.OpenFile("filename", os.O_RDWR|os.O_CREATE, 0644)
```

Cette ligne créerait un fichier avec des permissions de lecture/écriture pour le propriétaire, mais uniquement avec des
permissions de lecture pour tous les autres.

## Exemples

Voici quelques façons différentes d'utiliser la fonction `os.OpenFile` :

1. **Ouvrir un fichier en mode lecture seule :**

```go
file, err := os.OpenFile("file.txt", os.O_RDONLY, 0644)
```

2. **Ouvrir un fichier en mode écriture seule (le contenu du fichier existant sera supprimé) :**

```go
file, err := os.OpenFile("file.txt", os.O_WRONLY|os.O_TRUNC, 0644)
```

3. **Ouvrir un fichier en mode écriture seule et créer le fichier s'il n'existe pas :**

```go
file, err := os.OpenFile("file.txt", os.O_WRONLY|os.O_CREATE, 0644)
```

4. **Ouvrir un fichier en mode ajout pour l'écriture (ajoute au contenu de fichier existant) :**

```go
file, err := os.OpenFile("file.txt", os.O_APPEND|os.O_WRONLY, 0644)
```

5. **Ouvrir un fichier en mode lecture-écriture (le contenu du fichier existant sera supprimé) :**

```go
file, err := os.OpenFile("file.txt", os.O_RDWR|os.O_TRUNC, 0644)
```

6. **Ouvrir un fichier en mode lecture-écriture et créer le fichier s'il n'existe pas :**

```go
file, err := os.OpenFile("file.txt", os.O_RDWR|os.O_CREATE, 0644)
```

Dans tous ces exemples, vous devriez toujours vérifier l'erreur retournée (`err`) pour s'assurer que le fichier a été
ouvert avec succès :

```go
if err != nil {
	log.Fatal(err)
}
``` 

N'oubliez pas non plus de fermer le fichier lorsque vous avez terminé de l'utiliser. Vous pouvez utiliser
l'instruction `defer` pour vous assurer que `file.Close()` sera appelé à la fin de la fonction conteneur.

```go
defer file.Close()
```