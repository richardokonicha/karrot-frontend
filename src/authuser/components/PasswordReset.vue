<template>
  <div>
    <form
      name="passwordReset"
      @submit.prevent="submit"
    >
      <div class="content">
        <div>
          <QInput
            v-model="newPassword"
            type="password"
            :label="$t('USERDATA.PASSWORD')"
            autocorrect="off"
            autocapitalize="off"
            spellcheck="false"
            :error="hasError('newPassword')"
            :error-message="firstError('newPassword')"
          >
            <template v-slot:before>
              <QIcon name="fas fa-lock" />
            </template>
          </QInput>
        </div>
        <div
          v-if="hasError('code')"
          class="text-warning"
        >
          <i class="fas fa-exclamation-triangle" />
          {{ $t('GLOBAL.INVALID_LINK') }}
        </div>
        <div
          v-if="hasNonFieldError"
          class="text-warning"
        >
          <i class="fas fa-exclamation-triangle" />
          {{ firstNonFieldError }}
        </div>

        <div class="actions">
          <QBtn
            type="submit"
            class="submit shadow-4"
            :loading="isPending"
          >
            {{ $t('PASSWORD.RESET.OK') }}
          </QBtn>
        </div>
        <div style="clear: both" />
      </div>
    </form>
  </div>
</template>

<script>
import {
  QInput,
  QBtn,
  QIcon,
} from 'quasar'
import statusMixin from '@/utils/mixins/statusMixin'

export default {
  components: {
    QInput,
    QBtn,
    QIcon,
  },
  mixins: [statusMixin],
  props: {
    code: {
      required: true,
      type: String,
    },
  },
  data () {
    return {
      newPassword: null,
    }
  },
  methods: {
    submit () {
      if (!this.isPending) {
        this.$emit('submit', { newPassword: this.newPassword, code: this.code })
      }
    },
  },
}
</script>

<style scoped lang="stylus">
  .margin-bottom
    margin 0 0 24px 0
</style>
