<script setup lang="ts">
import { IconEntity } from '../../types';
import IconDetailName from './IconDetailName.vue';
import Badge from '../base/Badge.vue';
import CopySVGButton from './CopySVGButton.vue';
import CopyCodeButton from './CopyCodeButton.vue';
import VPButton from 'vitepress/dist/client/theme-default/components/VPButton.vue';
import {useData, useRouter} from 'vitepress';
import { computed } from 'vue';

const props = defineProps<{
  icon: IconEntity
  popoverPosition?: 'top' | 'bottom'
}>()

const { go } = useRouter()
const { page } = useData()

const tags = computed(() => {
  if (!props.icon || !props?.icon?.tags) return []
  return props.icon.tags.join(' • ')
})
</script>

<template>
  <div class="icon-info">
    <IconDetailName class="icon-name">
      {{ icon.name }}
    </IconDetailName>
    <div class="tags-scroller" v-if="tags.length">
      <p class="icon-tags horizontal-scroller">
        {{ tags }}
      </p>
    </div>
    <div class="group">
      <Badge
        v-for="category in icon.categories"
        class="category"
        :href="`/icons/categories#${category}`"
      >
        {{ category }}
      </Badge>
    </div>

    <div class="group buttons">
      <VPButton
        v-if="!page?.relativePath?.startsWith?.(`icons/${icon.name}`)"
        :href="`/icons/${icon.name}`"
        text="See in action"
        @click="go(`/icons/${icon.name}`)"
      />
      <CopySVGButton :name="icon.name" :popoverPosition="popoverPosition"/>
      <CopyCodeButton :name="icon.name" :popoverPosition="popoverPosition"/>
    </div>
    <slot name="footer" />
  </div>
</template>

<style scoped>
.group {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 16px;
}
.category {
  text-transform: capitalize;
}
.icon-name {
  margin-bottom: 4px;
}

.icon-tags {
  font-size: 16px;
  color: var(--vp-c-text-2);
  font-weight: 500;
  line-height: 28px;
  white-space: nowrap;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  margin-top: 0;
  margin-bottom: 0;
}

.tags-scroller {
  position: relative;
  max-width: 100%;
  width: 100%;
  height: 28px;
  padding: 8px 0 16px;
  margin-bottom: 16px;
  margin-top: 8px;
  align-items: center;

  --gradient-background: var(--tags-gradient-background, var(--vp-c-bg-elv))
}
.horizontal-scroller {
  overflow-x: scroll;
  /* Hide Scrollbar */
  -ms-overflow-style: none;
  scrollbar-width: none;
  scrollbar-width: thin; /* can also be normal, or none, to not render scrollbar */
  scrollbar-color: currentColor transparent; /* foreground background */
}
.horizontal-scroller::-webkit-scrollbar {
  width: 0;
  display: none
}

.horizontal-scroller::-webkit-scrollbar-track {
  background: transparent
}

.horizontal-scroller::-webkit-scrollbar-thumb {
  background: transparent;
  border: none
}


.tags-scroller::after {
  content: '';
  position: absolute;
  bottom: 0;
  width: 32px;
  height: 100%;
  /* Background Gradient left to right */
  background: linear-gradient(to right, rgba(255,255,255,0) 0%,var(--gradient-background) 100%);
  right: 0;
  pointer-events: none;
}

.buttons {
  margin-top: 24px;
}
</style>
