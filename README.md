# TP de Metagenomique

## IMPORTANT : Rendu en fin de TP
Travaillez en trinôme.
Vous devez rendre, à la fin de la deuxième séance, un document synthétisant vos réponses aux différentes questions présentes dans le document suivant. Pour chaque question identifiée, reportez la sur un document texte et répondez-y. Veuillez utilisez la trame de TP en fichier texte présente sur le git pour répondre. Veuillez copier-coller les commandes que vous avez utilisé pour répondre à l'exercice. Pour y accéder si vous avez oublié, vous pouvez utilisez la commande ```history``` qui listera les 2000 dernières commandes effectuées en commençant par les dernières. Attention, je n'ai pas reporté toutes les questions sur le document, donc lisez bien la trae de TP qui suit pour y reporter toutes les questions. 

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

## Conclusion utilisation du cluster
Pour finir, privilégiez le lancement de commandes avec ```sbatch script.sh``` plutôt que ```srun```, c'est plus pratique pour tout le monde et vous permet de conserver une trace durable de vos analyses.
Pour rappel, l'historique du terminal ne contient que les 2000 dernières commandes effectuées. Contrairement à ce que l'on pourrait penser, 2000 commandes, ça va vite.



## Petit point ChatGPT
Il est indéniable que ChatGPT sera votre professeur de bioinformatique et informatique dés que vous sortirez de cette pièce. 
Dans le cadre de ces exercies, ne l'utilisez pas tout de suite, lisez d'abord la doc des logiciels vace la command ```--help``` ou ```-h``` ou parfois ```man cmd``` où cmd est la commande shell pour laquelle vous cherchez des informations. ChatGPT n'est pas interdit, je vous recommande même vivement de vous en servir dans le futur. Le but de ce TP n'est pas de traiter des séquences, mais d'apprendre à manipuler les séquences. Si vous donnez les exercices à ChatGPT, il les fera super vite, mieux que vous et moi, mais vous n'aurez pas nécessairement saisi ce qu'il se passe. Bref, privilégez les discussions avec moi au cours du TP. Vous aurez tout le temps de vous faire basher part ChatGPT pendant votre stage.


## Prise en main du jeu de données

Les données fastq correspondent à du produit de séquenceur. Dans un fastq, le nucléotide n'est pas figé, c'est une **mesure**. Comme toute mesure, elle possède sa part d'incertitude. On détermine cette incertitude à l'aide de son score Phred+33. Le score est matérialisé par un caractère ASCI. Une façon de le comprendre au doigt mouillé où le caractère représente la probabilité d'éxactitude de mesure de la base: A>B>C>....>a>b>c>.....>$>,>;>!
Une façon plus simple de déterminer la qualité d'une séquence, ou d'un jeu de séquence, est d'utiliser un logiciel qui compile toutes ces informations de score et les matérialise sous la forme de graph :

### Exercice N°1 : FastQC 
Utilisez le logiciel ```fastqc``` pour obtenir des informations relatives à la qualité de votre jeu de donnée de départ.
Pour savoir comment utiliser fastqc, vous pouvez utiliser la commande suivante.
**ATTENTION à lancer uniquement sur un noeud !**
```
fastqc
```
Interprétez. Discutez les valeurs observées contre ce que l'on attendrait avec d'auitres techniques de séquençage. Vous pouvez vous fier à vos connaissances, chercher sur internet ou demander l'avis de ChatGPT sur les scores moyens etc. Combien de fragment ? Taille moyenne etc ?

## Assemblage

L'assemblage est un grand puzzle. Sauf qu'il est hardcore et qu'il existe plusieurs solutions qui se valent autant les unes que les autres. Et que l'on a pas la boîte pour voir si on ne fait pas n'importe quoi. utilisez flye pour reconstruire votre jeu de donnée en MAGs. N'oubliez pas qu'il s'agit d'un jeu de données **métagénomiques**.
L'assemblage est une étape longue. 
**ATTENTION Calcul lourd, à lancer uniquement sur un noeud !**
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

Quoi qu'il arrive, notez toujours bien chacune des étapes que vous réalisez, vous aurez besoin de les détailler sur votre rendu de TP.

