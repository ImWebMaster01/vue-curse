<script setup>
import {ref,computed} from "vue";
const name='Vue dinamico';
const styleColor="color:brown";
const arrayColores=["blue","green","yellow"];
const activo=true;

const arrayFrutas=['Manzana','Banana','Pera','Uvas','Durazno'];

const arrayRefrescos=[
    {
        name:"Coca-cola",
        price:"30.00",
        description:"A black soda"
    },
    {
        name:"Fanta-cola",
        price:"25.00",
        description:"A orange soda"
    },
    {
        name:"Pepsi-cola",
        price:"28.00",
        description:"A black soda"
    }
];

const heroe={
    name:"Superman",
    edad:"35",
    poder:"Volar y superfuerza"
}

const handlerClick=(message)=>{
alert(message);
}

const counter=ref(0);//indico que counter es un valor reactivo
const increment=()=>{
counter.value++;
}

const decrement=()=>{
counter.value--;
}

const reset=() => counter.value=0;


//propiedades computadas-> para realizar varias operaciones y en un solo lugar
const CounterComputado=computed(()=>{

    if(counter.value >= 0)
    return 'positive';
    else if(counter.value < 0)
    return 'negative';
    else
    return'zero';

});

//código para agregar numeros favoritos al array
const arrayFavoritos=ref([]);//definimos array reactivo con ref()
const add=()=>{
arrayFavoritos.value.push(counter.value);
}

//propiedad computada para bloquear y desbloquear botón
const bloquearBoton=computed(()=>{
const numSearch=arrayFavoritos.value.find(num=>num===counter.value);//buscamos si el numero que queremos ingresar ya esta en el array
    return numSearch || numSearch===0 ? true : false;//si existe el numero o si es cero entonces retorno true, en caso contrario retorno false
});

</script>

<template>
<h1>{{name.toUpperCase()}}</h1>
<h2 v-bind:style="styleColor">Color cafe</h2>
<h3 :style="`color: ${arrayColores[2]}`"> otro texto</h3>
<h4>
    <!--operador ternario-->
    {{ activo ? 'Estoy Activo' : 'Estoy inactivo' }}
</h4>
<p v-if="activo===true" :style="styleColor">Estoy activo</p>
<p v-else-if="activo===false" :style="styleColor">Estoy inactivo</p>
<p v-else>Estoy indeciso</p>

<!--muestra o esconde elementos del DOM en la plantilla-->
<p v-show="activo">Estoy activo y se muestra con v-show</p>

<br>
<!--recorrer array con v-for, el key no se recomienda usar el index, debe ser un valor irrepetible-->
<ul>
    <li v-for="elemento,index in arrayFrutas" :key="index">
       {{ index }} - {{ elemento }}
    </li>
</ul>

<br>
<!--recorrer array de bjetos de los refrescos-->
<ul>
    <li v-for="soda,index in arrayRefrescos" :key="soda.name">
       {{ soda.name }} - {{ soda.price }} - {{ soda.description }}
    </li>
</ul>

<br>
<!--recorrer Un solo y unico objeto y sus propiedades-->
<ul>
    <li v-for="(value,propiedad,index) in heroe" :key="value">
     {{ index }}   {{ propiedad }} : {{ value }}
    </li>
</ul>

<br>
<!--recorrer array de bjetos de los refrescos y mostrar solo los que tienen un precio menor a 30-->
<ul>
    <template v-for="soda in arrayRefrescos" :key="soda.name">
        <li v-if="soda.price<30">
            {{ soda.name }} - {{ soda.price }}
        </li>
    </template>
</ul>

<!---<h1 :class="counter>=0 ? 'positive' : 'negative'">{{ counter }}</h1>-->
<h1 :class="CounterComputado">{{ counter }}</h1>

<button v-on:click="handlerClick('Boton 1')">Boton 1</button>
<button @:click="handlerClick('Boton 2')">Boton 2</button>
<button @:click="increment">Increment</button>
<button @:click="decrement">Decrement</button>
<button @:click="reset">Reset</button>
<button @:click="add" :disabled="bloquearBoton">Add</button>

<div>
{{ arrayFavoritos }}
<ul>
    <li v-for="(num,index) in arrayFavoritos" :key="index">
        {{ num }}
    </li>
</ul>
</div>
</template>

<style>
h1{
    color:rgb(181, 214, 32);
}

.positive{
    color:green;
}
.negative{
    color:red;
}
.zero{
    color:blue;
}
</style>



