<template>
  <div class="footer_todo">
    <dl @click="changeCurrentViewAll()">
      <dt>All</dt>
      <dd v-text="all"></dd>
    </dl>
    <dl @click="changeCurrentViewActive()">
      <dt>Active</dt>
      <dd v-text="total.active"></dd>
    </dl>
    <dl @click="changeCurrentViewDone()">
      <dt>Done</dt>
      <dd v-text="total.done"></dd>
    </dl>
    <button class="btn-remove_done" v-show="show_remove_done" @click="removeDone()">clear done</button>
  </div>
</template>

<script>
export default {
  name: 'TodoFooter',
  props: {
    total: { type: Object, default: () => {} },
    all: { type: Number },
    changeCurrentView: { type: Function, default: () => {} },
    removeDoneList: { type: Function, default: () => {} },
  },
  methods: {
    changeCurrentViewAll() {
      this.changeCurrentView('all');
    },
    changeCurrentViewActive() {
      this.changeCurrentView('active');
    },
    changeCurrentViewDone() {
      this.changeCurrentView('done');
    },
    removeDone(){
      this.removeDoneList();
    }
  },
  computed: {
    show_remove_done: function() {
      return this.total.done > 0;
    }
  }
}
</script>

<style>
.footer_todo {overflow:hidden}
.footer_todo dl {float:left;margin:0 15px 0 0;color:#666}
.footer_todo dl:hover {color:#000;cursor:pointer}
.footer_todo dl dt {float:left;width:55px;text-align:left}
.footer_todo dl dd {float:left;width:20px;margin:0}
.footer_todo .btn-remove_done {float:right;width:100px;margin:0}
</style>