<template>
  <div>
    <br />
    <img :src="item.image" class="room-img" style="cursor: pointer" @click="$emit('openModal', item.id)" />
    <h4>{{ item.title }}</h4>
    <p>{{ item.price }}</p>
  </div>
</template>
<!-- style="cursor: pointer" @click="modalHandler(item.id)" -->
<!-- prop로 보낸 데이터는 수정 금지
그대로 그냥 쓰기만 해야된다. read-only -->
<script>
export default {
  name: "card-list",
  components: {},
  props: {
    item: Object,
    index: Number,
  },
  methods: {
    //   sendDate(){
    //       this.$emit('openModal',this.item.id)
    //   }
    // 위에 클릭을 위의 함수로 호출해도 된다.
  },
};
</script>

<style></style>
