<template>
  <div class="row justify-center q-pa-md">
    <q-card bordered style="width: 50vw">
      <q-card-section>
        <div class="text-weight-bold">{{ post.title }}</div>
        <div class="text-subtitle2">{{ post.author ? `by ${ post.author }` : '' }}</div>
      </q-card-section>
      <q-separator />
      <q-card-section>
        {{ post.body }}
      </q-card-section>

      <q-inner-loading :showing="isLoading">
        <q-spinner size="50px" color="primary" />
      </q-inner-loading>
    </q-card>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()

const post = ref({})
const isLoading = ref(true)

// uploading post with username of author
;(async () => {
  try {
    const postId = route.params.id

    const resPost = await fetch(`https://jsonplaceholder.typicode.com/posts/${ postId }`)
    const dataPost = await resPost.json()

    const authorId = dataPost.userId
    const resUser = await fetch(`https://jsonplaceholder.typicode.com/users/${ authorId }`)
    const dataUser = await resUser.json()

    post.value = {
      ...dataPost,
      author: dataUser.name
    }

	isLoading.value = false
  } catch (e) {
    console.log(e)
  }
})()
</script>
