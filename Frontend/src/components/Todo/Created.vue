<template>
  <div>
            

                    <b-form @submit="onSubmit"  v-if="show">
                    

                    <b-form-group id="input-group-2" label="Your Task:" label-for="input-2">
                        <b-form-input style="background-color:#040620; border:#040620;"
                        id="input-2"
                        v-model="form.name"
                        placeholder="Enter Task"
                        required
                        ></b-form-input>
                    </b-form-group>

                    <b-form-group id="input-group-3" label="Name" label-for="input-3">
                        <b-form-select style="background-color:#040620; border:#040620;"
                        id="input-3"
                        v-model="form.food"
                        :options="foods"
                        required
                        ></b-form-select>
                    </b-form-group>

                   

                    <b-button type="submit" variant="info">Submit</b-button>
                    </b-form>
                

  </div>
</template>

<script>
  export default {
    data() {
      return {
        form: {
          email: '',
          name: '',
          food: null,
          checked: []
        },
        foods: [{ text: 'Select One', value: null }, 'Omar El Ghiba'],
        show: true
      }
    },
    methods: {
      onSubmit(event) {
        event.preventDefault()
        alert(JSON.stringify(this.form))
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.food = null
        this.form.checked = []
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
    .form-control {
      background-color: #000;
 }
</style>