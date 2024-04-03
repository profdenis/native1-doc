# Exemple 4 : Entrée de texte

[Code](https://github.com/profdenis/native1/tree/master/chap9/04_entree_texte)

Ceci est un exemple qui montre comment permettre à un utilisateur d'entrer du texte, et d'associer une
fonction à un événement sur la boîte de texte :

La fonction `makeUI` est déclarée, elle crée et renvoie une nouvelle boîte verticale (`fyne.Container`) contenant une
étiquette (`widget.NewLabel`) et un champ d'entrée (`widget.NewEntry`).

La fonction `main` est le point d'entrée de l'application :

- `app.New()` initialise une nouvelle application Fyne.
- `a.NewWindow("Événement")` crée une nouvelle fenêtre avec le titre "Événement".
- `w.SetContent(makeUI())` définit le contenu de la fenêtre à l'interface utilisateur créée par `makeUI`.
- `w.ShowAndRun()` affiche la fenêtre et démarre la boucle principale de l'application, bloquant jusqu'à ce que
  l'application soit fermée.

Dans la fonction `makeUI`, un gestionnaire d'événement `OnChanged` est défini pour le champ d'entrée, ce qui signifie
que chaque fois que le texte dans le champ d'entrée est modifié, la fonction gestionnaire est appelée :

- Cette fonction met à jour le texte de l'étiquette (`out`) pour montrer le texte actuel du champ d'entrée (`content`),
  préfixé par _"Texte : "_.

Une ligne commentée est incluse dans la fonction `makeUI` :

```go
//in.Password = true
```

Si cette ligne est décommentée, elle convertira le champ d'entrée de texte en champ de mot de passe, cachant les
caractères entrés.