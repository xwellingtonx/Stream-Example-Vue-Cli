<template>
  <div>
    <h1>Feed</h1>
    <div>
      <activity-card
        v-for="activity in activities"
        :key="activity.id"
        :activity="activity">
      </activity-card>
    </div>
  </div>
</template>

<script>
import * as stream from 'getstream'
import ActivityCard from '@/components/ActivityCard'

export default {
  components: {
    ActivityCard
  },
  data () {
    return {
      activities: []
    }
  },
  destroyed () { },
  methods: {
    getActivities: function () {
      this.activities = []
      let client = stream.connect(
        process.env.STREAM_APP_KEY,
        null,
        process.env.STREAM_APP_ID
      )
      let feed = client.feed(
        process.env.STREAM_FEED_GROUP,
        process.env.STREAM_FEED_ID,
        process.env.STREAM_FEED_READ_ONLY_TOKEN
      )
      feed.get().then(response => {
        response['results'].map((a) => { this.activities.push(a) })
      })
    }
  },
  mounted () {
    this.getActivities()
  },
  name: 'FeedActivityList'
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
</style>
