<template>
  <div>
    <c-row>
      <c-col class="padding">
        {{ __('transition') }}
      </c-col>
      <c-col class="padding right">
        <c-checkbox
          :value="transition"
          @change="transitionEnabled = $event"></c-checkbox>
      </c-col>
    </c-row>
    <c-row :link="true" v-tap @tap.native="showPicker = !showPicker">
      <c-col class="padding">
        {{ __('language') }}
      </c-col>
      <c-col class="padding right">
        {{ Object.keys(languages)[languageIndex] }}
      </c-col>
    </c-row>
    <c-pane v-if="showPicker">
      <c-picker
        :index="languageIndex"
        @change="languageIndex = $event">
        <p v-for="(val, key) in languages">{{key}}</p>
      </c-picker>
    </c-pane>
  </div>
</template>

<script>
import tap from 'directives/tap'
import CPane from 'components/core/pane'
import CRow from 'components/core/row'
import CCol from 'components/core/col'
import CLink from 'components/core/link'
import CCheckbox from 'components/core/checkbox'
import CPicker from 'components/core/picker'

export default {
  data () {
    return {
      showPicker: false,
      languageIndex: 0,
      languages: {
        '简体中文': 'zh',
        'English': 'en',
        'العربية': 'ar'
      },
      transitionEnabled: false
    }
  },

  mapGetters: ['i18n/lang', 'transition'],
  mapActions: ['i18n/setI18n', 'setTransition'],

  mounted () {
    this.languageIndex = Object.keys(this.languages)
      .findIndex(key => this.languages[key] === this.lang)
    this.transitionEnabled = this.transition
  },

  watch: {
    languageIndex (val) {
      this.$nextTick(() => {
        this.setI18n({
          lang: this.languages[Object.keys(this.languages)[val]]
        })
      })
    },
    transitionEnabled (val) {
      this.$nextTick(() => {
        this.setTransition(val)
      })
    }
  },

  components: {
    CPane,
    CRow,
    CCol,
    CLink,
    CCheckbox,
    CPicker
  },

  directives: {
    tap
  }
}
</script>
