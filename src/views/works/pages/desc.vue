<template>
  <div class="works_desc_container">
    <div class="content_container">
      <PublicTitle title="队伍名称"></PublicTitle>
      <div class="team_info">
        <span>{{pageData.teamName}}</span>
      </div>
    </div>
    <!-- 队伍成员 -->
    <div class="content_container">
       <PublicTitle title="队伍成员"></PublicTitle>
       <!-- 队伍成员 -->
      <div v-if="pageData" class="member_contain">
        <div v-for="(item, index) in pageData.teamMembers" :key="index" class="member_item">
          <div class="member_name">
            <i class="iconfont icon-shouhuoren"></i>
            {{item.name}}
          </div>
          <div class="item_contain">
            <p class="item_detail">
              <span class="title">手机号：</span>
              <span class="detail">{{item.phone}}</span>
            </p>
            <p class="item_detail">
              <span class="title">邮箱：</span>
              <span class="detail">{{item.email}}</span>
            </p>
            <p class="item_detail">
              <span class="title">学校：</span>
              <span class="detail">{{item.school}}</span>
            </p>
            <p class="item_detail">
              <span class="title">专业：</span>
              <span class="detail">{{item.profession}}</span>
            </p>
          </div>
        </div>
      </div>
    </div>
    <!-- 作品附件 -->
    <div class="content_container">
      <PublicTitle title="作品附件"></PublicTitle>
      <div class="content_main">
        <!-- <div class="content_item file_item">
          <p>作品说明书</p>
          <div v-show="item.attachmentType === 0" v-for="(item, index) in pageData.attachments" :key="index" class="work_name">
            <el-tooltip class="item" effect="dark" :content="item.attachmentFileName" placement="top-start">
              <span @click="getFileDown(item.attachmentId)" class="work_item"><i @click.stop="prevewFile(item.attachmentId, item.attachmentFileName)" class="el-icon-view"></i>{{item.attachmentFileName}}</span>
            </el-tooltip>
          </div>
        </div>
        <div class="content_item file_item">
          <p>作品介绍</p>
          <div v-show="item.attachmentType === 1" v-for="(item, index) in pageData.attachments" :key="index" class="work_name">
            <el-tooltip class="item" effect="dark" :content="item.attachmentFileName" placement="top-start">
              <span @click="getFileDown(item.attachmentId)" class="work_item"><i @click.stop="prevewFile(item.attachmentId, item.attachmentFileName)" class="el-icon-view"></i>{{item.attachmentFileName}}</span>
            </el-tooltip>
          </div>
        </div> -->
        <div class="content_item file_item">
          <p class="">附件</p>
          <div v-show="item.attachmentType === 2" v-for="(item, index) in pageData.attachments" :key="index" class="work_name">
            <el-tooltip class="item" effect="dark" :content="item.attachmentFileName" placement="top-start">
              <span @click="getFileDown(item.attachmentId)" class="work_item"><i @click.stop="prevewFile(item.attachmentId, item.attachmentFileName)" class="el-icon-view"></i>{{item.attachmentFileName}}</span>
            </el-tooltip>
          </div>
        </div>
      </div>
    </div>
    <div class="content_container">
      <PublicTitle title="评分"></PublicTitle>
      <div class="score_main">
        <div class="score_item">
          <div class="source_left">
            <!-- <span class="source_title">方案完整性:</span>
            <span class="source_sub_title">方案结构完整，切题准确，提供明确的方案解决思路，无明显错误<span class="red_color">(满分15分)</span></span> -->
            <span>维度1（满分5分）</span>
          </div>
          <div class="source_right">
            <!-- <el-input @blur="calcScore" type="number" v-model="integrity" placeholder="方案完整性"></el-input> -->
            <el-slider @change="calcScore" :min="0" :max="5" v-model="integrity" :format-tooltip="formatTooltip"></el-slider>
          </div>
        </div>
        <div class="score_item">
          <div class="source_left">
            <!-- <span class="source_title">数据有效性:</span>
            <span class="source_sub_title">数据信息准确有效，对方案设计有支撑作用<span class="red_color">(满分15分)</span></span> -->
            <span>维度2（满分10分）</span>
          </div>
          <div class="source_right">
            <!-- <el-input @blur="calcScore" type="number" v-model="effect" placeholder="数据有效性"></el-input> -->
            <el-slider @change="calcScore" :min="0" :max="10" v-model="effect" :format-tooltip="formatTooltip"></el-slider>
          </div>
        </div>
        <div class="score_item">
          <div class="source_left">
            <!-- <span class="source_title">方案设计科学性:</span>
            <span class="source_sub_title">有明确、适用的设计方法，采用了定性和定量手段,设计方法科学、严谨<span class="red_color">(满分20分)</span></span> -->
            <span>维度3（满分15分）</span>
          </div>
          <div class="source_right">
            <!-- <el-input @blur="calcScore" type="number" v-model="scientificity" placeholder="方案设计科学性"></el-input> -->
            <el-slider @change="calcScore" :min="0" :max="15" v-model="scientificity" :format-tooltip="formatTooltip"></el-slider>
          </div>
        </div>
        <div class="score_item">
          <div class="source_left">
            <!-- <span class="source_title">方案实用性:</span>
            <span class="source_sub_title">方案依据充分，联系实际情况，可落地实施<span class="red_color">(满分25分)</span></span> -->
            <span>维度4（满分25分）</span>
          </div>
          <div class="source_right">
            <!-- <el-input @blur="calcScore" type="number" v-model="practicability" placeholder="方案实用性"></el-input> -->
            <el-slider @change="calcScore" :min="0" :max="25" v-model="practicability" :format-tooltip="formatTooltip"></el-slider>
          </div>
        </div>
        <div class="score_item">
          <div class="source_left">
            <!-- <span class="source_title">方案创新性:</span>
            <span class="source_sub_title">方案存在创新理念，亮点突出<span class="red_color">(满分25分)</span></span> -->
            <span>维度5（满分30分）</span>
          </div>
          <div class="source_right">
            <!-- <el-input v-model="innovation" @blur="calcScore" placeholder="方案创新性"></el-input> -->
            <el-slider @change="calcScore" :min="0" :max="30" v-model="innovation" :format-tooltip="formatTooltip"></el-slider>
          </div>
        </div>
        <div class="score_item">
          <div class="source_left">
            <div class="pass_item">
              <el-radio v-model="radio" label="1">通过</el-radio>
              <el-radio v-model="radio" label="2">不通过</el-radio>
            </div>
          </div>
          <div class="source_right">
            <div class="tips_box">
              <div class="tips">总分</div><span class="nubmer">{{score}}</span>
            </div>
          </div>
        </div>
        <!-- <div class="score_item">
          <div class="source_left">评语</div>
          <div class="source_right text_area">
            <el-input maxlength="200" @blur="calcScore" :rows="3" v-model="comments" type="textarea" placeholder="评语"></el-input>
          </div>
        </div>
        <div class="score_item">
          <div class="source_left"></div>
          <div class="source_right">
            <el-slider v-model="value" :format-tooltip="formatTooltip"></el-slider>
          </div>
        </div> -->
      </div>
    </div>
    <!-- <div class="pass_item">
      <el-radio v-model="radio" label="1">通过</el-radio>
      <el-radio v-model="radio" label="2">不通过</el-radio>
    </div> -->
    <div class="pass_contain">
      <PublicTitle title="评语"></PublicTitle>
      <div class="comment_box">
        <el-input maxlength="200" style="width:412px;" @blur="calcScore" :rows="3" v-model="comments" type="textarea" placeholder="评语"></el-input>
        <div class="submit_btn_container">
          <el-button @click="$router.go(-1)">返回</el-button>
          <el-button type="primary" @click="submit">提交</el-button>
        </div>
      </div>
    </div>
    <!-- 推荐级别 -->
    <!-- <div class="content_container">
      <PublicTitle title="推荐级别"></PublicTitle>
      <div class="radio_Container">
        <el-radio-group v-model="submitForm.level">
          <div class="radio_item"><el-radio :label="0">否决</el-radio></div>
          <div class="radio_item"><el-radio :label="1">一般推荐</el-radio></div>
          <div class="radio_item"><el-radio :label="2">强烈推荐</el-radio></div>
          <div class="radio_item"><el-radio :label="3">取消推荐</el-radio></div>
        </el-radio-group>
      </div>
    </div> -->
    <!-- <div class="comment_container">
      <el-input placeholder="评语"
        type="textarea"
        :autosize="{
          minRows: 2,
          maxRows: 6
        }"></el-input>
    </div> -->
    <!-- <div class="submit_btn_container">
      <el-button type="primary" @click="submit">提交</el-button>
      <el-button @click="$router.go(-1)">返回</el-button>
    </div> -->
  </div>
