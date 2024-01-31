# TP de Metagenomique

## Utiliser un cluster de calcul

Les clusters de calculs sont des machines multiples divisées en deux grandes entitées : Le serveur frontal (Marygold) et les noeuds (Nodes).
Le serveur frontal sert à accéder aux disques et à lancer des jobs sur les noeuds. La gestion des noeuds et l'éxécution des jobs est gérée par un cadenceur. 
Le cadenceur le plus utilisé pour Linux est ```slurm```, c'est ce qui est utilisé sur le cluster de l'université. 
Il ne faut pas lancer de calcul ni de job gourmand sur le serveur frontal. Il est partagé par tous les utilisateurs en même temps et chacun doit pouvoir avoir suffisamment de ressources pour naviguer facilement dans ses données et y lancer les jobs nécessaires. De plus, le serveur frontal a beau être un ordinateur puissant, il n'est pas taillé pour du calcul. Un job gourmand peut le faire planter, pénalisant ainsi tous les utilisateurs en même temps. 

Plusieurs façons de lancer un job avec slurm :
#### Pour lancer immédiatement une commande (```cmd```)sur un noeud
```
srun cmd [--options]
```
#### Pour utilise run noeud et se positionner dessus plutôt que sur le frontal
```
srun --mem nG --pty bash 
```




## Prise en main du jeu de données

Les données fastq correspondent à du produit de séquenceur. Dans un fastq, le nucléotide n'est pas figé, c'est une **mesure**. Comme toute mesure, elle possède sa part d'incertitude. On détermine cette incertitude à l'aide de son score Phred+33. Le score est matérialisé par un caractère ASCI. Une façon de le comprendre au doigt mouillé où le caractère représente la probabilité d'éxactitude de mesure de la base: A>B>C>....>a>b>c>.....>$>,>;>!
Une façon plus simple de déterminer la qualité d'une séquence, ou d'un jeu de séquence, est d'utiliser un logiciel qui compile toutes ces informations de score et les matérialise sous la forme de graph :

### FastQC 
```
fastqc seqs.fastq
```
Interprétez. Discutez les valeurs observées contre ce que l'on attendrait avec d'auitres techniques de séquençage

## Assemblage

L'assemblage est un grand puzzle. Sauf qu'il est hardcore et qu'il existe plusieurs solutions qui se valent autant les unes que les autres. Et que l'on a pas la boîte pour voir si on ne fait pas n'importe quoi. 

```
flye --meta seqs.fastq 
```

## Visualisation

Les contigs, c'est bien. Mais ce qui est encore mieux, c'est de savoir comment ils sont construits.

```
Bandage
```

Bandage est un logiciel de visualisation de graph d'assemblage. Il vous faut charger le graph d'assemblage dans le bouton en haut à gauche ```load graph```. Cliquez ensuite sur le bouton ```draw graph```. Laissez la machine travailer quelques secondes puis prenez quelques minutes pour essayer de comprendre ce que vous avez devant les yeux. N'hésitez pas à aller faire un tour sur le wiki de Bandage pour voir un peu comment ça fonctionne : https://github.com/rrwick/Bandage/wiki

## Identification

Je vais vous distribuer un identifiant de séquence. Vous allez pêcher cette séquence à l'aide de la fonction ```blastn``` intégré à Bandage en cliquant sur le bouton en bas à gauche ```Create/View Blast search```. Prenez conscience de ce que vous faites. Qu'avez vous identifié sur les séquences ? 

## Binning préhistorique

L'idée désormais c'est d'utiliser le marqueur que vous avez placé sur votre graph d'assemblage pour essayer de retrouver les séquences qui vont ensemble
