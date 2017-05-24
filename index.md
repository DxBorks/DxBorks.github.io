Préface

Le système d'exploitation DxBorks est une longue quête de succès d'un an. Né de l'envie de créer un Kernel minimaliste pour les appareils x86, l'OS a évolué en un projet avec pleins de possibilités, tout en gardant sa "preuve de concept" en tant qu'autre chose qu'un OS commercial. Agréé sous la License Publique Générale (GPL) GPU v3, tout le monde est libre d'utiliser cet OS comme ils le souhaitent, dans la mesure où ils n'empêchent pas d'autres utilisateurs de jouir de la même liberté.

DxBorks est un logiciel assez minimaliste. Il suit un paradigme important à nos yeux : KISS (Keep It Simple, Stupid !)/(Garde ça Simple, Imbécile); un paradigme utilisé par beaucoup de développeurs, ingénieurs et autres individus qui conçoivent du hardware/software complexe, dont l'équipe de développement d'Arch Linux ou les ingénieurs de l'armée Américaine.

Ce paradigme donne la priorité à la simplicité plutôt qu'à la complexité : un bout de code devrait être assez facile à comprendre pour qu'il n'y ait pas l'usage d'un nombre trop important de commentaires. La "Philosophie UNIX" a aussi été un aspect très fondamental du projet : un outil devrait servir à une seule chose, mais le faire bien. Dans notre cas, les outils sont les fonctions, variables, algorithmes, etc...

Pour résumer, DxBorks est un autre Kernel inspiré par UNIX, tout en gardant son aspect de projet scolaire.


Comme indiqué plus haut, ce document, le Kernel DxBorks, son image disque, et les contenus de son répertoire en ligne sont agrées sous la licence GNU GPL v3. Référez vous au fichier fourni pour plus d'infomations concernant la licence. Pour plus d'informations, contactez "garuda1@protomail.com"


