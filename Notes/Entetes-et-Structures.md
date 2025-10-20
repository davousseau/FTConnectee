# Entêtes & Structures

Un projet WPF (Windows Presentation Foundation) est un framework de développement d'applications qui utilise le XAML et le C# pour créer des applications de bureau riches et modernes sous Windows. Un fichier XAML (Extensible Application Markup Language) est un langage de balisage déclaratif basé sur XML, utilisé par Microsoft pour définir l'interface utilisateur (UI) d'une application de manière visuelle et structurée.

## Entête des fichiers

Le fichier XAML contient la Vue (View), décrivant la structure et l'apparence de l'interface utilisateur.

__Exemple :__

```xml
<Window x:Class="MyProject.Views.Windows.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"

        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"

        xmlns:local="clr-namespace:MyProject"
        xmlns:models="clr-namespace:MyProject.Models"
        xmlns:viewmodels="clr-namespace:MyProject.ViewModels"

        mc:Ignorable="d"

        Title="MainWindow" Height="450" Width="800"

        d:DesignHeight="600" d:DesignWidth="800">

    <!--Déclaration de la fenêtre principale.-->

</Window>
```

__Remarque :__

- x:Class : lie le fichier XAML à son fichier .xaml.cs (code-behind).
- xmlns : espace de noms (namespace) principal pour les éléments WPF (Button, Grid, etc.).
- xmlns:x : permet d’utiliser des fonctionnalités XAML comme `x:Name`, `x:Key`, `x:Class`.
- xmlns:d : utilisé uniquement à des fins de design dans *Visual Studio* ou *Blend*.
- xmlns:mc : permet de gérer la compatibilité entre les versions de XAML.
- xmlns:{prefix} : permet de référencer des classes ou des espaces de noms spécifiques.

Les attributs `d:` permettent de spécifier des valeurs exclusivement destinées à l’environnement de conception, facilitant ainsi les tests visuels sans influencer le comportement de l’application à l’exécution. Pour éviter qu’ils soient interprétés à tort comme des membres réels lors de la compilation, l’attribut `mc:Ignorable="d"` doit être ajouté afin qu’ils soient ignorés lors de la génération du build.

---

Le fichier C# associé (.xaml.cs) contient le Code-Behind, gérant la logique d'interaction et les événements de l'interface.

__Exemple :__

```csharp
// Directives du système (System.*) - Les plus générales.
using System; 
using System.Collections.Generic;
using System.ComponentModel;
using System.Runtime.CompilerServices;

// Directives des librairies tierces (Exemple simulé : gestion du JSON) - Optionnel.
using Newtonsoft.Json; 

// Directives locales du projet (MyProject.*) - Les plus spécifiques.
// Souvent, le Model n'en a pas, mais un ViewModel pourrait en avoir.
// using MyProject.Services;

namespace MyProject.Models
{
    // Composition du modèle de données.
}
```

__Remarque :__

La directive using permet d'importer un espace de noms (namespace) dans votre fichier C#. Elle a pour but de vous éviter de taper le nom complet du namespace chaque fois que vous faites référence à une classe qui s'y trouve.

Par exemple : Au lieu d'écrire `System.Windows.Controls.Button`, vous pouvez simplement écrire `Button` après avoir inclus using `System.Windows.Controls`.

## Pourquoi les lignes vides sont recommendées ?

| Contexte | Recommandation | Justification |
|:---|:---|:---|
| XAML (xmlns) | Séparer les groupes de xmlns (Framework, IDE, Local) par une ligne vide. | Améliore la maintenabilité et la lisibilité en organisant les déclarations par leur origine, ce qui est crucial dans les en-têtes XAML qui peuvent devenir longs. |
| C# (using) | Séparer les groupes de using (Système, Tiers, Local) par une ligne vide, puis les trier par ordre alphabétique. Retirer systématiquement les directives non utilisées. | Améliore la lisibilité en créant des blocs logiques et permet de distinguer rapidement les dépendances externes des dépendances internes. |

