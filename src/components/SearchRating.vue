<template>
  <div class="caption has-text-weight-semibold">
    <div v-if="status == 'NOT_SENT'">
      <span>{{ $t('browse-page.search-rating.content') }}</span>
      <button
        aria-label="relevant result? answer: yes"
        class="button is-text tiny is-paddingless rating is-shadowless"
        @click="sendSearchRatingEvent(true)"
      >
        {{ $t('browse-page.search-rating.yes') }}
      </button>
      •
      <button
        aria-label="relevant result? answer: no"
        class="button is-text tiny is-paddingless rating is-shadowless"
        @click="sendSearchRatingEvent(false)"
      >
        {{ $t('browse-page.search-rating.no') }}
      </button>
    </div>
    <div v-else-if="status == 'SENT'">
      <span class="thank-you">{{
        $t('browse-page.search-rating.feedback')
      }}</span>
    </div>
  </div>
</template>

<script>
import { SEND_SEARCH_RATING_EVENT } from '@/store/usage-data-analytics-types'

const Statuses = {
  NOT_SENT: 'NOT_SENT',
  SENT: 'SENT',
}

export default {
  name: 'search-rating',
  props: ['searchTerm'],
  data() {
    return {
      status: Statuses.NOT_SENT,
    }
  },
  methods: {
    sendSearchRatingEvent(isRelevant) {
      this.$store.dispatch(SEND_SEARCH_RATING_EVENT, {
        query: this.$props.searchTerm,
        relevant: isRelevant,
      })

      this.status = Statuses.SENT
      setTimeout(() => {
        this.status = null
      }, 1500)
    },
  },
}
</script>

<style lang="scss" scoped>
.button.rating {
  vertical-align: middle;
  color: rgb(5, 181, 218);
  font-size: 0.8rem;
  text-decoration: none;
  text-transform: none;

  &:hover {
    background: none;
  }

  &:focus {
    background: none;
  }
}

span {
  vertical-align: middle;
}
</style>
