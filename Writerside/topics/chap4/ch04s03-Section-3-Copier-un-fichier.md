# Section 3 : Copier un fichier en ajoutant des numéros de ligne

La fonction `CopyFileWithLineNumbers(srcFile string, dstFile string)` fait exactement comme son nom le suggère : Elle
copie le contenu du fichier source (spécifié par `srcFile`) vers le fichier de destination (spécifié par `dstFile`), et
insère des numéros de ligne pendant l'opération de copie.

```Go
func CopyFileWithLineNumbers(srcFilename string, dstFilename string) error {
	// Ouvrez le fichier source en lecture seule
	src, err := os.Open(srcFilename)
	if err != nil {
		return err
	}
	defer src.Close()

	// Ouvrez le fichier de destination en mode écriture
	dst, err := os.Create(dstFilename)
	if err != nil {
		return err
	}
	defer dst.Close()

	scanner := bufio.NewScanner(src)
	writer := bufio.NewWriter(dst)
	lineNumber := 1

	for scanner.Scan() {
		lineStr := fmt.Sprintf("%d\t%s\n", lineNumber, scanner.Text())
		if _, err := writer.WriteString(lineStr); err != nil {
			return err
		}
		lineNumber++
	}

	if err := scanner.Err(); err != nil {
		return err
	}

	if err := writer.Flush(); err != nil {
		return err
	}

	return nil
}
```

Voici ce que fait la fonction, étape par étape :

- Ouvre le fichier source en mode lecture seule, en utilisant `os.Open()`. S'il y a une erreur (par exemple, le fichier
  n'existe pas), elle retourne l'erreur.
- Ouvre le fichier de destination en mode écriture (le créant s'il n'existe pas), en utilisant `os.Create()`. Là encore,
  s'il y a une erreur, elle la retourne.
- Crée un `bufio.Scanner` pour lire le fichier source et un `bufio.Writer` pour écrire dans le fichier de destination.
- Démarre une boucle qui lit chaque ligne du fichier source.
    - Pour chaque ligne, elle formate une chaîne qui se compose du numéro de ligne (suivi d'un caractère de tabulation),
      du texte de la ligne du fichier source et d'un caractère de nouvelle ligne.
    - Écrit cette chaîne dans le fichier de destination. S'il y a une erreur, elle la retourne.
    - Incrémente le numéro de ligne pour la prochaine itération.
- Après la boucle (quand toutes les lignes ont été traitées), elle vérifie s'il y a eu des erreurs pendant le balayage.
  Si c'est le cas, elle retourne l'erreur.
- Appelle `writer.Flush()` pour s'assurer que toutes les opérations en mémoire tampon (s'il y en a) ont été appliquées
  au fichier de destination. S'il y a une erreur, elle la retourne.
- Si toutes les étapes ci-dessus se sont déroulées sans erreur, elle retourne nil indiquant que l'opération a réussi.

Ensuite, nous avons la fonction `section2c()`, qui appelle la fonction `CopyFileWithLineNumbers` :

```Go
func section2c() {
	err := CopyFileWithLineNumbers("allo.txt", "allo2.txt")
	if err != nil {
		fmt.Println("Erreur lors de la copie du fichier:", err)
	} else {
		fmt.Println("Le fichier a été copié avec succès avec des numéros de ligne.")
	}
}
```

- Elle appelle `CopyFileWithLineNumbers()` pour copier le contenu de `"allo.txt"` à `"allo2.txt"`.
- S'il y a une erreur, elle imprime un message indiquant que la copie du fichier a échoué, et elle donne l'erreur.
- S'il n'y a pas d'erreur, elle imprime un message indiquant le succès.

