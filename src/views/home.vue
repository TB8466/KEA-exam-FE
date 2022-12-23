<template>
  <div class="text-center flex justify-center">
    <div class="border-2 py-10 px-20 h-screen">
        <img class="m-auto w-80" src="../assets/img/VIASOL_LOGO.png" alt="Logo missing">
        <h2 class="text-xl font-semibold">Velkommen til Viasol support</h2>
        <p>For at oprette en support henvendelse, venligst udfyld telefonnummer og find dig selv på listen:</p>
            
            <label for="phone">Telefonnummer:</label>
            <input v-model="input" @input="sendPhoneNumber(this.input)" class="border ml-2" type="text" name="phone" id="phone">

            <br>
            <br>

            <select v-if="input" v-model="custID" class="h-10 border">
              <option v-for="contact in contacts" :key="contact.id" :value="contact.id">{{ contact.properties.firstname }} {{ contact.properties.lastname }}</option>
            </select>
            <br>
            <p v-if="custID">CustomerID : {{ custID }}</p>
    </div>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      input : "",
      contacts: [],
      custID : ""
    }
  },
  methods : {
     sendPhoneNumber(number){
      console.log("Number send");
       fetch(`http://localhost:5000/contact/${number}`)
      .then(res => res.json())
      .then(data => this.contacts = data)
      .catch(err => console.log(err.message))
    }
  }
}
</script>

<style>

</style>