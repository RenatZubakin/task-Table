<template>
  <tr  v-for="row in rows" :key="row.name">
    <td>{{row.name}}</td>
    <td>
      <textarea class="inputAddress" v-model="row.address"/>
    </td>
    <td>{{row.ogrn}}</td>
    <td>{{row.inn}}</td>
    <td>
      {{row.date}}
      <button class="deleteBtn" type='button' @click="removeRow(row)">
        <img class="deleteBtnImage" src="../assets/kisspng-tidy-man-logo-clip-art-trash-5abca179b3f8c0.0991583615223115457372.png">
      </button>
    </td>
  </tr>
</template>

<script>

export default {
  props:{
    rows:{
      type:Array,
      require:true
    }
  },
  methods:{
    removeRow(row){
      console.log(row)
      this.$emit('updateParent',row);
    },


  }
}

</script>

<style>
th {
  font-weight: bold;
  padding: 5px;
  background: #efefef;
  border: 1px solid #dddddd;
}

 td {
  border: 1px solid #dddddd;
  padding: 5px;
   height: 50px;
}

 .inputAddress{
   border:none;

 }
.deleteBtn{
  font-size:5px ;
  margin-bottom: auto;
  margin-left:auto;
  width:15px;
  height: 15px;
  background-color: white;
  border:none;
}
.deleteBtnImage{
  width: 20px;
  height: 20px;

}

</style>