> John Doe uses this secure server where plaintext is never shared. Our Forensics Analyst was able to capture this traffic and the source code for the server. Can you recover John Doe's secrets?
> Attachments
> files.zip  containing server.py and a capture .pcap. 

<details>
<summary>server.py</summary>
  
`
import os
from pwn import xor
print("With the Secure Server, sharing secrets is safer than ever!")
enc = bytes.fromhex(input("Enter the secret, XORed by your key (in hex): ").strip())
key = os.urandom(32)
enc2 = xor(enc,key).hex()
print(f"Double encrypted secret (in hex): {enc2}")
dec = bytes.fromhex(input("XOR the above with your key again (in hex): ").strip())
secret = xor(dec,key)
print("Secret received!")
`
</details>

On remarque dans wireshark les paquets qui ont des lengths > 100 (copie flux hex) : 
- paquet 4 : 080027063e5752550a0002020800450000923d9e0000400673de6c3b50a00a00020f0539c19e092ba802b236e8ee5018ffffc7660000576974682074686520536563757265205365727665722c2073686172696e672073656372657473206973207361666572207468616e2065766572210a456e74657220746865207365637265742c20584f52656420627920796f7572206b65792028696e20686578293a20
avec data : 576974682074686520536563757265205365727665722c2073686172696e672073656372657473206973207361666572207468616e2065766572210a456e74657220746865207365637265742c20584f52656420627920796f7572206b65792028696e20686578293a20
et copie en texte ascii : With the Secure Server, sharing secrets is safer than ever!
Enter the secret, XORed by your key (in hex):

- paquet 6 : 52550a000202080027063e57080045000069bd0240004006b4a20a00020f6c3b50a0c19e0539b236e8ee092ba86c5018fa86c9450000313531653731636534616464663639326435626163383362623837393131613230633339623731646133666135653766663035613262326230613833626130330a
avec data : 313531653731636534616464663639326435626163383362623837393131613230633339623731646133666135653766663035613262326230613833626130330a
et copie en texte ascii : 151e71ce4addf692d5bac83bb87911a20c39b71da3fa5e7ff05a2b2b0a83ba03

- paquet 8 : 080027063e5752550a0002020800450000b73da00000400673b76c3b50a00a00020f0539c19e092ba86cb236e92f5018ffff3a650000446f75626c6520656e63727970746564207365637265742028696e20686578293a20653139333031363432383065343433383662333839663765336263303262373037313838656137306439363137653363656439383966313564386131306437300a584f52207468652061626f7665207769746820796f7572206b657920616761696e2028696e20686578293a20
avec data : 446f75626c6520656e63727970746564207365637265742028696e20686578293a20653139333031363432383065343433383662333839663765336263303262373037313838656137306439363137653363656439383966313564386131306437300a584f52207468652061626f7665207769746820796f7572206b657920616761696e2028696e20686578293a20
et copie en texte ascii : Double encrypted secret (in hex): e1930164280e44386b389f7e3bc02b707188ea70d9617e3ced989f15d8a10d70
XOR the above with your key again (in hex):

- paquet 10 : 
52550a000202080027063e57080045000069bd0440004006b4a00a00020f6c3b50a0c19e0539b236e92f092ba8fb5018f9f7c9450000383765653032633331326137663166656638663932663735663165363062613132326466333231393235653831333230363862303837316666333033393630650a
avec data : 
383765653032633331326137663166656638663932663735663165363062613132326466333231393235653831333230363862303837316666333033393630650a
et copie en texte ascii : 87ee02c312a7f1fef8f92f75f1e60ba122df321925e8132068b0871ff303960e

Ici, la vulnérabilité est dans la réutilisation de la clé XOR. Le code du serveur fonctionne de la manière suivante :
- L'utilisateur envoie une chaîne hexadécimale, que le serveur nomme enc.
- Le serveur génère une clé aléatoire (key).
- Le serveur calcule enc2 = enc XOR key et l'envoie à l'utilisateur.
- L'utilisateur envoie une nouvelle chaîne hexadécimale, que le serveur nomme dec.
- Le serveur calcule secret = dec XOR key.

Le point faible est que la même clé aléatoire est utilisée pour les deux opérations. Grâce à l'analyse de la capture réseau (pcap), nous avons trois informations essentielles :
- Le premier message envoyé par l'utilisateur (enc_user_input).
- Le message chiffré renvoyé par le serveur (enc2_server_output).
- Le deuxième message envoyé par l'utilisateur (dec_user_input).

Notons que l'opération XOR est réversible: si on a A XOR B = C, alors on a aussi A XOR C = B et B XOR C = A.

Pour récupérer la clé (key) : nous savons que enc2 = enc XOR key. En réarrangeant cette équation, nous pouvons trouver la clé en faisant un XOR entre le premier message de l'utilisateur et le message chiffré du serveur : key = enc XOR enc2.
Pour récupérer le secret (secret) : une fois la clé retrouvée, nous pouvons l'utiliser pour déchiffrer le deuxième message de l'utilisateur et obtenir le secret final : secret = dec XOR key.

<details>
<summary>Script python</summary>

`
import sys

# Données extraites du .pcap (chaînes hexadécimales correspondant aux communications entre le client et le serveur)
enc_user_input_hex = "151e71ce4addf692d5bac83bb87911a20c39b71da3fa5e7ff05a2b2b0a83ba03"
enc2_server_output_hex = "e1930164280e44386b389f7e3bc02b707188ea70d9617e3ced989f15d8a10d70"
dec_user_input_hex = "87ee02c312a7f1fef8f92f75f1e60ba122df321925e8132068b0871ff303960e"

# fonction pour effectuer l'opération XOR sur deux chaînes de bytes
def xor_bytes(a, b):
    return bytes(x ^ y for x, y in zip(a, b)) # XOR bit à bit sur chaque paire de bytes

# Conversion des chaînes hexadécimales en bytes
enc_bytes = bytes.fromhex(enc_user_input_hex)
enc2_bytes = bytes.fromhex(enc2_server_output_hex)
dec_bytes = bytes.fromhex(dec_user_input_hex)

# Récupération de la clé secrète (XOR du premier message de l'utilisateur et de la réponse du serveur)
key = xor_bytes(enc_bytes, enc2_bytes) # key = enc XOR enc2

# Déchiffrement (XOR du 2e message de l'utilisateur avec la clé retrouvée)
secret = xor_bytes(dec_bytes, key) # secret = dec XOR key

print(f"Clé retrouvée (en hex) : {key.hex()}")
print(f"Secret déchiffré : {secret.decode('utf-8')}")
`
</details>

<details>
<summary>Flag</summary>

`scriptCTF{x0r_1s_not_s3cur3!!!!}`

</details>
