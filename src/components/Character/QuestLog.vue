<template>
  <div class="column" style="heigth: 100%">
    <div class="toolbar dark">
      <q-toolbar-title :padding="1">
        <i>visibility_off</i> {{ questStats[0] }} <i style="margin-left: 5px">directions_run</i>{{ questStats[1] }}  <i style="margin-left: 5px">done</i>{{ questStats[2] }}
      </q-toolbar-title>
      <button @click="toggleOrder()"><i>sort_by_alpha</i></button>
      <button>
        <i>visibility</i>
        <q-popover ref="visibilityPop">
          <div class="list">
            <label class="item">
              <q-checkbox v-model="stateVisibility[0]"></q-checkbox>
              Non découverte
            </label>
            <label class="item">
              <q-checkbox v-model="stateVisibility[1]"></q-checkbox>
              En cours
            </label>
            <label class="item">
              <q-checkbox v-model="stateVisibility[2]"></q-checkbox>
              Terminé
            </label>
          </div>
        </q-popover>
      </button>
    </div>
    <hidden-scrollbar class="auto">
      <div slot="scrollContent" class="list no-border item-delimiter">
        <q-collapsible
          v-for="(item, index) in questLog"
          :key="index"
          :icon="stateIcons[item.state]"
          :label="item.name"
          group="questLog"
          v-if="stateVisibility[item.state]"
          >
          <div>
            {{ item.description }}
          </div>
        </q-collapsible>
      </div>
    </hidden-scrollbar>
  </div>
</template>

<script>
import HiddenScrollbar from '../HiddenScrollbar'

export default {
  components: {
    HiddenScrollbar
  },
  data () {
    return {
      stateIcons: ['visibility_off', 'directions_run', 'done'],
      stateVisibility: [true, true, true],
      order: 'asc',
      range: { min: 1, max: 30 }
    }
  },
  computed: {
    questLog () {
      return this.$store.getters.charQuestLog(this.$route.params.bicFileName)
    },
    questStats () {
      return this.$store.getters.charQuestLogStats(this.$route.params.bicFileName)
    }
  },
  methods: {
    toggleOrder () {
      console.log('TODO: quest log orderBy')
      this.order = this.order === 'asc' ? 'desc' : 'asc'
    },
    setNav (path) {
      let params = path.split('/')
      this.$store.dispatch('setNav', {
        character: true,
        path: '/' + params[1] + '/' + params[2],
        sub: '/' + params[3]
      })
    }
  },
  beforeRouteUpdate (to, from, next) {
    this.setNav(to.path)
    next()
  },
  mounted () {
    this.setNav(this.$route.path)
  }
}
</script>

<style scoped>
</style>
