<template>
  <div class="board col-md-4 d-flex justify-content-center my-3">
    <div class="boards-card card">
      <div class="d-flex justify-content-center  ">
        <router-link :to="{ name: 'Board', params:{id: boardProp.id} }">
          <div class="icon">
            <i class="fab fa-galactic-republic" style="font-size:300px"></i>
          </div>
        </router-link>
      </div>
      <div class="d-flex justify-content-center">
        <h2>
          {{ boardProp.title }}
        </h2>
      </div>
      <button @click="deleteBoard" class="btnv btn-sm trash">
        <i class="fa fa-trash" aria-hidden="true"></i>
      </button>
    </div>
  </div>
</template>

<script>
import { computed, reactive } from 'vue'
import { AppState } from '../AppState'
import { useRoute } from 'vue-router'
import { boardsService } from '../services/BoardsService'
import Notification from '../utils/Notification'

export default {
  name: 'Board',
  props: {
    boardProp: {
      type: Object,
      required: true
    }
  },
  setup(props) {
    const route = useRoute()
    const state = reactive({
      newBoard: {},
      board: computed(() => AppState.board),
      user: computed(() => AppState.user),
      account: computed(() => AppState.account)
    })
    return {
      route,
      state,
      async deleteBoard() {
        try {
          await boardsService.deleteBoard(props.boardProp.id)
        } catch (error) {
          Notification.toast('Error: ' + error, 'warning')
        }
      }
    }
  },
  components: {}
}
</script>

<style lang="scss" scoped>
.boards-card{
  min-width: 20rem;
  min-height: 20rem;
}

.icon{
    color: #6930c3;
  }
.trash{
  background:#64dfdf;
}
</style>
