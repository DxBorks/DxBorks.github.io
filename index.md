---
layout: default
---

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](another-page).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# [](#header-1)Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## [](#header-2)Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### [](#header-3)Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### [](#header-4)Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### [](#header-5)Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### [](#header-6)Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
Partie 1 : Introduction

Préface
Comprendre DxBorks
Installer DxBorks
Partie 2 :

Préface :

Le système d'exploitation DxBorks est une longue quête de succès d'un an. Né de l'envie de créer un Kernel minimaliste pour les appareils x86, l'OS a évolué en un projet avec pleins de possibilités, tout en gardant sa "preuve de concept" en tant qu'autre chose qu'un OS commercial. Agréé sous la License Publique Générale (GPL) GPU v3, tout le monde est libre d'utiliser cet OS comme ils veulent, dans la mesure où ils n'empêchent pas d'autres utilisateur de jouir de la même liberté. 

DxBorks est un logiciel assez minimaliste. Il suit un paradigme important à mes yeux : KISS (Keep It Simple, Stupid !)/(Garde ça Simple, Imbécile); un paradigme utilisé par beaucoup de développeurs, ingénieurs et autres individus qui conçoivent du hardware/software complexes, dont l'équipe de développement d'Arch Linux ou les ingénieurs de l'armée Américaine. 

Ce paradigme donne la priorité à la simplicité plutôt qu'à la complexité : un bout de code devrait être assez facile à comprendre pour qu'il n'y ait pas l'usage d'un nombre trop important de commentaires. La "Philosophie UNIX" a aussi été un aspect très fondamental du projet : un outil devrait servir à une seule chose, mais le faire bien. Dans notre cas, les outils sont les fonctions, variables, algorithmes, etc... 

Pour résumer, DxBorks est un autre Kernel inspiré par UNIX, tout en gardant son aspect de projet scolaire 



Comme indiqué plus haut, ce document, le Kernel DxBorks, son image disque, et les contenus de son répertoire en ligne sont agrées sous la licence GNU GPL v3. Référez vous au fichier fourni pour plus d'infomations concernant la licence. Pour plus d'informations, contactez "garuda1@protomail.com"




Comprendre DxBorks

DxBorks, en utilisant les philosophies KISS et UNIX, reste un système d'exploitation, c'est à dire qu'il va quand même être un logiciel complexe. Donc, il requiert une compréhension profonde de l'architecture x86, en même temps que quelques années d'expérience en C et en programmation d'Assembleur. Sachant que la plupart des utilisateurs n'auront surement pas ces qualifications, ce document va présenter une grande insistance sur l'explication dee la mécanique interne de l'OS DxBorks grâce à des exemples, analyse de code, et des transcriptions algorithmiques détaillées de parties particulières du code pour aider les utilisateurs à comprendre l'OS. 

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

$ make	Compile DxBorks
$ make clean	Supprime l'objet et les fichiers binaire
$ make re	Alias pour $ make clean && make
$ make test	Lance une machine virtuelle qemu faisant tourner DxBorks
Vous devriez peut-être aussi comprendre l'arborescence de répertoire :

Répertoire principal -->
Fichiers binaires -->
Fichiers d'image disque
Fichiers compilés et image disque .iso
Code source Kernel -->
Fichiers source (.c et .s) -->
Fichiers source i386 -->
Sources libk (Kernel Library) -->
...
Code qui permet d'initialiser DxBorks -->
...
...
En-têtes C (.h) -->
En-têtes Kernel -->
...
...
...
Cette arborescence de répertoire est, tout comme DxBorks lui même, minimaliste. Remarque : L'arborescence de répertoire peut changer d'une verion à une autre, donc ne basez pas vos commandes dessus, mais analysez le plutôt pour comprendre la structure des sources. 

Après avoir compilé le programme, une image disque .iso sera produite et stockée dans le directoire /src/bin . Cette image peut être ouverte soit directement en utilisant le test make
