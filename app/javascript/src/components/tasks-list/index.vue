<template>
  <section class='tasks-list'>
    <h3>{{ title }}</h3>
    <Container
      @drop='onTaskDrop'
      group-name='column'
      :get-child-payload='getChildPayload'
      :drop-placeholder='dropPlaceholderOptions'
    >
      <Draggable v-for='task in tasks' :key='task.id'>
        <div class='task'>
          {{ task.name }}
        </div>
      </Draggable>
    </Container>
  </section>
</template>

<script>
  import { Container, Draggable } from 'vue-smooth-dnd'
  import { UPDATE_TASK } from '../../store/actions.type'
  export default {
    props: ['title', 'tasks'],
    components: {
      Container,
      Draggable
    },
    data: () => {
      return {
        dropPlaceholderOptions: {
          className: 'drop-preview',
          animationDuration: '150',
          showOnTop: true
        }
      }
    },
    methods: {
      onTaskDrop(dropResult) {
        const { removedIndex, addedIndex, payload } = dropResult

        if (removedIndex !== null || addedIndex !== null) {
          const { status } = this.$attrs
          this.$store.dispatch(UPDATE_TASK, {
            task: payload,
            status: status,
          })
        }
      },
      getChildPayload(index) {
        return this.tasks[index]
      }
    }
  }
</script>
