# Section 2 : Écrire dans un fichier

La façon la plus simple de d'ouvrir un fichier pour l'écriture est avec la fonction `os.Create`. Elle s'appelle de cette
façon parce que si le fichier n'existe pas, alors le fichier va être créé avant de pouvoir y écrire. Si le fichier
existe déjà, il sera _tronqué_, c'est-à-dire que sont contenu sera effacé et pour ensuite y écrire à partir du début.
C'est comme ai on écrivait par-dessus le fichier existant.

## Exemple simple d'écriture dans un fichier

```Go
func section2a(filename string) {
	// Créer un nouveau fichier, ou le tronquer s'il existe
	file, err := os.Create(filename)
	if err != nil {
		log.Fatal(err)
	}
	defer file.Close()

	// Écrire des bytes dans le fichier
	bytesWritten, err := file.WriteString("Bonjour tout le monde !")
	if err != nil {
		log.Fatal(err)
	}
	log.Printf("Écrit %d bytes.\n", bytesWritten)
}
```

1. La fonction a comme paramètre le nom du fichier `filename` dans lequel on doit écrire.
2. On commence par création du nouveau fichier en utilisant la fonction `os.Create` avec le nom de fichier fourni en
   argument. Cette fonction crée un nouveau fichier et si le fichier existe déjà, elle tronque le contenu du fichier. Le
   résultat de la fonction `os.Create` comprend deux variables. La première est un pointeur de de fichier `file` qui est
   utilisée pour les opérations de fichier ultérieures et la seconde est une erreur `err` qui aurait pu se produire.
3. On vérifie ensuite s'il y a eu une erreur lors de la création du fichier. S'il y a eu une erreur (`err` n'est pas
   `nil`), on logue l'erreur et termine immédiatement le programme en utilisant `log.Fatal(err)`.
4. L'instruction `defer file.Close()` fait en sorte que `file.Close()` soit appelée juste avant que la
   fonction `section2a` ne se termine, soit normalement ou via une erreur non traitée, afin de libérer les ressources du
   système (voir les détails plus bas).
5. Ensuite, on utilise la fonction `file.WriteString` pour écrire une chaîne de caractères dans le fichier. Le comptage
   en octets du contenu écrit est stocké dans `bytesWritten`, et toute erreur pendant le processus est stockée
   dans `err`.
6. Encore une fois, toute erreur lors de l'écriture dans le fichier est vérifiée et s'il y a eu une erreur, le programme
   enregistre l'erreur et se termine immédiatement.
7. Si l'écriture des bytes est réussie, le nombre de bytes écrits est enregistré dans le journal (`log`).

Si on n'utilisait pas `defer` ici, et qu'on appelait `Close` à la fin de la fonction comme dans les exemples
précédents, alors ça pourrait créer des problèmes. Quand on ouvre un fichier (ou toute autre resource), il faut
s'assurer de la fermer, ou libérer, avant de quitter la fonction. Si on a un `return` avant de se rendre à la fin de
la fonction, ou si on quitte à cause d'une erreur, comme avec `log.Fatal` ou avec une panique (une erreur grave au
moment de l'exécution), alors le fichier, ou en général la resource, ne sera pas fermée, et ça pourrait causer des
problèmes, comme des fuites de mémoires ou, dans notre cas, des données qui ne seraient pas enregistrées correctement
dans le fichier ouvert. Si on n'utilise pas `defer`, il faudrait appeler `file.Close()` possiblement à plusieurs
endroits, à chaque endroit où on pourrait potentiellement quitter la fonction. Dans certains cas, comme pour les
paniques, elles pourraient arriver à des endroits imprévus, donc la resource pourrait ne jamais être correctement
fermée ou libérée.

L'utilisation de `defer file.Close()` nous assure que le fichier sera fermé, peu importe la façon dont nous quittons la
fonction. Dans les exemples précédents, puisque les fichiers étaient ouverts en lecture seule, le fichier n'était pas
modifié, donc on ne pouvait pas perdre de données si le fichier n'était pas fermé correctement. Mais il est tout de même
préférable d'utiliser `defer` dans tous les cas pour s'assurer d'une bonne gestion de la mémoire, pour ne pas conserver
les contenu des fichiers en mémoire pour rien.

## Exemple : écrire des données entrées par l'utilisateur dans un fichier

La fonction suivante demande des entrées à l'utilisateur et écrit ces entrées dans un fichier _CSV_ en utilisant les
opérations d'E/S de fichiers de la bibliothèque standard.

