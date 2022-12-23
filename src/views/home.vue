<template>
  <div class="text-center flex justify-center">
    <div class="border-2 py-10 px-20 h-screen">
        <img class="m-auto w-80" src="../assets/img/VIASOL_LOGO.png" alt="Logo missing">
        <h2 class="text-xl font-semibold">Velkommen til Viasol support</h2>
        <p>For at oprette en support henvendelse, venligst udfyld telefonnummer og find dig selv på listen:</p>
            
            <label for="phone">Telefonnummer:</label>
            <input v-model="input" @input="findCustomer(this.input)" class="border ml-2" type="text" name="phone" id="phone">

            <br>
            <br>

            <select @change="findDeals(custID)" v-if="input" v-model="custID" class="h-10 border">
              <option v-for="contact in contacts" :key="contact.id" :value="contact.id">{{ contact.properties.firstname }} {{ contact.properties.lastname }}</option>
            </select>
            <br>
            <p v-if="custID">Dit kundenr er : {{ custID }}</p>
            <br>
            <p v-if="custID">Vælg hvilken sag der drejer sig om:</p>
            <select v-if="custID" class="h-10 border">
              <option v-for="deal,index in deals" :key="index">{{ deal.id }}</option>
            </select>
    </div>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      input : "",
      contacts : [],
      deals : [],
      custID : ""
    }
  },
  methods : {
     findCustomer(number){
      console.log("Number send");
       fetch(`http://localhost:5000/contacts/${number}`)
      .then(res => res.json())
      .then(data => this.contacts = data)
      .catch(err => console.log(err.message))
    },
    findDeals(contactId){
      console.log("contactId send");
      fetch(`http://localhost:5000/deals/${contactId}`)
      .then(res => res.json())
      .then(data => this.deals = data.results[0].to)
      .catch(err => console.log(err.message))
    }
  }
}
</script>

<style>

</style>