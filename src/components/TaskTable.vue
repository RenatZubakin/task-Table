<template>
  <div>

    <button @click="showModal=true">
      добавить
    </button>
    <div v-bind:class="showModal ? 'modalContainer' : 'hideModal'">
      <form class="form">
        <button type='button' class="closeModal" @click="showModal=false">x</button>
        <h4>Добавить компанию</h4>
        <label>Название
        <input  class='modalInput' type="text" v-model="modalData.name"/>
        </label>
        <label>
          Адрес
        <input placeholder="город ... улица .... дом ..." class='modalInput' type="text" v-model="modalData.address"/>
        </label>
        <label>
          Огрн
        <input  placeholder="введите 13 символов" class='modalInput' type="number" v-model="modalData.ogrn"/>
        </label>
          <label>ИНН
        <input placeholder="введите 10 символов" class='modalInput' type="text" v-model="modalData.inn"/>
          </label>
          <label>
            Дата
        <input  class='modalInput' type="date" v-model="modalData.date"/>
          </label>
        <button  type="button" @click="addCompany"> Добавить</button>
      </form>
    </div>
    <div v-bind:class="innModal ? 'modalContainer' : 'hideModal'">
      <form class="form">
        <button type='button' class="closeModal" @click="innModal=false">x</button>
        <h4>Добавить компанию по инн:</h4>
        <input  class='modalInput' type="text" v-model="innModalValue"/>
        <button  type="button" @click="addWithInn"> Добавить</button>
      </form>
    </div>
    <table>
      <tr>
        <th>Наименование</th>
        <th>Адрес</th>
        <th>Огрн</th>
        <th>
          ИНН
          <button @click="innModal=true">
            Загрузить
          </button>
        </th>
        <th>Дата регистрации</th>
      </tr>
      <TableRow @updateParent="deleteElement" :rows='rows'/>
    </table>
  </div>
</template>

<script>
import TableRow from "@/components/TableRow";


export default {
  components: {
    TableRow
  },
  data() {
    return {
      showModal: false,
      innModal:false,
      innModalValue:'',
      modalData:{
        name:'',
        address:'',
        ogrn:'',
        inn:'',
        date:'',
      },
      rows: []
    }
  },
  methods:{
    addCompany(){
      let newCompany={
        name:this.modalData.name,
        address:this.modalData.address,
        ogrn: this.modalData.ogrn,
        inn:this.modalData.inn,
        date:this.modalData.date
      }
      this.rows.push(newCompany)
      this.modalData={
        name:'',
        address:'',
        ogrn:'',
        inn:'',
        date:'',
      }
      this.showModal=false
    },
    deleteElement(row){

      this.rows.splice(this.rows.indexOf(row),1)
    },
    addWithInn(){
      let url = "https://suggestions.dadata.ru/suggestions/api/4_1/rs/findById/party";
      let token = "4c619fea8686e58ab6ec9dc74ddb1257420e8f8d";
      let query = this.innModalValue;

      let options = {
        method: "POST",
        mode: "cors",
        headers: {
          "Content-Type": "application/json",
          "Accept": "application/json",
          "Authorization": "Token " + token
        },
        body: JSON.stringify({query: query})
      }

      fetch(url, options)
          .then(response => response.json())
          .then(result => result.suggestions)
          .then(res=>res.forEach(elem=>{
            let date=new Date(elem.data.state.registration_date?? 'нет информации');
            let y = date.getFullYear()
            let m = date.getMonth() + 1;
            let d = date.getDate();
            m = (m < 10) ? '0' + m : m;
            d = (d < 10) ? '0' + d : d;
            this.rows.push({
              name:elem.value,
              address:elem.data.address.value,
              ogrn:elem.data.ogrn,
              inn:elem.data.inn,
              date:isNaN(m)?'Информации нет': [y, m, d].join('.'),
            })
          }))
          .catch(error => console.log("error", error));
    }
  }
}
</script>

<style>
.hideModal {
  display: none;
}
.modalContainer {
  height: 100vh;
  width: 100vw;
  background-color:rgba(0,0,0,0.4);;
  position: fixed;
  top:0;
  left:0;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.5s;
}

form{
  display: flex;
  align-items: center;
  flex-direction: column;
  background-color: #dddddd;
}

.modalInput{
  margin-bottom: 10px;
  width: 80%;
}

.closeModal{
  margin-left:auto;
  background: gray;
  text-decoration: none;
  border-radius: 30%;
  font-size: 20px;
  color: white;
}

table {
  width: 100%;
  margin-bottom: 20px;
  border: 1px solid #dddddd;
  border-collapse: collapse;
}

table th {
  font-weight: bold;
  padding: 5px;
  background: #efefef;
  border: 1px solid #dddddd;
}

table td {
  border: 1px solid #dddddd;
  padding: 5px;
}
</style>
