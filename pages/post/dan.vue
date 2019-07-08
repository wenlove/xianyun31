<template>
  <el-row type="flex" justify="space-between" class="other">
    <div class="publicity">
      <el-col v-for="(v,index) in item.images" :key="index" :span="8">
        <a :href="`/post/detail?id=${item.id}`">
          <img :src="v" alt />
        </a>
      </el-col>
    </div>

    <div class="other-1">
      <h4>
        <a :href="`/post/detail?id=${item.id}`">{{item.title}}</a>
      </h4>

      <div class="content-box">
        <div class="content">
          <a :href="`/post/detail?id=${item.id}`">
            <p v-html="item.summary"></p>
          </a>
        </div>

        <el-row class="userinfo" type="flex" align="middle">
          <div class="location">
            <i class="el-icon-location-outline"></i>
            <span>{{item.cityName}}</span>
          </div>
          <em>by</em>
          <a href="#">
            <img :src="$axios.defaults.baseURL + $store.state.user.userInfo.user.defaultAvatar" alt />
            <span class="user-id">{{item.account.nickname}}</span>
          </a>

          <i class="el-icon-view"></i>
          <span>{{item.watch}}</span>

          <span class="like">0赞</span>
        </el-row>
      </div>
    </div>
  </el-row>
</template>

<script>
export default {
  props: ["item"]
};
</script>

<style scoped lang='less'>
* {
  padding: 0;
  margin: 0;
}

.other {
  height: 170px;
  .other-1 {
    margin-left: 14px;
    h4 {
      height: 24px;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    /deep/ .article {
      margin-bottom: 20px;
    }
  }
}

.left-container {
  width: 260px;
}

.container {
  margin: 0 auto;
  width: 1000px;
  position: relative;
  padding-top: 20px;
}
.recommend {
  width: 260px;
  height: 160px;
  // border-right: #000;
  line-height: 40px;
  z-index: 2;

  p {
    font-size: 14px;
    padding-left: 20px;
    color: #666;
    border: 1px solid #ddd;
    background-color: #fff;
    border-bottom: none;
    z-index: 2;
    position: relative;
    &:hover {
      color: violet;
      border-right-color: #fff;
    }

    &:nth-child(4) {
      border-bottom: 1px solid #ddd;
    }

    span {
      font-size: 20px;
      margin-left: 150px;
    }
  }
}

.left-hiddle {
  width: 350px;
  height: 200px;
  position: absolute;
  top: 20px;
  left: 259px;
  z-index: 1;
  // z-index: -1;
  background-color: #fff;
  border: 1px solid #ddd;

  .introduce {
    padding-left: 15px;
    // background-color: #fff;

    li {
      list-style: none;
      line-height: 38px;
      i {
        font-style: italic;
        font-size: 20px;
        color: orange;
      }
      :nth-child(2) {
        color: orange;
        font-size: 14px;
      }

      span {
        margin-left: 10px;
        &:last-child {
          color: #999;
          font-size: 14px;
        }
        &:hover {
          text-decoration: underline;
        }
      }
    }
  }
}

.recommended {
  padding-top: 30px;

  p {
    font-size: 16px;
  }

  a {
    img {
      display: block;
      width: 260px;
      border-top: 1px solid #ccc;
      padding-top: 10px;
      margin-top: 14px;
    }
  }
}

.right-container {
  width: 700px;
}
.search-box {
  position: relative;
  width: 100%;

  .input-with-select {
    // border: none;
    box-sizing: border-box;
    // border: 3px solid violet;
    height: 40px;
    /deep/.el-input__inner {
      border: 3px solid orange;
    }
  }

  .el-icon-search {
    font-size: 30px;
    color: violet;
    position: absolute;
    top: 5px;
    right: 10px;
    cursor: pointer;
  }
}

.tuijian {
  margin-top: 10px;
  font-size: 12px;
  color: #666;
  margin-bottom: 6px;

  .tuijianer {
    span {
      padding-left: 10px;
    }
  }
}

.strategy {
  height: 50px;
  line-height: 50px;
  border-bottom: 1px solid #eee;

  span {
    border-bottom: 3px solid orange;
    font-size: 18px;
    color: orange;
  }
  .strategy-button {
    height: 40px;
    width: 106px;
  }
}

.article {
  width: 100%;
  margin-top: 20px;
  border-bottom: 1px solid #eee;
  h4 {
    font-weight: normal;
    font-size: 18px;
    margin-bottom: 10px;
    &:hover {
      color: violet;
    }
  }

  .content-box {
    width: 100%;
    height: 290px;
    margin-top: 10px;
  }

  .content {
    height: 60px;
    overflow: hidden;
    text-overflow: ellipsis;
    font-size: 14px;
    color: #666;
    padding-top: 5px;
    margin-bottom: 16px;
  }
}

.publicity {
  width: 100%;
  height: 150px;
  padding-bottom: 16px;
  overflow: hidden;
  a {
    img {
      width: 220px;
      height: 150px;
      object-fit: cover;
    }
  }
}

.userinfo {
  width: 100%;
  height: 26px;
  position: relative;
  text-align: center;
  color: #666;

  .location {
    text-align: center;
  }

  .el-icon-location-outline {
    font-size: 12px;
    color: #666;
    line-height: 26px;
    padding-right: 3px;
  }

  .user-id {
    color: orange;
  }

  span {
    font-size: 12px;
    padding-right: 12px;
  }

  img {
    width: 16px;
    text-align: center;
    vertical-align: middle;
    padding-left: 5px;
  }

  em {
    font-size: 12px;
  }

  .like {
    position: absolute;
    top: 0;
    right: 0;
    font-size: 16px;
    color: orange;
  }
}

.el-icon-view {
  font-size: 12px;
  padding-right: 5px;
}
</style>