Deux fichiers de séquences sont à votre disposition :
- 16S.fasta
- Carbon_related_genes.fasta
Choisissez l'un de ces fichiers pour la suite de l'exercice. Le choix n'a pas d'impact négatif sur vos résultats. 
Vous allez pêcher ces séquences à l'aide de la fonction ```blastn``` intégré à Bandage en cliquant sur le bouton en bas à gauche ```Create/View Blast search```. Prenez conscience de ce que vous faites.
Qu'avez vous identifié sur les séquences ? 

## Binning préhistorique

L'idée désormais c'est d'utiliser le marqueur que vous avez placé sur votre graph d'assemblage pour essayer de retrouver les séquences qui vont ensemble.

### Déterminer la contiguité d'une séquence d'intérêt
Définissez contiguité
Choissiez une séquence qui vous plait. Elle peut porter un marqueur 16S, un egène lié au métabolisme carboné ou juste être long. Ce que vous voulez. 
Sélectionnez cette séquence en cliquant dessus. Dans le menu de gauche, dans la section ```Graph Display``` - ```Color by contiguity``` - ``` Determine contiguity```.
Votre fragment d'intérêt se color en vert foncé, d'autres se colorent en vert foncé ou en vert clair. Foncé = Contiguité fortement probable. Vert clair = Contiguité possible. Explorez les limites de votre contiguité. Parfois il est difficile de déterminer la contiguité avec des courts fragments. Déterminez la contiguité des fragments aux extrémitié de la contiguité précédamment déterminée. Vous suivez toujours ? ;)
La somme des séquences contigües forme un MAG : __Metagenome-Assemble Genome__

### Extraction de votre MAG

Deux possibilités : 
- Vos fragment sont tous (potentiellement) contigües : Cliquez dans la barre du haut sur ```Select``` - ```Select nodes based on contiguity``` - ``` Select possibly contiguous nodes```. Vous avez donc sélectionné tous les fragments (nodes) concernés. Cliquez, toujours en haut sur ```Output``` - ```Save selected node sequences to fasta```. Enregistrez le fichier au bon endroit et nommez le de façon à vous souvenir de quoi il s'agit.
- Vos fragments ne sont pas tous contigües et vous avez identifié la contigüité en deux temps : Sélectionnez votre premier jeu de séquences possiblement contigües avec ```Select``` - ```Select nodes based on contiguity``` - ``` Select possibly contiguous nodes```. Copiez la liste des séquences sélectionnés, à droite : ```Selected nodes (N)```. Collez la liste dans un fichier texte pour ne pas la perdre. Faites la même chose avec l'autre set de séquences contigües déterminé. Sélectionnez les séquences à partir des identifiants. Puis extrayez les sésuences ```Output``` - ```Save selected node sequences to fasta```. Enregistrez le fichier au bon endroit et nommez le de façon à vous souvenir de quoi il s'agit. Quelle taille fait votre MAG ? Combien de fragments, combien de nucléotides ? Qu'est ce que ça représente par rapport au génome Bactérien moyen ? Pensez vous que votre MAG soit complet ? Quelle que soit votre réponse, justifiez la avec au moins deux éléments.  

## Annotation de votre MAG

Utilisez ```PROKKA``` pour annoter votre MAG. **ATTENTION Calcul lourd, à lancer uniquement sur un noeud !** Explorez la liste des gènes identifiés. 
Dressez un petit tableau synthétisant le nombre de CDS (Coding Sequences) détectées. Pouvez vous inférer un métabolisme de votre organisme à partir de cette liste. HINTS : Les enzymes sont elles anoxiques ou oxiques ? Photosynthèse ? Fixation d'Azote ? Dénitrification ? Respiration aérobie ? Etc. Etc. Etc.

### Détermination taxonomique de votre MAG
Fouillez dans les outputs de PROKKA, trouvez un gène de ménage marqueur (tel que RpoB, 16S rRNA CDS ou autre gène de ménage). Déterminez la taxonomie de cette séquence à l'aide de Blast.
Quelle est la taxonomie de l'organisme le plus proche dans la base de donnée du NCBI ? Quel est le pourcentage d'indetité entre la séquence connue et la votre ? D'après le cours de Métabarcoding, quel niveau taxonomique pensez vous avoir découvert ? 

## Anvi'o
En premier lieu, lancez la commande suivante :
```
export PATH=$PATH:/students/HAU802I/data/
```

