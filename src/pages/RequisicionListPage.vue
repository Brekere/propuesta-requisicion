<template>
    <div class="q-pa-md" style="max-width">
      <h5>Requisicion de Personal</h5>

        <q-list class="q-pa-md with-border">
            <h6>Informacion del puesto solicitado</h6>
            <q-item>
                <strong>Fecha de emicion: </strong>( Aqui la fecha cuando se creo )
            </q-item>
            <q-item>
                <strong>Puesto: </strong>{{ formData.puesto }}
            </q-item>
            <q-item>
                <strong>Sucursal solicitante: </strong>( Este dato se obtendria del login )
            </q-item>
            <q-item>
                <strong>Area solicitante: </strong>( Este dato se obtiene del login )
            </q-item>
        </q-list>

        <br>

        <q-list class="q-pa-md with-border">
            <h6>Tipo de vacante</h6>
            <q-item>
                <strong>Tipos de Vacante:</strong> {{ formData.tiposDeVacante }}
            </q-item>

            <q-item v-if="formData.motivo">
                <strong>Motivo: </strong>{{ formData.motivo }}
            </q-item>

            <q-item v-if="formData.aQuien">
                <strong>A quien: </strong>{{ formData.aQuien }}
            </q-item>

            <q-item v-if="formData.especifique">
                <strong>Especifique: </strong>{{ formData.especifique }}
            </q-item>
        </q-list>

        <br>

        <q-list class="q-pa-md with-border">
            <h6>Generales del puesto</h6>
            <q-item>
                <strong>Sexo: </strong>
                <div v-for="opcion in formData.sexo" :key="opcion.value">{{ opcion.value }}</div>
            </q-item>

            <q-item>
                <strong>edades: </strong>{{ formData.edades }}
            </q-item>

            <q-item>
                <strong>Escolaridad: </strong>{{ formData.escolaridad }}
            </q-item>

            <q-item>
                <strong>Habilidades: </strong>
                <div v-for="opcion in formData.habilidades" :key="opcion">{{ opcion }}</div>
            </q-item>

            <q-item>
                <strong>Manejo de equipos: </strong>
                <div v-for="opcion in formData.opcionesManejoDeEquipo" :key="opcion">{{ opcion }}</div>
            </q-item>
            <q-item>
                <strong>Idiomas: </strong>
                <div v-for="opcion in formData.idioma" :key="opcion.value">{{ opcion.value }}</div>
            </q-item>

            <q-item>
                <strong>Sueldo mensual inicial: </strong>${{ formData.sueldoMensualInicial }}
                <div v-if="formData.comision==true">Mas comiciones</div>
            </q-item>

            <q-item>
                <strong>Competencias: </strong>
                <div v-for="opcion in formData.competencia" :key="opcion.value">{{ opcion.value }}</div>
            </q-item>

            <q-item>
                <strong>Experiencias: </strong>
                <div v-for="opcion in formData.experiencias" :key="opcion">{{ opcion }}</div>
            </q-item>

            <q-item>
                <strong>Actividades principales: </strong>
                <div v-for="opcion in formData.actividades" :key="opcion">{{ opcion }}</div>
            </q-item>

            <q-item>
                <strong>Equipos a proporcionar: </strong>
                <div v-for="opcion in formData.equipo" :key="opcion.value">{{ opcion.value }}</div>
            </q-item>

            <q-item>
                <strong>Fecha limite para cubrir la vacante: </strong>{{ formData.date }}
            </q-item>
        </q-list>

        <q-list class="q-pa-md with-border">
            <h6>Autorizaciones</h6>

            <q-item class="with-border">
                <strong>Nombre y firma del solicitante</strong>
            </q-item>

            <q-item class="with-border">
                <strong>Nombre y firma de quien autoriza</strong>
            </q-item>

            <q-item class="with-border">
                <strong>Fecha en que se cubrio la vacante</strong>
            </q-item>

            <q-item class="with-border">
                <strong>observaciones</strong>
            </q-item>
        </q-list>

    </div>
</template>

<script>
export default {
  mounted () {
    // Obtener los parámetros de la URL
    const queryParams = new URLSearchParams(this.$route.query)

    // Obtener el valor del parámetro 'data'
    const jsonData = queryParams.get('data')

    if (jsonData) {
      // Parsear el JSON y utilizar los datos como necesites
      this.formData = JSON.parse(decodeURIComponent(jsonData))
    } else {
      console.error('No se encontraron datos en la URL.')
    }
  },

  data () {
    return {
      formData: {}
    }
  }
}
</script>

<style scoped>
.with-border {
  border: 1px solid #ccc;
  /* Puedes ajustar el grosor y el color del borde según tus necesidades */
  border-radius: 5px;
  /* Opcional: añade esquinas redondeadas */

}

.boton {
    margin-bottom: 16px;
    margin-top: 16px;
  }
</style>
