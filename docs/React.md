# React cheatsheet

Voici une courte fiche de révision qui permet de balayer 90% des notions de React. Cela ne remplace pas un cours tel que
ceux listé dans la partie [Cours](cours.md).

## Props

Les composants React peuvent prendre ce que l'on appelle des props. Les **props peuvent être comparés aux paramètres**
d'une fonction.

Exemple de déclaration de props :

```jsx
import PropTypes from 'prop-types';

// Définition d'un composant classique
const Welcome = (props) => (
    <h1>Bonjour, {props.name}</h1>
);

// Déclaration des props existant avec leur type
Welcome.propTypes = {
    name: PropTypes.string
}

// Valeur par défaut si le prop n'est pas donné
Welcome.defaultProps = {
    name: 'nobody'
}
```

On peut ensuite utiliser ce composant :

```jsx
const name = 'Edite';

const App = () => (
    <div style={{backgroundColor: 'purple'}}> {/* Les balises HTML basique peuvent également prendre des props */}
        <Welcome/> {/* Le prop utilisera la valeur par défaut */}
        <Welcome name="Cahal"/> {/* On donne un string directement */}
        <Welcome name={name}/> {/* On donne une variable au prop `name` */}
    </div>
)
```

## Hooks

React 16 a introduit les hooks, littéralement les crochets.  
Les hooks permettent de stocker des données tel une variable ou encore de déclencher des effets.

### `useState`

`useState` permet de stocker des données, dans un composant React il s'utilise comme suit :

```javascript
const [count, setCount] = useState(0);
```

`useState` renvoie un tableau que l'on déstructure. Le premier élément est la variable
(attention cette valeur doit être uniquement lû). Le second élément est un setter de cette variable.

Exemple complet :

```javascript
const Clicker = () => {
    const [count, setCount] = useState(0);

    return (
        <>
            <p>Vous avez cliqué {count} fois</p>
            <button onClick={() => setCount(count + 1)}>
                Cliquez ici
            </button>
        </>
    );
}
```

Il existe deux manières de définir la valeur d'un state. La première, ci-dessus, est la plus courante et souvent
suffisante.

La deuxième, s'écrit de cette manière :

```javascript
setCount((prevValue) => prevValue + 1);
```

On donne une fonction à `setCount`, cette fonction prend un paramètre, l'ancienne valeur du state.  
Cette syntaxe est parfois utile lorsque plusieurs modifications d'un state arrivent presque en même temps ou pour
réduire les dépendances (voir plus bas).

```javascript
const [count, setCount] = useState(0);

// ...

// count sera égale à 1 et pas à 2
setCount(count + 1);
setCount(count + 1);

// count sera égale à 2
setCount((prev) => prev + 1);
setCount((prev) => prev + 1);
```

### `useEffect`

`useEffect` permet de déclencher des effets.

Dans cet exemple, on veut un composant qui affiche les données d'un utilisateur.  
On va donc fait un appel d'API. Cet appel doit être fait une première fois, et à chaque fois que le prop `id` change.

Pour cela, on appelle le hook `useEffect` qui prend 2 paramètres, un **callback qui sera le code à exécuter et un
tableau**. Ce **tableau représente les dépendances** du hook, c'est-à-dire les **valeurs qui vont être surveillées** par
React. Lorsqu'une de ces **valeurs change**, la fonction callback va être **ré-exécuté**.  
Ici la fonction sera ainsi exécutée à chaque fois que le prop `id` change. Ce qui est parfait dans notre cas car nous
voulons changer les données affichées au changement d'utilisateur.

```javascript
const Clicker = (id) => {
    const [data, setData] = useState();

    useEffect(() => {
        fetch(`/api/users/${id}`)
            .then((res) => {
                setData(res);
            })
    }, [id]);

    return (
        <>
            {data}
        </>
    );
}
```

Les `useEffect` ont également la possibilité de définir des fonctions de nettoyage dites cleanup. On les définit de la
manière suivante :

```javascript
const Status = (id) => {
    useEffect(() => {
        fetch(`/api/users/${id}/connected`);

        return () => {
            fetch(`/api/users/${id}/disconnected`);
        }
    }, [id]);

    return (
        <>
            {data}
        </>
    );
}
```

Pour définir une **fonction cleanup**, un `useEffect` **retourne une fonction**.  
Dans cet exemple, on signale au serveur que nous sommes connectés au montage du composant. Puis, au démontage, React
appellera la fonction cleanup du `useEffect` pour signaler notre déconnexion.

### `useCallback`

Une fonction **composant React** peut être **appelée plusieurs fois** lors de la vie de l'application.  
Toutes les déclarations et calculs sont alors **ré-exécutés**. C'est pour cette raison que l'on utilise des states et
non pas des variables classiques.

Ainsi, dans le code suivant, la fonction callback de `onClick` est redéfinie à chaque rendu. Cela est inutile et utilise
des resources inutilement.

```javascript
const Clicker = () => {
    const [count, setCount] = useState(0);

    return (
        <>
            <p>Vous avez cliqué {count} fois</p>
            <button onClick={() => setCount(count + 1)}>
                Cliquez ici
            </button>
        </>
    );
}
```

On utilise alors un `useCallback` qui se construit de la même manière que `useEffect`, une fonction **callback** et
un **tableau de dépendance**.  
Avec cette syntaxe, le callback sera défini une première fois et enregistré dans la mémoire.

