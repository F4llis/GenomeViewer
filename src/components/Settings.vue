<template>
  <div id="main_container" class="sticky-top">
    <div class="d-flex justify-content-end">

      <ButtonWithIcon
          id="button_type"
          :icon="typeIcon"
          :text="typeText"
          @click="emitEvent('toggle-type')"
      />

      <ButtonWithIcon
          class="me-2"
          data-bs-target="#toggleDiv"
          data-bs-toggle="collapse"
          icon="bi bi-funnel"
          text="Filter"
      />

      <ButtonWithIcon
          id="button_hide"
          :icon="hideIcon"
          text="Detail"
          @click="emitEvent('toggle-hide')"
      />

      <ButtonWithIcon
          id="button_sorting"
          :icon="sortingIcon"
          :text="sortingText"
          @click="emitEvent('toggle-sorting')"
      />

      <ButtonWithIcon
          id="button_selection"
          :text="modeText"
          icon="bi bi-hand-index"
          @click="emitEvent('toggle-mode')"
      />

      <VerticalTextDivider text="General"/>

      <DropdownButton
          id="button_color_overview"
          v-model="localColorAccessorOverview"
          :options="settings.states_color_genes"
          icon="bi bi-paint-bucket"
          text="Color"
          @change="emitEvent('update-color-overview', $event)"
      />

      <DropdownButton
          id="button_height_overview"
          v-model="localHeightAccessorOverview"
          :options="settings.states_color_genes"
          icon="bi bi-arrows-vertical"
          text="Height"
          @change="emitEvent('update-height-overview', $event)"
      />

      <VerticalTextDivider text="Overview"/>

      <DropdownButton
          id="button_color_excerpt"
          v-model="localColorAccessorExcerpt"
          :options="settings.states_color_genes"
          icon="bi bi-paint-bucket"
          text="Color"
          @change="emitEvent('update-color-excerpt', $event)"
      />

      <DropdownButton
          id="button_height_excerpt"
          v-model="localHeightAccessorExcerpt"
          :options="settings.states_color_genes"
          icon="bi bi-arrows-vertical"
          text="Height"
          @change="emitEvent('update-height-excerpt', $event)"
      />

      <VerticalTextDivider text="Excerpt"/>

    </div>
    <div id="toggleDiv" class="collapse text-center" style="margin:24px; padding: 50px; border: #dddddd 1px solid">
      <b>Place holder for filtering widget.</b>
    </div>
  </div>
</template>

<script>
import VerticalTextDivider from './VerticalTextDivider.vue';
import ButtonWithIcon from './ButtonWithIcon.vue';
import DropdownButton from './DropdownButton.vue';

export default {
  name: 'SettingsUI',
  components: {
    VerticalTextDivider,
    ButtonWithIcon,
    DropdownButton
  },
  props: {
    settings: Object,
  },
  data() {
    return {
      localColorAccessorOverview: this.settings.colorAccessor_overview,
      localHeightAccessorOverview: this.settings.heightAccessor_overview,
      localColorAccessorExcerpt: this.settings.colorAccessor_excerpt,
      localHeightAccessorExcerpt: this.settings.heightAccessor_excerpt,
    };
  },
  watch: {
    localColorAccessorOverview(newVal) {
      this.emitEvent('update-color-overview', newVal);
    },
    localHeightAccessorOverview(newVal) {
      this.emitEvent('update-height-overview', newVal);
    },
    localColorAccessorExcerpt(newVal) {
      this.emitEvent('update-color-excerpt', newVal);
    },
    localHeightAccessorExcerpt(newVal) {
      this.emitEvent('update-height-excerpt', newVal);
    }
  },
  computed: {
    typeIcon() {
      return this.settings.type_position === 'loci' ? 'bi bi-rulers' : 'bi bi-rulers';
    },
    typeText() {
      return this.settings.type_position === 'loci' ? 'Locus' : 'Fixed';
    },
    hideIcon() {
      return this.settings.hide ? 'bi bi-eye-slash' : 'bi bi-eye-fill';
    },
    sortingIcon() {
      return this.settings.sorting_chromosome === 'size' ? 'bi bi-sort-up' : this.settings.sorting_chromosome === 'number_genes' ? 'bi bi-sort-numeric-up-alt' : 'bi bi-sort-alpha-up';
    },
    sortingText() {
      return this.settings.sorting_chromosome === 'size' ? 'Size' : this.settings.sorting_chromosome === 'number_genes' ? 'Genes' : 'Name';
    },
    modeText() {
      return this.settings.mode === 'zoom' ? 'Zoom' : 'Brush';
    },
  },
  methods: {
    emitEvent(eventType, payload = null) {
      this.$emit('settings-event', {eventType, payload});
    },

  },
  emits: ['settings-event'],
}
</script>

<style scoped>
#main_container {
  z-index: 1000;
  background-color: white;
  padding: 12px;
}

</style>