# Paramètres & Localisations

Le dossier `/Properties` est le conteneur standard pour les métadonnées et les fichiers de configuration spécifiques à l'assemblage et au projet. Il abrite traditionnellement le fichier `AssemblyInfo.cs` (désormais souvent géré via le `.csproj`), qui définit les attributs globaux comme la version et l'éditeur. On y trouve également les fichiers de ressources par défaut (`Resources.resx`) utilisés pour la localisation, ainsi que les paramètres de l'application (`Settings.settings`), garantissant l'identité et le comportement de base du programme.

## Structure des dossiers

```
/Properties                 ← Métadonnées et fichiers de configuration du projet.
├── AssemblyInfo.cs         ← Attributs de l'assemblage (version, titre, compagnie, etc.).
├── Resources.resx          ← Fichier de ressources par défaut pour l'application.
├── Resources.en.resx       ← Fichier de ressources spécifique pour la langue anglaise.
├── Resources.Designer.cs   ← Classe générée pour l'accès fortement typé aux ressources.
└── Settings.settings       ← Définit les paramètres de l'application (User et Application).
```

__Remarque :__

Le fichier `AssemblyInfo.cs` devient obsolète dans les projets .NET de style SDK (modernes) car les informations d'assemblage (telles que la version, la compagnie, et la langue neutre via la propriété `<NeutralLanguage>`) sont désormais définies de manière centralisée et déclarative dans le fichier de projet `.csproj.` Le système de construction (MSBuild) génère automatiquement les attributs nécessaires (comme `[assembly: NeutralResourcesLanguage]`), rendant l'édition manuelle dans un fichier source redondante, simplifiant ainsi la maintenance du projet et évitant les conflits de définition.

## Qu'est-ce qu'un fichier de ressources ?

Les fichiers `.resx` (Resource XML) sont des conteneurs standards qui associent des clés de ressources (utilisées dans le code) à des valeurs, notamment les traductions spécifiques à une langue. L'édition manuelle de ces fichiers est techniquement possible, mais l'utilisation du concepteur (designer) de *Visual Studio* est fortement préconisée afin de garantir l'intégrité et la gestion appropriée de tous les types de ressources.

## Ressources culturelles

L'ensemble des éléments qui changent selon la culture (localisation) de l'utilisateur est géré par les fichiers de ressources. Ces derniers ne se limitent pas aux chaînes de texte traduites, mais englobent également tout ce qui est nécessaire à une localisation complète : formats de dates et de devises, ou encore des images et icônes spécifiques à une région. Ils sont cruciaux pour une expérience utilisateur adaptée.

## Mécanisme de repli (fallback)

`Resources.{specific}.resx => Resources.{neutral}.resx => Resources.resx`

Le mécanisme de repli (fallback) garantit que l'application trouve toujours une ressource, même si la traduction exacte manque. Pour une ressource en Français Canadien, le système cherche d'abord dans `Resources.fr-CA.resx` (culture spécifique). Si la clé est absente, il se replie vers `Resources.fr.resx` (culture neutre parente). En cas d'échec, le repli ultime est `Resources.resx` (langue neutre de l'assemblage), qui sert de filet de sécurité pour afficher la version par défaut du texte sans erreur.