```javascript
const Clicker = () => {
    const [count, setCount] = useState(0);

    const handleClick = useCallback(() => {
        setCount(count + 1)
    }, [count]);

    return (
        <>
            <p>Vous avez cliqué {count} fois</p>
            <button onClick={handleClick}>
                Cliquez ici
            </button>
        </>
    );
}
```

Le callback ne sera **redéfini que lorsqu'une dépendance aura changé**. Or, ici, `count` est une dépendance et `count`
est changé à chaque fois par la fonction, elle sera donc redéfinie à chaque rendu : retour au point de départ.  
Dans ce genre de cas, on utilise la seconde syntaxe du `useState`, celle qui prend une fonction avec l'ancienne valeur,
puisque l'ancienne valeur est tout ce dont on a besoin.

```javascript
const Clicker = () => {
    const [count, setCount] = useState(0);

    const handleClick = useCallback(() => {
        setCount((prev) => prev + 1)
    }, []);

    return (
        <>
            <p>Vous avez cliqué {count} fois</p>
            <button onClick={handleClick}>
                Cliquez ici
            </button>
        </>
    );
}
```

On a ainsi supprimé la dépendance. Mieux encore, puisque le callback n'a **pas de dépendances**, il ne sera **défini que
lors du premier rendu**, puis stocker en mémoire pour le reste de la vie de l'application.

### `useMemo`

De la même manière que l'on peut sauvegarder un callback, on peut **sauvegarder un rendu**.  
Ce hook s'appel le `useMemo` et s'utilise aussi simplement que :

```javascript
const Clicker = () => {
    const [count, setCount] = useState(0);

    const render = useMemo(() => ( // Noter l'utilisation d'une fonction fléchée avec retour direct
        <div>{/* Rendu très couteux qui est rarement actualisé */}</div>
    ), [/* Tableau des dépendances, le rendu sera re-calculé si une des valeurs ici change */]);

    return (
        <>
            {/* Du contenu */}
            {render} {/* Utilisation du rendu pré-calculé est sauvegardé */}
            {/* Et des tags JSX */}
        </>
    );
}
```

### `useRef`

Ce hook est le plus compliqué à utiliser, il permet d'accéder à la balise HTML sous-jacente du
composant. [Exemples](https://www.w3schools.com/react/react_useref.asp).  
Les composants react doivent forcément être "traduit" en balise HTML valide, quelquefois il est utile de connaitre ces
balises. Comme dans cet exemple, où un bouton donne le focus à un `input`.

```javascript
const App = () => {
    const inputElement = useRef();

    // Cette fonction est redéfinie à chaque fois, un useCallback serait plus adapté.
    const focusInput = () => {
        inputElement.current.focus();
    };

    return (
        <>
            <input type="text" ref={inputElement}/>
            <button onClick={focusInput}>Focus Input</button>
        </>
    );
}
```

## Tests

Dans une application de taille entreprise, il est important de pouvoir tester son code. Cela est réalisé en partie par
les **tests unitaires**. Les tests peuvent être écrits avec **jest**, un framework de test.

Jest permet de tester du code JS, or nous avons du "code React". Il existe des adapters qui permettent de rajouter des
fonctionnalités pour tester du code React. C'est également une partie qui est cachée par `create-react-app`

La première étant de pouvoir simuler le rendu d'un composant React. On peut ensuite prendre une **snapshot** de ce
rendu, celle-ci est **enregistrée dans un fichier** et sera **comparée au prochain test**. On peut ainsi détecter si
l'ajout d'une feature introduit un changement graphique.

En vrac, on peut aussi tester si une balise est bien dans le DOM, le texte qu'elle contient. On peut aussi effectuer des
interactions, cliquer sur des boutons ...  
Tout cela dans le but de garantir le bon fonctionnement de l'application.

## HOC

[Higher order component](https://fr.reactjs.org/docs/higher-order-components.html).  
Un HOC est un **composant enveloppant un autre composant**, on peut ainsi mieux séparer la logique et donc mieux la
réutiliser.  
Par exemple, on peut avoir un HOC qui s'occupe de récupérer des données et un composant enveloppé qui s'occupe d'
afficher des données.

Un [context](https://fr.reactjs.org/docs/context.html) est un exemple de HOC.

## Remarques importantes React

### Cycle de vie des composants React

Les composants React ont un cycle de vie.

1. Ils sont d'abord **montés** (c.-à-d. affiché)
2. Ils peuvent être re-rendu, ou actualisé (un re-render en jargon React)
3. Enfin ils sont **démontés**

Les **re-render** se produisent quand le composant a **besoin d'être mis à jour visuellement**. Cela peut être le cas
lors de la modification d'un state, lors de la mise à jour d'un `useEffect` ou d'un `useMemo`.

### Controlled inputs

Une technique rendu possible par React est le contrôle des inputs. On appelle **champ contrôlé**, un champ dont la
valeur est forcée et dont les changements sont traités.

Exemple :

```javascript
const App = () => {
    const [value, setValue] = useState('toto');

    // Utilisation d'un useCallback
    const handleChange = useCallback(({target: {value}}) => {
        setValue(value.toLowerCase())
    }, []);

    return (
        <>
            <input type="text" value={value} onChange={handleChange}/>
        </>
    );
}
```

L'input est un champ contrôlé, on force sa valeur via le state `value`. À chaque changement dans le champ input, on
définit la nouvelle valeur comme la valeur entrée et transformée en minuscule.  
Pour résumer, ce composant force l'écriture en minuscule dans ce champ.