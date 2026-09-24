
# Chapitre 02 — Exo 7 : les dépendances triées

## La liste de départ

La démonstration du moteur est `NKXRDemo`, dans le dépôt Nkentseu : `Applications/NKXRDemo/NKXRDemo.jenga`. Son appel `nkentseudependson([...])` déclare vingt-trois modules :

```python
nkentseudependson(
    ["NKXR", "NKRenderer", "NKRHI", "NKSL", "NKGLSlang", "NKSPIRVCross",
     "NKSerialization", "NKReflection", "NKFileSystem", "NKFont", "NKImage", "NKGlad",
     "NKEvent", "NKWindow", "NKMath", "NKTime", "NKLogger", "NKStream",
     "NKContainers", "NKMemory", "NKCore", "NKPlatform", "NKThreading"],
    ...
)
```

Le préfixe `NK` (Nkentseu) est commun à tous ; je le mets de côté pour juger chaque nom.

## Groupe 1 — le nom suffit à deviner le rôle (13)

| Module | Rôle deviné |
|---|---|
| NKRenderer | le rendu graphique de la scène |
| NKWindow | la création et la gestion de la fenêtre |
| NKEvent | les événements (clavier, souris, fenêtre) |
| NKMath | vecteurs, matrices, quaternions |
| NKTime | la mesure du temps, les horloges, le temps entre deux images |
| NKLogger | la journalisation des messages |
| NKFont | le chargement et le rendu des polices de caractères |
| NKImage | le chargement et le décodage des images |
| NKFileSystem | l'accès aux fichiers et aux dossiers |
| NKSerialization | l'enregistrement et la relecture de données (sauvegarde) |
| NKContainers | les structures de données : tableaux, listes, tables |
| NKMemory | l'allocation et la gestion de la mémoire |
| NKThreading | les fils d'exécution et leur synchronisation |

## Groupe 2 — une idée, sans certitude (5)

| Module | Ce que je suppose | Ce qui me manque |
|---|---|---|
| NKXR | la réalité étendue (casque, suivi de tête, rendu par œil) | le périmètre exact : casque réel, simulateur, ou les deux |
| NKCore | les briques de base communes à tous les modules | ce qu'il contient vraiment, « core » peut tout vouloir dire |
| NKPlatform | l'adaptation à chaque système d'exploitation | s'il détecte seulement la plateforme ou s'il fournit aussi des services |
| NKReflection | l'introspection des types C++ à l'exécution | à quoi elle sert dans une démo XR |
| NKStream | des flux de lecture et d'écriture | s'il s'agit de fichiers, de réseau ou de mémoire |

## Groupe 3 — je n'en savais rien (5)

Pour chacun, j'ai ouvert l'en-tête principal du module. Deux de ces en-têtes (`NkRHI.h` et `NKSL.h`) sont des en-têtes « parapluie » qui regroupent des `#include` ; pour NKRHI, j'ai donc aussi regardé les dossiers du module. Les trois autres modules sont des sous-modules Git : leurs fichiers se trouvent dans des dépôts séparés.

| Module | En-tête ouvert | Une phrase |
|---|---|---|
| NKRHI | `Kernel/Runtime/NKRHI/src/NKRHI/NkRHI.h` | NKRHI (Render Hardware Interface) est la couche qui cache les API graphiques derrière une interface commune (périphérique, tampons de commandes, structures de description). Son en-tête n'est qu'une liste d'`#include` ; ce sont ses dossiers qui montrent un backend par API : Vulkan, DirectX 11, DirectX 12, Metal, OpenGL et un rendu logiciel (Software). |
| NKSL | `Kernel/Runtime/NKSL/src/NKSL/NKSL.h` | NKSL (Nkentseu Shader Language) est le langage de shaders du moteur et son compilateur, comme l'annonce son en-tête. Il peut s'appuyer sur glslang (GLSL vers SPIR-V) et sur SPIRV-Cross (SPIR-V vers GLSL, HLSL ou MSL) pour qu'un shader écrit une seule fois donne le format attendu par chaque API. |
| NKGLSlang | `Externals/Libs/NKGLSlang/glslang/Public/ShaderLang.h` | NKGLSlang est glslang, le compilateur de référence de Khronos, repris du projet d'origine (l'en-tête porte encore les droits de 3Dlabs, LunarG et Google) pour analyser les shaders GLSL et les compiler en SPIR-V (classes `TShader` et `TProgram`). |
| NKSPIRVCross | `Externals/Libs/NKSPIRVCross/src/spirv_cross/spirv_cross.hpp` | NKSPIRVCross est SPIRV-Cross, le projet de Khronos, qui lit un shader déjà compilé en SPIR-V (classe `Compiler`) pour le retraduire en GLSL, HLSL ou MSL et en extraire la description de ses ressources. |
| NKGlad | `Externals/Libs/NKGlad/include/glad/gl.h` | NKGlad est un chargeur OpenGL généré par glad 2.0.8 : au démarrage, `gladLoadGL` va chercher dans le pilote l'adresse de chaque fonction OpenGL 4.6 pour qu'on puisse les appeler. |

## Ce que le tri m'apprend

Le groupe 3 n'est pas pris au hasard : ses cinq modules forment la chaîne qui mène un shader jusqu'à la carte graphique. NKSL est le langage de shaders et son compilateur ; il peut s'appuyer sur NKGLSlang pour compiler en SPIR-V et sur NKSPIRVCross pour retraduire ce SPIR-V pour DirectX ou Metal. NKSL ne connaît pas NKRHI : c'est NKRHI qui convertit la sortie de NKSL et l'envoie à la bonne API, et NKGlad rend les fonctions OpenGL appelables.

Trois d'entre eux ne sont même pas écrits par Nkentseu : NKGLSlang, NKSPIRVCross et NKGlad sont des bibliothèques externes (Khronos et glad), présentes dans le dépôt sous forme de sous-modules Git. Leur nom de module ne dit rien, mais le nom du projet d'origine qu'il cache, lui, se retrouve facilement.

Un dernier détail explique la présence de deux d'entre eux dans la liste : NKGLSlang et NKSPIRVCross sont nommés explicitement dans `NKXRDemo.jenga` parce que, d'après le commentaire du fichier, l'éditeur de liens d'un exécutable qui tire NKSL et NKRHI ne les récupère pas de façon transitive.

## Références consultées

- [`NKXRDemo.jenga` : la liste des vingt-trois modules et le commentaire sur NKGLSlang et NKSPIRVCross](https://github.com/Rihen-Universe/Nkentseu/blob/main/Applications/NKXRDemo/NKXRDemo.jenga)
- [`.gitmodules` : NKGLSlang, NKSPIRVCross et NKGlad sont des sous-modules Git](https://github.com/Rihen-Universe/Nkentseu/blob/main/.gitmodules)
