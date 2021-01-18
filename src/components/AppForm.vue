<template>
  <form class="card card-w30" @submit.prevent="addBlock">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="type">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model="blockValue"></textarea>
    </div>

    <app-button
      color="primary"
      :disabled="isDisabled"
      type="submit"
    >Добавить</app-button>
  </form>
</template>

<script>
import AppButton from './AppButton'
export default {
  emits: {
    addBlock (type, value) {
      if (type && value) {
        return true
      }
      console.warn('Invalid addBlock emit params!')
      return false
    }
  },
  data () {
    return {
      type: 'title',
      blockValue: ''
    }
  },
  computed: {
    isDisabled () {
      return !(this.blockValue.length > 3)
    }
  },
  methods: {
    addBlock () {
      this.$emit('addBlock',
        this.type,
        this.blockValue
      )
      this.type = 'title'
      this.blockValue = ''
    }
  },
  components: {
    AppButton
  }
}
</script>

<style scoped>
</style>