Si il vous reste du temps, injectez votre MAG dans ```Anvi'o```. Produisez la ```contigs.db``` et le ```PROFILE```. Explorez votre MAG à l'aide de ```anvio anvi-interactive -c contigs.db -p PROFILE/PROFILE.db```. Est-ce que le taux de GC est consistant sur tout le MAG ? Pensez vous que votre MAG soit complet ? Pour le tester, utilisez ```anvio anvi-estimate-genome-completeness -c Circular.db```. Explorez l'output, que voyez vous ? Détaillez.

### Bravo pour être arrivé jusque là.



# Exercice : Pseudo-Métagénomique
Récupérez le graph d'assemblage ```assembly_graph.gfa``` du fichier partagé ```/data/```

## Visualisation avec ```Bandage```
Bandage est un logiciel de visualisation de graph d'assemblage. Il vous faut charger le graph d'assemblage dans le bouton en haut à gauche ```load graph```. Cliquez ensuite sur le bouton ```draw graph```. Laissez la machine travailer quelques secondes puis prenez quelques minutes pour essayer de comprendre ce que vous avez devant les yeux. N'hésitez pas à aller faire un tour sur le wiki de Bandage pour voir un peu comment ça fonctionne : https://github.com/rrwick/Bandage/wiki

 Utilisez ```Bandage``` pour visualiser le graph d'assemblage.

 ### QUESTIONS
- Détaillez vos observations. 
- Selon vous, combien d'organismes génétiquement distincts composent cet échantillon pseudo-métagénomique ? 
- Que pouvez vous dire sur la structure des MAGS ? Proposez des interprétations buiologiques des figures mathématiques observées. 
- Extrayez une portion de séquence de chaque graph. Blastez en brut sur le NCBI. Inférez une taxonomie potentielle. Expliquez sur quoi se base cette taxonomie.
- A partir de la taxonomie inféré avec Blast, trouvez l'organisme considéré sur le NCBI. Téléchargez la séquence codante de son ARN 16S. Utilisez la fonction ```Blast``` de ```Bandage``` pour placer l'ARN sur votre graph. Qu'observez vous ? N'hésitez pas à jouer avec les paramètres de Blast si vous n'êtes pas satisfaits du résultat.
- Faites un dessin sur du papier pour représenter, selon vous l'organisation potentielle des fragments d'ADN les uns par rapport aux autres.

## Annotations avec ```PROKKA```
Utilisez ```PROKKA``` pour annoter votre dataset. **ATTENTION Calcul lourd, à lancer uniquement sur un noeud !**
Jonglez avec l'output, fouillez dedans, qu'y trouvez vous ? 
Récupérez les séquences prédites d'ARN16S. Blastez les en ligne. Confirmez que vous retombez bien sur votre première estimation.

##  Manipulation avec ```Anvi'o```
En premier lieu, lancez la commande suivante :
```
export PATH=$PATH:/students/HAU802I/data/
```
### Préparation du dataset
Pour manipuler un métagénome avec Anvio, il faut commencer par créer un objket ```.db```, une database, la marque de fabrique d'Anvi'o
Le calcul prend moins d'une minute
```
anvio anvi-gen-contigs-database--contigs-fasta assembly.fasta --project-name PseudoMetagenomics --output-db-path contigs.db --num-threads 4
```
#### QUESTION
Lisez bien l'output visuel d'Anvi'o. Que vous apprend-t-il ? 

### Préparation pour visualisation
Commencez par créer un objet PROFILE afin de pouvoir le visualiser avec l'outil interactif
```
srun anvio anvi-profile -c contigs.db --blank-profile -o PROFILE --sample-name PseudoMetagenomics
```
### Complétude de génome
Combien de génomes dans l'échantillon estimés par Anvi'o ? Est ce cohérent avec ec que vous aviez inféré depuis le graph d'assemblage sur ```Bandage``` ?  
```
srun anvio anvi-run-hmms -c contigs.db
srun anvio anvi-estimate-genome-completeness -c contigs.db
```

### Visualisation 
Combien de génomes voyez vous ? D'après vous, qu'utilise Anvi'o pour les discriminer ? 
```
srun anvio anvi-interactive -c contigs.db -p PROFILE/PROFILE.db
```

# Optionnel - long et pas sûr de focntionner

### Annotation du dataset
```
srun anvio anvi-run-kegg-kofams -c contigs.db -T 4
````
Cette étape prend quelques minutes, allez boire un café

### Estimation du métabolism
Pas sur qu'on fasse cette partie
```
anvio anvi-estimate-metabolism -c contigs.db \
                         -O Pseudo-metagenomics_metabolism
```
