importance: 5

---

# Décorateur debounce

<<<<<<< HEAD
Le résultat du décorateur `debounce(f, ms)` devrait être un wrapper qui transmet l'appel à `f` au maximum une fois par `ms`, millisecondes.

En d'autres termes, lorsque nous appelons une fonction "debounce", cela garantit que tous les futurs appels à la fonction effectuée moins de `ms` millisecondes après l'appel précédent seront ignorés.

Par exemple:
=======
The result of `debounce(f, ms)` decorator is a wrapper that suspends calls to `f` until there's `ms` milliseconds of inactivity (no calls, "cooldown period"), then invokes `f` once with the latest arguments.

For instance, we had a function `f` and replaced it with `f = debounce(f, 1000)`.

Then if the wrapped function is called at 0ms, 200ms and 500ms, and then there are no calls, then the actual `f` will be only called once, at 1500ms. That is: after the cooldown period of 1000ms from the last call.
>>>>>>> b0464bb32c8efc2a98952e05f363f61eca1a99a2

![](debounce.svg)

<<<<<<< HEAD
f(1); // exécute immédiatement
f(2); // ignorée

setTimeout( () => f(3), 100); // ignorée (seulement 100 ms passé)
setTimeout( () => f(4), 1100); // exécute
setTimeout( () => f(5), 1500); // ignorée (moins de 1000 ms à partir de la dernière exécution)
```

En pratique, `debounce` est utile pour les fonctions qui récupèrent/mettent à jour quelque chose quand on sait que rien de nouveau ne peut être fait dans un laps de temps aussi court, il est donc préférable de ne pas gaspiller de ressources.
=======
...And it will get the arguments of the very last call, other calls are ignored.

Here's the code for it (uses the debounce decorator from the [Lodash library](https://lodash.com/docs/4.17.15#debounce):

```js
let f = _.debounce(alert, 1000);

f("a"); 
setTimeout( () => f("b"), 200);
setTimeout( () => f("c"), 500); 
// debounced function waits 1000ms after the last call and then runs: alert("c")
```


Now a practical example. Let's say, the user types something, and we'd like to send a request to the server when the input is finished.

There's no point in sending the request for every character typed. Instead we'd like to wait, and then process the whole result.

In a web-browser, we can setup an event handler -- a function that's called on every change of an input field. Normally, an event handler is called very often, for every typed key. But if we `debounce` it by 1000ms, then it will be only called once, after 1000ms after the last input.

```online

In this live example, the handler puts the result into a box below, try it:

[iframe border=1 src="debounce" height=200]

See? The second input calls the debounced function, so its content is processed after 1000ms from the last input.
```

So, `debounce` is a great way to process a sequence of events: be it a sequence of key presses, mouse movements or something else.


It waits the given time after the last call, and then runs its function, that can process the result.

The task is to implement `debounce` decorator.

Hint: that's just a few lines if you think about it :)
>>>>>>> b0464bb32c8efc2a98952e05f363f61eca1a99a2
