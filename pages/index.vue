<template>
  <div style="padding: 15%">
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group  label="Tu edad:">
        <b-form-input
          v-model="form.age"
          required
          type="number"
          placeholder="Entra edad"
        ></b-form-input>
      </b-form-group>
      <b-form-group  label="Tu dinero:">
        <b-form-input
          v-model="form.fnlwgt"
          required
              type="number"
          placeholder="Tu dinero"
        ></b-form-input>
      </b-form-group>
      <b-form-group  label="Tu educacion:">
        <b-form-input
          v-model="form.educationNum"
          required
              type="number"
          placeholder="Entra educacion (primaria:4, secundaria:7,poca universidad:9, universidad: 13, master:14, doc:15"
        ></b-form-input>
      </b-form-group>
      <b-form-checkbox
      id="checkbox-1"
      v-model="form.sex"
      name="checkbox-1"
      value="10"
      style="padding-bottom: 15px"
      unchecked-value="0"
    >
      Soy hombre
    </b-form-checkbox>
      <b-form-group  label="Tu ingreso mensual:">
        <b-form-input
          v-model="form.capitalGain"
          required
              type="number"
          placeholder="Entra ingreso por mes"
        ></b-form-input>
      </b-form-group>
      <b-form-group  label="Your perdida mensual:">
        <b-form-input
          v-model="form.capitalLoss"
          required
              type="number"
          placeholder="Entra perdida mensual"
        ></b-form-input>
      </b-form-group>
      <b-form-group  label="Horas que trabajas por semana">
        <b-form-input
          v-model="form.hours"
          required
              type="number"
          placeholder="Horas que trabajas por semana"
        ></b-form-input>
      </b-form-group>
      <b-form-checkbox
      id="checkbox-2"
      v-model="form.Ke"
      name="checkbox-2"
      value=">50K"
      unchecked-value="<=50K"
    >
      Gano mas de 50K al anio
    </b-form-checkbox>
    <div style="padding-bottom: 15px"></div>
      <b-button type="submit" variant="primary">Enviar</b-button>
      <b-button type="reset" variant="danger">Resetear</b-button>
    </b-form>

  </div>
</template>

<script>
import axios from 'axios';
import swal from 'sweetalert2/dist/sweetalert2.all.min.js'

  export default {
    data() {
      return {
        form: {
          age: '',
    fnlwgt: "",
    educationNum: "",
    sex: "0",
    capitalGain: "",
    capitalLoss: "",
    hours: "",
    Ke: "<=50K",
        },
        show: true
      }
    },
    methods: {
      onSubmit(evt) {
        evt.preventDefault()
        this.form.age = Number(this.form.age)
        this.form.fnlwgt = Number(this.form.fnlwgt)
        this.form.educationNum = Number(this.form.educationNum)
        this.form.sex = Number(this.form.sex)
        this.form.capitalGain = Number(this.form.capitalGain)
        this.form.capitalLoss = Number(this.form.capitalLoss)
        this.form.hours = Number(this.form.hours)
        let request = new Request("http://localhost:8000/knn", {
                method: 'POST',
                mode: 'cors',
                body: JSON.stringify(this.form),
            });
            fetch(request)
                .then(res => res.json())
                .then(prediction => {
                  // this.$swal(JSON.stringify(prediction));
                    this.$swal({
                        title: prediction.predicted? 'Predecido!': 'No Predecido',
                        text: 'Prediccion ' + prediction.knn,
                        type: 'success'
                    })
            });
        // fetch('http://localhost:8000/knn', this,form)
    //   axios
    //   .post('http://localhost:8000/knn',this.form,
    //   {headers: {
    //   'Access-Control-Allow-Origin': '*',
    //   'Content-Type': 'application/json',
    // }})
    //   .then(response => (alert(response)))
      },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.age= '',
        this.form.fnlwgt= "",
        this.form.educationNum= "",
        this.form.sex= "",
        this.form.capitalGain= "",
        this.form.capitalLoss= "",
        this.form.hours= "",
        this.form.Ke= "",
        
      
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
  }

</script>

<style>

</style>