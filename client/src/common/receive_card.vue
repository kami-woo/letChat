<template>
  <transition
    enter-active-class="animated bounceIn"
    leave-active-class="animated bounceOut"
  >
    <div class="card-wrap" ref='card' v-show="re_cardInfo.isShow">
      <div class="card-hd" name="hd">
        系统消息
        <span class="iconfont" @click="handleHiddenReCard">&#xe69a;</span>
      </div>
      <div class="card-bd">
        <div class="info">
          <!-- <img class="img" :src="re_cardInfo.imgUrl">
          <span class="name">{{ re_cardInfo.name }}</span> -->
          <img class="img" :src="re_cardInfo.imgUrl">
          <div class="content">
            <span class="name">{{ re_cardInfo.name }}</span>
            <span class="account"> ( {{ re_cardInfo.account }} ) </span><br>
            <span class="addition">附加消息：{{ re_cardInfo.addition }}</span>
            <div class="btn">
              <Button class="deny" @click="handleHiddenReCard">拒绝</Button>
              <Button class="accept" type="primary" @click="handleAcceptFriend">接受</Button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import drag from '../utils/drag.js'
import { mapState, mapMutations } from 'vuex'

export default {
  name: 're_card',
  data() {
    return {
      msg: ''
    }
  },
  methods: {
    ...mapMutations(['handleHiddenReCard']),
    handleAcceptFriend() {
      this.handleHiddenReCard()

      let list = [this.userInfo.id, this.re_cardInfo.id]
      this.$socket.emit('client_acceptFriend', list)
    }
  },
  computed: mapState(['re_cardInfo', 'userInfo']),
  mounted() {
    drag(document, this.$refs.card)
  }
}
</script>

<style lang="less" scoped>
  .card-wrap {
    position: absolute;
    height: 200px;
    width: 350px;
    left: 300px;
    top: 250px;
    border-radius: 5px;
    background-color: rgb(245, 245, 245);
    box-shadow: -2px 2px 10px #ccc;
    overflow: hidden;

    .card-hd {
      height: 42px;
      line-height: 42px;
      background-color: #333;
      color: white;
      font-size: 14px;
      padding-left: 20px;
      padding-right: 20px;

      .iconfont {
        font-size: 14px;
        float: right;
        cursor: pointer;
      }
    }

    .card-bd {
      padding: 20px;

      .info {
        display: flex;

        .img {
          height: 100px;
          width: 100px;
          border-radius: 50px;
        }

        .content {
          margin-left: 30px;

          .name {
            font-size: 20px;
          }

          .account {
            font-size: 10px;
          }

          .btn {
            margin-top: 20px;

            .accept {
              margin-left: 30px;
            }
          }
        }
      }
    }
  }
</style>