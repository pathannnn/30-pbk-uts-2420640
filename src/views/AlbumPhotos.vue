
<!-- views/AlbumPhotos.vue -->
<template>
    <Menu />
    <q-page>
        <q-list bordered>
            <q-item v-for="photo in photos" :key="photo.id" clickable @click="viewPhoto(photo.url)">
                <q-item-section thumbnail>
                    <q-img :src="photo.thumbnailUrl" />
                </q-item-section>
                <q-item-section>{{ photo.title }}</q-item-section>
            </q-item>
        </q-list>
    </q-page>
</template>

<script>
import { useAlbumStore } from '../stores/albums'
import { onMounted } from 'vue'
import { useRoute } from 'vue-router'
import Menu from '../components/Menu.vue'

export default {
    components: { Menu },
    setup() {
        const albumStore = useAlbumStore()
        const route = useRoute()

        onMounted(() => {
            albumStore.fetchPhotos(route.params.id)
        })

        const viewPhoto = (url) => {
            window.open(url, '_blank')
        }

        return { photos: albumStore.photos, viewPhoto }
    }
}
</script>