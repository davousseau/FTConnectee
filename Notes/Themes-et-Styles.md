# Thèmes & Styles

Pour garantir la modularité et la maintenabilité de vos thèmes, la méthodologie en couches repose sur une stricte séparation des responsabilités : les `Couleurs` définissent les valeurs brutes, les `Pinceaux` les intègrent, et les `Styles` les consomment.

## Structure des dossiers

```
/Themes                ← Styles visuels pour les thèmes (couleurs, polices, contrôles).
├── /Dark              ← Ressources de style spécifiques pour le thème sombre.
├── /Light             ← Ressources de style spécifiques pour le thème clair.
│   ├── Colors.xaml    ← Définitions de la palette de couleurs de base (nommées).
│   ├── Brushes.xaml   ← Définitions des brosses basées sur les couleurs nommées.
│   ├── Button.xaml    ← Styles par défaut (Template) pour le contrôle Button.
│   ├── TextBox.xaml   ← Styles par défaut (Template) pour le contrôle TextBox.
│   └── Theme.xaml     ← Point d'entrée des ressources pour le thème clair.
└── Generic.xaml       ← Obligatoire pour les styles par défaut des Custom Controls.
```

## Qu'est-ce qu'un dictionnaire de ressources ?

Un dictionnaire de ressources (`ResourceDictionary`) est un conteneur en XAML utilisé pour regrouper et réutiliser des objets à travers une application WPF. Les fichiers contenus dans le dossier `/Themes/` sont notamment des dictionnaires de ressources qui permettent d'organiser tous les éléments réutilisables du *look* and *feel* de l'application. Leur rôle est de centraliser les définitions des styles, des modèles de contrôle (`ControlTemplate`), des pinceaux et des couleurs pour faciliter la thématisation.

__Exemple :__

```xml
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <!--Définitions des ressources.-->

</ResourceDictionary>
```

## Colors.xaml

Ce fichier contient des ressources de type `Color`, c’est-à-dire des définitions de couleurs brutes (hexadécimales) sans opacité ni transformation.

__Nomenclature de la clé :__ `{Role}.{Control}.{State}.{Property}.Color`

Le point (`.`) est utilisé comme un séparateur logique pour créer une structure hiérarchique et descriptive, rendant la clé de couleur facile à lire et à organiser.

| Élément | Exemple de valeurs |
| :--- | :--- |
| __`{Role}`__ | `Primary`, `Accent`, `Default`, `Error`, `Warning` |
| __`{Control}`__ | `Button`, `TextBox`, `Grid`, `Window` |
| __`{State}`__ | `Normal`, `Hover`, `Pressed`, `Disabled`, `Focused` |
| __`{Property}`__ | `Background`, `Foreground`, `Border` |

__Exemple :__

```xml
<Color x:Key="Error.TextBox.Normal.Border.Color">#FFE53935</Color>
<Color x:Key="Primary.Button.Normal.Background.Color">#FF2196F3</Color>
<Color x:Key="Primary.Button.Hover.Background.Color">#FFFFC107</Color>
````

## Brushes.xaml

Ce fichier contient des ressources de type `Brush`, c'est-à-dire des définitions de pinceaux prêts à l'emploi.

__Nomenclature de la clé :__ `{Role}{Control}{State}{Property}Brush`

L'utilisation du `PascalCase` sans séparateur est une convention courante pour les pinceaux.

__Exemple :__

```xml
<SolidColorBrush x:Key="ErrorTextBoxNormalBorderBrush"
                 Color="{StaticResource Error.TextBox.Normal.Border.Color}" />
<SolidColorBrush x:Key="PrimaryButtonNormalBackgroundBrush"
                 Color="{StaticResource Primary.Button.Normal.Background.Color}" />
<SolidColorBrush x:Key="PrimaryButtonHoverBackgroundBrush"
                 Color="{StaticResource Primary.Button.Hover.Background.Color}" />
```

## Styles.xaml

Ce fichier contient des ressources de type `Style`, c'est-à-dire des définitions de styles qui
applique les pinceaux aux propriétés des contrôles.

__Nomenclature de la clé :__ `{Role}{Control}Style`

La balise `<style>` permet d'appliquer plusieurs définitions de style à la fois, ce qui raccourcit considérablement la clé `x:Key` d'un contrôle.

__Exemple :__

```xml
<Style x:Key="PrimaryButtonStyle" TargetType="{x:Type Button}">
    <Setter Property="Background"
            Value="{StaticResource PrimaryButtonNormalBackgroundBrush}" />
    <Setter Property="Foreground"
            Value="{StaticResource PrimaryButtonNormalForegroundBrush}" />
    <Setter Property="Padding" Value="10,5" />

    <Style.Triggers>
        <Trigger Property="IsMouseOver" Value="True">
            <Setter Property="Background"
                    Value="{StaticResource PrimaryButtonHoverBackgroundBrush}" />
        </Trigger>
    </Style.Triggers>
</Style>
```

__Remarque :__

Si la clé n'est pas spécifiée, le style s'applique à tous les contrôles du type défini.

## Theme.xaml

Ce fichier est le point d'entrée unique de votre thème (sombre ou clair) qui gère la fusion en cascade de toutes les ressources du thème (Couleurs => Pinceaux => Styles de contrôles) afin de faciliter la bascule de thème via le code C#.

__Exemple :__

```xml
<ResourceDictionary.MergedDictionaries>
    <ResourceDictionary Source="Colors.xaml" />
    <ResourceDictionary Source="Brushes.xaml" />
    <ResourceDictionary Source="Button.xaml" />
    <ResourceDictionary Source="TextBox.xaml" />
</ResourceDictionary.MergedDictionaries>
```

## Generic.xaml

Ce fichier est __obligatoire__ et est recherché par le framework WPF à la racine du dossier `/Themes/` par convention. Il sert de point d'ancrage en définissant les styles implicites et les `ControlTemplates` par défaut pour vos contrôles personnalisés.

__Exemple :__

```xml
<Style TargetType="{x:Type local:MyCustomControl}">

    <Setter Property="Background" Value="Transparent" />
    <Setter Property="BorderBrush" Value="Gray" />
    <Setter Property="BorderThickness" Value="1" />
    <Setter Property="HorizontalContentAlignment" Value="Center" />
    <Setter Property="VerticalContentAlignment" Value="Center" />

    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="{x:Type local:MyCustomControl}">
                <Border CornerRadius="5" 
                        Background="{TemplateBinding Background}"
                        BorderBrush="{TemplateBinding BorderBrush}"
                        BorderThickness="{TemplateBinding BorderThickness}">

                    <ContentPresenter HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}" 
                                      VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```
