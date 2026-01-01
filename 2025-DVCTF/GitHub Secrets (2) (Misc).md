> J'ai reçu une copie d’un projet abandonné... il semble propre au premier abord.
> 
> Mais une rumeur prétend qu’un ancien développeur y aurait laissé un secret avant de le supprimer.
> 
> Flag format : DVC{...}
> 
> +fichier “rev-chaine”

On ouvre GitSecret2\.git pour observer : "git log --oneline --graph --all"
```
* d8f9113 (HEAD -> master) Initial commit
```

Puis avec "Get-ChildItem" pour voir si on trouve des éléments intéressants : 
```
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        13/09/2025     11:07                .git
-a----        13/09/2025     11:07             15 README.md
```

Puis avec "git log -p"
```
commit d8f9113e8c76e3e8ffd27678e05614521a831a55 (HEAD -> master)
Author: Tad <Tad@Tad.fr>
Date:   Mon Jul 28 10:47:20 2025 +0200

    Initial commit

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..84fb69b
--- /dev/null
+++ b/README.md
:
commit d8f9113e8c76e3e8ffd27678e05614521a831a55 (HEAD -> master)       
Author: Tad <Tad@Tad.fr>
Date:   Mon Jul 28 10:47:20 2025 +0200

    Initial commit

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..84fb69b
--- /dev/null
+++ b/README.md
@@ -0,0 +1 @@
+# Find Me 👀
```
Avec "it" : 
```
applet de commande It à la position 1 du pipeline de la commande
Fournissez des valeurs pour les paramètres suivants :
name: ^V^V
The It command may only be used inside a Describe block.
Au caractère C:\Program Files\WindowsPowerShell\Modules\Pester\3.4.0\F 
unctions\Describe.ps1:125 : 9
+         throw "The $CommandName command may only be used inside a    
Des ...
+
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : OperationStopped: (The It command ...De  
   scribe block.:String) [], RuntimeException
    + FullyQualifiedErrorId : The It command may only be used inside   
   a Describe block.
```

Ensuite avec "git show d8f9113"
```
commit d8f9113e8c76e3e8ffd27678e05614521a831a55 (HEAD -> master)
Author: Tad <Tad@Tad.fr>
Date:   Mon Jul 28 10:47:20 2025 +0200

    Initial commit

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..84fb69b
--- /dev/null
+++ b/README.md
@@ -0,0 +1 @@
+# Find Me 👀
(END)
Date:   Mon Jul 28 10:47:20 2025 +0200

    Initial commit

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..84fb69b
--- /dev/null
+++ b/README.md
@@ -0,0 +1 @@
+# Find Me 👀
```

Puis avec "git rev-list --objects --all", on  a : 
```
d8f9113e8c76e3e8ffd27678e05614521a831a55
449b0fda9bf1a5efd1df7f374ab2112f4bd82069
84fb69b904d95ab473225b51527fcfd3c88ea088 README.md
```

Et avec "git show 449b0fda9bf1a5efd1df7f374ab2112f4bd82069", on a : 
```
tree 449b0fda9bf1a5efd1df7f374ab2112f4bd82069

README.md
```

Ensuite avec "git ls-tree 449b0fda9bf1a5efd1df7f374ab2112f4bd82069" : 
```
100644 blob 84fb69b904d95ab473225b51527fcfd3c88ea088    README.md
```

Et avec "git show 84fb69b904d95ab473225b51527fcfd3c88ea088" :
```
# Find Me 👀
```

Donc : 
git rev-list --objects --all a listé trois objets :
d8f9113e8c76e3e8ffd27678e05614521a831a55 -> le commit
449b0fda9bf1a5efd1df7f374ab2112f4bd82069 -> un tree
84fb69b904d95ab473225b51527fcfd3c88ea088 -> README.md (actuel)

Il manque quelque chose : le flag est sans doute dans un blob non référencé dans --all car il n'est pas accessible via les branches (ou bien effacé avant le commit).
Pour explorer tout l’objet .git, y compris les objets inaccessibles :
"git fsck --lost-found | Select-String "blob""

Cela va te donner une liste de blobs "perdus" mais toujours présents dans .git.

Ainsi, avec "git fsck --lost-found | Select-String "blob"", on a : 
```
Checking object directories: 100% (256/256), done.

dangling blob 664218d18e40018dd45e3574e280107e561d2521
```
Et enfin la commande "git show 664218d18e40018dd45e3574e280107e561d2521" donne le flag.

<details>
<summary>Flag</summary>

`Bravo ! Flag : DVC{BLOB_TR4CK3R}`

</details>
