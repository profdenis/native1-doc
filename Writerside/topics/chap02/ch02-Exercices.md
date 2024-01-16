# Exercices

1. Écrire et tester la fonction `ReadNonEmptyLine`, qui, tant que l'entrée est la chaîne vide ou qu'il y a
   une erreur lors de la lecture de la ligne, refuse cette entrée et redemande d'entrer une chaîne. Vous pouvez vous
   baser sur la fonction `ReadInt`, présentée dans la prochaine section.
2. Créez une copie de la fonction `section4a` nommée `section4b` qui a fait la même chose, mais qui utilise les
   fonctions `ReadNonEmptyLine` et `ReadInt` pour valider les entrées.
3. Faites une version de `ReadInt` qui utilise `Scanf` à la place de `ReadLine`.
4. Faites une autre version de `ReadInt` (à partir de l'originale ou de celle de l'exercice précédent), qui,
   lorsque `nAttempts` est plus petit que 1, va toujours redemander une entrée, donc il n'y aura pas de limite au nombre
   d'essais. Lorsque `nAttempts` est au moins 1, la fonction doit faire la même chose qu'avant.
5. Faites une version de `ReadNonEmptyLine` qui accepte en paramètre un nombre d'essais, comme pour `ReadInt`. Gérez le
   nombre d'essais de la même façon que l'exemple précédent.