[Download DxBorks](https://github.com/DxBorks/DxBorks)

[Instruction set](https://dxborks.github.io/instructions.html)


Comprendre DxBorks


DxBorks, même s'il emprunte les philosophies KISS et UNIX, reste un système d'exploitation, c'est à dire qu'il va quand même être un logiciel complexe. Il requiert donc d'avoir une compréhension profonde de l'architecture x86, en même temps que quelques années d'expérience en C et en programmation d'Assembleur. Sachant que la plupart des utilisateurs n'auront surement pas ces qualifications, ce document va présenter une grande insistance sur l'explication de la mécanique interne de l'OS DxBorks grâce à des exemples, analyses de code, et des transcriptions algorithmiques détaillées de parties particulières du code pour aider les utilisateurs à comprendre l'OS.

Néanmoins, cette documentation n'est PAS un tutoriel en programmation. Pour le moment, les choses suivantes sont considérées acquises :

    Le lecteur a une compréhension basique de la programmation syntaxe assembleur x86.
    Le lecteur a des connaissances avancées de la programmation en langage C, l'interface POSIX, l'architecture x86 et 32 bits, et la programmation makefile.
    Le lecteur a accès à une machine capable de lancer DxBorks, ainsi que l'OS DxBorks lui même, et a une compréhension avancée de ce logiciel

Pour comprendre complètement le code, vous pouvez considérer de bricoler avec. Le code source de DxBorks est disponible sur GitHub, au lien https://github.com/Garudal/DxBorks . Pour reproduire le code source du lien vers une machine basée sur UNIX en utilisant git, utilisez la commande suivante :


$ git clone https://github.com/Garudal/DxBorks

Faire ceci téléchargera un dossier dans le répertoire de travail actuel contenant la dernière version du code source du système d'exploitation DxBorks. Les instructions de compilation sont décrites plus tard dans cette documentation.


Installer DxBorks


DxBorks, en tant qu'OS en open source (ou plus particulièrement gratuit), peut être compilé et installé par l'utilisateur, alors que la plupart des autres systèmes d'exploitation sont distribués avec un logiciel d'installation automatisée. Comme cela contredit l'aspect minimaliste de DxBorks, aucun logiciel d'installation automatisée ne sera fourni
Compiler DxBorks demande plusieurs composants différentes, dont :

    Un système hôte UNIX (on utilisera Arch Linux)
    GNU make
    Le GCC (Gnu Compiler Collection) compilateur en C (gcc)
    L'assembleur GNU standard (as)
    Le chargeur d'amorçage (bootloader) GRUB2
    Une connection internet :)

Plusieurs autres outils sont requis, vous serez donc notifiés durant la compilation quels composants manquent dans le système hôte.

Vous devrez d'abord acquérir les fichiers source DxBorks . Si vous utilisez la version CD (Compact Disc), le code source DxBorks est situé dans /src/, autrement l'utilisateur devra le télécharger en utilisant git.


$ git clone https://github.com/Garudal/DxBorks

Note : Assurez vous de garder une sauvegarde du code source. Il vous faudra peut-être recommencer toute la compilation si vous ratez ne serait-ce qu'une partie de cette dernière.

Vous pouvez dorénavant naviguer vers le répertoire source. Pour des raisons pratiques, on présumera qu'il est situé dans :
/home/user/Documents/src.

Le processus de compilation est simple : un Makefile est situé dans le fichier src, qui automatise le processus de compilation. Notez que le Makefile utilise des extensions GNU, donc vous pourriez aussi télécharger le Gnu make en utilisant votre gestionnaire de paquets. Utilisez donc tout simplement les commandes suivantes :

$ make     Compile DxBorks
$ make clean     Supprime l'objet et les fichiers binaire
$ make re     Alias pour "$ make clean && make"
$ make test     Lance une machine virtuelle qemu faisant tourner DxBorks

Vous pouvez aussi comprendre l'arbre de répertoire suivant : 


Annexe 1



Après avoir compilé le programme, une image disque .iso sera produite et stockée dans le répertoire /src/bin. Cette image peut soit être démarrée directement en utilisant le cible make test, ou copiée sur un objet (physique) pour la démarrer sur une autre machine. Pour ce faire, utilisez la commande : 
$ dd if=$SRC/bin/$ISO of=/dev/$DEV

Où : 
$SRC est le chemin pour le dossier src
$ISO est le nom de l'image disque .iso
$DEV est le nom du dispositif de bloc cible

Démarrage

Allumer un système d'exploitation n'est pas une tâche facile. Pour comprendre pourquoi, nous devons d'abord comprendre le processus de démarrage de la plupart des appareils x86. Ce chapitre sera dédié à la compréhension de ce processus.
Quand un utilisateur allume un appareil, un circuit commence à alimenter du courant dans les différentes parties qui composent le dit appareil. Ceci va déclencher un contrôle de matériel non blocable appelé POST (Power On Self Test), qui va (en gros) s'assurer que tout est OK (surtout pour checker le système d'expédition de circuit. Mais après que le POST est fini, qu'est-ce qu'il se passe ?
Maintenant, presque rien n'est initialisé. Le(s) unité(s) de traitement fonctionnent toujours en mode réel 16 bits, rien n'est chargé, et quelque chose doit  trouver une solution aux premières étapes de l'initialisation. Ce bout de logiciel est appelé le BIOS (Basic Input Output System). Même si ceci est remplacé par un étalon beaucoup plus complexe appelé UEFI (Unified Extended Firmware Interface), presque tous les appareils x86 ont un BIOS. Le logiciel BIOS est situé sur une puce de mémoire flash sur l'appareil, ce qui veut dire que c'est presque impossible de jouer avec (même si dans les ordinateurs modernes ont souvent des façons pour mettre à jour le logiciel BIOS). Le BIOS va trouver des solutions à quelques étapes simples : Dire à l'ordinateur quels appareils sont connectées, combien de mémoire est disponible, coordonner différentes parties ensemble, etc. Après avoir fait cela, presque tout est près pour allumer le système d'exploitation.
L'ordinateur va chercher un périphérique d'amorçage. Un périphérique d'amorçage est un périphérique dont les octets situés sur les coordonnées 0x1FE et 0x1FF figurent la valeur 0x55AA. Le BIOS va chercher pour trouver un appareil comme cela. La première qu'il va trouver s'appelle le périphérique sélectionné, et ses premiers 512 octets seront copiés à l'adresse mémoire 0x7C00 avant d'être exécuté. Et à ce moment là, l'exécution du programme commence.
Pourtant, DxBorks n'est pas situé sur le secteur de démarrage (les 512 premiers octets de l'appareil). Plutôt, un programme appelé bootloader est utilisé dans ce secteur. DxBorks utilise GRUB2, un bootloader bien connu utilisé par presque tous les dérivés d'UNIX aujourd'hui. Ce programme va initialiser beaucoup de caractéristiques utiles pour nous, comme passer les unités de traitement de 16 bits mode réel à 32 bits mode protégé. GRUB2 va ensuite permettre à l'utilisateur de sélectionner le système d'exploitation qu'il veut démarrer. Notez que l'utilisateur ne pourra pas revenir en arrière lorsque l'OS est choisi sans redémarrer l'appareil. Notez aussi que même si GRUB2 permet à l'utilisateur de choisir un OS, cette fonctionnalité a été désactivée sur le CD d'allumage DxBorks, parce que DxBorks est le seul OS qui n'existe pas sur l'appareil.
Le langage de programmation C, utilisé pour DxBorks, ne génère pas des binaires "linéaires" (comme les programmes .COM sous Microsoft DOS). Voilà le contenu du dossier linker.ld, décrivant la disposition du Kernel dans la mémoire :
Annexe 2
Ce code est utilisé comme un patron pour l'éditeur de liens, lui disant à quoi le programme va ressembler à partir du point de vue de la mémoire. La ligne . = 0x100000; est celle qui nous importe le plus : elle dit à l'éditeur de liens que le programme sera chargé à l'adresse 0x100000 dans la mémoire. Nous avons donc la disposition de mémoire suivante :
Annexe 3
L'exécution commencera à l'adresse Ox100000. Pourtant, comme dit précédemment, le langage de programmation en C ne génère pas des fichiers binaires linéaires. Cela veut dire que le point d'entrée ne sera pas situé à l'adresse 0x100000, où l'exécution est supposée commencer. Nous devons donc utiliser le code assembleur qui sera situé à cette adresse et qui va transférer l'exécution à notre point d'entrée désigné. Ce code assembleur est appelé le boot code. Le dossier $SRC/kernel/arch/i386/boot/boot.s contient ce code.
Annexe 4
La première ligne est utilisée pour indiquer que l'étiquette _start est une étiquette globale, c'est à dire qu'elle peut être appelée globalement (de n'importe quel dossier). La seconde ligne indique que l'étiquette _start est en fait une fonction. Les lignes 4 à 14 sont spéciales : elles sont nécessaires pour le multiboot standard. Elles donnent l'information par rapport à comment le Kernel devrait démarrer. 
Les lignes 16 à 19 et 25 sont dédiés à mettre en place la mémoire de pile. Ceci est nécessaire pour utiliser les instructions push et pop, vitales à tout programme assez sophistiqué. Notez que l'étiquette pile est située après avoir sauté 65536 octets, parce que dans l'architecture x86, la pile croît "vers le bas"
Nous procédons donc vers le véritable code de démarrage :
Annexe 5
Ce code constitue la partie la plus importante de tout l'OS. Sans lui, rien ne pourrait démarrer, et même si on pouvait démarrer des fonctions basiques comme utiliser le clavier ou les ports série, cela échouerait misérablement. Utiliser les instructions push/pop aurait aussi des résultats vagues, car la value contenue dans le pointeur de pile est soit peanuts soit nulle. Voilà une traduction algorithmique : 
Annexe 6











