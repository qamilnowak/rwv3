<template>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <div class="pagination">
      <router-link
        id="page-prev"
        :to="{ name: 'EventList', query: { page: page - 1 } }"
        rel="prev"
        v-if="page !== 1"
        >&#60; Previous</router-link
      >
      <template v-if="page">
        <router-link
          :class="{ 'active-page': page == eventPage }"
          v-for="eventPage in eventPages"
          :key="eventPage"
          :to="{ name: 'EventList', query: { page: eventPage } }"
        >
          {{ eventPage }}
        </router-link>
      </template>
      <router-link
        id="page-next"
        :to="{ name: 'EventList', query: { page: page + 1 } }"
        rel="next"
        v-if="hasNextPage"
        >Next &#62;
      </router-link>
    </div>
  </div>
</template>

<script>
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService'
// import NProgress from 'nprogress'
import _ from 'lodash'

export default {
  name: 'EventList',
  props: {
    page: {
      type: Number,
      required: true,
    },
  },
  components: {
    EventCard,
  },
  data() {
    return {
      events: [],
      totalEvents: 0,
    }
  },
  beforeRouteEnter(routeTo, routeFrom, next) {
    EventService.getEvents(2, parseInt(routeTo.query.page) || 1)
      .then((response) => {
        next((comp) => {
          comp.events = response.data
          comp.totalEvents = response.headers['x-total-count']
        })
      })
      .catch(() => {
        next({
          name: 'NetworkError',
        })
      })
      .finally(() => {
      })
  },
  beforeRouteUpdate(routeTo) {
    return EventService.getEvents(2, parseInt(routeTo.query.page) || 1)
      .then((response) => {
        this.events = response.data
        this.totalEvents = response.headers['x-total-count']
      })
      .catch(() => {
        return {
          name: 'NetworkError',
        }
      })
      .finally(() => {
      })
  },
  computed: {
    eventPages() {
      return _.range(1, Math.ceil(this.totalEvents / 2) + 1)
    },
    hasNextPage() {
      let totalPages = Math.ceil(this.totalEvents / 2)
      return this.page < totalPages
    },
  },
}
</script>
<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}
.router-link-active.active-page {
  font-weight: bold;
  color: #42b983;
  text-decoration: underline;
}
</style>
