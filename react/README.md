# ReactJS 
Aprendido del curso https://www.udemy.com/course/react-cero-experto/

* [Repaso de javascript](#Repaso-de-javascript)
* [Iniciando con react](#Iniciando-con-react)


## Repaso de javascript 
* Ya no se debe de usar var, solo let o cons 
* [Template string](#Template-string)
* [Desestrucutacion](#Desestructuracion)
* [Clonar objetos](#Clonar-objetos)
* [Exportaciones](#Exportaciones)
* [Promesas](#Promesas)
* [Peticiones fetch](#Peticiones-fetch)
* [Async y await](#Async-y-await)
### Template string
Template string sirve para poder escribir en muchas lineas sin tener que hacer '' + ''(en la siguiente liena) 
tambien ayuda a la hora de poner variables de javascript ya que se ponene solo usando {variable}, puede que no se muri muy util pero puede ahorar tiempo 

Parea usar un template string se usa ` y eso en mi teclado lo pongo con altgr + } 

```r
const nombre_completo = nombre +  ' ' + apellido; 
const nCompleto = `${nombre} ${apellido}`;
console.log(nCompleto  === nombre_completo) ; 
```

### Desestructuracion 

Si se tiene un objeto o un arreglo se puede realizar una desestructuración, esto nos puede servir a no tener que escribir cosas como arreglo[indice] o arreglo objeto.elemento 
* Ejemplo desestructuracion de un arreglo 

```r
  const retornaArreglo = () => {
    return [ 'abc', 123];
  }

  const [v1, v2] = retornaArreglo(); 
```
* Ejemplo desestructuracion de un objeto 
   * Si se quiere poner un aleas a la variable se pone el nombre como esta en el objeto:elNombreComoQuierenPonerl 
   * si no se le quiere cambiar el nombre solo se le pone lo mismo (como en edad) 
```r
    var obj = {nombre : "nombre" , edad : 12 } ;
    var {nombre:nom , edad  } = obj; 
    console.log(nom , edad); 
```
* Tambien se puede hacer desestructuración de objetos con objetos dentro. 
```r
  var {nombreCLave , anios, latlng:{lat, lng}} = { nombreCLave : clave,anios : edad,latlng:{lat: 12, lng: -12}    }
  console.log(nombreCLave , anios, lat, lng); 
```

### Clonar objetos
si se iguala un arreglo a otro solo se guardara la referencia y se modifica uno se modificara tambien el otro arreglo. 
Para igualarlos por valor se hace poniendo ...NombreDelObjetoOArreglo 
```r
  var arr1R = [1,2, 3 , 4 , 5]; 
  var arr2R = arr1; 
  arr2R[0] = 5; 
  console.log(arr2R[0] == arr1R[0]) //el resultado es true 
  
  var arr1V = [1,2,3,4,5]; 
  var arr2V = [...arr2V]; 
  arr2V[0] = 5; 
  console.log(arr1V[0] == arr2V[0]);   
```
* Si se tiene un arreglod entro del de ese no se copiara el valor sino que seguira siendo por referencia, es decir si se tiene [[0,1] , [0,1]] 
* Con objetos es similar 

```r
  var obj = {nombre : "pensa", arr : [1,2] };
  var obj2 = {...obj};
  
  obj2.nombre = 'jj' ; 
  obj2.arr[0] = 3; 
  
  /*
    obj es igual a {nombre : 'pensa' , arr[3,2] } 
    obj2 es igual a {nombre : 'jj' , arr[3,2] } 
  */
```

### Exportaciones 
* Esisten dos maneras, exportar procedimiento por procedimiento  o creando un export al final 
```r 
  export const varAExportar = [1, 2 , 3 ,4 ]; 
  export default const varAExportarDefault = [1,2,3,4]; 
  
  //---- Seria lo mismo que 
  const varAExportar = [1, 2 , 3 ,4 ];
  const varAExportarDefault = [1,2,3,4];
  
  export {varAExportar, varAExportarDefault as default};
```
* y se importarian de la siguiente forma, por defecto busca un .js (no es necesario poner el .js) 
```r
  import varAExportarDefault, { varAExportar } from "./rutaArchivo/nombreArchivo";
```

### Promesas
en las promesas se tienen dos objetos resolve y reject, hay que tener cuidado ya que cuando llamammos al resolve o reject no es como un return y el proceso sigue
si tenemos if(true) reject("esto es un error");      resolve("todo bien")  se ejecutarian ambas y podria haber confusión, es recomendado poner un return 


cuando llamamos a una promesa hay que usar .then() que sera el handler que se llama despues del resolve 
y .catch que es el handler que se llama cuando ocurre un error , tambien existe finally que se llamara despues del then y catch 
```r
const getHeroeByIdAsync = (id) => {
    return new Promise( (resolve, reject) => {
        setTimeout(() => {
           // console.log('2 segundos despues');
            const aux = getHeroeById(id); 
           // console.log(aux);
    
           if(aux !== undefined ){
               resolve( aux); 
           }else{
               reject('no existe el hereo');
           }
        }, 2000);
    } ); 
}
getHeroeByIdAsync(3).then(console.log ).catch( console.warn);
```

### Peticiones fetch
* En este ejemplo usamos api.giphy que devuelve un gif random, creo que fetch tiene por defecto get. 
```r
const apiKEY = 'apiKey :D';
const peticion = fetch(`https://api.giphy.com/v1/gifs/random?api_key=${apiKEY}`);

peticion.then(  resp => resp.json() )
.then(({data}) =>{ 
    const {url} = data.images.original; 
    console.log(url); 

    const img = document.createElement('img'); 
    img.src = url; 

    document.body.append(img); 
} ).catch(console.warn); 
```

### Async y await
* Esto hace lo mismo que le anterior pero con async y usando await 
```r
  const getImagen = async () => {
    try {
        const apiKEY = 'apiKey :D';
        const respuesta = await fetch(`https://api.giphy.com/v1/gifs/random?api_key=${apiKEY}`);
        const {data} = await respuesta.json();
        const {url} = data.images.original;
        const img = document.createElement('img');
        img.src = url;
        document.body.append(img);
    } catch (error) {
        console.error(error);
    }
};
```

## Iniciando con react
* [Hola mundo](#Hola-mundi)
* [Prototypes](#prototypes)

### Hola mundo 
* react utiliza jsx y es recomendado utilizarlo con babel para que sea mas facil 
* jsx nos permite utilizar etiquetas de html en el codigo de react como `h1Tag = <h1> Hola soy {nombre}</h1>`
* babel nos ayuda a pasar javascritp mas reciente a javascript mas bajo y que pueda ser usado en varios navegadores como por ejempo 
   * \`${algo} {algo2}\` =>   algo + " " + algo2 

```r
  <!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <!-- Cargat React -->
    <script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    <title>Curso de React</title>
</head>
<body>
    <div id="root"></div>
    <script type="text/babel">
        var nombre = 'Goku'; 
        const h1Tag = <h1>Hola soy {nombre}</h1>;
        const divRoot = document.querySelector('#root');
        ReactDOM.render(h1Tag, divRoot);
    </script>
</body>
</html>
```

## Prototypes 
Si estas usando visual studio code la forma mas facil es usando rafcp y te crea el esqueleto del prototype 

```r
  import React, {useState} from 'react'
import PropTypes from 'prop-types';

const CounterApp = ( {value = 0} ) => {
    //handeAdd
    const handelAdd = (e) => {
        setCounter(counter + 1);   
    }
    const handelDecrese = (e) => {
        setCounter(counter - 1); 
    }
    const [counter, setCounter] = useState(value); 

    return <> 
        <h1>CounterApp</h1>
        <h2> { counter } </h2>
        <button onClick={  handelAdd }> +1 </button>
        <button onClick={ () => {setCounter(value)}}> Reset </button>
        <button onClick={ handelDecrese}> -1 </button>
    </>; 
}

CounterApp.propTypes = {
    value : PropTypes.number.isRequired
}

export default CounterApp; 
// rafcp  shortcut to create a prototype 
```

Y desde el index.js se manda a llamar de la siguiente manera. 

```r
import React from 'react'; 
import ReactDOM from 'react-dom';
import CounterApp from './CounterApp';
import './index.css';

const divRoot = document.querySelector('#root'); 

ReactDOM.render( <CounterApp value={15} /> , divRoot); 
```

## Pruebas en react 
Se puede poner un describe para almacenar varias pruebas 
y para cada prueba se usa un test un ejemplo es el siguiente. 


```r
describe('Pruebas del archivo demo.test.js', () => {   
    test('String iguales', () => {
        // 1 inicializacion 
        const mensaje = 'Hola Mundo'; 
        
        //2 
        const mens2 = `Hola Mundo`; 
        
        expect( mensaje).toBe(mens2); 
    })
})
```
## Utilizando import por defecto en todos los .test.js
Para realizar pruebas se puede crear un archivo llamado `setupTest.js` y en ese se ponen los imports que necesitamos. 

```r
import '@testing-library/jest-dom/extend-expect';

import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
import {createSerializer} from 'enzyme-to-json';

Enzyme.configure({ adapter: new Adapter() });


expect.addSnapshotSerializer(createSerializer({mode: 'deep'}));
```

Para esta parte vamos a utilizar enzyme 
para esto es necesario instalar unos paquetes 


* npm install --save-dev @wojtekmaj/enzyme-adapter-react-17
* npm i --save-dev enzyme enzyme-adapter-react-16
* npm install --save-dev enzyme-to-json


# Pruebas con las vistas 

Veremos lo que son snapshots y 
```r
   test('should render', () => {
        const saludo = "hola";   
        const wrap = render(<PrimeraApp saludo={ saludo }  />)       //con u se guarda el snapchot 

        expect( wrap).toMatchSnapshot();
    })
```

este guarda un snapchot de la pagina como esta en el momento de crear la prueba, despues de darle npm run test se pueden notar si hicieron algun cambio que cambiara la vista del html que genera, y nos indicara que se cambio, si queremos dejar los nuevos cambios escribimos la tecla "U" 

```r
import React from 'react';
import { shallow} from "enzyme";
import {render} from '@testing-library/react'
import '@testing-library/jest-dom';
import CounterApp from '../CounterApp';


describe('Prueba en <Counter app />', () => {

    const wrap = render(<CounterApp value={0}  />)       //con u se guarda el snapchot 
    let wrapSH = shallow(<CounterApp value={10}  />)       //con u se guarda el snapchot 
    let valor = 10; 

    beforeEach(() => {
        // configurar un elemento del DOM como objetivo del renderizado
        wrapSH = shallow(<CounterApp value={valor}  />);
      });

    test('should render', () => {
        expect( wrap).toMatchSnapshot();
    })
    
    test('should show the subtitle', () => {
        const valor = 100;
        const wrap2 = shallow(<CounterApp value={valor}  />)       //con u se guarda el snapchot 
        const textoParrafo = wrap2.find('h2').text().trim(); 
        expect( textoParrafo ).toBe( `${valor}` ); 

    })
    
    test('should increse 1', () => {
        const btn = wrapSH.find('button').at(0); 
        console.log(btn.html); 
        btn.simulate('click'); 
        const textoParrafo = wrapSH.find('h2').text().trim(); 
        expect( textoParrafo ).toBe( `${valor + 1}` ); 
    })

    test('should decrese 1', () => {
        wrapSH.find('button').at(2).simulate('click'); 
        const textoParrafo = wrapSH.find('h2').text().trim(); 
        expect( textoParrafo ).toBe( `${valor - 1}` ); 
    })


    test('should reset to 10', () => {
       
       wrapSH.find('button').at(0).simulate('click'); 
       wrapSH.find('button').at(0).simulate('click'); 
       wrapSH.find('button').at(0).simulate('click'); 
       wrapSH.find('button').at(0).simulate('click'); 
       wrapSH.find('button').at(0).simulate('click'); 
       wrapSH.find('button').at(0).simulate('click'); 
       wrapSH.find('button').at(0).simulate('click'); 
       let textoParrafo = wrapSH.find('h2').text().trim(); 
       wrapSH.find('button').at(1).simulate('click'); 
       console.log(textoParrafo); 
       textoParrafo = wrapSH.find('h2').text().trim(); 
        console.log(textoParrafo); 
        expect( textoParrafo ).toBe( `${valor}`)
    })    
})
```


