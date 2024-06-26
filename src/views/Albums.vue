
<!-- views/Albums.vue -->
<template>
    <Menu />
    <q-page>
        <q-list bordered>
            <q-item v-for="album in albums" :key="album.id" clickable @click="viewPhotos(album.id)">
                <q-item-section>{{ album.title }}</q-item-section>
            </q-item>
        </q-list>
    </q-page>
</template>

<script>
import { useAlbumStore } from '../stores/albums'
import { onMounted } from 'vue'
import { useRouter } from 'vue-router'
import Menu from '../components/Menu.vue'

export default {
    components: { Menu },
    setup() {
        const albumStore = useAlbumStore()
        const router = useRouter()

        onMounted(() => {
            albumStore.fetchAlbums()
        })

        const viewPhotos = (albumId) => {
            router.push(`/albums/${albumId}`)
        }

        return { albums: albumStore.albums, viewPhotos }
    }
}
</script>