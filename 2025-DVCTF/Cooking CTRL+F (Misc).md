> Le Flag est dans ce document, comment le retrouver ?
> 
> J'ai remarqué que le document avait un nom spécial, peut être un indice ?
> 
> Flag format : DVC{...}
> R3g-EX.txt

Utiliser les expression régulière (regex), programme python par exemple :
```
import re

# Chemin vers le fichier
file_path = 'R3g-EX.txt'

# Regex : DVC{ suivi de quelque chose qui n'est pas }
pattern = re.compile(r'DVC\{[^}]+')

# Lecture du fichier
with open(file_path, 'r', encoding='utf-8') as file:
    lines = file.readlines()

# Recherche ligne par ligne
for line_num, line in enumerate(lines, start=1):
    matches = pattern.findall(line)
    for match in matches:
        print(f"Ligne {line_num}: {match}")
```

<details>
<summary>Flag</summary>

`DVC{I_l0V3_R3G3X}`

</details>
