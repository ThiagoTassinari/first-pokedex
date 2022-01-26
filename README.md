# Pokedex

![print do projeto](https://user-images.githubusercontent.com/38354809/151235266-e5765045-fc75-427b-934b-6ef0764962fb.PNG)

[API pra puxar os dados JSON](https://pokeapi.co/)

[API para buscar imagens dos pokemons com mais qualidade](https://pokeres.bastionbot.org)

<h2 align="center">Detalhes sobre o código javascript</h2>

<strong>1. Pegando dentro da API todos os IDs</strong>

``` js
const getPokemonUrl = id => `https://pokeapi.co/api/v2/pokemon/${id}`
```

<strong>2. Vamos encadear a invocação do método nesse array, onde recebe as 150 promises dos pokemons que é gerado no fetch</strong>

``` js
const generatePokemoPromises = () => Array(150).fill().map((_, index) =>
    fetch(getPokemonUrl(index + 1)).then(response => response.json()))
```

<strong>3. Aqui vamos gerar os li da minha página HTML dinamicamente puxando as props da API, onde o reduce vai reduzir a minha lista de arrays em JSON</strong>

``` js
const generateHTML = pokemons => pokemons.reduce((accumulator, { name, id, types }) => {
    const elementTypes = types.map(typeInfo => typeInfo.type.name)

    accumulator += `
        <li class="card ${elementTypes[0]}">
        <img class="card-image" alt="${name}" src="https://pokeres.bastionbot.org/images/pokemon/${id}.png" />
            <h2 class="card-title">${id}. ${name}</h2>
            <p class="card-subtitle">${elementTypes.join(' | ')}</p>
        </li>
    `
    return accumulator;
}, '')
```

<strong>4. Aqui será alterado dentro do meu html nas tags li as informações guardadas pela variável generatePokemonPromises, onde a requisição será chamada no array</strong>

``` js
const insertPokemonsIntoPage = pokemons => {
    const ul = document.querySelector('[data-js="pokedex"]')
    ul.innerHTML = pokemons
}

const pokemonPromises = generatePokemoPromises()

Promise.all(pokemonPromises)
    .then(generateHTML)
    .then(insertPokemonsIntoPage)
```
