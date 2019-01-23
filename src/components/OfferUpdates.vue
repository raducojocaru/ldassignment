<template>
  <div>
    <div v-bind:key="activity.uuid" v-for="activity in parsedActivities">
      <template v-if="activity.hasSeparator">
        <ActivitySeparator v-bind:activity="activity"/>
      </template>
      <Activity v-bind:activity="activity"/>
    </div>
  </div>
</template>

<script>
import Activity from './Activity';
import ActivitySeparator from './ActivitySeparator';

export default {
  name: "OfferUpdates",
  components: {
    Activity,
    ActivitySeparator
  },
  props: ["activities"],
  computed: {
    // order the activities by date of the activity (not created_at)
    orderedActivities: function() {
      let activities = this.activities;
      
      // create a proper date object for each activity 
      activities.forEach(activity => {
        let activityDateString = activity.activity_date;      
        let year = activityDateString.substring(0,4);
        let month = activityDateString.substring(4,6);
        let day = activityDateString.substring(6,8);
        activity.activity_date_object = new Date(year,month-1,day);
      });

      // sort
      activities.sort(function(a,b) {
        return b.activity_date_object - a.activity_date_object;
      })

      return activities;
    },
    // parse activities to decide which has a separater obove and the coloring based on the rules
    parsedActivities: function() {
      let activities = this.orderedActivities;
      // reverse the activities to parse them in chronological order (ascending)
      activities.reverse();
      // count the current state, needed for color determination in multi-stage types (notes and contacts)
      let currentState = '';

      // start parsing the activities, keeping track of each type and the context of the state
      activities.forEach(activity => {
        // reset each activity
        activity.hasSeparator = false;
        activity.separator = '';
        activity.color = '';

        // start parsing according to the rules
        // accepted event
        if (activity.activity_type == 'accept') {
          currentState = 'contact'
          activity.hasSeparator = true;
          activity.separator = 'contact';
          activity.color = 'afternoon-apricot';
        }
        // contact event
        if (activity.activity_type == 'contact') {
          if (currentState == 'contact') {
            currentState = 'application';
            activity.hasSeparator = true;
            activity.separator = 'application';
            activity.color = 'desert-flower';
          }
          else if (currentState == 'application') {
            activity.color = 'purple-haze';
          }
          else if (currentState == 'pending') {
            activity.color = 'aqua-ocean';
          }
          else if (currentState == 'settled') {
            activity.color = 'mountain-breeze';
          }
          else if (currentState == 'closed') {
            activity.color = 'mojo';
          }
        }
        // note event
        if (activity.activity_type == 'note') {
          if (currentState == 'contact') {
            activity.color = 'desert-flower';
          }          
          else if (currentState == 'application') {
            activity.color = 'purple-haze';
          }
          else if (currentState == 'pending') {
            activity.color = 'aqua-ocean';
          }
          else if (currentState == 'settled') {
            activity.color = 'mountain-breeze';
          }
          else if (currentState == 'closed') {
            activity.color = 'mojo';
          }
        }
        // application event
        if (activity.activity_type == 'application') {
          if (currentState == 'application') {
            currentState = 'pending';
            activity.hasSeparator = true;
            activity.separator = 'pending';
            activity.color = 'purple-haze';
          }
        }
        // settled event
        if (activity.activity_type == 'closed_won') {
          if (currentState == 'pending') {
            currentState = 'settled';
            activity.color = 'mountain-breeze';
          }
        }
        // closed event
        if (activity.activity_type == 'closed_lost') {
          if (currentState == 'contact') {
            activity.color = 'mojo';
          }
          else if (currentState == 'application') {
            activity.color = 'mojo';
          }
          else if (currentState == 'pending') {
            currentState = 'closed';
            activity.hasSeparator = true;
            activity.separator = 'closed';
            activity.color = 'mojo';
          }
        }
      })
      // go back to the descending order
      activities.reverse();
      return activities;
    }
  }
}
</script>