## Structure complète des dossiers

Cette structure respecte les bonnes pratiques et les conventions établies par Microsoft pour les projets WPF en MVVM, avec une organisation modulaire et une séparation claire des responsabilités.

```
/YourProjectName
├── /Properties                             ← Métadonnées et fichiers de configuration du projet.
│   └── {Voir : Paramètre & Localisation}
├── /Behaviors                              ← Comportements personnalisés pour étendre les contrôles XAML.
│   └── UserSelectionCommandBehavior.cs     ← Ajoute un ICommand à l'événement de sélection utilisateur.
├── /Commands                               ← Implémentations concrètes du patron ICommand.
│   └── RelayCommand.cs                     ← Commande de base pour les ViewModels.
├── /Constants                              ← Constantes globales (chemins, chaînes de connexion, clés).
│   └── AppConstants.cs                     ← Regroupe toutes les valeurs constantes de l'application.
├── /Converters                             ← IValueConverter/IMultiValueConverter pour transformer des données UI.
│   └── UserRoleToBooleanConverter.cs       ← Convertit un rôle d'utilisateur en booléen (ex. : IsEnabled).
├── /CustomControls                         ← Contrôles hérités de System.Windows.Controls.Control
│   └── SplitButton.cs                      ← Exemple d'un contrôle avec une apparence/logique unique.
├── /Enums                                  ← Types énumérés pour la logique métier et les états.
│   └── UserRoleEnum.cs                     ← Définit les différents rôles d'utilisateur dans l'application.
├── /Helpers                                ← Méthodes utilitaires générales et statiques sans état.
│   └── UserValidatorHelper.cs              ← Logique pour valider les propriétés d'un utilisateur.
├── /Interfaces                             ← Contrats (interfaces) pour le découplage via Injection de Dépendances.
│   └── IMessenger.cs                       ← Contrat pour la communication ViewModel-à-ViewModel.
├── /Messages                               ← Classes de données (payloads) utilisées par le Messenger.
│   └── UserUpdatedMessage.cs               ← Message envoyé lorsqu'un utilisateur est mis à jour.
├── /Models                                 ← Entités métier et structures de données (pas de logique UI).
│   └── User.cs                             ← Représentation de l'entité utilisateur (données métier).
├── /Resources                              ← Ressources statiques de l'application (localisations, thèmes, etc.).
│   ├── /Assets                             ← Images, icônes, polices, etc.
│   └── /Themes                             ← Styles visuels pour les thèmes (couleurs, polices, contrôles).
│       └── {Voir : Thèmes & Styles}
├── /Services                               ← Implémentations concrètes de l'accès aux données, de la localisation, etc.
│   ├── LocalizationService.cs              ← Fournit les traductions des chaînes de texte et gère le changement de langue.
│   └── MessengerService.cs                 ← Implémentation du service de messagerie (ex: Event Aggregator).
├── /ViewModels                             ← Logique de présentation (MVVM) et gestion de l'état de la Vue.
│   ├── MainWindowViewModel.cs              ← ViewModel principal pour la fenêtre principale.
│   └── UserProfilViewModel.cs              ← ViewModel pour la gestion du profil utilisateur.
├── /Views                                  ← Interfaces utilisateur (XAML) et Code-Behind minimal.
│   ├── /UserControls                       ← Composants réutilisables (parties de fenêtres).
│   │   └── UserProfilControl.xaml          ← Contrôle utilisateur pour afficher le profil.
│   └── /Windows                            ← Fenêtres principales (l'enveloppe de l'interface).
│       ├── MainWindow.xaml                 ← Fenêtre principale de l'application.
│       └── MainWindow.xaml.cs              ← Code-behind minimal (souvent juste pour l'instanciation/DI).
├── App.xaml                                ← Point d'entrée de l'application (ressources globales).
├── App.xaml.cs                             ← Logique de démarrage, configuration du conteneur DI.
└── YourProjectName.csproj                  ← Fichier de projet, généré automatiquement par le SDK.
```
