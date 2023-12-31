<script setup lang="ts">
import { computed, onMounted } from 'vue';
import workItems from './work.yml';
import ImageCarousel from '../components/ImageCarousel.vue';
import { changeTitle } from '../useDocumentTitle';

const props = defineProps<{
  workSlug: string;
}>();

const workIndex = computed(() => {
  return workItems.findIndex(workItem => workItem.slug === props.workSlug)!;
})
const work = computed(() => {
  return workItems[workIndex.value];
});

onMounted(() => {
  changeTitle(work.value.name);
})

const index = computed(() => {
  const nonZeroIndex = workIndex.value + 1;
  return (nonZeroIndex > 9) ? nonZeroIndex : `0${nonZeroIndex}`;
});
</script>

<template>
  <div class="work-item-grid">
    <div class="desktop-number">{{ index }}</div>
    <div class="content">
      <div class="title"><span class="mobile-number">{{ index }}</span> {{ work.name }}</div>
      <div v-html="work.description"></div>
    </div>
    <ImageCarousel :images="work.images" work />
  </div>
</template>

<style scoped lang="postcss">
.desktop-number {
  display: none;
}

.mobile-number {
  display: inline;
  margin-right: var(--spacing-md);
}

.title {
  margin-bottom: var(--spacing-md);
  text-transform: uppercase;
}

.content {
  margin-bottom: var(--spacing-md);
}

@media (min-width: 1000px) {
  .work-item-grid {
    margin: 0 auto;
    display: grid;
    grid-template-columns: auto 1fr 3fr;
    gap: var(--spacing-md);
  }
  .content {
    margin-bottom: 0;
  }

  .desktop-number {
    display: block;
  }

  .mobile-number {
    display: none;
  }
}

</style>
