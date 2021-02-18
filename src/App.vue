<template>
  <div class="grid">
    <div v-for="column in columns"
         @drop="drop"
         @dragover="allowDrop"
         class="grid-column">
      <h5>{{ column.name }}</h5>
      <div v-for="(card, index) in column.cards"
           class="card"
           :id="`${column.name}_card_${index}`"
           draggable="true"
           @dragstart="drag"
           @click="clickCard(column, card, index)">
        <h5>{{ card.title }}</h5>
        {{ card.id }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App',
  setup() {
    function clickCard() {

    }

    function drop(event: DragEvent) {
      event.preventDefault();

      const data = event.dataTransfer!.getData('text');
      console.info(data);
      console.info(event.target);

      event.target!.appendChild(document.getElementById(data));
    }

    function allowDrop(event: Event) {
      const el = (event.target! as Element);

      const card = document.createElement('div');
      card.style.width = '200px';
      card.style.height = '59px';
      card.style.backgroundColor = 'red';

      el.appendChild(card)

      event.preventDefault();
    }

    function drag(event: DragEvent) {
      const el = (event.target! as Element);

      event.dataTransfer!.setData('text', el.id);

      console.info(el.clientHeight);
      console.info(el.clientWidth);
    }

    return {
      clickCard,
      drop,
      allowDrop,
      drag,
      columns: [
        {
          name: 'Todo',
          cards: [
            {
              title: 'upgrade eventpublisher',
              id: 'PD-1234',
              index: 1
            },
            {
              title: 'MS: AllizoInvoiceRenderedEventHandler ',
              id: 'PD-9876',
              index: 2
            }
          ]
        },
        {
          name: 'Code review',
          cards: []
        }
      ]
    }
  }
})
</script>

<style lang="scss">
.grid {
  display: flex;

  .grid-column {
    width: 200px;
    background-color: blue;

    padding: 10px;
    margin-right: 10px;

    &:last-child {
      margin: 0;
    }

    h5 {

    }

    .card {
      background-color: red;
      cursor: pointer;
    }
  }
}
</style>
