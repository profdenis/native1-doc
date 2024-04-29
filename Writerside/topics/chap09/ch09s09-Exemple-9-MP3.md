# Exemple 9: MP3

[Code](https://github.com/profdenis/native1/tree/master/chap9/09_mp3)

Ce programme Go est une application graphique minimale qui permet à l'utilisateur d'ouvrir et de lire des
fichiers MP3.

Voici une répartition du code bloc par fonction :

1. `func main()` :
    - C'est le point d'entrée du programme.
    - Il crée une nouvelle application et une nouvelle fenêtre avec le titre "MP3".
    - Dans la fenêtre, il crée un nouveau bouton intitulé "Ouvrir". Ce bouton execute la fonction retournée
      par `getOpenButtonFunc(w)` lorsqu'on clique dessus.
    - La taille de la fenêtre est définie à 600 par 300 unités et est affichée avant de lancer la boucle d'événements de
      l'application.

2. `func getOpenButtonFunc(w fyne.Window) func()` :
    - Cette fonction retourne une fonction anonyme qui s'exécute lorsque le bouton "Ouvrir" est pressé.
    - Elle ouvre une boîte de dialogue de fichier qui permet à l'utilisateur de sélectionner un fichier .mp3 dans le
      répertoire de travail actuel.
    - La fonction retournée par `getOpenFileDialogFunc(w)` est déclenchée lorsque l'utilisateur sélectionne un fichier
      ou annule la boîte de dialogue.

3. `func getOpenFileDialogFunc(w fyne.Window) func(reader fyne.URIReadCloser, err error)` :
    - Cette fonction retourne une fonction qui est exécutée lorsqu'un fichier est sélectionné ou que la boîte de
      dialogue est annulée.
    - Si une erreur se produit lors de l'ouverture du fichier, elle sera affichée dans la fenêtre de dialogue.
    - Si l'utilisateur annule l'opération de la boîte de dialogue, la fonction imprime "Annulé" dans le journal.
    - Si un fichier est sélectionné, il est décodé en tant que fichier mp3 et joué à l'aide du package Speaker.

4. `func getCurrentDirURI() (fyne.ListableURI, error)` :
    - Cette fonction retourne le répertoire actuel sous forme d'URI listable.
    - Si une erreur se produit lors de l'obtention du répertoire de travail actuel ou de la conversion du répertoire en
      URI listable, elle renvoie l'erreur.

L'application utilise l'outil Fyne pour l'interface graphique, le package dialog de Fyne pour créer des boîtes de
dialogue de fichiers, et la bibliothèque Speaker pour jouer les fichiers mp3.

Ce programme n'est pas très pratique parce qu'une fois que la lecture d'un fichier MP3 a débutée, on ne peut pas
l'arrêter, sauf si on ferme l'application au complet. La lecture est exécutée dans le même fil d'exécution que
l'interface graphique, donc l'interface est gelée jusqu'à la lecture soit terminée.

L'exemple suivant est plus pratique, parce que la lecture du fichier MP3 est effectuée dans un autre fil d'exécution, et
l'interface graphique peut contrôler l'exécution du fichier. 

