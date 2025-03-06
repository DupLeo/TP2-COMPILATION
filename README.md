# Compilation TP - Compilation d'un langage simple

Ce projet implémente un compilateur simple qui transforme un langage personnalisé en code assembleur x86. Le programme utilise la librairie `java_cup` pour générer un parseur et la gestion des expressions arithmétiques et logiques.

## Auteurs
COULEMEAU Paul & Dupraz-Roget Léo

## Fonctionnement du programme

### Structure du projet

Le code est divisé en plusieurs classes, mais la partie principale se trouve dans le fichier `Parser.java`. Le compilateur prend en entrée un fichier texte contenant des instructions dans notre langage et génère un code assembleur qui peut être exécuté.

Le programme est basé sur des **arbres binaires** pour représenter les expressions et les instructions. Chaque nœud de l'arbre correspond à une opération ou une expression. Les opérations de base incluent les affectations, les conditions `IF`, les boucles `WHILE`, les opérations arithmétiques, et les sorties avec `OUTPUT`.

### Fonctionnalité

Le programme gère des expressions comme :

- `LET` pour l'affectation de variables.
- `IF` pour les conditions avec une syntaxe similaire à `IF expr THEN expr ELSE expr`.
- `WHILE` pour les boucles avec une syntaxe du type `WHILE expr DO expr`.
- Les opérations arithmétiques standards (`+`, `-`, `*`, `/`, `%`).
- La sortie avec `OUTPUT`.

### Fonctionnement des actions

1. **Expression arithmétiques et logiques :** Le programme analyse des expressions de type `expr`, en utilisant des nœuds binaires pour gérer les opérations et les nœuds unaires pour les opérations comme `-` ou `OUTPUT`.
2. **Génération de code assembleur :** Chaque nœud génère une séquence de commandes en langage assembleur pour l'architecture x86. Par exemple, l'affectation d'une valeur à une variable génère un code comme `mov eax, <valeur>`.

### Limites

- La partie récursive du compilateur n'est pas encore implémentée.
- Le programme ne gère pas la hiérarchisation des opérations de manière complète pour des expressions complexes.
- Le traitement d'erreurs pour la syntaxe et les opérations est basique.

## Comment tester le programme

### Prérequis

Avant de pouvoir tester le programme, assurez-vous d'avoir installé les outils suivants :

- Java 8 ou supérieur.
- Gradle (pour la construction du projet).

### Étapes de construction

1. **Clonez le repository** (si vous ne l'avez pas encore fait).
   
   ```bash
   git clone <url-du-repository>
   cd <nom-du-dossier-du-repository>
   ```

2. **Construisez le projet avec Gradle.** Utilisez la commande suivante pour compiler le projet :

   ```bash
   ./gradlew build
   ```

### Tester le programme

Le programme prend un fichier source en entrée, où chaque fichier contient des exemples de code dans notre langage. Vous pouvez tester le compilateur avec les fichiers suivants :

- `exemple_exo1.txt`
- `exemple_exo2.txt`
- `exemple_exo3.txt`

Voici comment tester :

1. **Compiler un fichier d'exemple** :

   Pour tester le compilateur avec un fichier exemple, exécutez la commande suivante :

   ```bash
   java -jar ./build/libs/TP2-COMPILATION.jar ./exemple_exo1.txt
   ```

   Remplacez `exemple_exo1.txt` par l'exemple que vous souhaitez tester. Vous pouvez tester les autres fichiers (`exemple_exo2.txt` et `exemple_exo3.txt`) en changeant le nom du fichier d'entrée.

2. **Vérifier la sortie** :

   Après avoir exécuté la commande, le code assembleur généré sera affiché dans la sortie standard. Vous pouvez vérifier que le programme génère correctement les instructions en assembleur pour chaque fichier d'exemple.

# Exemple de génération en ASM

## Exemple 1 : Calcul du prix TTC

Code source JavaScript :
```javascript
let prixHt = 200;
let prixTtc =  prixHt * 119 / 100 ;
```

Code généré en langage assembleur :
```asm
DATA SEGMENT
	prixHt DD
	prixTtc DD
DATA ENDS

CODE SEGMENT
	mov eax, 200
	mov prixHt, eax
	mov eax, prixHt
	push eax
	mov eax, 119
	pop ebx
	mul eax, ebx
	push eax
	mov eax, 100
	pop ebx
	div ebx, eax
	mov eax, ebx
	mov prixTtc, eax
CODE ENDS
```

## Exemple 2 : Algorithme de calcul du plus grand commun diviseur (PGCD)

Code source JavaScript :
```javascript
let a = input;
let b = input;
while (0 < b)
do (let aux=(a mod b); let a=b; let b=aux );
output a ;
```

Code généré en langage assembleur :
```asm
DATA SEGMENT
	a DD
	b DD
	aux DD
DATA ENDS

CODE SEGMENT
	in eax
	mov a, eax
	in eax
	mov b, eax
debut_while_0:
	mov eax, 0
	push eax
	mov eax, b
	pop ebx
	sub ebx, eax
	jl vrai_jl_1
	mov eax, 0
	jmp fin_jl_1
vrai_jl_1:
	mov eax, 1
fin_jl_1:
	jz fin_while_0
	mov eax, a
	push eax
	mov eax, b
	pop ebx
	mov ecx, eax
	mov eax, ebx
	div ebx, ecx
	mul ebx, ecx
	sub eax, ebx
	mov aux, eax
	mov eax, b
	mov a, eax
	mov eax, aux
	mov b, eax
	jmp debut_while_0
fin_while_0:
	mov eax, a
	out eax
CODE ENDS
```

## Problèmes connus

- **Partie récursive non implémentée :** Le programme ne gère pas encore les appels récursifs dans les expressions ou les fonctions.