</template>

<script>
import { mapActions } from 'vuex'
import PublicTitle from '@/components/public_title.vue'
export default {
  components: {
    PublicTitle
  },
  data () {
    return {
      // 页面数据
      pageData: '',
      submitForm: {
        level: ''
      },
      // 创新性
      integrity: 0,
      // 实用性
      effect: 0,
      // 安全性
      scientificity: 0,
      // 通用性
      practicability: 0,
      // 价值
      innovation: 0,
      // 评语
      comments: '',
      // 总分
      score: 0,
      // 通过
      radio: null,
      value: 0
    }
  },

  created () {
    this.getTeamDetail()
  },
  methods: {
    ...mapActions(['PREVIEW_JUDGE_DOWN_FILE', 'GET_JUDGE_TEAM_DETAIL', 'GET_JUDGE_DOWN_FILE', 'POST_JUDGE_SCORE']),
    formatTooltip (val) {
      return val
    },
    calcScore () {
      let all = 0
      const reg = /(^[0-9]{1,3}$)|(^[0-9]{1,2}[.]{1}[0-9]{1,2}$)/
      reg.test(this.integrity) ? (all = this.integrity) : all = 0
      reg.test(this.effect) && (all += (this.effect))
      reg.test(this.scientificity) && (all += (this.scientificity))
      reg.test(this.practicability) && (all += (this.practicability))
      reg.test(this.innovation) && (all += (this.innovation))
      this.score = all
    },
    async prevewFile (attachmentId, file) {
      const patternFileExtension = /.([0-9a-z]+)(?:[?#]|$)/i
      const fileExtension = (file).match(patternFileExtension)
      attachmentId = attachmentId + fileExtension[0]
      await this.PREVIEW_JUDGE_DOWN_FILE(attachmentId)
    },
    // 下载文件
    async getFileDown (attachmentId) {
      await this.GET_JUDGE_DOWN_FILE(attachmentId)
    },
    // 获取队伍详情
    async getTeamDetail () {
      const res = await this.GET_JUDGE_TEAM_DETAIL({
        teamNo: this.$route.query.teamNo,
        teamProgress: this.$route.query.teamProgress
      })
      // const res = { result: '0', msg: 'Success', data: { teamNo: '0003', teamName: '汤臣一品', progress: null, comments: null, totalScore: null, teamMembers: [{ teamNo: '0003', teamMemberId: 3, name: '杨泽林', phone: '17395545631', email: '2627244069@qq.com', school: '宁夏工商职业技术学院', profession: '金融管理', grade: '大二', leaveMessege: null }, { teamNo: '0003', teamMemberId: 66, name: '崔兴刚', phone: '18792197141', email: '1839753305@qq.com', school: '中国石油大学（华东）', profession: '机械类', grade: '大一', leaveMessege: '我是来自中国石油大学的学生，申请加入队伍，希望通过比赛提升自己。绝不划水。' }, { teamNo: '0003', teamMemberId: 84, name: '杜文超', phone: '18739634180', email: 'dwcdwc2020@163.com', school: '宁夏大学', profession: '电子信息类', grade: null, leaveMessege: '共创辉煌' }, { teamNo: '0003', teamMemberId: 87, name: '冯宇轩', phone: '19122142280', email: 'iacfyx@qq.com', school: '重庆城市科技学院', profession: '建筑工程技术', grade: null, leaveMessege: '一起努力拿大奖哈哈哈' }, { teamNo: '0003', teamMemberId: 174, name: '马金虎', phone: '13259502975', email: '3304546576@qq.com', school: '宁夏大学', profession: '电子信息工程', grade: null, leaveMessege: 'qq3304546576' }, { teamNo: '0003', teamMemberId: 451, name: '彭湘怡', phone: '18281937051', email: '1981206006@qq.com', school: '成都理工大学', profession: '投资学', grade: null, leaveMessege: '态度认真' }], scoreDimensions: [], attachments: [{ attachmentId: 504, attachmentType: 2, attachmentFileName: '汤臣一品   智慧家庭-未来家居.docx' }] } }
      if (res.result === '0' && res.data) {
        this.comments = res.data.comments
        this.score = res.data.totalScore / 100 || 0
        res.data.scoreDimensions.map(item => {
          this[item.dimensionCode] = item.score / 100
        })
        this.pageData = res.data
      }
      console.log(res, '队伍详情')
    },
    open () {
      this.$confirm('是否确认提交, 提交后将不可更改', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.submit()
      }).catch(() => {})
    },
    // 提交
    async submit () {
      // if (!this.integrity) {
      //   this.$message.error('请填写方案完整性评分')
      //   return
      // }
      // if (!this.effect) {
      //   this.$message.error('请填写数据有效性')
      //   return
      // }
      // if (!this.scientificity) {
      //   this.$message.error('请填写方案设计科学性')
      //   return
      // }
      // if (!this.practicability) {
      //   this.$message.error('请填写方案实用性')
      //   return
      // }
      // if (!this.innovation) {
      //   this.$message.error('请填写方案创新性')
      //   return
      // }
      if (!this.radio) {
        this.$message.error('请选择判定结果')
        return
      }
      const arr = ['integrity', 'effect', 'scientificity', 'practicability', 'innovation']
      const objArr = []
      let obj = {}
      for (let i = 0; i < arr.length; i++) {
        obj = {}
        obj.dimensionCode = arr[i]
        obj.score = this[arr[i]] * 100
        objArr.push(obj)
      }
      let progress = Number(this.$route.query.teamProgress)
      if (Number(this.radio) === 1) progress < 2 && ++progress
      if (Number(this.radio) === 2 && progress !== 0) --progress
      const params = {
        comments: this.comments,
        teamNo: this.$route.query.teamNo,
        totalScore: this.score * 100,
        scoreDimensions: objArr,
        teamProgress: progress
      }
      const res = await this.POST_JUDGE_SCORE(params)
      if (res.result === '0' && res.data) {
        this.$message.success(res.msg)
        this.$router.push('list')
      }
      console.log(res, '评分结果')
      // this.$router.push('/works/list')
    },
    // 预览文件
    previewFile (file) {
      console.log('file:', file)
    }
  }
}
</script>

<style lang="scss" scoped>
.works_desc_container {
  .el-icon-view {
    margin-right: 5px;

    color: #dc1e32;
   &:hover {
      color: #333;
}
  }
  .submit_btn_container {
    text-align: right;
  }
  .red_color {
    color: #dc1e32;
  }
  .title {
    margin-bottom: 10px;
    font-size: 16px;
    font-weight: bold;
  }
  .content_container {
    padding-bottom: 20px;
    .team_info {
      margin-top: 20px;

      font-size: 16px;
      font-weight: 600;
    }
  }
  .content_main {
    display: flex;
    padding-top: 32px;
    .content_item {
      margin-bottom: 20px;
      margin-right: 150px;
      p {
        width: 170px;
        height: 32px;
        line-height: 32px;
        background: #48B7FF;
        border-radius: 14px;
        color: #fff;
        text-align: center;
        font-size: 16px;
        font-weight: bold;
      }
      .blank {
        background: #fff;
        color: #333333;
      }
      &.file_item {
        color: #333;
      }
      .work_name {
        display: flex;
        align-items: center;
        color: #dc1e32;
        margin-top: 5px;
        .work_item {
          display: inline-block;
          width: 70%;
          font-size: 16px;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          cursor: pointer;
          color: #FF0000;
        }
        .el-icon-download {
          margin-right: 10px;
          cursor: pointer;
        }
      }
    }
  }
  .score_main {
    padding-top: 36px;
    .score_item {
      display: flex;
      justify-content: space-between;
      margin-bottom: 30px;
      font-size: 18px;
      font-weight: 400;
      color: #333333;
      .source_left {
        width: 70%;
      }
      .source_right {
        width: 30%;
        .tips_box {
          width: 100%;
          display: flex;
          align-items: center;
          .tips {
            width: 119px;
            height: 32px;
            text-align: center;
            line-height: 32px;
            background: #48B7FF;
            border-radius: 6px;
            color: #fff;
          }
          .nubmer {
            font-size: 36px;
            font-weight: bold;
            color: #48B7FF;
            padding-left: 10px;
          }
        }
      }
    }
  }
  .radio_Container {
    .radio_item {
      margin-bottom: 10px;
    }
  }
  .comment_container {
    margin: 20px 0;
  }
  .pass_contain {
    .comment_box {
      display: flex;
      justify-content: space-between;
      align-items: flex-end;
      margin-top: 36px;
    }
  }
  .pass_item {
    // margin: 20px 0;
  }
  .member_contain {
    display: flex;
    flex-wrap: wrap;
    padding: 0 24px;
    margin-top: -.05rem;
    .member_item {
      position: relative;
      width: 250px;
      margin-top: 20px;
      margin-bottom: 20px;
      overflow: hidden;
      margin-right: 20px;

      background: #fff;
      border-radius: 10px;
      transition: all .2s linear;
      box-shadow: 0px 0px 9px 0px rgba(51, 51, 51, 0.25);
      // &:nth-child(3n + 1) {
      //   margin-right: 0;
      // }
      &:hover {
        box-shadow: 0 15px 30px rgba(0,0,0,.1);
        transform: translate3d(0,-2px,0);
      }
      .member_name {
        height: 57px;
        padding-top: 18px;
        text-align: center;
        font-size: 14px;
        font-weight: 400;
        color: #333;
        i {
          margin-right: 10px;
          color: #333;
        }
      }
      .item_contain {
        padding: 0 20px;

        .item_detail {
          padding-bottom: 17px;
          font-size: 14px;
          color: #333;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }
      }
      .btn_contain {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 48px;

        background: #dc1e32;
        font-size: 18px;
        font-weight: bold;
        color: #FFFFFF;
        cursor: pointer;
      }
    }
  }
}
</style>

<style lang="scss">
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
input[type="number"]{
  -moz-appearance: textfield;
}
.text_area {
  width: 300px;
}
</style>
