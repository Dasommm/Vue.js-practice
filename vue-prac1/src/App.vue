<template>
  <!-- <div class="black-bg" v-if="modalStatus">
    <div class="white-bg">
      <h2>{{ items[clickId].title }}</h2>
      <img :src="items[clickId].image" class="room-img" />
      <h3>{{ items[clickId].content }}</h3>
      <p>{{ items[clickId].price }}</p>
      <button @click="modalHandler()">닫기</button>
    </div>
  </div> -->
  <!-- <modal-page :data="items" :clickId="clickId" :modalStatus="modalStatus" /> -->
  <!-- <div class="start" :class="{ end: modalStatus }">
    class 명을 조건부로 넣고싶을떄 
    <modal-page :modalStatus="modalStatus" :items="items" :clickId="clickId" @closeModal="modalHandler()" />
  </div> -->
  <DiscountBanner v-if="showDiscount == true" :discountLevel="discountLevel" />

  <!-- 위와 같지만 더 쉽게 transition으로 감싸서 사용할 수 있다. 작명한 name과 동일하게 css를 만들어야된다. -->
  <transition name="fade">
    <modal-page :modalStatus="modalStatus" :items="items" :clickId="clickId" @closeModal="modalHandler()" />
  </transition>

  <h1 :style="mainTitle">원룸샵</h1>
  <div>
    <nav id="primary_nav_wrap">
      <ul>
        <li v-for="menu in menuArr" :key="menu.id" :class="{ 'current-menu-item': menu.clickable == true }" @click="clickMenuItem(menu)">
          <a href="#">{{ menu.name }}</a>
        </li>
      </ul>
    </nav>
  </div>
  <button @click="sortList()">가격순 정렬</button>
  <button @click="sortBack()">되돌리기</button>

  <!-- <div v-for="(item, index) in items" :key="index">
    <br />
    <img :src="item.image" class="room-img" style="cursor: pointer" @click="modalHandler(item.id)" />
    <h4>{{ item.title }}</h4>
    <p>{{ item.price }}</p>
     <button v-on:click="increase(index)">허위매물 신고</button>
    <br /><span> 신고 횟수 : {{ 신고수[index] }}</span>
  </div> -->
  <card-list v-for="(item, index) in items" :key="index" :item="item" :index="index" @openModal="modalHandler($event)" />
</template>
<!-- $evnet는 자식에서 보낸 데이터 받는 값 -->
<script>
import data from "./assets/oneroom.js";
import DiscountBanner from "./DiscountBanner.vue";
import ModalPage from "./ModalPage.vue";
import CardList from "./CardList.vue";
// import { clearInterval } from "timers";

export default {
  name: "App",
  data() {
    return {
      mainTitle: "color: blue",
      menuArr: [
        { id: 0, name: "Home", clickable: true },
        { id: 1, name: "About", clickable: false },
        { id: 2, name: "Produce", clickable: false },
      ],
      itemArr: [
        { id: 0, name: "서울 원룸", price: 2000, warning: false },
        { id: 1, name: "경기 원룸", price: 5000, warning: false },
        { id: 2, name: "천안 원룸", price: 5050, warning: false },
        { id: 3, name: "마포구 원룸", price: 5100, warning: true },
        { id: 4, name: "일산 원룸", price: 6600, warning: true },
      ],
      신고수: [0, 0, 0, 0, 0],
      modalStatus: false,
      items: data,
      clickId: 0,
      itemsOri: [...data], //copy
      showDiscount: true,
      discountLevel: 30,
    };
  },
  mounted() {
    //mount 되고나서(장착된 이후) 실행할 것..
    //beforeMount()이런 것도 있다.
    //ajax 등 서버와 통신하는 것 같은거 쓸때는 create나 mounted로 쓴다.
    // setTimeout(() => {
    //   //실행할 코드
    //   //this.~쓰려면 arrow function으로 쓴다.
    //   //1초마다 변하는 것은 setInterval 사용한다.(1초마다 안의 코드를 실행시켜준다.)
    //   this.showDiscount = false;
    // }, 2000);
    var discountTimer = setInterval(() => {
      if (this.discountLevel > 0) {
        //0이하로 떨어지면 타이머클리어
        this.discountLevel--;
      } else {
        clearInterval(discountTimer);
        this.showDiscount = false;
      }
    }, 1000);
  },
  components: { ModalPage, CardList, DiscountBanner },
  methods: {
    //함수 만드는 곳
    warningItem: function (item) {
      if (item) return { warningItem: true };
      else return { warningItem: false };
    },
    clickMenuItem: function (item) {
      for (let i = 0; i < this.menuArr.length; i++) {
        this.menuArr[i].clickable = false;
      }
      item.clickable = true;
    },
    increase(index) {
      this.신고수[index]++;
    },
    modalHandler(id) {
      this.modalStatus = !this.modalStatus;
      this.clickId = id;
    },
    sortList() {
      //가격순 정렬
      this.items.sort(function (a, b) {
        return a.price - b.price; //음수면 a를 왼쪽에 보낸다
      });
    },
    sortBack() {
      //되돌리기
      //map, filter,sort 차이 찾아보기
      this.items = [...this.itemsOri];
      //등호를 쓰면 왼쪽 오른쪽 공유해주세요 라는 의미
      //따라서 copy해서 따로 쓴다.
    },
  },
};
</script>

<style>
.fade-enter-from {
  opacity: 0;
}
.fade-enter-active {
  transition: all 1s;
}
.fade-enter-to {
  opacity: 1;
}

/* 나가는css */
/* .fade-leave-from {
  transform: translateY(-1000px);
}
.fade-leave-active {
  transition: all 1s;
}
.fade-leave-to {
  transform: translateY(0px);
} */

/* 

.start {
  opacity: 0;
  transition: all 1s;
}
.end {
  opacity: 1;
} */

.discount {
  background: #eee;
  padding: 10px;
}
body {
  margin: 0;
}
div {
  box-sizing: border-box;
}
.black-bg {
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  padding: 20px;
}
.white-bg {
  width: 100%;
  background: white;
  border-radius: 8px;
  padding: 20px;
}

.room-img {
  width: 100%;
  margin-top: 40px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.item {
  font-family: "Courier New", Courier, monospace;
}

.warningItem {
  color: red;
  font-weight: bold;
}

#primary_nav_wrap {
  margin-top: 15px;
}

#primary_nav_wrap ul {
  list-style: none;
  position: relative;
  float: left;
  margin: 0;
  padding: 0;
}

#primary_nav_wrap ul a {
  display: block;
  color: #333;
  text-decoration: none;
  font-weight: 700;
  font-size: 12px;
  line-height: 32px;
  padding: 0 15px;
  font-family: "HelveticaNeue", "Helvetica Neue", Helvetica, Arial, sans-serif;
}

#primary_nav_wrap ul li {
  position: relative;
  float: left;
  margin: 0;
  padding: 0;
}

#primary_nav_wrap ul li.current-menu-item {
  background: #ddd;
}

#primary_nav_wrap ul li:hover {
  background: #f6f6f6;
}

#primary_nav_wrap ul ul {
  display: none;
  position: absolute;
  top: 100%;
  left: 0;
  background: #fff;
  padding: 0;
}

#primary_nav_wrap ul ul li {
  float: none;
  width: 200px;
}

#primary_nav_wrap ul ul a {
  line-height: 120%;
  padding: 10px 15px;
}

#primary_nav_wrap ul ul ul {
  top: 0;
  left: 100%;
}

#primary_nav_wrap ul li:hover > ul {
  display: block;
}
</style>
