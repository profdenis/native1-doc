# Exemple 7 : Menu

[Code](https://github.com/profdenis/native1/tree/master/chap9/07_menu)

La fonction `main` crée une nouvelle application Fyne, une fenêtre au sein de cette application avec le titre _"Exemple
07 : Menu et raccourcis"_, ainsi que des labels et des champs de texte. Voici une explication plus détaillée ligne par
ligne :

- `a := app.New()` initialise une nouvelle application Fyne.

- `w := a.NewWindow("Exemple 07 : Menu et raccourcis")` crée une nouvelle fenêtre dans cette application avec le titre
  donné.

- `src, dst := makeSrcDstEntries()` initialise `src` et `dst`, où `src` est un champ de texte à lignes multiples
  et `dst` est un widget affichant du texte enrichi. L'entrée `dst` se met à jour pour afficher le texte de
  l'entrée `src` analysé en tant que Markdown.

- Ensuite, il crée un élément de menu _"Ouvrir"_ qui déclenche l'ouverture d'une boîte de dialogue pour sélectionner un
  fichier Markdown (`.md`). Le contenu du document sélectionné est défini dans les entrées `src` et `dst` de
  l'application. Cet élément de menu est également lié au raccourci clavier `Ctrl-O`.

- Un menu principal (`mainMenu`) est ensuite créé, qui comprend l'élément de menu _"Ouvrir"_ et un élément de menu
  _"Enregistrer"_. Lorsque l'on clique sur l'élément de menu _"Enregistrer"_, une boîte de dialogue de fichier s'ouvre
  pour sauvegarder le contenu actuel de l'entrée `src` en tant que fichier Markdown.

- `w.SetMainMenu(mainMenu)` définit le `mainMenu` dans la fenêtre.

- Un séparateur (ou diviseur) horizontal est créé, qui sépare les entrées `src` et `dst`.

- `w.SetContent(container.NewBorder(filename, nil, nil, nil, split))` crée un nouveau conteneur avec le
  label `filename`, qui est mis à jour avec le chemin du fichier actuellement ouvert, en haut, et le séparateur créé au
  centre.

- `w.Resize(fyne.NewSize(800, 600))` définit ensuite la taille de la fenêtre à `800x600` pixels.

- `w.ShowAndRun()` affiche enfin la fenêtre et commence la boucle principale de l'application.

## Construction du menu et des raccourcis

1. Un élément de menu _"Ouvrir"_ est créé avec
   l'appel `fyne.NewMenuItem("Open", getOpenMenuFunc(w, src, dst, filename))`. Ici, _"Open"_ est le texte qui apparaîtra
   dans l'élément de menu, et `getOpenMenuFunc(w, src, dst, filename)` est la fonction qui sera exécutée lorsque
   l'élément de menu sera cliqué. La fonction `getOpenMenuFunc` retourne une autre fonction qui ouvre un dialogue de
   sélection de fichier lorsque l'utilisateur clique sur le bouton "Open". Le fichier sélectionné sera lu et son contenu
   sera affiché dans les widgets `src` et `dst`.

2. L'icône et le raccourci de l'élément de menu _"Ouvrir"_ sont définis avec les deux instructions suivantes :

    ```go
    openMenuItem.Icon = theme.DocumentIcon()
    openMenuItem.Shortcut = &desktop.CustomShortcut{
            KeyName:  fyne.KeyO,
            Modifier: fyne.KeyModifierShortcutDefault,
    }
    ```

   Ici, `theme.DocumentIcon()` est une icône standard présente dans la bibliothèque Fyne
   et `desktop.CustomShortcut{KeyName: fyne.KeyO, Modifier: fyne.KeyModifierShortcutDefault}` est un raccourci qui sera
   déclenché lorsque l'utilisateur appuie sur _"Cmd+O"_ (sur macOS) ou _"Ctrl+O"_ (sur Windows/Linux).

3. Le raccourci est également ajouté au Canvas de la fenêtre
   avec `w.Canvas().AddShortcut(openMenuItem.Shortcut, func(shortcut fyne.Shortcut) { getOpenMenuFunc(w, src, dst, filename)() })`.
   Cela permet au raccourci d'être écouté à tout moment lorsque la fenêtre est en focus, et pas seulement lorsque le
   menu est déroulé.

4. Un menu _"Fichier"_ contenant les éléments de menu _"Ouvrir"_ et _"Enregistrer"_ est créé
   avec `fyne.NewMenu("File", openMenuItem, fyne.NewMenuItem("Save", getSaveMenuFunc(w, src)))`. Lorsque l'élément de
   menu _"Save"_ est cliqué, il déclenche également un dialogue de sélection de fichier, mais cette fois pour sauver le
   contenu des deux zones de texte `src` et `dst` dans un fichier.

5. Enfin, le menu principal de la fenêtre est défini avec `w.SetMainMenu(fyne.NewMainMenu(mainMenu))`.

Donc, essentiellement, le menu est construit en ajoutant des `MenuItem` individuels à un `Menu`, qui est ensuite ajouté
à un `MainMenu` qui est défini comme le menu principal de la fenêtre. Chaque `MenuItem` peut avoir une fonction associée
qui sera déclenchée lors du clic sur cet élément de menu, ainsi qu'un raccourci clavier qui peut déclencher la même
fonction.