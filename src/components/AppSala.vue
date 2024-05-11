<template lang="html">
    <b-container>
        <div class="pantalla">
            <p class="fw-bold" style=" color: white; font-size: 18px">Pantalla</p>
        </div>
        <div class="asientos">
            <b-row>
                <b-col class="asiento disponible" v-for="asiento in asientos" :key="asiento.id" v-text="asiento.id"
                    :class="{ disponible: asiento.disponible, ocupado: !asiento.disponible }"
                    @click="seleccionarAsiento" :id="asiento.id">
                </b-col>
            </b-row>
        </div>
    </b-container>
</template>
<script>
import { child, get, getDatabase, ref, set } from "firebase/database";

const path = 'salas';
const pathId = 1;

export default {
    created () {
        const dbRef = ref(getDatabase());
        get(child(dbRef, `${path}/${pathId}`))
        .then((snapshot) => {
            if(snapshot.exists()) {
                console.log(snapshot.val());
                this.cargarElementos(snapshot.val())
            } else {
                console.log("No data available");
            }
        }).catch((error) => {
            console.error(error);
        })
    },
    data() {
        return {
            asientos: []
        }
    },
    methods: {
        seleccionarAsiento: function (event) {
            let asiento = this.asientos.find(a => a.id == event.target.id)
            asiento.disponible = !asiento.disponible
            console.log(asiento);
        },
        actualizarElementos: function () {
            const db = getDatabase();
            set(ref(db, `${path}/${pathId}`), this.asientos)
        },
        cargarElementos: function(data) {
            this.asientos = data
        }
    }
}
</script>
<style lang="css">
.pantalla {
    background-color: #2b6282;
}

.asientos {
    margin-top: 60px;
}

.asiento {
    color: white;
    margin: 3px;
    font-weight: bold;
    cursor: pointer;
}

.disponible {
    background-color: #2d4d86;
}

.ocupado {
    background-color: #73264f;
}
</style>