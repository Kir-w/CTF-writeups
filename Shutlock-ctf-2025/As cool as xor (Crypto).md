> 100
>
> Analyser la signature de ce fichier mp4 devrait nous indiquer s’il a été modifié entre sa production au laboratoire et son arrivée au festival.
Format de flag : SHLK{Hash SHA-256 du fichier déchiffré}.
> 
> 546b207b9b38d75a078a36986650af7759bbe27e0f785d332ca4d7c9727df7d8 as_cool_as_xor.py(1131 bytes)
> 22ed4e81f61e83bad0659ee2347fc65b5cf24ab4bee4006b613c071f0d4d3e93 video_encrypted.mp4(146482988 bytes)
> 
> https://static.shutlock.fr/video_encrypted.mp4
> 
> as_cool_as_xor.py

Après quelques recherches, le fichier MP4 original semble chiffré avec “un schéma de Feistel à 1 tour”. La clé secrète “key” est un tuple de 4 octets (32 bits) utilisé dans la fonction “func_key”.
Le schéma à 1 tour permet un déchiffrement symétrique (c’est réversible).

L’en-tête du MP4 est connu et connaître les 8 premiers octets (\x00\x00\x00\x18ftyp)casse la complexité de la clé.

Le fichier chiffré (video_encrypted.mp4) est structuré en deux moitiés : “L1 = R0” et “R1 = L0 XOR F(R0, key)”. 

L1_8 = premiers 8 octets (début de R0). 
R1_8 = premiers 8 octets de la seconde moitié (début de L0 XOR F(R0, key)).

On calcule la fonction F avec F(L1_8, key) pour l'en-tête : F_8 = R1_8 XOR \x00\x00\x00\x18ftyp.

Pour trouver la clé key : on fait pour chaque indice j dans [1], la résolution de : 
```
(L1_8[j] * key_j) % 256 == F_8[j]
(L1_8[j+4] * key_j) % 256 == F_8[j+4]
```

On teste les candidats possibles (0-255) pour chaque octet de la clé.

Pour déchiffrer le fichier, on implémente la fonction de déchiffrement Feistel :
```
def feistel_decrypt(ciphertext, key):
    mid = len(ciphertext) // 2
    L1, R1 = ciphertext[:mid], ciphertext[mid:]
    F_val = func_key(L1, key)
    L0 = bytes(R1[i] ^ F_val[i] for i in range(len(R1)))
    R0 = L1
    return L0 + R0
```
  
Et on calcule le SHA-256 du fichier déchiffré.
Code complet : 
```
import os
import hashlib
from itertools import product

def func_key(word, key):
    return bytes((word[i] * key[i % 4]) % 256 for i in range(len(word)))

def feistel_decrypt(ciphertext, key):
    mid = len(ciphertext) // 2
    L1, R1 = ciphertext[:mid], ciphertext[mid:]
    F_val = func_key(L1, key)
    L0 = bytes(R1[i] ^ F_val[i] for i in range(len(R1)))
    R0 = L1
    return L0 + R0

# Charger le fichier chiffré en entrée
with open("osint_xor_video_encrypted.mp4", "rb") as f:
    ciphertext = f.read()

# Paramètres connus
total_len = len(ciphertext)
mid = total_len // 2
L1_8 = ciphertext[:8]
R1_8 = ciphertext[mid:mid+8]
known_header = b'\x00\x00\x00\x18ftyp'

# Calculer F_8
F_8 = bytes(R1_8[i] ^ known_header[i] for i in range(8))
A = list(L1_8)

# Trouver les candidats pour chaque octet de la clé
candidates = []
for j in range(4):
    cands = []
    for candidate in range(256):
        i0, i1 = j, j + 4
        if (A[i0] * candidate) % 256 == F_8[i0] and (A[i1] * candidate) % 256 == F_8[i1]:
            cands.append(candidate)
    candidates.append(cands)

# Tester les combinaisons de clés
for key_tuple in product(*candidates):
    plaintext = feistel_decrypt(ciphertext, key_tuple)
    if plaintext.startswith(known_header):
        sha256_hash = hashlib.sha256(plaintext).hexdigest()
        print(f"Flag: SHLK{{{sha256_hash}}}")
        break
```
Après avoir téléchargé la vidéo (aller sur le lien et faire ctrl+s), enregistrer au bon nom. Et lancer le programme : 
<details>
<summary>Flag</summary>

`SHLK{821db7160a76db43d0e7f73d5361939f6c9ee8c95359d8e074473c5353cc1965}`

</details>
