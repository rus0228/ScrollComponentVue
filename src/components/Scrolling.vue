<template>
  <v-container>
    <v-flex class="align-center justify-center mt-10" style="display: flex">
      <v-btn
          class="mx-10 align-center"
          fab
          dark
          color="indigo"
          @click="onOpenMenu"
      >
        <v-icon dark>
          mdi-plus
        </v-icon>
      </v-btn>
    </v-flex>
    <div class="modal" v-if="isOpenMenu">
      <div class="modal-content" @scroll="handleScroll">
        <div class="d-flex justify-end items-end">
          <v-btn
              icon
              color="black"
              @click="onOpenMenu"
          >
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </div>
        <div id="scroll-content">
          <div v-for="(item, i) in list" class="px-5 py-1" :key="i">
            {{ item }}
          </div>
        </div>
      </div>
    </div>
  </v-container>
</template>

<script>
  export default {
    name: 'Scrolling',
    props: {
      items: Array
    },
    watch: {
      items: {
        handler(val) {
          console.log(val)
          this.chunkItems(val)
        },
        deep: true
      }
    },
    data: () => ({
      chunkArray: [],
      isOpenMenu: false,
      list: [],
      index: 0,
    }),
    methods: {
      onOpenMenu() {
        this.isOpenMenu = !this.isOpenMenu;
      },
      chunkItems(items) {
        const chunkArray = [];
        let i,j, chunk = 20;
        for (i = 0,j = items.length; i < j; i += chunk) {
          chunkArray.push(items.slice(i, i + chunk));
        }
        this.chunkArray = chunkArray;
        this.list = chunkArray[0]
        this.index = 0;
      },
      handleScroll(el) {
        if((el.target.offsetHeight + el.target.scrollTop) >= el.target.scrollHeight) {
          console.log('bottom reached')
          if (this.index + 1 < this.chunkArray.length) {
            this.list = [...this.list, ...this.chunkArray[this.index + 1]];
            this.index += 1;
          }
        }
      }
    }
  }
</script>

<style lang="css" scoped="scoped">
  .modal {
    position: fixed;
    background: #00000080;
    left: 0; right: 0; top: 0; bottom: 0;
    z-index: 100;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .modal-content {
    background: white;
    padding: 20px 10px;
    border-radius: 20px;
    width: 300px;
    height: 400px;
    overflow-y: scroll;
  }
</style>
