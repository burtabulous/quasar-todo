<template>
  <q-page class="q-pa-lg">
   <div v-if="!image">
    <h2>Select an image</h2>
    <q-input type="file" @change="onFileChange"/>
  </div>
  <div v-else>
    <img :src="image" />
    <button @click="removeImage">Remove image</button>
  </div>     
  </q-page>
</template>

<script>
export default {
props:{
// image: this.image
},
data(){
  return{
    image: this.get("profile_img")
  }
},
   methods: {
    get(key) {
     
      this.image=localStorage.getItem(key);
    },
    set(key){
      var vm = this
      try{
       
        localStorage.setItem(key,this.image);
      }
      catch(e) {
        console.log( `Storage failed: ${e}`);
      }
    },
    onFileChange(e) {
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length)
        return;
      this.createImage(files[0]);
    },
    createImage(file) {
      var image = new Image();
      var reader = new FileReader();
      var vm = this;
     
      reader.onload = (e) => {
        vm.image = e.target.result;
        vm.set('profile_img');
      };
      reader.readAsDataURL(file);
    },
    removeImage: function (e) {
      this.image = '';
      localStorage.removeItem('profile_img')
    }
  },
  watch:{
    image(newImage, oldImage){
      if(newImage != oldImage && newImage != null && newImage != "undefined"){
        this.$emit(newImage)
      }
    }
  }


}
</script>
