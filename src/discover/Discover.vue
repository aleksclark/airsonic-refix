<template>
  <ContentLoader :loading="loading">
    <div v-for="section in sections" :key="section.key" class="mb-4">
      <template v-if="result[section.key].length > 0">
        <h1>
          {{ section.name }}
          <router-link
            :to="{name: 'albums', params: {sort: section.key}}"
            class="text-muted"
          >
            <Icon icon="chevron-right" />
          </router-link>
        </h1>
        <AlbumList :items="result[section.key]" allow-h-scroll />
      </template>
    </div>
    <EmptyIndicator v-if="empty" />
  </ContentLoader>
</template>
<script lang="ts">
  import { defineComponent } from 'vue'
  import AlbumList from '@/library/album/AlbumList.vue'
  import { Album } from '@/shared/api'

  export default defineComponent({
    components: {
      AlbumList,
    },
    data() {
      return {
        loading: true as boolean,
        result: {
          'recently-added': [] as Album[],
          'recently-played': [] as Album[],
          'most-played': [] as Album[],
          random: [] as Album[],
        }
      }
    },
    computed: {
      sections(): any[] {
        return [
          { name: 'Recently added', key: 'recently-added' },
          { name: 'Recently played', key: 'recently-played' },
          { name: 'Most played', key: 'most-played' },
          { name: 'Random', key: 'random' },
        ]
      },
      empty() {
        return Object.values(this.result).findIndex(x => x.length > 0) === -1
      }
    },
    created() {
      const size = 18
      this.$api.getAlbums('recently-added', size).then(result => {
        this.result['recently-added'] = result
        this.loading = false
      })
      this.$api.getAlbums('recently-played', size).then(result => {
        this.result['recently-played'] = result
      })
      this.$api.getAlbums('most-played', size).then(result => {
        this.result['most-played'] = result
      })
      this.$api.getAlbums('random', 50).then(result => {
        this.result.random = result
      })
    }
  })
</script>