Annexe 1 : 


	src						Repertoire principal
	├- bin					Fichiers binaire
	│  ├- isofiles				Fichiers image disque
	│  └- ...					Fichiers compilés et image disque .iso
	├- kernel					Code source Kernel
	│  ├- arch				Fichiers source (.s et .c)
	│  │  └- i386				Fichiers source i386 
	│  │     ├- libk			sources (kernel library) libk
	│  │     │  └- ...
	│  │     ├- boot			Code qui permet à DxBorks de démarrer
	│  │     │  └- ...
	│  │     └- ...
	│  └- include				En-têtes C (.h)
	│     ├- kernel			En-têtes Kernel 
	│     │  └- ...				
	│     └- ...
	└-... 

Annexe 2 : 

1	ENTRY(_start)
2	
3	SECTIONS
4	{
5	  . = 0x100000;
6	
7	  .text BLOCK(4K) : ALIGN(4K)
8	  {
9	    *(.multiboot_header)
10	    *(.text)
11	  }
12	
13	  .rodata BLOCK(4K) : ALIGN(4K)
14	  {
15	    *(.rodata)
16	  }
17	
18	  .data BLOCK(4K) : ALIGN(4K)
19	  {
20	    *(.data)
21	  }
22	
23	  .bss BLOCK(4K) : ALIGN(4K)
24	  {
25	    *(COMMON)
26	    *(.bss)
27	  }
28	}

Annexe 3 : 

	ADRESSE			DESCRIPTION

	0x100000			Point d'entrée
	...
	0x??????			Fin du programme 


Annexe 4 : 

1	.global _start	
2	.type   _start, @function
3	
4	.set ALIGN,    1 << 0
5	.set MEMINFO,  1 << 1
6	.set FLAGS,    (ALIGN | MEMINFO)
7	.set MAGIC,    0x1BADB002
8	.set CHECKSUM, -(MAGIC + FLAGS)
9	
10	.section .multiboot_header
11	.align 4
12	.long MAGIC
13	.long FLAGS
14	.long CHECKSUM
15	
16	.section .bss
17	.align 16
18	.skip 65536
19	stack:
20	
21	.align 4
22	.section .text
23	_start:
24	  cli
25	  movl $stack, %esp
26	  call kernel_main
27	  sti
28	  hlt

.size _start, . - _start



Annexe 5 : 

23	_start:
24	  cli
25	  movl $stack, %esp
26	  call kernel_main
27	  sti
28	  hlt


Annexe 6 : 

START
	CLEAR_INTERRUPT_FLAGS
	MOVE $stack TO %esp
	CALL THE FUNCTION kernel_main
	SET_INTERRUPT_FLAGS
	HALT


