<template lang="html">
    <b-container>
        <div class="pantalla">
            <p class="fw-bold" style=" color: white; font-size: 18px">Pantalla</p>
        </div>
        <div class="asientos">
            <b-row>
                <b-col class="asiento disponible" v-for="asiento in asientos" :key="asiento.id" v-text="asiento.id"
                    :class="{ disponible: asiento.disponible, ocupado: !asiento.disponible, pointer: asientoDisponible(asiento) }"
                    @click="seleccionarAsiento" :id="asiento.id">
                </b-col>
            </b-row>
        </div>
        <div class="botones">
            <b-button variant="success" @click="guardar">Guardar</b-button>
            <b-button style="margin-left: 10px" @click="liberar">Liberar</b-button>
        </div>
    </b-container>
</template>
<script>
import { getDatabase, onValue, ref, set } from "firebase/database";

const path = 'salas';
const pathId = 1;

export default {
    created() {
        onValue(ref(getDatabase(), `${path}/${pathId}`),(snapshot) => {
                if (snapshot.exists()) {
                    console.log(snapshot.val());
                    this.cargarElementos(snapshot.val())
                } else {
                    console.log("No data available");
                }
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
            if (asiento.adquirido) {
                console.log("No es posible seleccionar el asiento " + asiento.id);
                return
            }
            asiento.disponible = !asiento.disponible
            this.actualizarElementos()
            console.log(asiento);
        },
        actualizarElementos: function () {
            const db = getDatabase();
            set(ref(db, `${path}/${pathId}`), this.asientos)
        },
        cargarElementos: function (data) {
            this.asientos = data
        },
        guardar: function () {
            this.validarAsientos();
            this.actualizarElementos();
            console.log("transaccion ejecutada");
        },
        validarAsientos: function () {
            this.asientosSeleccionados().forEach(asiento => {
                asiento.adquirido = true
            });
        },
        asientoDisponible: function (asiento) {
            return !asiento.adquirido
        },
        asientosSeleccionados: function () {
            return this.asientos.filter(a => !a.disponible && !a.adquirido)
        },
        liberar: function() {
            this.asientos.forEach(function(asiento) {
                asiento.disponible = true
                asiento.adquirido = false
            })
            this.actualizarElementos()
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

.botones {
    margin-top: 60px;
}

.pointer {
    cursor: pointer;
}
</style>