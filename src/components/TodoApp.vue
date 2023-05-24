<template>
  <div class="container">
    <h2 class="text-center mt-5 ">TODO APP</h2>
    <!-- input -->
    <div class="d-flex">
      <input v-model="text" type="text" placeholder="Entertext" class="form-control">
      <button @click="submittext" class="btn btn-warning rounded-0">Submit</button>
    </div>
    <!-- text table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Text </th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(text, index) in texts " :key="index">
          <td>
            <span :class="{ 'finished': text.status === 'finished' }">{{text.name }}</span>
          </td>
          <td style="width:120px">
            <span @click="changestatus(index)" class="pointer" :class="{
              'text-danger': text.status === 'to-do',
              'text-warning': text.status === 'in-progress',
              'text-success': text.status === 'finished'
            }">

              {{ firstCharUpper(text.status) }}
            </span>
          </td>
          <td>
            <div class="text-center" @click="edittext(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="Deletetext(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data() {
    return {
      text: '',
      editedtext: null,

      availablestatuses: ['to-do', 'in-progress', 'finished'],
      texts: [
        {
          name: 'He is going to mall.',
          status: 'to-do'
        },
        {
          name: 'he is a good guy',
          status: 'in-progress'
        }
      ]
    }
  },
  methods: {
    submittext() {
      if (this.text.length === 0) return;
      if (this.editedtext === null) {
        this.texts.push({
          name: this.text,
          status: 'to-do'
        });
      } else {
        this.texts[this.editedtext].name = this.text;
        this.editedtext = null;
      }
      this.text = '';
    },
    Deletetext(index) {
      this.texts.splice(index, 1);
    },
    edittext(index) {
      this.text = this.texts[index].name;
      this.editedtext = index;
    },
    changestatus(index) {
      let newindex = this.availablestatuses.indexOf(this.texts[index].status);
      if (++newindex > 2) newindex = 0;
      this.texts[index].status = this.availablestatuses[newindex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  }
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}
</style>
