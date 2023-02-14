<template>
  <div class="moadal" @click="closeModal">
    <div class="modal__block" @click.stop="">
      <h2 class="modal__title">
        {{ edit ? 'Изменить заметку' : 'Добавить заметку' }}
      </h2>
      <div class="modal__inputs">
        <label>
          <span>Title</span>
          <input type="text" v-model="title" />
        </label>
        <label>
          <span>Content</span>
          <textarea v-model="descr"></textarea>
        </label>
      </div>
      <div class="modal__btns">
        <button class="modal__btn cancel" @click="closeModal">Отмена</button>
        <button class="modal__btn add" @click="addNotes" v-if="!edit">Добавить</button>
        <button class="modal__btn add" @click="changeNote" v-else>Изменить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      descr: "",
      id: this.currentId,
    };
  },
  props: {
    currentId: Number,
    edit: Boolean,
    editNote: Object
  },
  methods: {
    closeModal() {
      this.$emit("closeModal", false);
      this.title = "";
      this.descr = "";

    },
    addNotes() {
      if (this.title != "" && this.descr != "") {
        const item = {
          id: this.id++,
          title: this.title,
          descr: this.descr,
          date: new Date().toLocaleDateString(),
        };
        this.$emit("addNote", item);
        this.title = "";
        this.descr = "";
      }
    },
    changeNote(){
      if (this.title != '' && this.descr != '') {
        const editNotes = {
          id: this.editNote.id,
          title: this.title,
          descr: this.descr,
          date: new Date().toLocaleString()
        }
        this.$emit('editedNote', editNotes)
        this.closeModal()
      }
    }
  },
};
</script>

<style>
.moadal {
  background: rgba(0, 0, 0, 0.35);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 999;
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal__block {
  background: linear-gradient(
      0deg,
      rgba(103, 80, 164, 0.11),
      rgba(103, 80, 164, 0.11)
    ),
    #fffbfe;
  border-radius: 28px;
  max-width: 312px;
  width: 100%;
  padding: 24px;
}
.modal__title {
  font-size: 24px;
  line-height: 32px;
  color: #1c1b1f;
  margin-bottom: 16px;
}
.modal__inputs {
  font-size: 12px;
  line-height: 16px;
  letter-spacing: 0.4px;
  color: #6750a4;
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-bottom: 24px;
}
.modal__inputs label {
  position: relative;
}
.modal__inputs span {
  position: absolute;
  top: 8px;
  left: 16px;
  font-size: 12px;
  line-height: 16px;
  color: #6750a4;
}
.modal__inputs input,
.modal__inputs textarea {
  background: #e7e0ec;
  border-radius: 4px 4px 0px 0px;
  width: 100%;
  border: none;
  outline: none;
  resize: none;
  padding: 23px 0 9px 16px;
  height: 56px;
  overflow: hidden;
  font-size: 16px;
  line-height: 24px;
  color: #49454f;
  border-bottom: 1px solid #1c1b1f;
}
.modal__btns {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 8px;
}
.modal__btn {
  padding: 10px 12px;
  font-size: 14px;
  line-height: 20px;
  font-family: "RM";
  text-transform: uppercase;
  transition: background 0.3s;
}
.cancel {
  color: #cf1b1b;
  border-radius: 5px;
}
.add {
  color: #6750a4;
  border-radius: 5px;
}
.cancel:hover {
  background: #ffe1e1;
}
.add:hover {
  background: #e6ddff;
}
</style>