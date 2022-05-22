<template>
  <div class="black-bg" v-if="modalStatus">
    <div class="white-bg">
      <h2>{{ items[clickId].title }}</h2>
      <img :src="items[clickId].image" class="room-img" />
      <h3>{{ items[clickId].content }}</h3>
      <p>{{ inputMonth }}개월 선택함 : {{ items[clickId].price }} = {{ items[clickId].price * inputMonth }}</p>
      <!-- <input @input="inputMonth = $event.target.value" /> -->
      <!-- textArea select 등등에도 v-model 사용할 수 있다. 
      input에 입력한 것은 문자로 인식한다. 초기값을 숫자로 해서 곱셈은 인식한다.
      그러나 + 같은 것은 문자+문자로 인식해서 연산이 되지 않는다. 
      number로 인식하고 싶으면 v-model.number로 받는다. -->
      <input v-model="inputMonth" />
      <!-- <DiscountBanner /> -->
      <button @click="$emit('closeModal')">닫기</button>
      <!-- watcher는 데이터 감시할때 쓴다. 사용자의 input데이터가 필요한 경우 대부분 필수로 사용한다. -->
    </div>
  </div>
</template>

<script>
// import DiscountBanner from "./DiscountBanner.vue";

export default {
  name: "modal-page",
  components: {},
  beforeUpdate() {
    if (this.inputMonth == 2) {
      alert("최소 3개월입니다.");
      this.inputMonth = 3;
    }
  },
  watch: {
    inputMonth(a) {
      //함수명을 데이터 명으로 만들어야된다.
      //param을 받으면 inputMonth의 데이터를 받는다. 앞의 변수는 변하는 데이터, 뒤의 변수는 변하기 전의 데이터
      //inputMonth가 변할떄 마다 watch도 실행된다.
      if (a > 12) {
        alert("13이상 입력 불가");
      }
      if (isNaN(a)) {
        alert("숫자만 입력가능합니다.");
        this.inputMonth = 1;
      }

      //input에 입력하면 재렌더링이 되기 때문에 hook을 쓰려면 update를 사용하면된다.
    },
  },
  props: {
    items: Array,
    clickId: Number,
    modalStatus: Boolean,
  },
  data() {
    return {
      inputMonth: 1,
    };
  },
};
</script>

<style></style>
