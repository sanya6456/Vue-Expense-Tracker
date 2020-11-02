<template>
  <div class="container">
    <!--  input section -->
    <h1 class="add-new-item text-success text-center my-3">Add a new item:</h1>
    <div class="name-section d-flex justify-content-center align-items-center">
      <strong class="name pr-2">Name:</strong>
      <input type="text" placeholder="Name ..." class="input-name form-control w-75 p-1" id="inputNameValue" v-model="state.name">
    </div>
    <div class="row my-4">
      <div class="col-6 d-flex justify-content-center align-items-center">
        <strong class="date-name pr-2">Date:</strong>
        <input class="form-control" type="date" placeholder="dd/mm/yyyy" id="inputDateValue" v-model="state.date">
      </div>
      <div class="col-6 d-flex justify-content-center align-items-center">
        <strong class="amount-name pr-2">Amount:</strong>
        <input class="form-control" type="number" placeholder="Amount ..." id="inputAmountValue" v-model="state.amount">
      </div>
    </div>
    <!--  Add button -->
    <button class="btn btn-success btn-lg d-block mx-auto mt-5 px-4" @click="addRecord" id="addRecord">Add record</button>
    <!-- Results section -->
    <table class="table text-center mt-5">
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Date
        <svg @click="dateSorting" id="date-sorting" width="1.1em" height="1.1em" viewBox="0 0 16 16" class="bi bi-arrow-down-up ml-1" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd" d="M11.5 15a.5.5 0 0 0 .5-.5V2.707l3.146 3.147a.5.5 0 0 0 .708-.708l-4-4a.5.5 0 0 0-.708 0l-4 4a.5.5 0 1 0 .708.708L11 2.707V14.5a.5.5 0 0 0 .5.5zm-7-14a.5.5 0 0 1 .5.5v11.793l3.146-3.147a.5.5 0 0 1 .708.708l-4 4a.5.5 0 0 1-.708 0l-4-4a.5.5 0 0 1 .708-.708L4 13.293V1.5a.5.5 0 0 1 .5-.5z"/>
        </svg>
      </th>
      <th scope="col">Amount
        <select name="amount" id="amount" class="ml-1" v-model="state.currency">
          <option value="&dollar;">USD</option>
          <option value="&pound;">GBP</option>
          <option value="&euro;">EUR</option>
          <option value="&yen;">YEN</option>
          <option selected value="HUF">HUF</option>
        </select>
        <svg @click="moneySorting" id="money-sorting" width="1.1em" height="1.1em" viewBox="0 0 16 16" class="bi bi-arrow-down-up ml-2" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd" d="M11.5 15a.5.5 0 0 0 .5-.5V2.707l3.146 3.147a.5.5 0 0 0 .708-.708l-4-4a.5.5 0 0 0-.708 0l-4 4a.5.5 0 1 0 .708.708L11 2.707V14.5a.5.5 0 0 0 .5.5zm-7-14a.5.5 0 0 1 .5.5v11.793l3.146-3.147a.5.5 0 0 1 .708.708l-4 4a.5.5 0 0 1-.708 0l-4-4a.5.5 0 0 1 .708-.708L4 13.293V1.5a.5.5 0 0 1 .5-.5z"/>
        </svg>
      </th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(result,index) in state" :key="index">
      <td id="name" contenteditable="true">{{result.name}}</td>
      <td id="date" contenteditable="true">{{result.date}}</td>
      <td id="amount" contenteditable="true">{{formatter.format(result.amount)}} {{result.currency}}</td>
      <td id="trash">
        <!-- EDIT -->
        <!--<svg @click="editItem(index)" width="1.65em" height="1.65em" viewBox="0 0 16 16" class="bi bi-tools text-primary" style="cursor:pointer;" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd" d="M0 1l1-1 3.081 2.2a1 1 0 0 1 .419.815v.07a1 1 0 0 0 .293.708L10.5 9.5l.914-.305a1 1 0 0 1 1.023.242l3.356 3.356a1 1 0 0 1 0 1.414l-1.586 1.586a1 1 0 0 1-1.414 0l-3.356-3.356a1 1 0 0 1-.242-1.023L9.5 10.5 3.793 4.793a1 1 0 0 0-.707-.293h-.071a1 1 0 0 1-.814-.419L0 1zm11.354 9.646a.5.5 0 0 0-.708.708l3 3a.5.5 0 0 0 .708-.708l-3-3z"/>
          <path fill-rule="evenodd" d="M15.898 2.223a3.003 3.003 0 0 1-3.679 3.674L5.878 12.15a3 3 0 1 1-2.027-2.027l6.252-6.341A3 3 0 0 1 13.778.1l-2.142 2.142L12 4l1.757.364 2.141-2.141zm-13.37 9.019L3.001 11l.471.242.529.026.287.445.445.287.026.529L5 13l-.242.471-.026.529-.445.287-.287.445-.529.026L3 15l-.471-.242L2 14.732l-.287-.445L1.268 14l-.026-.529L1 13l.242-.471.026-.529.445-.287.287-.445.529-.026z"/>
        </svg>-->
        <!-- TRASH -->
        <svg @click="deleteItem(index)" width="1.8em" height="1.8em" viewBox="0 0 16 16" class="bi bi-trash text-danger ml-3" style="cursor:pointer;" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
          <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
          <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4L4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
        </svg>
      </td>
    </tr>
    <tr class="last-border">
      <td></td>
      <td></td>
      <td class="sum">
        {{formatter.format(sum.money)}} {{state.currency}}
      </td>
      <td></td>
    </tr>
  </tbody>
