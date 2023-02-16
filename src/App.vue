<script>
export default {
  data() {
    return {
      list1: [],
      list2: [],
      selectedItem: null
    };
  },
  methods: {
    selectItem(item) {
      if (this.selectedItem === item) {
        this.selectedItem = null;
      } else {
        this.selectedItem = item;
      }
    },
    addItem(list) {
      list.push({ color: '', text: '' });
      this.validateLists();
    },
    deleteItem(item) {
      if (this.selectedItem === item) {
        this.selectedItem = null;
      }
      const index1 = this.list1.indexOf(item);
      if (index1 !== -1) {
        this.list1.splice(index1, 1);
      }
      const index2 = this.list2.indexOf(item);
      if (index2 !== -1) {
        this.list2.splice(index2, 1);
      }
      this.validateLists();
    },
    moveItem(list1, list2) {
      const item = this.selectedItem;
      const index1 = list1.indexOf(item);
      if (index1 !== -1) {
        list1.splice(index1, 1);
        list2.push(item);
      } else {
        const index2 = list2.indexOf(item);
        if (index2 !== -1) {
          list2.splice(index2, 1);
          list1.push(item);
        }
      }
      this.selectedItem = null;
      this.validateLists();
    },
    copyItem(item) {
      const newItem = { color: item.color, text: item.text };
      this.list1.push(newItem);
      this.validateLists();
    },
    validateLists() {
      if (this.list1.length > 6) {
        this.list1 = this.list1.slice(0, 6);
      }
      if (this.list2.length > 6) {
        this.list2 = this.list2.slice(0, 6);
      }
    }
  },
  watch: {
    selectedItem(newValue, oldValue) {
      if (oldValue && oldValue !== newValue) {
        oldValue.selected = false;
      }
      if (newValue) {
        newValue.selected = true;
      }
    }
  }
};
</script>

<template>
  <div class="container">
    <div class="list">
      <div class="header">List 1</div>
      <div class="item"
           v-for="(item, index) in list1"
           :key="index"
           :class="{ selected: selectedItem === item }"
           :style="{ backgroundColor: item.color }"
           @click="selectItem(item)">
        <input type="text" v-model="item.color">
        <button @click="deleteItem(item)">x</button>
      </div>
      <button @click="addItem(list1)" :disabled="list1.length >= 6">+</button>
    </div>
    <div class="list">
      <div class="header">List 2</div>
      <div class="item"
           v-for="(item, index) in list2"
           :key="index"
           :class="{ selected: selectedItem === item }"
           :style="{ backgroundColor: item.color }"
           @click="selectItem(item)">
        <input type="text" v-model="item.color">
        <button @click="deleteItem(item)">x</button>
      </div>
      <button @click="addItem(list2)" :disabled="list2.length >= 6">+</button>
    </div>
    <div class="controls">
      <button @click="moveItem(list1, list2)" :disabled="!selectedItem || list2.length >= 6">Move</button>
      <button @click="deleteItem(selectedItem)" :disabled="!selectedItem">Delete</button>
      <button @click="copyItem(selectedItem)" :disabled="!selectedItem || list1.length + list2.length >= 12">Copy</button>
    </div>
  </div>
</template>


<style>
.container {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

.list {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.item {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: 200px;
  margin: 5px;
  border: 1px solid black;
}

.selected {
  border: 2px solid green;
}

.controls {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 20px;
}

button {
  margin: 5px;
}
</style>
