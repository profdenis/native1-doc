# Chapitre 4 : La gestion des fichiers

Lire ou écrire dans un fichier est très semblable à la lecture à partir du `stdin` et à l'écriture sur le `stdout`. La
différence principale est qu'il faut ouvrir et fermer les fichiers correctement avant et après la lecture ou l'écriture
des données dans les fichiers. Du point de vue du code, il n'y a pratiquement pas de différences entre lire des données
du `stdin` et lire des données d'un fichier ouvert en mode lecture, et réciproquement, écrire des données sur
le `stdout` ou dans un fichier ouvert en mode écriture sont presque identiques en termes de code. `os.Stdin`
et `os.Stdout` sont de type `*os.File`, donc ils sont des pointeurs vers des fichiers, des fichiers spéciaux du système
d'exploitation qui correspondent à l'entrée et à la sortie associées à un terminal.