</table>

  </div>
</template>


<script>
import {onUpdated, reactive, onMounted} from 'vue';
export default {
  name: 'ExpenseTracker',
  setup(){
    //inputs value will be push here
    const state=reactive([])
    //sum up money
    const sum=reactive({
      money:''
    });

    //onClick button "add record" push inputs to state (reactive) array AND sum amount
    function addRecord(){
      state.push({name:state.name,date:state.date,amount:state.amount, currency: state.currency})
      state.name=''
      state.date=''
      state.amount=''
      state.currency=''
    }

    onUpdated(()=>{
      //on update inputs, watch if statements
      var addRecord=document.getElementById('addRecord');
      if(state.name==="" || state.date==="" || state.amount===""){
        addRecord.disabled=true;
      } else{
        addRecord.disabled=false;
      }
      //if currency is empty it's => select
      if(state.currency===""){
        state.currency="HUF"
      }
      //if date === ''; => we got current date!
      if(state.date===""){
        var today=new Date();
        var yyyy=today.getFullYear();
        var mm=today.getMonth()+1;
        var dd=today.getDate();
        state.date=`${yyyy}-${mm}-${dd}`;
      }
      //sum up amount
      var sumUp=state.reduce((a,b)=>{return a+parseInt(b.amount)},0)
      sum.money=sumUp
    })

    //sort by date
    var sortClicks=0;
    function dateSorting(){
      sortClicks++;
      var dateSorting=document.getElementById('date-sorting');
      if(sortClicks%2==0){
        state.sort((a,b)=>{return new Date(a.date).getTime() - new Date(b.date).getTime()});
        dateSorting.classList.remove('text-success');
        dateSorting.classList.add('text-danger');
      } else{
        state.sort((a,b)=>{return new Date(b.date).getTime() - new Date(a.date).getTime()});
        dateSorting.classList.remove('text-danger');
        dateSorting.classList.add('text-success');
      }

    }

    //sort by amount
    var sortMoneyClicks=0;
    function moneySorting(){
      sortMoneyClicks++;
      var moneySorting=document.getElementById('money-sorting');
      if(sortMoneyClicks%2==0){
        state.sort((a,b)=>{return a.amount-b.amount});
        moneySorting.classList.remove('text-success');
        moneySorting.classList.add('text-danger');
      } else{
        state.sort((a,b)=>{return b.amount-a.amount});
        moneySorting.classList.remove('text-danger');
        moneySorting.classList.add('text-success');
      }
    }
    //onMounted disable "add record" button
    onMounted(()=>{
      var addRecord=document.getElementById('addRecord');
      addRecord.disabled=true;
      state.name=''
      state.date=''
      state.amount=''
      state.currency='HUF'
    })

    //delete item
    function deleteItem(index){
      this.state.splice(index, 1);
    }

    //format amount
    const formatter = new Intl.NumberFormat('hu-HU', {
      currency: 'HUF',
    })


    return{
      state,
      addRecord,
      deleteItem,
      formatter,
      sum,
      dateSorting,
      moneySorting
    }
  }
}
</script>


<style scoped lang="scss">

</style>
