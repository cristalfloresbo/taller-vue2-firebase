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
export default {
    data() {
        return {
            asientos: [
                { id: 'A1', disponible: false },
                { id: 'A2', disponible: true },
                { id: 'A3', disponible: true },
                { id: 'A4', disponible: false },
                { id: 'A5', disponible: true },
                { id: 'A6', disponible: true },
            ]
        }
    },
    methods: {
        seleccionarAsiento: function (event) {
            let asiento = this.asientos.find( a => a.id == event.target.id)
            asiento.disponible = !asiento.disponible
            console.log(asiento);
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