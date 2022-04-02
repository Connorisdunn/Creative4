<template>
<div class="admin">
    <h1 class="main_header">Lets add some recipes!</h1>
    <div class="heading">
      <div class="circle">+</div>
      <h2>Add a Recipe</h2>
    </div>
    <div class="add">
      <div class="form">
        <input v-model="title" placeholder="Name" class="name">
        <p></p>
        <input type="file" name="photo" @change="fileChanged">
        <p></p>
        <textarea v-model="description" cols=50 rows=4 placeholder="Instructions" class ="instructions"></textarea>
        <button @click="upload">Upload</button>
      </div>
      <div class="upload" v-if="addItem">
        <h2>{{addItem.title}}</h2>
	<p> {{addItem.description}}</p>
        <img :src="addItem.path" />
      </div>
    </div>

    <div class="heading">
      <div class="circle">-</div>
      <h2>Change/Delete a Recipe</h2>
    </div>
    <div class="edit">
      <div class="form">
        <input v-model="findTitle" placeholder="Search" class="search">
        <div class="suggestions" v-if="suggestions.length > 0">
          <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}
          </div>
        </div>
      </div>
      <div class="upload" v-if="findItem">
        <input v-model="findItem.title" class="name">
        <p></p>
        <img :src="findItem.path" />
        <p></p>
        <textarea v-model="findItem.description" cols=50 rows=4 placeholder="Instructions" class="instructions"></textarea>
      </div>
      <div class="actions" v-if="findItem">
        <button @click="deleteItem(findItem)">Delete</button>
        <button @click="editItem(findItem)">Change</button>
      </div>
    </div>
</div>
</template>

<style scoped>
.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}

.circle {
  width: 23px;
  height: 23px;
  padding: 8px;
  background: #ffbd5b;
  color: #000417;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #edca79;
  color: #fff;
}
.main_header{
  text-align: center;
  text-decoration: underline;
}


.name{
  color: #edca79;
}
.name:hover{
  border: 3px solid #edca79;
  transform: rotate(5deg);
}

.instructions{
  color: #edca79;
}
.instructions:hover{
  border: 3px solid #edca79;
  transform: rotate(5deg);
}

.search{
  color: #edca79;
}
.search:hover{
  border: 3px solid #edca79;
  transform: rotate(5deg);
}
</style>

<script>
import axios from 'axios';

export default {
  name: 'Admin',
  data() {
    return {
      title: "",
      file: null,
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
      description: "",
    }
  },
  created() {
    this.getItems();
  },
  computed: {
    suggestions() {
      let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.title > b.title);
    }
  },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
      this.findDescription = "";
    },
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          description: this.findItem.description,
         });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
async getItems() {
  try {
    let response = await axios.get("/api/items");
    this.items = response.data;
    return true;
  } catch (error) {
    console.log(error);
  }
},
    async upload() {
      try {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        let r2 = await axios.post('/api/items', {
          title: this.title,
          path: r1.data.path,
          description: this.description,
        });
        this.addItem = r2.data;
      } catch (error) {
        console.log(error);
      }
    },
  }
}
</script>
