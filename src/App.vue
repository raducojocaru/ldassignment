<template>
  <div id="app">
    <OfferUpdates v-bind:activities="activities"/>
  </div>
</template>

<script>
import OfferUpdates from './components/OfferUpdates'

export default {
  name: 'app',
  components: {
    OfferUpdates
  },
  data() {
    return {
      // just for demo purposes, in a real world scenario the component calling OfferUpdates would have to provide the list of offerUpdates (or we can fetch them inside OfferUpdates)
      "activities": [
        {
          "uuid": "ad4f6edf",
          "activity_type": "contact",
          "activity_date": "20180910",
          "amount": null,
          "description": "Spoke to customer ­ running a product comparison",
          "created_at": "2018­09­10T14:51:06.099+10:00",
          "updated_at": "2018­09­10T14:51:06.099+10:00"
        },
        {
          "uuid": "782c41e5",
          "activity_type": "application",
          "activity_date": "20181001",
          "amount": "460000.0",
          "description": "Application lodged",
          "created_at": "2018­10­04T16:49:24.871+10:00",
          "updated_at": "2018­10­04T16:49:24.871+10:00"
        },
        {
          "uuid": "8a75fcca",
          "activity_type": "accept",
          "activity_date": "20180905",
          "amount": null,
          "description": "Bid accepted",
          "created_at": "2018­09­05T19:22:10.257+10:00",
          "updated_at": "2018­09­05T19:22:10.257+10:00"
        },
        {
          "uuid": "4cd22945",
          "activity_type": "note",
          "activity_date": "20180924",
          "amount": null,
          "description": "Customer has purchased property, has sent the receipt",
          "created_at": "2018­09­24T11:14:10.794+10:00",
          "updated_at": "2018­09­24T11:14:10.794+10:00"
        },
        {
          "uuid": "2ecf6768",
          "activity_type": "closed_won",
          "activity_date": "20181119",
          "amount": "608152.0",
          "description": "Application settled",
          "created_at": "2018­11­19T20:59:43.602+11:00",
          "updated_at": "2018­11­19T20:59:43.602+11:00"
        },
        {
          "uuid": "4f9f290b",
          "activity_type": "note",
          "activity_date": "20181016",
          "amount": null,
          "description": "Looking to settle mid November ­ TN",
          "created_at": "2018­10­16T18:01:19.129+11:00",
          "updated_at": "2018­10­16T18:01:19.129+11:00"
        }
      ]
    }
  }
}
</script>

<style lang="scss">
@import 'assets/styles/main.scss';
</style>
