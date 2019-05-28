# JavaScript Array - reduce

`reduce` est une méthode très puissante et polyvalente. Avec seulement `reduce`,
on peut faire tout ce qu'on fait avec `map`, `filter`, `find`, etc.

Voici comment on utilise `reduce` sur un tableau `array` :

    const result = array.reduce(
      (carry, item) => { // ... }, initialCarry
    )

`reduce` prend deux paramètres :
1. une fonction.
2. une valeur initiale pour l'*accumulateur* (*carry* en anglais) : on l'a appelée
ici `initialCarry`.

Le 1er paramètre, la fonction, doit avoir la "forme" suivante :

    function(carry, item) {
      // Renvoyer une valeur qui sera l'accumulateur pour la prochaine itération
    }

Le 2ème paramètre `initialCarry` permet d'initialiser l'accumulateur, qui est la valeur finale qu'on veut obtenir.

L'accumulateur est construit itération par itération grâce à la fonction, le plus souvent en le combinant avec la valeur de l'élément en cours (`item`), pour obtenir une nouvelle valeur.

Ça paraît compliqué comme ça, mais quelques exemples devraient éclaircir tout cela !

## Exemples

### Calculer une somme de nombres

Sans `reduce`, on utiliserait un code comme celui-ci pour calculer une somme :

    const sumOfNumbers = numbersArray => {
      let sum = 0;
      for (let i = 0 ; i < numbersArray.length ; i++) {
        sum += numbersArray[i];
      }
      return sum;
    }
    console.log(sumOfNumbers([1, 3, 5])); // Affiche 9

On commence par initialiser la variable `sum`, qui va servir à "accumuler" les
valeurs des différents nombres, en additionnant chacun d'eux à `sum`.

À la fin de la boucle, `sum` vaut la somme de tous les nombres.

Écrivons la même chose avec `reduce` :

    const sumWithReduce = numbersArray => numbersArray.reduce(
      (sum, number) => sum + number, 0
    );
    console.log(sumWithReduce([1, 3, 5])); // Affiche 9

Comment ça marche ? Ici, `sum` est l'accumulateur. La fonction passée à `reduce`
est appelée pour chaque élément du tableau (comme `map`, `filter`, etc., `reduce`
gère lui-même son compteur pour traiter tous les éléments l'un après l'autre).

Cette fonction reçoit la valeur de l'accumulateur renvoyée par la fonction lors de l'itération précédente,
et calcule la valeur à passer à la fonction pour l'itération à suivre. Elle combine
cette valeur (ici en l'additionnant) avec l'élément en cours `element`.

Lors de la 1ère itération, il n'y a évidemment pas d'itération précédente. L'accumulateur
prend alors la valeur initiale donnée par le 2ème argument de `reduce` (ici 0).

Si on détaille ce qui se passe ici, itération par itération, avec l'exemple `[1, 3, 5]`.

#### 1ère itération

En entrée, `sum` vaut 0 (2ème argument de `reduce`), `element` vaut 1. On renvoie 1.

#### 2ème itération

`sum` vaut la valeur renvoyée par la 1ère itération, donc 1. `element` vaut 3. On renvoie 4.

#### 3ème itération.

`sum` vaut la valeur renvoyée par la 2ème itération, donc 4. `element vaut 5. On renvoie 9,
et c'est le résultat final, car on a 3 éléments dans le tableau.

### Récupérer la 1ère lettre de chaque mot

Si on part d'un tableau (par exemple `['New', 'York', 'Police', 'Departement']`),
et qu'on veut obtenir une chaîne contenant la première lettre de chaque mot (`NYPD`),
voici comment on le ferait avec une boucle. On en profite pour introduire une variante
de la boucle `for`, la boucle `for...of`, qui gère toute seule le compteur, et qui
permet de récupérer tour à tour chaque mot du tableau :

    const getFirstLetters = wordsArray => {
      let firstLetters = '';
      for (let word of wordsArray) {
        firstLetters += word[0];
      }
      return firstLetters;
    }
    console.log(getFirstLetters(['New', 'York', 'Police', 'Departement'])) // NYPD

La même chose avec `reduce` :

    const getFirstWithReduce = wordsArray => wordsArray.reduce(
      (firstLetters, word) => firstLetters + word[0], ''
    );
    console.log(getFirstWithReduce(['New', 'York', 'Police', 'Departement'])) // NYPD

Cette fois, la valeur initiale de l'accumulateur est une chaîne. À chaque itération,
on ajoute à la fin de cette chaîne le 1er caractère du mot en cours.

Itération par itération :

#### 1ère itération

`firstLetters` est une chaîne vide (2ème argument de `reduce`, après la fonction).
`word` vaut `New`, sa 1ère lettre est `N`, on renvoie `N`.

#### 2ème itération

`firstLetters` vaut `N` (valeur renvoyée à la fin de la 1ère itération). `word`
vaut `York`, sa 1ère lettre est `Y`, on renvoie `NY`.

#### 3ème itération

`firstLetters` vaut `NY` (valeur renvoyée à la fin de la 2ème itération). `word`
vaut `Police`, sa 1ère lettre est `P`, on renvoie `NYP`.

#### 4ème itération

`firstLetters` vaut `NYP` (valeur renvoyée à la fin de la 3ème itération). `word`
vaut `Department`, sa 1ère lettre est `D`, on renvoie `NYPD`, la valeur finale.

### Créer un tableau

On va créer un tableau d'objets à partir d'un tableau de chaînes. Cette exemple pourrait
être tout aussi bien fait avec `map`.

On part d'un tableau de noms (prénom et nom de famille dans une même chaîne) :

    ['Julie Brown', 'Sebastian Hodgson', 'Mia Marsh']

On veut obtenir un tableau d'objets :

    [
      { firstName: 'Julie', lastName: 'Brown' },
      { firstName: 'Sebastian', lastName: 'Hodgson' },
      { firstName: 'Mia', lastName: 'Marsh' }
    ]

Voici comment on ferait avec une boucle :

    const decomposeNames = namesArray => {
      const objArray = [];
      for (let i = 0 ; i < namesArray.length ; i++) {
        const fullName = namesArray[i];
        const nameParts = fullName.split(' ');
        objArray.push({ firstName: nameParts[0], lastName: nameParts[1] });
      }
      return objArray;
    }
    console.log(decomposeNames(['Julie Brown', 'Sebastian Hodgson', 'Mia Marsh']));

Avec `map` :

    const decomposeWithMap = namesArray => namesArray.map(
      fullName => {
        const nameParts = fullName.split(' ');
        return { firstName: nameParts[0], lastName: nameParts[1] };
      }
    );
    console.log(decomposeWithMap(['Julie Brown', 'Sebastian Hodgson', 'Mia Marsh']));

Enfin, avec `reduce` :

    const decomposeWithReduce = namesArray => namesArray.reduce(
      (objArray, fullName) => {
        const nameParts = fullName.split(' ');
        //return objArray.concat({ firstName: nameParts[0], lastName: nameParts[1] }); // ES5
        return [...objArray, { firstName: nameParts[0], lastName: nameParts[1] }]; // ES6
      },
      []
    );
    console.log(decomposeWithReduce(['Julie Brown', 'Sebastian Hodgson', 'Mia Marsh']));

C'est très proche de `map`, à deux choses près :
* On concatène le nouvel objet à l'accumulateur
* On initialise l'accumulateur grâce au 2ème argument de `reduce`

### Créer un objet

On part d'une chaîne de caractères, et on veut obtenir un objet indiquant le nombre de fois qu'on
a trouvé chaque caractère.

Exemple de chaîne de départ : `anticonstitutionnellement`.

Objet attendu en sortie :

    {
      a: 1
      c: 1
      e: 3
      i: 3
      l: 2
      m: 1
      n: 5
      o: 2
      s: 1
      t: 5
      u: 1
    }

Avec une boucle... euh non, je ne vais pas te le montrer ! fais-le comme exercice :D.

Avec `reduce` :

    const countLetters = word => word.split('').reduce(
      (carry, letter) => carry[letter]
        ? { ...carry, [letter]: carry[letter] + 1 }
        : { ...carry, [letter]: 1 },
      {}
    );
    console.log(countLetters('anticonstitutionnellement'));

Ah, c'est un peu plus impressionnant comme code ! Décomposons !

Dans un premier temps, avant l'appel à `reduce`, on doit "exploser" la chaîne, avec `split`, pour obtenir un tableau.

L'accumulateur initial (2ème argument de `reduce`) est un objet vide.

Ensuite, voici le fonctionnement de la fonction passée en 1er argument :

* C'est une fonction fléchée qui renvoie directement le résultat d'un ternaire
* La condition du ternaire c'est `carry[letter]`. Cela revient à tester si on trouve
la clé donnée par `letter` dans l'objet.
* Si on trouve cette clé, cela veut dire qu'on était déjà tombé sur cette lettre,
donc que `carry[letter]` avait déjà une valeur (un nombre). On renvoie donc l'objet,
en ajoutant 1 à la valeur associée à la clé donnée par la variable `letter`.
* Si on ne trouve pas cette clé, il faut l'ajouter à l'objet, en lui associant la valeur 1.

## Ressources

Si tu veux d'autres exemples :
* [Reduce, le couteau suisse du développeur JS](https://medium.com/@hkairi/reduce-le-couteau-suisse-du-d%C3%A9veloppeur-javascript-8cf4b6f98304)
* [Array.reduce par l'exemple](https://putaindecode.io/fr/articles/js/array-reduce/)

## Exercices

### Calculer une moyenne

Utilise `reduce` pour revisiter l'exercice du calcul de la moyenne d'un tableau de nombres.
Par exemple, avec `[11, 14, 9, 18]` comme tableau d'entrée, la fonction `reduceAverage` retourne le résultat `13`.

Utilise le "squelette" de programme suivant, qui contient un test permettant de vérifier la validité de ton code (avec Node) :

```javascript
// reduceAverage.js
const assert = require('assert');

// Enlever les accolades si besoin
const reduceAverage = numbersArray => {}

// Test
assert.strictEqual(reduceAverage([11, 14, 9, 18]), 13);
```

### Trouver le plus long mot

Utilise `reduce` pour écrire `longestWord`, afin de trouver le plus long mot dans un tableau de chaînes.
Par exemple, avec `['Choucroute', 'Cassoulet', 'Tartiflette']`, on doit obtenir `Tartiflette`.

Tu peux (ou pas) essayer la variante de `reduce` où on ne passe pas le 2ème paramètre (valeur initiale de l'accumulateur). C'est documenté [sur MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Objets_globaux/Array/reduce).

```javascript
// longestWord.js
const assert = require('assert');

// Enlever les accolades si besoin
const longestWord = wordsArray => {}

// Test
assert.strictEqual(longestWord(['Choucroute', 'Cassoulet', 'Tartiflette']), 'Tartiflette');
// Export
module.exports = longestWord;
```

### Vérifier si toutes les valeurs sont vraies ou "truthy"

Une valeur "truthy", c'est une valeur non booléenne, pouvant être ramenée à un booléen `true` :
* nombre différent de 0
* chaîne non-vide
* objet, vide ou non (et par extension tableau, vide ou non)

On peut tester une valeur, pour savoir si elle est "truthy", de cette façon :

    if(value) {
      // ... Si on entre ici, c'est que value est "truthy"
    }

Ou encore de cette manière un peu "brutale" :

    const isTruthy = !!value;

Tu peux te référer à [cet article](https://medium.com/@pddivine/javascript-bang-bang-i-shot-you-down-use-of-double-bangs-in-javascript-7c9d94446054) pour en comprendre le fonctionnement en détail.

Mais en résumé : si `value` est "truthy", `!value` vaudra le *booléen* `false`, donc `!!value` vaudra le *booléen* `true`. Et inversement.

C'est une façon de convertir une value "truthy" ou "falsy" en `true` ou `false`.

Ecrire la fonction `allTruthy`, qui renvoie `true` si toutes les valeurs du tableau testé sont "truthy".
La fonction renverra donc `true` si le tableau d'entrée est `[4, 'Hello', {}, true]`, et `false` si le tableau d'entrée est `['Hi there', 983, '', { some: 'value' }]`

```javascript
// allTruthy.js
const assert = require('assert');

// Enlever les accolades si besoin
const allTruthy = valuesArray => {}

// Test
assert.strictEqual(allTruthy([4, 'Hello', {}, true]), true);
assert.strictEqual(allTruthy(['Hi there', 983, '', { some: 'value' }]), false);
```

### Récupérer les longueurs des plus longs mots

Ecrire `longestWordsLengths` qui prend en entrée un tableau de tableaux de chaînes.
Il s'agit de trouver le plus long mot de chaque tableau, puis de créer un objet
utilisant chaque mot trouvé comme clé, et lui associant comme valeur, sa longueur.

Exemple de tableau d'entrée :

    [
      ['Kiwi', 'Framboise', 'Poire', 'Rhubarbe'],
      ['Courgette', 'Aubergine', 'Artichaut', 'Topinambour', 'Rutabaga'],
      ['Thym', 'Romarin', 'Sarriette', 'Sauge']
    ]

Sortie attendue :

    {
      Framboise: 9,
      Topinambour: 11,
      Sarriette: 9
    }

Tu peux utiliser la fonction `longestWord` écrite précédemment (que tu peux importer avec `require`).

Squelette du programme :

```javascript
// longestWordsLengths.js
const assert = require('assert');

// Enlever les accolades si besoin
const longestWordsLengths = valuesArray => {}

// Test
assert.deepStrictEqual(longestWordsLengths([
  ['Kiwi', 'Framboise', 'Poire', 'Rhubarbe'],
  ['Courgette', 'Aubergine', 'Artichaut', 'Topinambour', 'Rutabaga'],
  ['Thym', 'Romarin', 'Sarriette', 'Sauge']
]), {
  Framboise: 9,
  Topinambour: 11,
  Sarriette: 9
});
```

### Regrouper des personnes par leurs initiales

Ecrire `groupByInitials` qui prend en tableau de chaînes, et retourne un objet, permettant de regrouper ensemble des personnes qui ont les mêmes initiales.

Exemple de tableau d'entrée (par avance pardon d'avoir classé Britney Spears et Brian May dans le même tableau :sweat_smile:) :

```javascript
[
  'Barbara Streisand', 'John Malkovich', 'Bruce Springsteen', 'Alicia Keys', 'Bob Marley', 'Julianne Moore', 'Brian May', 'Britney Spears', 'Ashton Kutcher'
]
```

Objet en sortie attendu :

```javascript
{
  BS: [ 'Barbara Streisand', 'Bruce Springsteen', 'Britney Spears' ],
  JM: [ 'John Malkovich', 'Julianne Moore' ],
  AK: [ 'Alicia Keys', 'Ashton Kutcher' ],
  BM: [ 'Bob Marley', 'Brian May' ]
}
```

Squelette du programme :

```javascript
// groupByInitials.js
const assert = require('assert');

// Enlever les accolades si besoin
const groupByInitials = namesArray => {}

// Test
const input = [
  'Barbara Streisand', 'John Malkovich', 'Bruce Springsteen', 'Alicia Keys', 'Bob Marley', 'Julianne Moore', 'Brian May', 'Britney Spears', 'Ashton Kutcher'
];
const expected = {
  BS: [ 'Barbara Streisand', 'Bruce Springsteen', 'Britney Spears' ],
  JM: [ 'John Malkovich', 'Julianne Moore' ],
  AK: [ 'Alicia Keys', 'Ashton Kutcher' ],
  BM: [ 'Bob Marley', 'Brian May' ]
};
assert.deepStrictEqual(groupByInitials(input), expected);
```

### Regrouper des URLs par noms de domaine

On te fournit une liste d'URLs. Quelques exemples d'URLs :

* https://www.example.com
* https://blog.example.com
* https://admin.mydomain.org
* http://www.mydomain.org

On souhaite regrouper les sites correspondant à un même nom de domaine : le nom de domaine est la partie de l'URL telle que `example.com` ou `myblog.com`. On va donc écrire une fonction `groupSitesByDomain`.

Tableau d'entrée :

```javascript
['https://www.example.com', 'http://phpmyadmin.myprivatesite.org', 'https://blog.example.com', 'https://admin.mydomain.org', 'http://webmin.myprivatesite.org', 'http://www.mydomain.org']
```

Objet attendu en sortie :

```javascript
{
  'example.com': ['https://www.example.com', 'https://blog.example.com'],
  'mydomain.org': ['https://admin.mydomain.org', 'http://www.mydomain.org'],
  'myprivatesite.org': ['http://phpmyadmin.myprivatesite.org', 'http://webmin.myprivatesite.org']
}
```

Squelette du programme :

```javascript
// groupSitesByDomain.js
const assert = require('assert');

// Enlever les accolades si besoin
const groupSitesByDomain = sitesArray => {}

// Test
const input = ['https://www.example.com', 'http://phpmyadmin.myprivatesite.org', 'https://blog.example.com', 'https://admin.mydomain.org', 'http://webmin.myprivatesite.org', 'http://www.mydomain.org'];

const expected = {
  'example.com': ['https://www.example.com', 'https://blog.example.com'],
  'mydomain.org': ['https://admin.mydomain.org', 'http://www.mydomain.org'],
  'myprivatesite.org': ['http://phpmyadmin.myprivatesite.org', 'http://webmin.myprivatesite.org']
};
assert.deepStrictEqual(groupSitesByDomain(input), expected);
```

### Classer des élèves selon leur moyenne annuelle

Attention les yeux, ici un reduce dans le reduce est envisageable et même conseillé !

On te donne un tableau d'objets, chacun représentant un élève et ses notes des 3 trimestres de l'année scolaire. Exemple de tableau d'entrée :

```javascript
[
  {
    "name": "Carine Pichette",
    "notes": [12.25, 14, 9]
  },
  {
    "name": "Viollette Barjavel",
    "notes": [12, 14.5, 10.25]
  },
  {
    "name": "Julien Garcia",
    "notes": [9, 10, 14]
  },
  {
    "name": "Laurene Duplessis",
    "notes": [13, 7.5, 8]
  },
  {
    "name": "Pierre Labrosse",
    "notes": [10, 7, 11.5]
  },
  {
    "name": "Laetitia Louis",
    "notes": [14.5, 13, 11.5]
  }
]
```

Ecrire une fonction `getStudentResults` qui va :

* Calculer une moyenne par élève,
* Ajouter cette moyenne aux propriétés de l'élève sous la clé `average`,
* Trier les élèves entre ceux qui passent à l'année suivante (>= 10 de moyenne) et ceux qui échouent.

L'objet de sortie, avec le tableau d'entrée ci-dessus, devra être :

```javascript
{
  success: [
    { name: 'Carine Pichette', notes: [12.25, 14, 9], average: 11.75 },
    { name: 'Viollette Barjavel', notes: [12, 14.5, 10.25], average: 12.25 },
    { name: 'Julien Garcia', notes: [9, 10, 14], average: 11 },
    { name: 'Laetitia Louis', notes: [14.5, 13, 11.5], average: 13 }
  ],
  failure: [
    { name: 'Laurene Duplessis', notes: [13, 7.5, 8], average: 9.5 },
    { name: 'Pierre Labrosse', notes: [10, 7, 11.5], average: 9.5 }
  ]
}
```

Squelette du programme :
```javascript
const assert = require('assert');

const getStudentResults = studentsArray => {};

// Test
const input = [
  {
    "name": "Carine Pichette",
    "notes": [12.25, 14, 9]
  },
  {
    "name": "Viollette Barjavel",
    "notes": [12, 14.5, 10.25]
  },
  {
    "name": "Julien Garcia",
    "notes": [9, 10, 14]
  },
  {
    "name": "Laurene Duplessis",
    "notes": [13, 7.5, 8]
  },
  {
    "name": "Pierre Labrosse",
    "notes": [10, 7, 11.5]
  },
  {
    "name": "Laetitia Louis",
    "notes": [14.5, 13, 11.5]
  }
];

const expected = {
  success: [
    { name: 'Carine Pichette', notes: [12.25, 14, 9], average: 11.75 },
    { name: 'Viollette Barjavel', notes: [12, 14.5, 10.25], average: 12.25 },
    { name: 'Julien Garcia', notes: [9, 10, 14], average: 11 },
    { name: 'Laetitia Louis', notes: [14.5, 13, 11.5], average: 13 }
  ],
  failure: [
    { name: 'Laurene Duplessis', notes: [13, 7.5, 8], average: 9.5 },
    { name: 'Pierre Labrosse', notes: [10, 7, 11.5], average: 9.5 }
  ]
};
assert.deepStrictEqual(getStudentResults(input), expected);
```

