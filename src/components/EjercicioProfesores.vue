<template>
    <input type="number" v-model="numeroMaterias" />
    <button @click="seleccionaMaterias">Seleccionar numero de materias</button>

        <input v-show="presionoBotonMaterias === true" v-for="a in arreglo" :key="a" v-model="profe.materias[a]"/>
        <label>Nombre</label>
        <input type="text" v-model="profe.nombre" />
        <label>Apellidos</label>
        <input type="text" v-model="profe.apellidos" />
        <label>DNI</label>    
        <input type="text" v-model="profe.dni" />
        <input type="checkbox" v-model="profe.documentacion">tiene documentacion
        <button @click="AgregarProfe">Agregar</button>

    <table>
        <tr>
            <th>Nombre</th>
            <th>Apellidos</th>
            <th>DNI</th>
            <th>Documentacion</th>
            <th>Materias</th>
        </tr>
        <tr v-for="(profesor,index) in arregloProfes" :key="index">
            <td>{{ profesor.nombre }}</td>
            <td>{{ profesor.apellidos }}</td>
            <td>{{ profesor.dni }}</td>
            <td v-if="btnAcept === true">{{ profesor.documentacion ? "Sí" : "No" }}</td>
            <td v-for="m in profesor.materias" :key="m">{{ m }}</td>
        </tr>
    </table>
</template>

<script lang="ts" setup>
    // para settear el numero de inputs para las materias
    let numeroMaterias : Ref<number> = ref(0);
    let presionoBotonMaterias: Ref<boolean> = ref(false);

    let arreglo : Ref<Array<number>> = ref([]);
    const seleccionaMaterias = () => {
        presionoBotonMaterias.value = true;
        let i = 0;
        while (i < numeroMaterias.value){
            arreglo.value.push(i);
            i++;
        }
    }

    import {Ref, ref} from 'vue';
    interface Profesor {
        nombre: string,
        apellidos: string,
        dni: string,
        materias: Array<string>,
        documentacion: boolean
    }

    let profe: Ref<Profesor> = ref({
        nombre : '',
        apellidos : '',
        dni : '',
        materias : [],
        documentacion : false
    });

    let btnAcept: Ref<boolean> = ref(false);
    let arregloProfes:Ref<Array<Profesor>> = ref([]);
    
    const AgregarProfe = () => {
        btnAcept.value = true;
        
        //arregloProfes.value.push(profe.value);
        /* No podemos hacer esto ya que siempre va a tomar la misma referencia, ya que es un valor
        reactivo, SOLUCION: Agregar un nuevo objeto que vamos a crear que no sea reactivo al arreglo
        de profes*/
        arregloProfes.value.push({
            nombre : profe.value.nombre,
            apellidos : profe.value.apellidos,
            dni : profe.value.dni,
            materias : llenado(),
            documentacion : profe.value.documentacion
        });
        presionoBotonMaterias.value = false;
        
        while(arreglo.value.length > 0){
            arreglo.value.pop();
        }
        while(profe.value.materias.length > 0){
            profe.value.materias.pop();
        }
    }

    const llenado = ():Array<string> =>{
        let ms : Array<string> = [];
        let j = 0;
        while(j < profe.value.materias.length){
            ms.push(profe.value.materias[j]);
            j++;
        }
        return ms;
    }

</script>

<style scoped>

</style>