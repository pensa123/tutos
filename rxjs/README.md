# ReactiveX RXJS 

Este resumen lo saque del siguiente curso https://www.udemy.com/course/rxjs-de-cero-hasta-los-detalles/learn/lecture/16447042#overview 

* Por que usar extensiones reactivas 
   * por que todos quieren informacion en tiempo real 
   
   
* ¿Cuando usar rx? 
   * Eventos en la interfaz de usuario 
   * Comunicacion por sockets 
   * Cuando se trabaja con flujos

* Observables 
   * son la fuente de informacion 
   * pueden emitir valores 
   * pueden ser finitos e infinitos 
   * puede ser sincrona o asincrona 
	
* Suscribers 
   * se suscriben a un observable

* Operators 
   * transforman los observables 
   * filtran observables 
   * combinan observables 
   
## Patrones que usa RX 
* Observer pattern 
   * notifica cuando hay cambios 
* Iterator pattern 
   * Poder ejecutar operaciones secuenciales 
* Programacion funcional 
   * Tienen funciones expecificas que no mutan informacion 


## Proyecto base 
https://github.com/Klerith/curso-rxjs-inicio 

Despues de descargarlo se  ejecuta `npm install`

Podemos cambiar el puerto en el que se lanza (esta en el 8081) en config.json 

ya solo falta darle `npm run start` para iniciarlo 

## Observables 
Se recomienda ponerle un $ al final para hacer saber que es un observable 

Ejemplo basico 
```r
import { Observable } from 'rxjs';

const obs$ = new Observable(subs => {

    subs.next('Hola'); 
    subs.next('Mundo'); 
    

    subs.next('Hola'); 
    subs.next('Mundo'); 

    subs.complete(); //no mostrara los valores de abajo ↓

    subs.next('no sale');
});

obs$.subscribe(console.log);  
```

Ejemplo manejando complete y error 

```r
import { Observable } from 'rxjs';

const obs$ = new Observable<string>(subs => {

    subs.next('Hola'); 
    subs.next('Mundo'); 
    
    subs.next('Hola'); 
    subs.next('Mundo'); 

    // const a = undefined; 
    // a= 'hola';           //Es un error forzado para ver que si muestra el error del suscribe. 
    subs.complete(); 

    subs.next('no sale');
});

/*
Con esto si funciona pero esta depreciado y es el que enseñaron en el video 
obs$.subscribe(
    valor => console.log('Next', valor), 
    error => console.warn('Error' , error), 
    () => console.log('Completado')
);
*/

//Este si es el bueno 
obs$.subscribe(
    {
        complete: () => console.log('Completado'), 
        error : err => console.warn('Error...error...error', err), 
        next: next => console.log('Next', next)
    }
);
```


 ## Suscribe y unsuscribe 
 
 ```r
 import { Observable, Observer } from 'rxjs';

const observer: Observer<any> = {
    complete: () => console.log('Completado'),
    error: err => console.warn('Error', err),
    next: next => console.log('Next', next)
}

const intervalo$ = new Observable<number>(subs => {

    let cont = 0;
    setInterval(() => {
        subs.next(++cont);
        console.log(cont);
    }, 1000)
});
var sub1 = intervalo$.subscribe(num => console.log('Num: ', num));

setTimeout(() => {
    sub1.unsubscribe();
}, 3000);
 ```
 
El problema con unsuscribe es que solo deja de escuchar pero el interval que esta dentro de intervalo$ seguira funcionando mientras este activo 
y eso puede gastar mucha memoria (si hubieran muchos claro) 

esto se soluciona de la siguiente forma

```r
import { Observable, Observer } from 'rxjs';

const observer: Observer<any> = {
    complete: () => console.log('Completado'),
    error: err => console.warn('Error', err),
    next: next => console.log('Next', next)
}

const intervalo$ = new Observable<number>(subs => {

    let cont = 0;
    const interval = setInterval(() => {
        subs.next(++cont);
        console.log(cont);
    }, 1000)

    return () => {
        clearInterval(interval); 
        console.log("mantando al intervalo."); 
    }
});
var sub1 = intervalo$.subscribe(num => console.log('Num: ', num));

setTimeout(() => {
    sub1.unsubscribe();
}, 3000);
```

aunque ahora hay observables que hacen eso por ti es importante saber esta base ;) //o eso dijo el del video :D 


```r
import { Observable, Observer } from 'rxjs';

const observer: Observer<any> = {
    complete: () => console.log('Completado'),
    error: err => console.warn('Error', err),
    next: next => console.log('Next', next)
}

const intervalo$ = new Observable<number>(subs => {

    let cont = 0;
    const interval = setInterval(() => {
        subs.next(++cont);
        console.log(cont);
    }, 1000);

    setInterval(() => {
        subs.complete();
    }, 2500);

    return () => {
        clearInterval(interval);
        console.log("mantando al intervalo.");
    }
});
var sub1 = intervalo$.subscribe( observer );

setTimeout(() => {
    console.log('me desinscribi')
    sub1.unsubscribe();
}, 3000);
```
si se llama de esta manera primero se suscribe, manda el 1 , 2 , luego dicec completado, luego mantando al intervalo y de ultimo el me desinscribi 
por que al completarse primero se ejecuta la funcion que le mandamos de complete al observable y luego el return y si alguien se desinscribe no hacemos nada 

ahora miraremos como hacer que los observadores esten juntos y terminen a la vez 

```r
...
var sub1 = intervalo$.subscribe(observer);
var sub2 = intervalo$.subscribe(observer);
var sub3 = intervalo$.subscribe(observer);

sub1.add(sub2);
sub2.add(sub3);

setTimeout(() => {
    console.log('me desinscribi')
    sub1.unsubscribe();
}, 3000);
```

cuando se termina el sub1 se terminan tambien los otros dos subs 

## Agregando subject para tener mismos resultados 
```r
/* lo que estaba antes xd :D */
const subject$ = new Subject();
const intervalSubject = intervalo$.subscribe(subject$);

const sub1 = subject$.subscribe(x => console.log("sub1", x));
const sub2 = subject$.subscribe(x => console.log("sub2", x));

//Debido a que el subject tambien es un observable se le puede agregar 
setTimeout(() => {
    subject$.next(10); 
    subject$.complete(); 
    intervalSubject.unsubscribe(); 
}, 3000);
```
intervalo es conocido como un cold observable por que la data es producida adentro de si mismo 

subject es conocido como un hot observable por que la data es producida afuera de si mismo 

## Observable OF 
Este procesa los datos en sequencia. y este no es asyncrono 

```r
import { of } from 'rxjs';

const obs$ = of(1,2,3,4,5,6);

console.log('inicio del observable');
//Este observable es sincrono :D 
obs$.subscribe({
    next: next => console.log('next', next),
    complete: () => console.log('secuencia terminada')
})
console.log('fin del observable');
```

## FromEvent 

```r
	import { fromEvent } from 'rxjs'
/* Eventos del dump */

const src1$ = fromEvent<PointerEvent>(document, 'click');
const src2$ = fromEvent<KeyboardEvent>(document, 'keyup');

const obs = {
    next: val => console.log('next', val)
}


src1$.subscribe(({ x, y }) => console.log(x, y));
src2$.subscribe(even => console.log(even.key));
```

cuando no sabes cual es el nombre de un evento puedes dejarlo sin especificar, imprmirlo como tal y copiar 
![image](https://user-images.githubusercontent.com/36494183/143140582-8237d197-f029-4339-91a1-ca90a5c24a54.png)
el event que nos tira


