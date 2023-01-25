<template>
  <v-container>
    <h2>Donation List</h2>
    <v-data-table :items="Donation" :headers="headers" class="elevation-1">
      <template v-slot:item.ID="{ item }">
        {{ item.ID }}
      </template>
      <template v-slot:item.Title="{ item }">
        {{ item.Title }}
      </template>
      <template v-slot:item.description="{ item }">
        {{ item.description }}
      </template>
      <template v-slot:item.location="{ item }">
        {{ item.location }}
      </template> 
      <template v-slot:item.PickUpDate="{ item }">
        {{ item.PickUpDate }}
      </template>
      <template v-slot:item.transportMethod="{ item }">
        {{ item.transportMethod }}
      </template>
      <template v-slot:item.deliverySchedule="{ item }">
        {{ item.deliverySchedule }}
      </template>
      <template v-slot:item.actions="{ item }">
        <v-dialog v-model="showDialog" max-width="390">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark v-on="on">Create Schedule</v-btn>
          </template>
          <v-card>
            <v-card-title class="headline">Select Delivery Date</v-card-title>
            <v-card-text>
              <v-date-picker v-model="selectedDate"
                             :show-current="false" 
                             :no-title="true" 
                             :close-on-content-click="false"></v-date-picker>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" @click="saveDeliverySchedule(selectedId)">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </template>
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
        { text: 'ID', value: 'ID' },
        { text: 'Title', value: 'Title' },
        { text: 'Description', value: 'Description' }, 
        { text: 'Delivery Date', value: 'PickUPDate' },
        { text: 'TransportMethod', value: 'TransportMethod' },
        { text: 'Delivery Schedule', value: 'DeliverySchedule' },
        { text: '', value: 'actions' },
     ],
        showDialog: false,
        selectedId: null,
        selectedDate: null,
        }
      },
      created () {
        firebase.database().ref('Donation').on('value', snapshot => {
        this.Donation = Object.values(snapshot.val())
        })
      },
      methods: {
        methods: {
        saveDeliverySchedule(id) {
  firebase.database().ref(`Donation/${id}`).once('value', snapshot => {
    let donation = snapshot.val()
    if (donation) {
      let pickUpDate = new Date(donation.pickUpDate)
      let today = new Date()
      if (pickUpDate <= today) {
        console.log('Cannot set delivery schedule for an item with past pick up date.')
        return
      }
      if (this.selectedDate) {
        let deliveryDate = new Date(this.selectedDate)
        if (deliveryDate < pickUpDate) {
          console.log('Cannot set delivery schedule for an item earlier than pick up date.')
          return
        }
        let deliverySchedule = deliveryDate.toISOString()
        firebase.database().ref(`Donation/${id}`).update({ deliverySchedule: deliverySchedule })
        this.showDialog = false
      }
    }
  })
}

      }
    }


}
</script>