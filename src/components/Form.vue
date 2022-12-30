<template>
    <div class="border-2 py-10 px-20 h-fit" :class="{'blur' : okMsg}">
      <img class="m-auto w-80" src="../assets/img/VIASOL_LOGO.png" alt="Logo missing">
      <h2 class="text-xl font-semibold">Velkommen til Viasol support</h2>

      <p>For at oprette en support henvendelse, venligst udfyld telefonnummer og find dig selv på listen:</p>
      <label for="phone">Telefonnummer:</label>
      <input :disabled="okMsg" v-model="input" @input="findCustomer(this.input), resetSearch(this.input)" class="border ml-2" type="text" name="phone" id="phone">
      <br>
      <br>
      <select :disabled="okMsg" @change="findDeals(custID)" v-if="input" v-model="custID" class="h-10 border">
        <option v-for="contact in contacts" :key="contact.id" :value="contact.id">{{ contact.properties.firstname }} {{ contact.properties.lastname }}</option>
      </select>
      <br>
      <p v-if="custID && input">Dit kundenr er : {{ custID }}</p>
      <br>
      <p v-if="custID && input">Vælg hvilken sag der drejer sig om:</p>
      <select :disabled="okMsg" v-if="custID && input" v-model="dealID" class="h-10 border">
        <option v-for="deal,index in deals" :key="index" :value="deal.id">Sagsnr : {{ deal.id }}</option>
      </select>

      <div v-if="dealID && input">
        <label for="issue">Vælg hvad du oplever problemer med:</label>
        <select :disabled="okMsg" v-model="ticketName" id="issue" class="mx-5 border">
          <option>Solcelleanlæg</option>
          <option>Inverter</option>
          <option>Batteribank</option>
          <option>Andet</option>
        </select>
        <br><br>
        <label for="serial" class="text-xl font-semibold">Serienummer Inverter/Batteri:</label>
        <br>
        <input :disabled="okMsg" v-model="serialNumber" id="serial" type="text" class="border">
        <br><br>
        <label for="description" class="text-xl font-semibold">Beskriv dit problem:</label>
        <br>
        <textarea :disabled="okMsg" v-model="description" id="description" cols="60" rows="10" placeholder="skriv her" class="border"></textarea>
        <br><br>
        <label for="date" class="text-xl font-semibold">Hvornår er problemt opstået?:</label>
        <br>
        <input :disabled="okMsg" v-model="date" id="date" type="date" class="border">
        <br><br>
        <button :disabled="okMsg" @click="sendTicketInfo" class="text-lg font-semibold border p-2 " :class="{'hover:bg-slate-200' : !okMsg}">Opret Servicesag</button>
      </div>      
    </div>

    <div v-if="okMsg" class="absolute top-1/3 bg-slate-50 border p-20 z-20">
      <h2 class="text-3xl">Tak for din henvendelse</h2>
      <p>Din henvendelse vil nu blive behandlet og vi vil vende tilbage til hurtigst muligt</p>
      <br>
      <a  href="/"><button class="border px-5 hover:bg-slate-200">OK</button></a>
    </div>
</template>

<script>
export default {
    data() {
    return {
      input : "",
      contacts : [],
      deals : [],
      custID : "",
      dealID : "",

      ticketName : "",
      serialNumber : "",
      description : "",
      date : "",

      okMsg : false
    }
  },
  methods : {
    resetSearch(input){
      if(input === ""){
        this.custID = ""
        this.dealID = ""
        this.contacts = []
        this.deals = []
      }
    },
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
    },
    async sendTicketInfo(){
      const response = await fetch(`http://localhost:5000/ticket`, {
        method : "POST",
        headers : {
          "Content-Type" : "application/json"
        },
        body : JSON.stringify({
          dealId : this.dealID,
          name : this.ticketName,
          serialNumber : this.serialNumber,
          description : this.description,
          date : this.date
        })
      })
      if (response) {
        this.okMsg = true;
      }
    }
  }
}
</script>