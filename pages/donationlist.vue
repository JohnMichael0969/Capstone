<template>
  <v-container>
    <h2>Donation List</h2>
    <v-data-table :items="Donation" :headers="headers" class="elevation-1">
      <template v-slot:item.Title="{ item }">
        {{ item.Title}}
      </template>
      <template v-slot:item.Description="{ item }">
        {{ item.Description }}
      </template>


      <template v-slot:item.Location="{ item }">
        {{ item.Location }}
      </template>
      <template v-slot:item.FoodType="{ item }">
        {{ item.FoodType }}
      </template>
      <template v-slot:item.FoodExpiry="{ item }">
        {{ item.FoodExpiry }}
      </template>
      
      <template v-slot:item.DeliverySchedule="{ item }">
        {{ item.DeliverySchedule }}
      </template>
      <template v-slot:item.TransportMethod="{ item }">
        {{ item.TransportMethod }}
      </template>
      <!--<template v-slot:item.DeliverySchedule="{ item }">
        {{ item.DeliverySchedule }}
      </template>-->
      <!--<template v-slot:item.Edit="{ item }">
            <v-btn color="primary" @click="navigateToDelivery">Set Delivery</v-btn>
      </template> -->
        

    </v-data-table>
       
  </v-container>
  
</template>

<script>
import firebase from '~/plugins/firebase'

export default {
  data () {
    return {
      Donation: [],
      headers: [
        { text: 'Title', value: 'Title' },
        { text: 'Description', value: 'Description' },      
        { text: 'Food Type', value: 'FoodType' },
        { text: 'Food Expiry', value: 'FoodExpiry' },
        { text: 'Delivery Schedule', value: 'PickUPDate' },
        { text: 'Transport Method', value: 'TransportMethod' },
        //{ text: 'Delivery Schedule', value: 'DeliverySchedule' },
        { text: '', value: 'Edit' },
        
      ]
    }
  },
  created () {
    firebase.database().ref('Donation').on('value', snapshot => {
      this.Donation = Object.values(snapshot.val())
    })
  },
   methods: {
    navigateToDelivery() {
      this.$nuxt.$router.push('/delivery')
    }
  }
}
</script>
