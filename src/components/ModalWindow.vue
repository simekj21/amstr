<template>
  <div 
    tabindex="0"
    @keyup.esc="closeModal()" 
  >
    <div id="myModal" class="modal" >
      <div 
        class="modal-content"
        @keyup="closeModal()"
      >
          <span 
            class="modal-content-close"
            tabindex="0"
            @click="closeModal()"
          >
            &times;
          </span>
          <h2>
            {{ venue.name }}
          </h2>
          <p><i>
            {{ venue.description }}
          </i></p>
          <img 
            class = "modal-content-images"
            v-for="(picture, index) in venue.pictures" :key="index"
            width="200"
            :src="picture.url" 
          >
          <p>
            {{ venue.adress }}<br>
            {{ venue.city }}<br>
            {{ venue.zipcode }}
          </p>
          <p>
            <a :href="venue.URL" target="_BLANK">Web Pages</a>
          </p>  
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ModalWindow',
  props: {
    venue: Object
  },
  methods: {
    closeModal () {
      this.$emit('closeModal')
    }
  }
}
</script>

<style scoped lang="scss">
  a {
    text-decoration: none;
    color: palevioletred;
    cursor: pointer;
  }

  .modal {
    display: block; 
    position: fixed; 
    z-index: 1; 
    padding-top: 400px; 
    left: 0;
    top: 0;
    width: 100%; 
    height: 100%; 
    overflow: auto; 
    background-color: rgba(0,0,0,0.4);

    &-content {
      background-color: #fefefe;
      margin: auto;
      padding: 20px;
      border: 1px solid #888;
      width: 80%;

      &-close {
        color: #aaaaaa;
        float: right;
        font-size: 28px;
        font-weight: bold;
      
        &:hover,
        &:focus {
          color: #000;
          text-decoration: none;
          cursor: pointer;
        }
      }

      &-images {
        margin: 2px;
        cursor: pointer;

        &:hover {
          filter: saturate(1.5)
        }
      }
    }
  }
</style>
