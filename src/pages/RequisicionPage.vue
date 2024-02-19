<template>
  <div class="q-pa-md" style="max-width">
    <div class="q-pa-md with-border" style="max-width">
    <h5>Vacante por cubrir</h5>

    <q-list>

      <q-list class="with-border">
        <q-item>
          <q-item-section>
            <q-select
              transition-show="jump-up"
              transition-hide="jump-up"
              borderless
              v-model="form.puesto"
              :options="puestos"
              label="Puesto que desea solicitar"
            />
          </q-item-section>
        </q-item>
      </q-list>

    </q-list>
  </div>

  <br>

  <div class="q-pa-md with-border" style="max-width">
    <h5>Tipo de vacante</h5>

    <q-list>

      <q-list class="q-ma-none">
        <q-item>
          <q-item-section>
            <q-option-group
              v-model="form.tiposDeVacante"
              :options="tiposDeVacantes"
              color="primary"
              inline
            />
          </q-item-section>
        </q-item>

        <q-item v-if="showMotivoInput">
          <q-item-section>
            <q-input outlined v-model="form.motivo" label="Motivo" />
          </q-item-section>
        </q-item>

        <q-item v-if="showAQuienInput">
          <q-item-section>
            <q-input outlined v-model="form.aQuien" label="A quien" />
          </q-item-section>
        </q-item>

        <q-item v-if="showEspecifiqueInput">
          <q-item-section>
            <q-input outlined v-model="form.especifique" label="Especifique" />
          </q-item-section>
        </q-item>

      </q-list>

    </q-list>
    </div>

    <br>

    <div class="q-pa-md with-border" style="max-width">
    <h5>Generales del puesto</h5>

    <q-list class="with-border">
        <q-item>
          <q-item-section>
            <q-select
              transition-show="jump-up"
              transition-hide="jump-up"
              borderless
              multiple
              v-model="form.sexo"
              :options="sexos"
              label="Sexo"
            />
          </q-item-section>
        </q-item>
      </q-list>

      <br>

      <q-list class="with-border">
        <q-item>
          <q-item-section>
            <q-select
              transition-show="jump-up"
              transition-hide="jump-up"
              borderless
              v-model="form.escolaridad"
              :options="escolaridades"
              label="Escolaridad requerida"
            />
          </q-item-section>
        </q-item>
      </q-list>

      <br>

      <q-list class="with-border">
        <q-item-section>Rango de edades</q-item-section>
        <q-item>
          <q-item-section>
            <q-range
              class="q-mt-xl"
              v-model="form.edades"
              color="deep-orange"
              label-always
              markers
              marker-labels
              :min="18"
              :max="60"
              :inner-min="18"
              :inner-max="60"
            >
              <template v-slot:marker-label-group="scope">
                <div
                  v-for="marker in scope.markerList"
                  :key="marker.index"
                  :class="[`text-deep-orange-${2 + Math.ceil(marker.value / 2)}`, marker.classes]"
                  :style="marker.style"
                  @click="model = marker.value"
                >{{ marker.value }}</div>
              </template>
            </q-range>
          </q-item-section>
        </q-item>
      </q-list>

      <br>

      <q-list class="with-border">
        <div>
          <q-input
            v-model="form.habilidad"
            @keyup.enter="agregarElemento"
            label="Habilidades"
          />
          <q-list>
            <q-item v-for="(elemento, index) in form.habilidades" :key="index">
              <q-item-section>
                <q-item-label>{{ elemento }}</q-item-label>
              </q-item-section>
              <q-item-section side>
                <q-btn @click="eliminarElemento(index)" icon="delete" />
              </q-item-section>
            </q-item>
          </q-list>
        </div>
      </q-list>

      <br>

      <q-list class="with-border">
        <div>
          <q-input
            v-model="form.manejoDeEquipo"
            @keyup.enter="agregarManejo"
            label="Manejo de equipo"
          />
          <q-list>
            <q-item v-for="(elemento, index) in form.opcionesManejoDeEquipo" :key="index">
              <q-item-section>
                <q-item-label>{{ elemento }}</q-item-label>
              </q-item-section>
              <q-item-section side>
                <q-btn @click="eliminarManejo(index)" icon="delete" />
              </q-item-section>
            </q-item>
          </q-list>
        </div>
      </q-list>

      <br>

      <q-list class="with-border">
        <q-item>
          <q-item-section>
            <q-select
              transition-show="jump-up"
              transition-hide="jump-up"
              borderless
              multiple
              v-model="form.idioma"
              :options="idiomas"
              label="Que idioma(s) se requiere para el puesto"
            />
          </q-item-section>
        </q-item>
      </q-list>

      <br>

      <q-list class="with-border">
        <q-item>
          <q-item-section><strong>Sueldo Mensual inicial</strong></q-item-section>
          <q-item-section>
            <q-input
              class="with-border"
              v-model.number="form.sueldoMensualInicial"
              type="number"
              filled
              style="max-width"
            />
          </q-item-section>
          <q-item-section>
            <q-toggle
              v-model="form.comision"
              color="green"
              label="Comision"
            />
          </q-item-section>
        </q-item>
      </q-list>
      </div>

      <br>

      <div class="q-pa-md with-border" style="max-width">
        <h5>Desarrollo de competencias</h5>

        <q-list class="with-border">
        <q-item>
          <q-item-section>
            <q-select
              transition-show="jump-up"
              transition-hide="jump-up"
              borderless
              multiple
              v-model="form.competencia"
              :options="competencias"
              label="Seleccione las competencias"
            />
          </q-item-section>
        </q-item>
      </q-list>

      <br>

      <q-list class="with-border">
        <div>
          <q-input
            v-model="form.experiencia"
            @keyup.enter="agregarExperiencia"
            label="Experiencias y/o conocimientos"
          />
          <q-list>
            <q-item v-for="(elemento, index) in form.experiencias" :key="index">
              <q-item-section>
                <q-item-label>{{ elemento }}</q-item-label>
              </q-item-section>
              <q-item-section side>
                <q-btn @click="eliminarExperiencia(index)" icon="delete" />
              </q-item-section>
            </q-item>
          </q-list>
        </div>
      </q-list>

      <br>

      <q-list class="with-border">
        <div>
          <q-input
            v-model="form.actividad"
            @keyup.enter="agregarActividad"
            label="Actividades basicas a desempeñar"
          />
          <q-list>
            <q-item v-for="(elemento, index) in form.actividades" :key="index">
              <q-item-section>
                <q-item-label>{{ elemento }}</q-item-label>
              </q-item-section>
              <q-item-section side>
                <q-btn @click="eliminarActividad(index)" icon="delete" />
              </q-item-section>
            </q-item>
          </q-list>
        </div>
      </q-list>

      </div>

      <br>

      <div class="q-pa-md with-border" style="max-width">
        <h5>Equipo a proporcionar</h5>
        <q-list class="with-border">
        <q-item>
          <q-item-section>
            <q-select
              transition-show="jump-up"
              transition-hide="jump-up"
              borderless
              multiple
              v-model="form.equipo"
              :options="equipos"
              label="Seleccione los equipos a proporcionar"
            />
          </q-item-section>
        </q-item>
      </q-list>
      </div>

      <br>

      <div class="q-pa-md with-border" style="max-width">
      <h5>Fecha limite para cubrir la vacante</h5>
      <q-list class="with-border">
        <q-item>
          <q-item-section>
            <q-date
              v-model="form.date"
              minimal
            />
          </q-item-section>
        </q-item>
      </q-list>
      </div>

      <q-btn @click="submit" label="Enviar" color="primary" class="boton" />

  </div>
