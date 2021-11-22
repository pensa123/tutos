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

