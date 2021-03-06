
Examinons ce qui se fait à l'intérieur de `makeArmy`, et la solution deviendra évidente.

1. Elle crée un tableau vide `shooters`:

    ```js
    let shooters = [];
    ```
2. Le remplit dans la boucle via `shooters.push(function...)`.

    Chaque élément est une fonction, le tableau résultant ressemble à ceci :

    ```js no-beautify
    shooters = [
      function () { alert(i); },
      function () { alert(i); },
      function () { alert(i); },
      function () { alert(i); },
      function () { alert(i); },
      function () { alert(i); },
      function () { alert(i); },
      function () { alert(i); },
      function () { alert(i); },
      function () { alert(i); }
    ];
    ```

3. Le tableau est renvoyé depuis la fonction.

Ensuite, plus tard, l'appel à `army[5]()` obtiendra l'élément `army[5]` du tableau (ce sera une fonction) et l'appellera.

Maintenant, pourquoi toutes ces fonctions affichent la même chose ?

C'est parce qu'il n'y a pas de variable locale `i` dans les fonctions `shooter`. Quand une telle fonction est appelée, elle tire `i` de son environnement lexical externe.

Quelle sera la valeur de `i` ?

Si on regarde la source :

```js
function makeArmy() {
  ...
  let i = 0;
  while (i < 10) {
    let shooter = function() { // fonction shooter
      alert( i ); // devrait afficher son numéro
    };
    ...
  }
  ...
}
```

... Nous pouvons voir qu'il réside dans l'environnement lexical associé à l'exécution actuelle de `makeArmy()`. Mais lorsque l'option `army[5]()` est appelée, `makeArmy` a déjà terminé son travail et `i` a la dernière valeur : `10` (la fin de `while`).

En conséquence, toutes les fonctions `shooter` obtiennent la même valeur, dernière valeur `i = 10`, à partir de l'environnement lexical externe.

Nous pouvons réparer cela en déplaçant la définition de variable dans la boucle :

```js run demo
function makeArmy() {

  let shooters = [];

*!*
  for(let i = 0; i < 10; i++) {
*/!*
    let shooter = function() { // fonction shooter
      alert( i ); // devrait afficher son numéro
    };
    shooters.push(shooter);
  }

  return shooters;
}

let army = makeArmy();

army[0](); // 0
army[5](); // 5
```

Maintenant, cela fonctionne correctement, car à chaque fois que le bloc de code dans `for (let i = 0 ...) {...}` est exécuté, un nouvel environnement lexical est créé pour celui-ci, avec la variable correspondante `i`.

Ainsi, la valeur de `i` vit maintenant un peu plus près. Pas dans l'environnement lexical `makeArmy()`, mais dans l'environnement lexical qui correspond à l'itération de la boucle en cours. C'est la raison pour laquelle maintenant ça fonctionne.

![](lexenv-makearmy.svg)

Ici, nous avons réécrit `while` dans` for`.

Une autre astuce pourrait être possible, voyons-la pour une meilleure compréhension du sujet :

```js run
function makeArmy() {
  let shooters = [];

  let i = 0;
  while (i < 10) {
*!*
    let j = i;
*/!*
    let shooter = function() { // fonction shooter
      alert( *!*j*/!* ); // devrait afficher son numéro
    };
    shooters.push(shooter);
    i++;
  }

  return shooters;
}

let army = makeArmy();

army[0](); // 0
army[5](); // 5
```

La boucle `while`, tout comme `for`, crée un nouvel environnement lexical pour chaque exécution. Nous nous assurons donc ici que la valeur obtenue pour un `shooter` soit correcte.

Nous copions `let j = i`. Cela crée un corps de boucle local `j` et y copie la valeur de `i`. Les primitives sont copiées "par valeur", nous obtenons donc une copie complète et indépendante de `i`, appartenant à l'itération de la boucle en cours.