```Go
func section2b() {
	nAttempts := 3
	// demander le nom du fichier
	fmt.Println("Nom du fichier dans lequel enregistrer les données : ")
	filename, err := ReadNonEmptyLine(nAttempts)
	if err != nil {
		log.Fatalf("Impossible de lire le nom du fichier : %s", err)
	}

	// ouvrir le fichier
	file, err := os.Create(filename)
	if err != nil {
		log.Fatalf("Impossible d'ouvrir le fichier : %s", err)
	}
	defer file.Close()

	done := false
	for !done {
		// lire le nom
		fmt.Print("Veuillez entrer votre nom : ")
		name, err := ReadNonEmptyLine(nAttempts)
		if err != nil {
			log.Printf("Impossible de lire le nom : %s", err)
		} else {
			// s'il n'y a pas d'erreur, lire l'âge
			fmt.Print("Veuillez entrer votre âge : ")
			age, err := ReadInt(nAttempts)
			if err != nil {
				log.Printf("Impossible de lire l'âge : %s", err)
			} else {
				// s'il n'y a pas d'erreur
				line := fmt.Sprintf("%s,%d\n", name, age)
				_, err = file.WriteString(line)
				if err != nil {
					log.Fatalf("Impossible d'écrire dans le fichier : %s", err)
				}
			}
		}

		// continuer ou non ?
		fmt.Println("Voulez-vous entrer continuer (O/N) ?")
		continueInput, err := ReadNonEmptyLine(nAttempts)
		if err != nil {
			log.Printf("Impossible de lire la réponse : %s", err)
		} else {
			if strings.ToUpper(continueInput) == "N" {
				done = true
			}
		}
	}
}
```

Voici un pas à pas de la fonction `section2b` :

1. La fonction `section2b` commence par déclarer une variable `nAttempts` qui définit une limite au nombre de tentatives
   de lecture des entrées.
2. Elle demande à l'utilisateur de saisir le nom du fichier dans lequel les données doivent être enregistrées. Ceci est
   fait via l'appel à `ReadNonEmptyLine(nAttempts)`. Si le nom du fichier est lu avec succès (non vide et pas d'erreur
   de lecture), il continue, sinon il quitte le programme en enregistrant l'erreur fatale.
3. Ensuite, elle tente de créer le fichier spécifié à l'aide de `os.Create(filename)`. Si une erreur se produit pendant
   la création du fichier, une erreur fatale est enregistrée et le programme quitte. La fermeture du fichier est reporté
   après que toutes les opérations suivantes ont été terminées.
4. Elle entre ensuite dans une boucle qui continue jusqu'à ce que `done` devienne vraie.
5. Dans la boucle, elle demande d'abord à l'utilisateur d'entrer son nom, en utilisant une vérification des erreurs
   similaire à l'entrée précédente. Si le nom est lu avec succès, elle demande ensuite l'âge à l'aide de la
   fonction `ReadInt(nAttempts)`. Sinon, elle enregistre l'erreur et répète la boucle.
6. S'il n'y a pas d'erreur lors de la lecture de l'âge, elle formate le nom et l'âge en une chaîne avec `fmt.Sprintf`,
   puis tente d'écrire cette chaîne dans le fichier ouvert à l'aide de `file.WriteString(line)`. Elle
   vérifie les erreurs lors de l'opération d'écriture, enregistre l'erreur fatale et quitte si nécessaire.
7. Ensuite, elle demande à l'utilisateur s'il souhaite continuer à entrer plus de données. Si l'utilisateur saisit `'N'`
   ou `'n'`, elle change `done` à `true` ce qui brisera la boucle, sinon elle retourne à la demande du nom et de l'âge.

**Note** : le paquet `encoding/csv` offre une meilleure interface pour lire et écrire des fichiers _CSV_. L'exemple
précédent écrit du texte directement dans le fichier. `encoding/csv` permet de lire et d'écrire plus efficacement. Deux
fonctions qui utilisent `encoding/csv` suivent. La première lit un fichier _CSV_, et l'autre écrit dans un fichier
_CSV_.

```Go
func readCSVFile() {
	f, err := os.Open("abc.csv")
	if err != nil {
		log.Fatal(err)
	}
	defer f.Close()

	reader := csv.NewReader(f)
	records, err := reader.ReadAll()
	if err != nil {
		log.Fatal(err)
	}

	for _, record := range records {
		fmt.Println(record) // record est une tranche de chaînes de 
		                    // caractères représentant les lignes du fichier
	}
}

func writeCSVFile() {
	f, err := os.Create("def.csv")
	if err != nil {
		log.Fatal(err)
	}
	defer f.Close()

	writer := csv.NewWriter(f)
	data := []string{"Col1", "Col2", "Col3"}

	if err := writer.Write(data); err != nil {
		log.Fatal(err)
	}
	writer.Flush()

	if err := writer.Error(); err != nil {
		log.Fatal(err)
	}
}
```