# Section 4 : WaitGroup

En général, dans le monde de la programmation parallèle, une structure similaire à un "WaitGroup" est souvent utilisée
pour synchroniser l'exécution de multiples threads. Le concept principal est d'avoir un moyen de notifier un certain
morceau de code (qui pourrait être un autre thread) que plusieurs autres tâches ou threads ont terminé leur exécution.

Un "WaitGroup" est généralement composée d'un compteur qui est incrémenté chaque fois qu'une nouvelle tâche est lancée
et décrémenté chaque fois que cette tâche s'achève. Une opération pour attendre (habituellement appelée "wait") va
bloquer jusqu'à ce que le compteur atteigne zéro, indiquant que toutes les tâches ont terminé leur exécution.

Dans le contexte de la programmation Go, un `WaitGroup` appartient au package `sync` et offre une manière simple et
effective de synchroniser l'exécution de goroutines. C'est un compteur de tâches. Le `WaitGroup` fournit principalement
trois méthodes : `Add`, `Done`, et `Wait`.

- `Add` augmente le compteur de tâches du `WaitGroup`. Il est utilisé pour informer le `WaitGroup` qu'il y a une
  nouvelle tâche à attendre.
- `Done` décrémente le compteur de tâches. Il est généralement mis à la fin de la goroutine pour indiquer que cette
  goroutine a terminé son exécution.
- `Wait` est utilisé pour bloquer l'exécution jusqu'à ce que le compteur de tâches soit revenu à zéro. Il est
  généralement utilisé là où vous souhaitez attendre que les tâches se terminent.

Ainsi, dans un programme Go, `WaitGroup` fournit un moyen simple, mais puissant de gérer et attendre l'achèvement de
plusieurs goroutines. C'est très utile lorsqu'un nombre dynamique de goroutines est créé ou lorsqu'un morceau de code
dépend de l'achèvement d'un ensemble de goroutines.