</template>

<script>
import { ref } from 'vue'
import { useQuasar } from 'quasar'
export default {
  data () {
    return {
      $q: useQuasar(),

      group: ref('op1'),
      mostrarCuadroDeTexto: false,
      form: {
        puesto: null,
        tiposDeVacante: null,
        sexo: null,
        motivo: null,
        escolaridad: null,
        idioma: null,
        sueldoMensualInicial: null,
        comision: ref(false),
        competencia: null,
        equipo: null,
        date: null,
        habilidades: [],
        opcionesManejoDeEquipo: [],
        experiencias: [],
        actividades: []

      },
      errors: {
        puesto: null,
        tiposDeVacante: null,
        sexo: null,
        motivo: null,
        escolaridad: null,
        idioma: null,
        sueldoMensualInicial: null,
        comision: ref(false),
        competencia: null,
        equipo: null,
        date: null,
        habilidades: [],
        opcionesManejoDeEquipo: [],
        experiencias: [],
        actividades: []
      },
      puestos: [
        { label: 'Vendedor', value: 'Vendedor' },
        { label: 'Mecanico', value: 'Mecanico' },
        { label: 'gerente', value: 'gerente' }
      ],
      tiposDeVacantes: [
        { label: 'Remplazo', value: 'remplazo', motivo: '', aQuien: '' },
        { label: 'Nueva Creacion', value: 'nuevaCreacion', motivo: '' },
        { label: 'Temporal', value: 'temporal', especifique: '' },
        { label: 'Permanente', value: 'permanente' }
      ],
      sexos: [
        { label: 'Masculino', value: 'masculino' },
        { label: 'Femenino', value: 'femenino' }
      ],
      escolaridades: [
        { label: 'Licenciatura', value: 'licenciatura' },
        { label: 'Carerra trunca', value: 'carreraTrunca' },
        { label: 'Preparatoria', value: 'preparatoria' }
      ],
      idiomas: [
        { label: 'Español', value: 'Español' },
        { label: 'Ingles', value: 'Ingles' },
        { label: 'Japones', value: 'Japones' }
      ],
      competencias: [
        { label: 'Adaptacion al cambio', value: 'adaptacionAlCambio' },
        { label: 'Autoconfianza', value: 'autoconfianza' },
        { label: 'Autodominio', value: 'autodominio' },
        { label: 'Capacidad de planeacion estrategica y oranizacional', value: 'capacidadDePlaneacionEstrategicaYOrganizacional' },
        { label: 'Compromizo e involucramiento con la marca', value: 'compromizoEInvolucramientoConLaMarca' },
        { label: 'Conocimiento ORG', value: 'onocimientoORG' },
        { label: 'Control y seguimiento', value: 'controlYSeguimiento' },
        { label: 'Desarrollo de relaciones interpersonales', value: 'desarrolloDeRelacionesInterpersonales' },
        { label: 'Iniciativa', value: 'iniciativa' },
        { label: 'Liderazgo', value: 'liderazgo' },
        { label: 'Orientacion a resultados', value: 'orientacionAResultados' },
        { label: 'Orientacion a la calidad', value: 'orientacionALaCalidad' },
        { label: 'Responsabilidad', value: 'responsabilidad' },
        { label: 'Orientacion al cliente y actitud de servicio', value: 'orientacionAlClienteYActitudDeServicio' }
      ],
      equipos: [
        { label: 'Zapato', value: 'zapato' },
        { label: 'Playera', value: 'playera' },
        { label: 'Pantalon', value: 'pantalon' },
        { label: 'Faja', value: 'faja' },
        { label: 'Lentes', value: 'lentes' },
        { label: 'Mandil', value: 'mandil' },
        { label: 'Guantes', value: 'guantes' },
        { label: 'Overol', value: 'overol' },
        { label: 'Vehiculo', value: 'vehiculo' },
        { label: 'Celular', value: 'celular' },
        { label: 'Alquiler de vivienda', value: 'alquilerDeVivienda' },
        { label: 'Computadora', value: 'computadora' },
        { label: 'Impresora', value: 'impresora' },
        { label: 'Herramienta', value: 'herramienta' },
        { label: 'Papeleria', value: 'papeleria' }
      ],
      edades: ref({
        min: null, max: null
      })

    }
  },
  methods: {

    agregarElemento () {
      if (this.form.habilidad.trim() !== '') {
        this.form.habilidades.push(this.form.habilidad)
        this.form.habilidad = '' // Limpiar el campo después de agregar
      }
    },
    eliminarElemento (index) {
      this.form.habilidades.splice(index, 1)
    },

    agregarManejo () {
      if (this.form.manejoDeEquipo.trim() !== '') {
        this.form.opcionesManejoDeEquipo.push(this.form.manejoDeEquipo)
        this.form.manejoDeEquipo = '' // Limpiar el campo después de agregar
      }
    },
    eliminarManejo (index) {
      this.form.opcionesManejoDeEquipo.splice(index, 1)
    },

    agregarExperiencia () {
      if (this.form.experiencia.trim() !== '') {
        this.form.experiencias.push(this.form.experiencia)
        this.form.experiencia = '' // Limpiar el campo después de agregar
      }
    },
    eliminarExperiencia (index) {
      this.form.experiencias.splice(index, 1)
    },

    agregarActividad () {
      if (this.form.actividad.trim() !== '') {
        this.form.actividades.push(this.form.actividad)
        this.form.actividad = '' // Limpiar el campo después de agregar
      }
    },
    eliminarActividad (index) {
      this.form.actividades.splice(index, 1)
    },

    submit () {
      const jsonData = JSON.stringify(this.form)
      console.log(jsonData)
      console.log(this.form)
      this.$router.push({ name: 'VisualizarDatos', query: { data: encodeURIComponent(jsonData) } })
    }

  },

  computed: {
    showMotivoInput () {
      const selectedType = this.form.tiposDeVacante
      return selectedType === 'nuevaCreacion' || selectedType === 'remplazo'
    },
    showEspecifiqueInput () {
      const selectedType = this.form.tiposDeVacante
      return selectedType === 'temporal'
    },
    showAQuienInput () {
      const selectedType = this.form.tiposDeVacante
      return selectedType === 'remplazo'
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
