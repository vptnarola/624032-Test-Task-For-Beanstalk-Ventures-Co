<template>
  <section class="list-section">
    <div class="container">
      <div class="card-wrapper">
        <q-card class="my-card" flat bordered v-for="data in info" :key="data.id">
          <q-img
            src="https://cdn.quasar.dev/img/parallax2.jpg"
          />
          <q-card-section>
            <div class="text-overline text-orange-9">{{data.address.city}}, {{data.address.country}}</div>
            <div class="text-h5 q-mt-sm q-mb-xs">{{data.title}}</div>
            <div class="text-grey" style="display: flex;">
              <div>{{data.detail.bedrooms}} bedrooms</div> &nbsp;<b>•</b>&nbsp;
              <div>{{data.detail.beds}} beds</div> &nbsp;<b>•</b>&nbsp;
              <div>{{data.detail.bathrooms}} bathrooms</div>
            </div>
            <p>{{data.description}}</p>
          </q-card-section>

          <q-card-actions>
            <q-btn color="primary" text-color="white" label="Book" v-show="isLogin" @click="modelShow(data.id)"/>
          </q-card-actions>
        </q-card>
    </div>
    </div>
  </section>
  <q-dialog v-model="alert">
      <q-card>
        <q-card-section>
          <div class="text-h6">Book</div>
        </q-card-section>

      <q-card-section>
        <q-form @submit.prevent="booking()" class="q-gutter-md">
          <div class="q-pa-md">
            <q-date v-model="bookDate" range :options="optionsFn"/>
          </div>
          <q-input type="number" label="Number of Guests" v-model="noOfGuest"  :rules="[ val => val !== null && val !== '' || 'Please type valid Data', val => val > 0 && val < 100 || 'Please type valid Data' ]"/>
          <q-btn label="Submit" type="submit" color="primary"/>
        </q-form>
      </q-card-section>

        <q-card-actions align="right">
        </q-card-actions>
      </q-card>
    </q-dialog>
</template>
<script>

import { defineComponent, ref } from 'vue'
import { api } from 'boot/axios'

export default defineComponent({
  name: 'IndexPage',
  data () {
    return {
      info: null,
      isLogin: false,
      noOfGuest: 0
    }
  },
  mounted(){
    api.get('/property')
      .then((response) => {
        this.info = response.data
      })
      this.isLogin = localStorage.getItem('isLogin')
  },
  setup() {
    const currentDate = () => {
      var today = new Date();
      var dd = String(today.getDate() + 1).padStart(2, '0');
      var mm = String(today.getMonth() + 1).padStart(2, '0');
      var yyyy = today.getFullYear();

      return yyyy + '/' + mm + '/' + dd;
    };
    const nextdate = () => {
      var today = new Date();
      var dd = String(today.getDate() + 6).padStart(2, '0');
      var mm = String(today.getMonth() + 1).padStart(2, '0');
      var yyyy = today.getFullYear();

      return yyyy + '/' + mm + '/' + dd;
    };
    return {
      alert: ref(false),
      propertyId: ref(null),
      bookDate: ref({ from: currentDate(), to: nextdate() }),
      optionsFn (date) {
        return date >= currentDate()
      }
    }
  },
  methods: {
    booking: function () {
      const userId = parseInt(localStorage.getItem('userId'))
      const bookingData = {
        checkInDate: this.bookDate.from,
        checkOutDate: this.bookDate.to,
        propertyId: this.propertyId,
        userId: userId
      }
      api.post('/booking', bookingData).catch(e => console.log(e))
      this.alert = false
      this.noOfGuest = 0
    },
    modelShow: function (id) {
      this.alert = true
      this.propertyId = id
    }
  }

})
</script>

<style scoped>
.card-wrapper{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
}
.searchproperties-list-inner{
  margin-right: 30px
}
.my-card{
  width: 100%;
}
@media only screen and (max-width: 768px){
  .card-wrapper{
    grid-template-columns: 1fr 1fr;
    grid-column-gap: 20px;
    grid-row-gap: 20px;
  }
}
@media only screen and (max-width: 570px){
  .card-wrapper{
    grid-template-columns: 1fr;
    grid-column-gap: 15px;
    grid-row-gap: 15px;
    max-width: 75%;
    margin: auto;
  }
}
</style>
<style>
.q-dialog__inner.flex.no-pointer-events.q-dialog__inner--minimized.q-dialog__inner--right.fixed-right.items-center{
  padding: 0px;
}
.q-card.full-height{
  max-height: none
}
</style>
