<template>
  <div class="container column">
    <app-form @addBlock="addBlock"></app-form>

    <div class="card card-w70">
      <h3 v-if="blocks.length === 0">Добавьте первый блок, чтобы увидеть результат</h3>
      <template
        v-for="block in blocks"
        v-else
        :key="block.id">
        <component
          :is="block.component"
          v-bind="{ value: block.value }"></component>
      </template>
    </div>

  </div>

  <div class="container">
    <p>
      <app-button
        color="primary"
        @action="loadComments"
        type="button"
      >Загрузить комментарии</app-button>
    </p>

    <template v-if="showComments">
      <app-loader v-if="loading"></app-loader>
      <div class="card" v-else>
        <h2>Комментарии</h2>
        <app-comments :comments="comments"></app-comments>
      </div>
    </template>

  </div>
</template>

<script>
import AppTitle from './components/AppTitle'
import AppSubtitle from './components/AppSubtitle'
import AppAvatar from './components/AppAvatar'
import AppText from './components/AppText'
import AppComments from './components/AppComments'
import AppLoader from './components/AppLoader'
import AppButton from './components/AppButton'
import AppForm from './components/AppForm'
import axios from 'axios'

export default {
  data () {
    return {
      loading: false,
      showComments: false,
      blocks: [],
      comments: []
    }
  },
  methods: {
    async addBlock (type, value) {
      const response = await axios.post('https://vue3-resume-default-rtdb.firebaseio.com/resume.json', {
        component: `app-${type}`,
        value: value
      })
      const firebaseData = await response.data

      this.blocks.push({
        component: `app-${type}`,
        value: value,
        id: firebaseData.name
      })
    },
    async loadComments () {
      this.showComments = true
      try {
        this.loading = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        this.comments = response.data
        this.loading = false
      } catch (e) {
        console.log(e.message)
      }
    }
  },

  components: {
    AppTitle,
    AppSubtitle,
    AppAvatar,
    AppText,
    AppComments,
    AppLoader,
    AppButton,
    AppForm
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
