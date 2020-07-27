<<<<<<< HEAD
# Erreur lors de la lecture d'une propriété inexistante

Habituellement, une tentative de lecture d'une propriété inexistante renvoie `undefined`.

Créez à la place un proxy qui génère une erreur pour une tentative de lecture d'une propriété inexistante.
=======
# Error on reading non-existent property

Usually, an attempt to read a non-existent property returns `undefined`.

Create a proxy that throws an error for an attempt to read of a non-existent property instead.
>>>>>>> b0464bb32c8efc2a98952e05f363f61eca1a99a2

Cela peut aider à détecter précocement les erreurs de programmation.

Écrivez une fonction `wrap(target)` qui prend un objet `target` et retourne un proxy qui ajoute cet aspect fonctionnel.

Voilà comment cela devrait fonctionner:

```js
let user = {
  name: "John"
};

function wrap(target) {
  return new Proxy(target, {
*!*
      /* your code */
*/!*
  });
}

user = wrap(user);

alert(user.name); // John
*!*
alert(user.age); // ReferenceError: la propriété n'existe pas "age"
*/!*
```
