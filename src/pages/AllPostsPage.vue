<template>
  <q-table
    :rows="posts"
    :columns="columns"
    :grid="showAsGrid"
    :rows-per-page-options="[]"
    v-model:pagination="pagination"
    row-key="id"
    wrap-cells
    class="no-shadow absolute-full"
  >
    <template v-slot:top>
      <div class="text-h6">
        {{ `AVG POST TITLE LENGTH: ${averageTitleLength || ''}` }}
      </div>
      <q-space />
      <q-toggle
        v-model="showAsGrid"
        :label="showAsGrid ? 'Grid' : 'Table'"
        left-label
      />
    </template>

    <template v-slot:body="props">
      <q-tr :props="props" @click="clickRowHandler(props.row)" class="cursor-pointer">
        <q-td key="author" :props="props" class="text-weight-bolder">
          {{ props.row.author }}
        </q-td>
        <q-td key="title" :props="props">
          {{ props.row.title }}
        </q-td>
        <q-td key="body" :props="props">
          {{ props.row.body.slice(0, 80) + '...' }}
        </q-td>
      </q-tr>
    </template>

    <template v-slot:item="props">
      <div class="q-px-lg col-xs-12 col-sm-6 col-md-4">
        <q-card bordered>
          <q-card-section>
            <div class="text-weight-bold">{{ props.row.title }}</div>
            <div class="text-subtitle2">by {{ props.row.author }}</div>
          </q-card-section>

          <q-separator />

          <q-card-section>
            {{ props.row.body.slice(0, 100) + '...' }}
          </q-card-section>
        </q-card>
      </div>
    </template>
  </q-table>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const showAsGrid = ref(false)
const posts = ref([])

// props for q-table
const columns = [
  {
    name: 'author',
    label: 'Author',
    align: 'left',
    field: 'author'
  },
  {
    name: 'title',
    label: 'Title',
    align: 'left',
    field: 'title'
  },
  {
    name: 'body',
    label: 'Body',
    align: 'left',
    field: 'body'
  }
]

const pagination = ref({
  page: 1,
  rowsPerPage: 10
})
// 

// uploading posts and preparing them (finding usernames)
;(async () => {
  try {
    const resPosts = await fetch('https://jsonplaceholder.typicode.com/posts')
    const data = await resPosts.json()

    const resUsers = await fetch('https://jsonplaceholder.typicode.com/users')
    const users = await resUsers.json()

    const namesOfUsers = new Map()
    for (const user of users) namesOfUsers.set(user.id, user.username)

    for (const post of data) post.author = namesOfUsers.get(post.userId)

    posts.value = data
  } catch (e) {
    console.log(e)
  }
})()

const averageTitleLength = computed(() => {
  const firstVisiblePostId =
    (pagination.value.page - 1) * pagination.value.rowsPerPage

  const activePosts = posts.value.slice(
    firstVisiblePostId,
    firstVisiblePostId + pagination.value.rowsPerPage
  )

  let sumOfTitleLength = 0
  for (const activePost of activePosts) {
    sumOfTitleLength += activePost.title.length
  }

  return sumOfTitleLength / activePosts.length
})

const clickRowHandler = row => router.push(`/post/${row.id}`)
</script>
