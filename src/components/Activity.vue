<template>
  <div class="activity">
    <div class="activity__descriptor" v-bind:class="backgroundColor">
      <div class="activity__type">{{prettyType}}</div>
      <div class="activity__date-counter">{{dateCounter}}</div>
    </div>
    <div class="activity__info">
      <p class="activity__note">{{activity.description}}</p>
      <p class="activity__date">{{prettyActivityDate}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Activity",
  props: ["activity"],
  computed: {
    prettyType: function() {
      let activityType = this.activity.activity_type;
      if (activityType == 'accept') {
        activityType = 'accepted';
      }
      if (activityType == 'closed_won') {
        activityType = 'settled';
      }
      if (activityType == 'closed_lost') {
        activityType = 'closed';
      }
      return activityType;
    },
    dateCounter: function() {
      // get the date for the date counter
      let activityDateString = this.activity.activity_date;      
      let year = activityDateString.substring(0,4);
      let month = activityDateString.substring(4,6);
      let day = activityDateString.substring(6,8);
      let activityDateObject = new Date(year,month-1,day);
      let currentDate = new Date();
      
      // calculate the difference between days and round it up
      let dateDifference = Math.trunc((currentDate-activityDateObject)/(1000*60*60*24));

      // decide on what to output
      if (dateDifference > 7) {
        return;
      }
      else if (dateDifference == 0) {
        return 'Today';
      }
      else if (dateDifference == 1) {
        return 'Yesterday';
      }
      else {
        return dateDifference + ' days ago'
      }
    },
    prettyActivityDate: function() {
      //reorder the date in format DD/MM/YYYY
      let activityDate = this.activity.activity_date;
      let year = activityDate.substring(0,4);
      let month = activityDate.substring(4,6);
      let day = activityDate.substring(6,8);
      return day + '/' + month + '/' + year;
    },
    backgroundColor: function() {
      // depending on the already calculated activity.color propoerty we define the corresponding coloring class
      return {
        'background--afternoon-apricot': this.activity.color == 'afternoon-apricot',
        'background--desert-flower': this.activity.color == 'desert-flower',
        'background--purple-haze': this.activity.color == 'purple-haze',
        'background--mountain-breeze': this.activity.color == 'mountain-breeze',
        'background--aqua-ocean': this.activity.color == 'aqua-ocean',
        'background--mojo': this.activity.color == 'mojo'
      }
    }
  }
}
</script>

<style lang="scss" scoped>

.activity {
  margin: 12px 12px;
  background-color:#f9f9f9;
  border-radius: 6px;
  height: 100px;
  display:flex;

  .activity__descriptor {
    color: #fff;
    width:100px;
    border-top-left-radius: 6px;
    border-bottom-left-radius: 6px;
    display:flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-size:12px;

    .activity__type {
      font-size:14px;
      font-weight: 400;
      text-transform: uppercase;
    }

    .activity__date-counter {
      font-size:10px;
      font-weight: 300;
    }
  }

  .activity__info {
    padding: 12px;
    display:flex;
    flex-direction: column;
    justify-content: space-between;

    .activity__note {
      font-size:13px;
    }

    .activity__date {
      font-size:13px;
      font-style: italic;
    }
  }

}
</style>