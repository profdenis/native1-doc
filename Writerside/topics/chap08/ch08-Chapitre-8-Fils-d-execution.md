# Chapitre 8 : Fils d&apos;exécution

Les _threads_, aussi connus sous le nom de **fils d'exécution**, sont une façon pour un programme d'effectuer plusieurs
tâches simultanément dans le même espace de processus. Un processus peut contenir plusieurs _threads_, tous partageant
les mêmes ressources, comme la mémoire, mais pouvant s'exécuter indépendamment les uns des autres.

Chaque _thread_ dans un processus fonctionne comme un flux séquentiel unique d'exécution, signifiant qu'il suivra une
séquence d'instructions dans un ordre spécifié. Cela permet aux applications d'exécuter diverses tâches de manière
simultanée, comme gérer des frappes au clavier pendant la lecture d'un disque dur.

Les _threads_ ont deux types principaux : les _threads_ utilisateur et les _threads_ noyau. Les _threads_ utilisateur
sont gérés par l'application, et le système d'exploitation n'en a pas connaissance. D'autre part, les _threads_ noyau
sont reconnus par le système d'exploitation et ont accès à ses ressources.

**Avoir plusieurs _threads_ dans un programme peut améliorer l'efficacité et la performance**, car plusieurs tâches
peuvent être effectuées en parallèle. **Cependant, cela peut également amener des problèmes de complexité, de
synchronisation et de concurrence**. Par exemple, deux threads peuvent tenter d'accéder à la même ressource
simultanément, provoquant une **situation de compétition** (un état de _race condition_).

Il convient de noter que, bien que les _threads_ puissent offrir une efficacité significative, leur utilisation et leur
gestion nécessitent une conception soigneuse pour **éviter des problèmes comme les conditions de concurrence, les
interblocages et les problèmes de synchronisation**.

Dans ce chapitre, nous verrons les bases de l'utilisation de fils d'exécution dans le langage Go. Mais avant, il est bon
de revenir sur la nature des fonctions en Go, et les différentes façons d'utiliser les fonctions.