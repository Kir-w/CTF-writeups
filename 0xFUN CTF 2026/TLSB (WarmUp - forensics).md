> You might know about Least Significant Bit (LSB) steganography, but have you ever heard of Third Least Significant Bit (TLSB) steganography? (Probably not, I invented it for this challenge).
> 
> + TLSB file

Sous Windows, avec PowerShell, on identifie d'abord le fichier :
Format-Hex TLSB | Select-Object -First 5

Résultat : premiers bytes 42 4D -> BMP 16x16 24-bit
- Header BMP classique = 54 bytes
- Pixels = 16 × 16 × 3 = 768 bytes
- 
Conclusion : le message est caché dans les pixels BMP, pas dans le header.

Puis, on essaie de comprendre le TLSB.
Chaque byte de pixel contient un bit caché dans le 3rd Least Significant Bit d'après l'énoncé.
On doit donc extraire le 3rd LSB de tous les pixels et reconstituer le flux de bits.

Ensuite, on extrait le bitstream. On ignore le header BMP (54 bytes), on lit tous les pixels (24-bit, ordre BMP = BGR) et on extrait le 3rd LSB de chaque byte consécutivement.
On recompose les bits en octets dans l’ordre du flux (pas de permutation ou inversion). Avec Python : 

```
# Extraction du flag 0xfun{...} depuis le BMP 16x16 fourni

with open("TLSB", "rb") as f:
    data = f.read()

# Taille header BMP classique
header_size = 54
pixel_data = data[header_size:]  # on prend uniquement les pixels

# Extraire le 3rd LSB de chaque byte
bits = []
for byte in pixel_data:
    bits.append((byte >> 2) & 1)

# Reconstituer les octets du flag à partir du flux de bits
flag_bytes = []
for i in range(0, len(bits), 8):
    byte_bits = bits[i:i+8]
    if len(byte_bits) == 8:
        # On assemble bits en un octet
        val = 0
        for b in byte_bits:
            val = (val << 1) | b
        flag_bytes.append(val)

flag = bytes(flag_bytes)

# Afficher le flag proprement
decoded = flag.decode(errors="ignore")
start = decoded.find("0xfun{")
print(decoded)
```

Le flux récupéré contient un Base64 encodé.

```
import base64

b64 = "MHhmdW57VGg0dDVfbjB0X0wzNDV0X1MxZ24xZjFjNG50X2IxdF81dDNnfQ=="
flag = base64.b64decode(b64).decode()
print(flag)
```

<details>
<summary>Flag</summary>

`0xfun{Th4t5_n0t_L345t_S3gn1f1c4nt_b1t_5t3g}`

</details>
