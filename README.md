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
Où _n_ est un entier proportionnel aux ressources nécessaires pour la commande lancée
#### Lancer un script 
```
sbatch script.sh
```

### Rédaction d'un script shell ```script.sh```
Le langage d'un script shell est le ```bash```, souvent noté ```/bin/bash```, ce qui correspond à sa localisation exacte au sein de l'architecture Unix.
Shell signifie terminal, ```bash``` est le langage terminal du shell Bourne Again Shell ou ```bash```. Il en existe d'autres comme le ```zsh```, le ```fish```, le ```dash``` etc.

#### Exemple de script shell
Attention, contrairement à  ce qui se fait dans tous la plupart des autres langages, le ```#``` en début de ligne n'indique pas un commentaire, mais un paramètre de slurm.
```
# !/bin/bash
#SBATCH --time=xxxxxx #job time limit; Format days-hours:minutes:seconds
#SBATCH --mem=nG #memory reservation where n is an integer
#SBATCH --cpus-per-task=n #ncpu on the same node where n is an integer

echo Hello world #Command
```
#### Défintion de variables ```bash```
Les chemins d'accès aux fichiers sont longs, compliqués et ne tolèrent pas la moindre erreur de caractère. Je vous conseil d'utilisez du copier-coller ou bien du clic molette pour copier-coller rapidement (Triple doigts sur un pad tactile). Pour éviter les commandes longues comme le bras et les erruers, on définit souvent des variables avant de travailler. Exemple :
```
FASTQ="/path/to/data/sequence.fastq"

echo ${FASTQ} #Outputs /path/to/data/sequence.fastq

cmd $FATSQ > output
```



## Petit point ChatGPT
Il est indéniable que ChatGPT sera votre professeur de bioinformatique et informatique dés que vous sortirez de cette pièce. 
Dans le cadre de ces exercies, ne l'utilisez pas tout de suite, lisez d'abord la doc des logiciels vace la command ```--help``` ou ```-h``` ou parfois ```man cmd``` où cmd est la commande shell pour laquelle vous cherchez des informations. ChatGPT n'est pas interdit, je vous recommande même vivement de vous en servir dans le futur. Le but de ce TP n'est pas de traiter des séquences, mais d'apprendre à manipuler les séquences. Si vous donnez les exercices à ChatGPT, il les fera super vite, mieux que vous et moi, mais vous n'aurez pas nécessairement saisi ce qu'il se passe. Bref, privilégez les discussions avec moi au cours du TP. Vous aurez tout le temps de vous faire basher part ChatGPT pendant votre stage.


## Prise en main du jeu de données

Les données fastq correspondent à du produit de séquenceur. Dans un fastq, le nucléotide n'est pas figé, c'est une **mesure**. Comme toute mesure, elle possède sa part d'incertitude. On détermine cette incertitude à l'aide de son score Phred+33. Le score est matérialisé par un caractère ASCI. Une façon de le comprendre au doigt mouillé où le caractère représente la probabilité d'éxactitude de mesure de la base: A>B>C>....>a>b>c>.....>$>,>;>!
Une façon plus simple de déterminer la qualité d'une séquence, ou d'un jeu de séquence, est d'utiliser un logiciel qui compile toutes ces informations de score et les matérialise sous la forme de graph :

### Exercice N°1 : FastQC 
Utilisez le logiciel ```fastqc``` pour obtenir des informations relatives à la qualité de votre jeu de donnée de départ.
Pour savoir comment utiliser fastqc, vous pouvez utiliser la commande suivante.
```
fastqc
```
Interprétez. Discutez les valeurs observées contre ce que l'on attendrait avec d'auitres techniques de séquençage. Vous pouvez vous fier à vos connaissances, chercher sur internet ou demander l'avis de ChatGPT sur les scores moyens etc. 

## Assemblage

L'assemblage est un grand puzzle. Sauf qu'il est hardcore et qu'il existe plusieurs solutions qui se valent autant les unes que les autres. Et que l'on a pas la boîte pour voir si on ne fait pas n'importe quoi. utilisez flye pour reconstruire votre jeu de donnée en MAGs. N'oubliez pas qu'il s'agit d'un jeu de données **métagénomiques**.
L'assemblage est une étape longue. 

```
flye 
```

Il est peu probable que votre assemblage se termine avant la fin de la séance. Lorsque vous avez lancé l'assemblage, RDV à l'Exercice de pseudo métagénomique en fin de document

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

# Exercice : Pseudo-Métagénomique
Récupérer le graph d'assemblage ```assembly_graph.gfa``` du fichier partagé ```/data/```

## Visualisation avec ```Bandage```
Bandage est un logiciel de visualisation de graph d'assemblage. Il vous faut charger le graph d'assemblage dans le bouton en haut à gauche ```load graph```. Cliquez ensuite sur le bouton ```draw graph```. Laissez la machine travailer quelques secondes puis prenez quelques minutes pour essayer de comprendre ce que vous avez devant les yeux. N'hésitez pas à aller faire un tour sur le wiki de Bandage pour voir un peu comment ça fonctionne : https://github.com/rrwick/Bandage/wiki

 Utilisez ```Bandage``` pour visualiser le graph d'assemblage.

 ### QUESTIONS
- Détaillez vos observations. 
- Selon vous, combien d'organismes génétiquement distincts composent cet échantillon pseudo-métagénomique ? 
- Que pouvez vous dire sur la structure des MAGS ? Proposez des interprétations buiologiques des figures mathématiques observées. 
- Extrayez une portion de séquence de chaque graph. Blastez en brut sur le NCBI. Inférez une taxonomie potentielle. Expliquez sur quoi se base cette taxonomie.
- A partir de la taxonomie inféré avec Blast, trouvez l'organisme considéré sur le NCBI. Téléchargez la séquence codante de son ARN 16S. Utilisez la fonction ```Blast``` de ```Bandage``` pour placer l'ARN sur votre graph. Qu'observez vous ? N'hésitez pas à jouer avec les paramètres de Blast si vous n'êtes pas satisfaits du résultat.



