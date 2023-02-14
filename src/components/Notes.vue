<template>
  <div class="notes">
    <div class="container">
      <div class="notes__top">
        <h2 class="notes__title">
          {{notes.length > 0 ? words.infobar[lang] : 'Нет заметок'}}
          </h2>
        <button class="notes__btn" @click="grid = !grid">
          <img src="@/assets/img/grid.svg" alt="" v-if="grid" />
          <img src="@/assets/img/list.svg" alt="" v-else/>
          <span>{{grid ? 'Список' : 'Сетка'}}</span>
        </button>
      </div>
      <div class="notes__list" :class="{active: !grid}">
        <NoteItem
        @changeNote="$emit('changeNote', note.id)"
        @delNote="$emit('delNote', note.id)"
        :grid="grid"
        v-for="note in notes" :key="note.id"
        :note="note"
        
        />
      </div>
    </div>
  </div>
  
</template>

<script>
import NoteItem from '@/components/NoteItem.vue';
export default {
  components:{
    NoteItem
  },
  inject: ['words'],
  props:{
    lang: String,
    notes: Array,
    required: true
  },
  data(){
    return{
      grid: true
    }
  }
};
</script>

<style>
.notes {
  margin-top: 30px;
}
.notes__top {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.notes__title {
  font-size: 22px;
  line-height: 28px;
  color: #323232;
}
.notes__btn {
  background: linear-gradient(
      0deg,
      rgba(103, 80, 164, 0.11),
      rgba(103, 80, 164, 0.11)
    ),
    #fffbfe;
  box-shadow: 0px 4px 8px 3px rgba(0, 0, 0, 0.15),
    0px 1px 3px rgba(0, 0, 0, 0.3);
  border-radius: 16px;
  width: 120px;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
}
.notes__btn span {
  font-family: "RM";
  font-size: 14px;
  line-height: 20px;
  color: #6750a4;
  letter-spacing: 0.1px;
}
.notes__list{
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  margin-top: 30px;
}
.notes__list.active{
    grid-template-columns: 1fr;
}
</style>