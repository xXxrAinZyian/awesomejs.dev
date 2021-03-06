<script>
import PackageLogo from './PackageLogo.vue'
import PackageCount from './PackageCount.vue'
import { useEmoji } from '@/util/emoji'
import { useTags } from '@/util/tags'

export default {
  components: {
    PackageLogo,
    PackageCount,
  },

  inheritAttrs: false,

  props: {
    pkg: {
      type: Object,
      required: true,
    },
  },

  setup (props) {
    const { isOfficial } = useTags(() => props.pkg)
    const { parsedText: parsedDescription } = useEmoji(() => props.pkg.description)

    return {
      isOfficial,
      parsedDescription,
    }
  },
}
</script>

<template>
  <router-link
    v-bind="$attrs"
    class="package-list-item button-like flex items-center bg-gray-800 rounded p-2 sm:px-6 sm:py-4 hover:bg-gray-700 leading-snug sm:leading-normal"
  >
    <PackageLogo
      :pkg="pkg"
      class="mr-2 sm:mr-6"
    />

    <div class="flex-1 w-0 mr-2 sm:mr-6 overflow-hidden">
      <div class="w-full truncate text-gray-600">
        <span class="text-gray-100">
          {{ pkg.name }}
        </span>

        <!-- <span
          v-if="isOfficial"
          class="text-orange-400 bg-yellow-900 px-1 mx-1 rounded"
        >
          official
        </span> -->
      </div>

      <div class="w-full truncate text-gray-500">
        <span v-if="pkg.description">
          {{ parsedDescription }}
        </span>
        <span v-else>No description</span>
      </div>
    </div>

    <slot />

    <PackageCount
      v-if="pkg.upvotes != null"
      :count="pkg.upvotes"
      icon="thumb_up"
      class="package-count"
    />
    <PackageCount
      v-else-if="pkg.stars != null"
      :count="pkg.stars"
      :color="isOfficial ? 'orange' : 'purple'"
      class="package-count"
    />
  </router-link>
</template>

<style lang="postcss" scoped>
.package-list-item.router-link-active {
  @apply bg-purple-900;

  &:hover {
    @apply bg-purple-800;
  }

  .text-gray-100 {
    @apply text-purple-100;
  }

  .text-gray-600 {
    @apply text-purple-600;
  }

  .text-gray-500 {
    @apply text-purple-500;
  }

  .package-count {
    @apply text-purple-400;

    &.text-orange-500 {
      @apply text-orange-400;
    }
  }
}
</style>
