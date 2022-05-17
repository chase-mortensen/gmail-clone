<template>
  <div class="email-display">
    <div>
      <button @click="toggleArchive">{{  email.archived ? 'Move to Inbox (e)' : 'Archive (e)' }}</button>
      <button @click="toggleRead">{{  email.read ? 'Mark Unread (r)' : 'Mark Read (r)' }}</button>
      <button @click="goNewer">Newer (j)</button>
      <button @click="goOlder">Older (k)</button>
    </div>
    <h2 class="mb-0">Subject: <strong>{{ email.subject }}</strong></h2>
    <p><em>From {{ email.from }} on {{ format(new Date(email.sentAt), 'MMM do yyyy') }}</em></p>
    <div v-html="marked(email.body)" />
  </div>
</template>

<script>
import { format } from 'date-fns'
import marked from 'marked'
import useKeydown from '../composables/use-keydown.js'

export default {
  setup(props, { emit }) {
    let toggleRead = () => { emit('changeEmail', { toggleRead: true, save: true })}
    let toggleArchive = () => { emit('changeEmail', { toggleArchive: true, save: true, closeModal: true })}
    let goNewer = () => { emit('changeEmail', {changeIndex: -1 })}
    let goOlder = () => { emit('changeEmail', {changeIndex: 1 })}
    let goNewerAndArchive = () => { emit('changeEmail', {changeIndex: -1, toggleArchive: true, save: true })}
    let goOlderAndArchive = () => { emit('changeEmail', {changeIndex: 1, toggleArchive: true, save: true })}

    useKeydown([
      { key: 'r', fn: toggleRead },
      { key: 'e', fn: toggleArchive },
      { key: 'j', fn: goNewer },
      { key: 'k', fn: goOlder },
      { key: '[', fn: goNewerAndArchive },
      { key: ']', fn: goOlderAndArchive },
    ])

    return {
      format,
      marked,
      toggleRead,
      toggleArchive,
      goNewer,
      goOlder
    }
  },

  props: {
    email: {
      type: Object,
      required: true,
    }
  }

}
</script>

<style scoped>

</style>