<!--
  Created by: Carlos E. Salazar <ce.salazar@gmail.com>
  Repository: https://github.com/cesalazar/berlinblockchainweek
  License: MIT
-->

<template>
  <p class="datetime">
    <slot name="before"></slot>
    {{ datetime }}
    <slot name="after"></slot>
  </p>
</template>

<script>
export default {
  props: ['date', 'endDate', 'time', 'endTime'],
  data: () => ({
    dayNames: [
      'Sunday',
      'Monday',
      'Tuesday',
      'Wednesday',
      'Thursday',
      'Friday',
      'Saturday',
    ],
    monthNames: [
      "January",
      "February",
      "March",
      "April",
      "May",
      "June",
      "July",
      "August",
      "September",
      "October",
      "November",
      "December",
    ]
  }),
  computed: {
    datetime () {
      // First date of the event
      let date = this.date || this.$page.frontmatter.date
      date = new Date(date)

      // Set the names for both the day and the month
      let day = this.dayNames[date.getUTCDay()]
      let month = this.monthNames[date.getUTCMonth()]

      // The full date
      let datetime = `${day}, ${date.getUTCDate()}`

      // Last date of the event
      let endDate = this.endDate || this.$page.frontmatter.endDate

      // Set the end date only if defined and different from start date
      if (endDate) {
        endDate = new Date(endDate)
        if (endDate.toJSON() !== date.toJSON()) {
          day = this.dayNames[endDate.getUTCDay()]
          datetime += ` - ${day}, ${endDate.getUTCDate()}`
        }
      }

      datetime += ` ${month}`

      // Starting time
      let time = this.time || this.$page.frontmatter.time
      if (time) datetime += ` @ ${time}`

      // Ending time
      let endTime = this.endTime || this.$page.frontmatter.endTime
      if ((endTime && time) && (endTime !== time)) datetime += `-${endTime}`

      return datetime
    }
  }
}
</script>
