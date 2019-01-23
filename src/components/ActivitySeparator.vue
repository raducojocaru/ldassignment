<template>
  <div class="separator">
    <div class="separator__information">
      <p>{{activity.separator}}</p>
      <span>{{formalDate}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "ActivitySeparator",
  props: ["activity"],
  computed: {
    formalDate: function() {
      // get the date for the activity and generate the required format date
      const monthNames = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];
      let activityDateString = this.activity.activity_date;      
      let year = activityDateString.substring(0,4);
      let month = activityDateString.substring(4,6);
      let day = activityDateString.substring(6,8);
      let activityDateObject = new Date(year,month-1,day);
      return activityDateObject.getDate() + ' ' + monthNames[activityDateObject.getMonth()] + ' ' + activityDateObject.getFullYear();
    }
  }
}
</script>

<style lang="scss" scoped>

.separator {
  position:relative;
  z-index: 1;
  height:100px;

  &:before {
    border-top: 1px solid #999;
    content:"";
    margin: 0 auto;
    position: absolute;
    top: 50%; left: 0; right: 0; bottom: 0;
    width: calc( 100% - 24px );
    z-index: -1;
  }

  .separator__information {
    background: #fff; 
    padding: 0 15px;
    width: 180px;
    margin: auto auto;
    text-align: center;
    height:100%;
    display:flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    p {
      font-weight:600;
      font-size:13px;
      text-transform: uppercase;
      line-height: 14px;
    }
    span {
      font-weight:200;
      font-size:9px;
      line-height: 10px;
      color : #888;
    }
  }
}

</style>