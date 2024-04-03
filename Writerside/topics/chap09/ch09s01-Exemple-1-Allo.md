# Exemple 1 : Allo

[Code](https://github.com/profdenis/native1/tree/master/chap9/01_allo)

Cet exemple est une version du fameux "Hello World!" avec ube interface graphique simple construite en utilisant la
bibliothèque Fyne. Il crée une fenêtre d'application avec une étiquette et un bouton.

Voici une explication détaillée :

1. Les trois premières lignes importent les packages nécessaires pour cette application :

    - `"fyne.io/fyne/v2/app"` : Ce package fournit le point d'entrée de l'API Fyne. Il est utilisé pour créer une
      instance de l'application.
    - `"fyne.io/fyne/v2/container"` : Le _package_ `container` fournit des types qui peuvent contenir des _widgets_ ou
      des objets enfants.
    - `"fyne.io/fyne/v2/widget"` : Le _package_ `widget` fournit les blocs de construction de l'interface utilisateur,
      tels que les boutons et les étiquettes.

2. Dans la fonction `main`, `a := app.New()` crée une nouvelle instance de l'application.

3. `w := a.NewWindow("Allo")` crée une fenêtre d'application avec le titre `"Allo"`.

4. `allo := widget.NewLabel("Allo")` crée un nouveau _widget_ d'étiquette avec le texte "Allo".

5. `w.SetContent(container.NewVBox(...))` définit le contenu de la fenêtre avec un conteneur `VBox`. La
   fonction `NewVBox` fournit une façon de disposer les _widgets_ verticalement. À l'intérieur de cette `VBox`, vous
   avez deux _widgets_ :

    - L'étiquette `allo`
      -Un bouton avec le texte `"Salut !"`. Le deuxième paramètre est une fonction qui est appelée lorsque le bouton est
      cliqué - elle utilise la méthode `SetText` pour changer le texte de l'étiquette `allo` en `"Bienvenue 😀"`.

6. `w.ShowAndRun()` rend la fenêtre visible et lance l'application.

Donc, lorsque vous exécutez ce programme, il affichera une fenêtre intitulée `"Allo"` avec une étiquette montrant le
texte `"Allo"` et un bouton étiqueté `"Salut !"`. Lorsque vous cliquez sur le bouton `"Salut !"`, le texte de
l'étiquette change pour `"Bienvenue 😀"`.