> A strange transmission has been recorded.
>
> Something valuable lies within.
> 
> +fichier uart.sr

On commence par analyser le fichier : le fichier .sr est un fichier Sigrok (utilisé pour stocker des captures logiques de signaux).

En examinant le fichier avec PowerShell :
```
Format-Hex uart.sr | Select-Object -First 16
```
on remarque que c’est un fichier binaire, pas du texte.

En inspectant le répertoire extrait, on trouve :
```
uart_extracted/
  logic-1-1
  metadata
  version
```

Le fichier logic-1-1 contient les échantillons bruts du signal UART.

On analyse le contenu : le fichier binaire contient des octets représentant les niveaux logiques (0 ou 1) du signal.
La metadata indique :
```
samplerate = 1 MHz
probe1 = uart.ch1 (la ligne UART)
unitsize = 1
```

On décode ensuite l'UART : 

UART typique : 8N1 (1 start bit, 8 bits de données, 1 stop bit).

Avec un baudrate probable de 115200 bps, on calcule le nombre d’échantillons par bit :

samples_per_bit = samplerate / baudrate ≈ 8.68

On peut lire le signal bit par bit en détectant le start bit (0) et en reconstruisant chaque octet à partir des échantillons.

Enfin, on utilise un script Python pour décoder : 
```
import numpy as np

data = np.fromfile("uart_extracted/logic-1-1", dtype=np.uint8)

# convertir en bits logiques (0 ou 1)
bits = (data > 0).astype(int)

baudrate = 115200
samplerate = 1000000
samples_per_bit = samplerate / baudrate  # ~8.68

decoded = ""
i = 0
while i < len(bits):
    if bits[i] == 0:  # start bit
        byte = 0
        for b in range(8):
            idx = int(i + (b+1) * samples_per_bit)
            if idx < len(bits):
                byte |= bits[idx] << b
        decoded += chr(byte)
        i += int(10 * samples_per_bit)  # start + 8 + stop
    else:
        i += 1

print(decoded)
```

<details>
<summary>Flag</summary>

`0xfun{UART_82_M2_B392n9dn2}`

</details>
