# Business Central Application Language (AL)
## Comments
```pascal
// this is a comment
```

## Object numbers
All objects are assigned an unique number.

|       Range | Usage                     |
| -----------:| ------------------------- |
|          1+ | Base application objects  |
|     10.000+ | Contry specific objects   |
|     50.000+ | Customer specific objects |
|    100.000+ | Partner created objects   |
| 99.000.000+ | Microsoft defined objects |

## Create an AL Project
- Install VS Code Extension `AL Language`
- Execute `>AL: GO!`
- Edit JSON files to create a connection with the BC server
    - Properties of the JSON files are explained in [Business Central JSON Files](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-json-files)
- Download the symbols using `>AL: Download symbols` command

## Publish to Server
- Execute `>AL: Publish`

## Declare Objects
- [[Application Language Table]]
- [[Application Language Table Extension]]
- [[Application Language Page]]
- [[Application Language Page Extension]]
- [[Application Language Codeunit]]
- [[Application Language Query]]