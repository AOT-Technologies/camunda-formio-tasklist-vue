<template>
  <b-container fluid class="cftf-form-conatiner">
    <h3 class="cftf-taskhead">Form</h3>
    <div class="overflow-auto">

      <!-- <b-table :items="formList" :fields="fields"
       head-variant="light" :bordered=true :outlined=true
       :current-page="currentPage" :per-page="perPage">
      </b-table> -->

      <b-table-simple hover small caption-top responsive :bordered=true :outlined=true
      :per-page="perPage" class="cftf-table-align">
        <b-thead class="table-secondary">
          <b-tr>
            <b-th>Form</b-th>
            <b-th>Operations</b-th>
          </b-tr>
        </b-thead>
        <b-tbody>
          <b-tr v-for="form in formList" :key="form.formId">
            <b-th> {{form.formName}}</b-th>
            <b-th><b-button variant="primary"><i class="bi bi-pencil"></i> Submit New</b-button> </b-th>
          </b-tr>
        </b-tbody>
      </b-table-simple>


{{numPages}}
      <b-pagination-nav
          :number-of-pages="numPages"
          v-model="currentPage"
        />

      <p> Current page is: {{currentPage}}</p>
    </div>
  </b-container>
</template>

<script lang="ts">
import 'bootstrap/dist/css/bootstrap.min.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
import '../camundaFormIOTasklist.scss'
import 'bootstrap-icons/font/bootstrap-icons.css';
import { Component, Vue } from 'vue-property-decorator'
import { BootstrapVue } from 'bootstrap-vue'
import CamundaRest from '../services/camunda-rest'
import { Form } from 'vue-formio';

Vue.use(BootstrapVue)

@Component({
  components: {
    Form,
  }
})
export default class FormList extends Vue{
  private formList: Array<object> = []
  private actualFormNames = []
  private fields = [
    "formName",
    "Operations"
  ]
  private perPage = 5
  private currentPage = 1
  private numPages = 0

  linkGen() {
    this.formListItems();
  }

  formListItems() {
    const token = localStorage.getItem('authToken')
    const bpmUrl = localStorage.getItem('bpmApiUrl')
    CamundaRest.listForms(token, bpmUrl).then((response) =>
    {

      this.formList = response.data.splice(
        (this.currentPage - 1) * this.perPage,
        this.currentPage * this.perPage
      );
      console.log(response.data.length)
      this.numPages = Math.ceil(response.data.length/this.perPage)
    });
  }

  mounted() {
    this.formListItems(); 
  }
}
</script>