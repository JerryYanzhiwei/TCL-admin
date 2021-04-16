<template>
  <div class="works_list_container">
    <!-- <div class="page_title">评委审核功能</div> -->
    <div v-if="userInfo.superAdmin === 1" class="down_list">
      <el-button @click="downloadAccount" type="primary">下载注册列表<i class="el-icon-download el-icon--right"></i></el-button>
      <el-button @click="downloadTeamList" type="primary">下载队伍列表<i class="el-icon-download el-icon--right"></i></el-button>
      <div style="marginTop: 20px">
        <span>评委姓名: </span>
        <el-input v-model="judgeUserName" style="width: 20%; marginRight: 10px" size="mini"/>
        <span>邮箱地址: </span>
        <el-input v-model="emailAddress" style="width: 20%; marginRight: 10px" size="mini"/>
        <el-button @click="sendEmail" type="primary">发送提醒邮件<i class="el-icon-s-promotion el-icon--right"></i></el-button>
      </div>
    </div>
    <div class="filter_contain">
      <p>
        <span>编号：</span>
        <el-input v-model="pageForm.teamNo" size="mini"></el-input>
      </p>
      <!-- <p>
        <span>方向：</span>
        <el-select size="mini" v-model="pageForm.directionId" placeholder="请选择">
          <el-option
            v-for="item in directArr"
            :key="item.directionName + '1'"
            :label="item.directionName"
            :value="item.directionId">
          </el-option>
        </el-select>
      </p> -->
      <p>
        <span>课题：</span>
        <el-select size="mini" v-model="pageForm.subjectId" placeholder="请选择">
          <el-option
            v-for="item in subjectArr"
            :key="item.subjectName"
            :label="item.subjectName"
            :value="item.subjectId">
          </el-option>
        </el-select>
      </p>
      <p>
        <span>赛区：</span>
        <el-select size="mini" v-model="pageForm.matchZone" placeholder="请选择">
          <el-option
            v-for="item in provinceData"
            :key="item.code"
            :label="item.matchZone"
            :value="item.code">
          </el-option>
        </el-select>
      </p>
      <p>
        <span>队伍：</span>
        <el-select size="mini" v-model="pageForm.status" placeholder="请选择">
          <el-option
            v-for="item in workOption"
            :key="item.label"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </p>
      <el-button @click="resetFilter" class="res" type="primary">重置</el-button>
      <el-button @click="filterSearch" >搜索</el-button>
    </div>
    <!-- <div class="process_banner">
      <el-tabs v-model="activeType" @tab-click="handleClick">
        <el-tab-pane label="初筛" name="0"></el-tab-pane>
        <el-tab-pane label="半决赛" name="1"></el-tab-pane>
        <el-tab-pane label="决赛" name="2"></el-tab-pane>
      </el-tabs>
    </div> -->
    <el-table
      :data="tableData"
      @selection-change="handleSelectionChange"
      stripe>
      <el-table-column
        type="selection"
        width="55">
      </el-table-column>
      <el-table-column
        prop="teamNo"
        label="队伍编号">
      </el-table-column>
      <el-table-column
        label="队伍名称">
        <template slot-scope="scope">
          <el-popover width="200" trigger="hover" :content="scope.row.teamName" placement="top">
            <div>{{scope.row.teamName}}</div>
            <div slot="reference" class="name-wrapper">
              <div class="clickable nowrap" @click="setSource(scope.row)">{{ scope.row.teamName }}</div>
            </div>
          </el-popover>
        </template>
      </el-table-column>
      <!-- <el-table-column
        label="作品名称">
        <template slot-scope="scope">
          <el-tooltip :content="scope.row.opusName" placement="top">
            <span style="color: #33394E">{{scope.row.opusName}}</span>
          </el-tooltip>
        </template>
      </el-table-column> -->
      <!-- <el-table-column
        prop="directionName"
        label="方向">
      </el-table-column> -->
      <el-table-column
        label="课题">
        <template slot-scope="scope">
          <el-popover width="200" trigger="hover" :content="scope.row.subjectName" placement="top">
            <div>{{scope.row.subjectName}}</div>
            <div slot="reference" class="name-wrapper">
              <div class="nowrap">{{ scope.row.subjectName }}</div>
            </div>
          </el-popover>
        </template>
      </el-table-column>
      <el-table-column
        min-width="40"
        label="赛区">
        <template slot-scope="scope">
          <span>{{getZone(scope.row.matchZone)}}</span>
        </template>
      </el-table-column>
      <el-table-column
        prop=""
        label="作品附件">
        <template slot-scope="scope">
          <div>
            <p v-for="(item, index) in scope.row.attachments" :key="index">
              <el-tooltip :content="item.attachmentFileName" placement="top">
                <span @click="getFileDown(item.attachmentId)"><i @click.stop="prevewFile(item.attachmentId, item.attachmentFileName)" class="el-icon-view"></i>{{item.attachmentFileName}}</span>
              </el-tooltip>
            </p>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        min-width="40"
        label="得分">
        <template slot-scope="scope">
          <span>{{scope.row.totalScore / 100 || '---'}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="评委姓名">
        <template slot-scope="scope">
          <span>{{scope.row.judgeUserName}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="标记状态">
        <template slot-scope="scope">
          <span>{{scope.row.pass === 1 ? '不合格' : '合格'}}</span>
        </template>
      </el-table-column>
      <el-table-column
        prop=""
        label="评审情况">
        <template slot-scope="scope">
          <!-- <PublicButton v-if="activeType !== '2'" @clickHandle="pass(scope.row, 1)">通过</PublicButton>
          <PublicButton v-if="activeType !== '0'" @clickHandle="pass(scope.row, 0)">不通过</PublicButton> -->
          <!-- <span>详情</span> -->
          <div class="operation_contain">
            <span class="clickable" @click="teamPass(scope.row)">合格</span>
            <span class="clickable" @click="teamPass(scope.row)">不合格</span>
            <span class="clickable" v-if="activeType !== '2'" @click="pass(scope.row, 1)">通过</span>
            <span class="clickable" v-if="activeType !== '0'" @click="open(scope.row, 0)">不通过</span>
            <span class="clickable" @click="resetScore(scope.row)">重置</span>
          </div>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="handleSizeExamChange"
      @current-change="pageChange"
      :current-page="pageData.pageNo"
      layout="total, sizes, prev, pager, next"
      :page-size="pageData.pageSize"
      :page-sizes="[10, 30, 50, 100]"
      :total="pageData.recordNumber">
    </el-pagination>

    <el-dialog
      title="提示"
      :visible.sync="dialogVisible"
      width="30%">
      <span v-if="!singleSubject">您已选择不同赛题方向的作品，确认后将进行发送</span>
      <span v-else>您已选择了【{{subjectName}}】方向的作品，确认后将进行发送</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirmSend">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog
      title="提示"
      :visible.sync="dialogReset"
      width="30%">
      <span>是否确认重置该队成绩</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogReset = false">取 消</el-button>
        <el-button type="primary" @click="resetConfirm">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { mapActions } from 'vuex'
// import PublicButton from '@/components/public_button.vue'
import { BASE_URL } from '@/utils/http.js'
import provinceData from '@/config/province.js'
export default {
  components: {
    // PublicButton
  },
  data () {
    return {
      provinceData,
      activeType: '0',
      directArr: [],
      subjectArr: [],
      workOption: [
        {
          label: '有作品的队伍',
          value: 123
        },
        {
          label: '所有队伍',
          value: null
        }
      ],
      pageForm: {
        pageSize: 30,
        pageNo: 1,
        teamProgress: 0,
        directionId: null,
        matchZone: null,
        subjectId: null,
        teamNo: null,
        status: null
      },
      pageData: {},
      tableData: [],
      userInfo: {},
      selectTeam: [],
      dialogVisible: false,
      dialogReset: false,
      curRow: null,
      singleSubject: null,
      emailAddress: '',
      subjectName: '',
      judgeUserName: ''
    }
  },
  watch: {
    $route (val) {
      this.pageForm = {
        pageSize: 30,
        pageNo: 1,
        teamProgress: val.query.type,
        directionId: null,
        matchZone: null,
        subjectId: null,
        teamNo: null
      }
      this.activeType = val.query.type
      console.log(this.pageForm)
      this.getData()
    }
  },
  created () {
    this.userInfo = JSON.parse(localStorage.getItem('adminInfo'))
    this.getData()
    this.getCategory()
  },
  methods: {
    ...mapActions(['PUT_TEAM_TAG', 'PREVIEW_DOWN_FILE', 'PUT_EDIT_PROCESS', 'GET_CATEGORYS', 'GET_TEAM_LIST', 'GET_ACCOUNT_LIST', 'GET_DOWN_FILE', 'PUT_JUDGE_NOTIFY', 'PUT_RESET_SCORE']),
    async getFileDown (attachmentId) {
      await this.GET_DOWN_FILE(attachmentId)
    },
    async downloadAccount () {
      window.open(`${BASE_URL}/admin/account/dowload`)
    },
    async downloadTeamList () {
      window.open(`${BASE_URL}/admin/team/dowload`)
    },

    async prevewFile (attachmentId, file) {
      const patternFileExtension = /.([0-9a-z]+)(?:[?#]|$)/i
      const fileExtension = (file).match(patternFileExtension)
      attachmentId = attachmentId + fileExtension[0]
      await this.PREVIEW_DOWN_FILE(attachmentId)
    },
    // 分页个数
    handleSizeExamChange (val) {
      this.pageForm.pageSize = val
      this.getData()
    },
    // 搜索
    filterSearch () {
      // todo
      this.pageForm.pageNo = 1
      this.getData()
    },
    resetFilter () {
      this.pageForm.directionId = null
      this.pageForm.matchZone = null
      this.pageForm.subjectId = null
      this.pageForm.teamNo = null
      this.pageForm.status = null
    },
    // 获取类目
    async getCategory () {
      const res = await this.GET_CATEGORYS()
      if (res.result === '0' && res.data) {
        this.directArr = res.data
        res.data.map(item => {
          this.subjectArr = this.subjectArr.concat(item.subjects)
        })
      }
    },
    // 跳转详情
    setSource (row) {
      this.$router.push({
        path: '/works/adminDesc',
        query: {
          teamNo: row.teamNo,
          teamProgress: this.activeType
        }
      })
    },
    // 切换分页
    pageChange (data) {
      this.pageForm.pageNo = data
      this.getData()
    },
    // 切换赛事类型
    handleClick (e) {
      this.pageForm = {
        pageSize: 30,
        pageNo: 1,
        teamProgress: e.name,
        directionId: null,
        matchZone: null,
        subjectId: null,
        status: null,
        teamNo: null
      }
      this.activeType = e.name
      this.getData()
    },
    open (row, status) {
      this.$confirm('确认后将回退到上一个评审阶段', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.pass(row, status)
      }).catch(() => {})
    },

    // 标记队伍是否合格
    async teamPass (row) {
      const res = await this.PUT_TEAM_TAG({
        pass: row.pass === 1 ? 0 : 1,
        teamNo: row.teamNo
      })
      this.getData()
      this.$message.success(res.msg)
    },
    // 通过
    async pass (row, status) {
      const teamProgress = status ? Number(this.activeType) + 1 : Number(this.activeType) - 1
      const res = await this.PUT_EDIT_PROCESS({
        teamProgress: teamProgress,
        teamNo: row.teamNo
      })
      if (res.result === '0' && res.data) {
        this.getData()
        this.$message.success(res.msg)
      }
    },
    // 获取页面数据
    async getData () {
      const params = this.pageForm
      const res = await this.GET_TEAM_LIST(params)
      // const res = { result: '0', msg: 'Success', data: { pageSize: 10, pageNo: 1, total: 22, recordNumber: 213, records: [{ teamNo: '0002', teamName: '江湖战队', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '大数据挖掘和算法-电影CTR预测', matchZone: '1003', provinceCode: '14', totalScore: 3000, attachments: [{ attachmentId: 500, attachmentType: 2, attachmentFileName: 'README.md' }] }, { teamNo: '0003', teamName: '汤臣一品', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '智慧家庭-未来场景', matchZone: '1004', provinceCode: '22', totalScore: null, attachments: [{ attachmentId: 504, attachmentType: 2, attachmentFileName: '汤臣一品   智慧家庭-未来家居.docx' }] }, { teamNo: '0004', teamName: '实事求是', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '智慧家庭-未来场景', matchZone: '1004', provinceCode: '18', totalScore: null, attachments: [{ attachmentId: 489, attachmentType: 2, attachmentFileName: '0004实事求是队-项目计划书.pdf' }] }, { teamNo: '0005', teamName: 'Starlink', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '大数据挖掘和算法-电影CTR预测', matchZone: '1001', provinceCode: '1', totalScore: null, attachments: [] }, { teamNo: '0006', teamName: '守住发际线队', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '智慧家庭-未来场景', matchZone: '1001', provinceCode: '1', totalScore: null, attachments: [{ attachmentId: 367, attachmentType: 2, attachmentFileName: '守住发际线队  红外室内定位.doc' }] }, { teamNo: '0007', teamName: '三只小猪', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '智慧家庭-未来场景', matchZone: '1001', provinceCode: '1', totalScore: null, attachments: [{ attachmentId: 317, attachmentType: 2, attachmentFileName: '三只小猪基于IoT的节能自发电智能主卧系统项目计划书.docx' }] }, { teamNo: '0008', teamName: '还有这么个', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '智慧家庭-未来场景', matchZone: '1001', provinceCode: '1', totalScore: null, attachments: [{ attachmentId: 156, attachmentType: 2, attachmentFileName: '项目计划书.pdf' }] }, { teamNo: '0009', teamName: '花园宝宝', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '大数据挖掘和算法-电影CTR预测', matchZone: '1001', provinceCode: '1', totalScore: null, attachments: [{ attachmentId: 497, attachmentType: 2, attachmentFileName: '花园宝宝+基于深度学习的电影 CTR 预测.pdf' }] }, { teamNo: '0010', teamName: '底层学渣在线找虐', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '大数据挖掘和算法-电影CTR预测', matchZone: '1006', provinceCode: '28', totalScore: null, attachments: [] }, { teamNo: '0011', teamName: 'RUSH_B', opusName: null, categoryName: 'xx1', directionName: 'xxx1', subjectName: '大数据挖掘和算法-电影CTR预测', matchZone: '1006', provinceCode: '28', totalScore: null, attachments: [{ attachmentId: 309, attachmentType: 2, attachmentFileName: 'RUSH_B+CTR预测.zip' }] }], startIndex: 1, prePage: 1, nextPage: 2 } }
      if (res.result === '0' && res.data) {
        console.log(res.data.records)
        this.pageData = res.data
        this.tableData = res.data.records
      }
    },

    // 多选队伍
    handleSelectionChange (val) {
      this.selectTeam = val
    },

    // 发送右键
    sendEmail () {
      if (!this.selectTeam.length) {
        this.$message.error('请选择要发送提醒邮件的队伍')
        return
      }
      if (!this.emailAddress) {
        this.$message.error('请输入邮箱地址')
        return
      }
      if (!this.judgeUserName) {
        this.$message.error('请输入评委姓名')
        return
      }
      const len = this.selectTeam.length
      const direct = this.selectTeam[0].subjectName
      const arr = this.selectTeam.filter(item => item.subjectName === direct)
      this.singleSubject = arr.length === len
      this.subjectName = this.selectTeam[0].subjectName
      this.dialogVisible = true
    },

    async confirmSend () {
      const teamArr = []
      this.selectTeam.map(item => {
        teamArr.push(item.teamNo)
      })
      const res = await this.PUT_JUDGE_NOTIFY({
        email: this.emailAddress,
        teamProgress: this.activeType,
        judgeUserName: this.judgeUserName,
        teamNo: teamArr
      })
      this.$message.success(res.msg)
      this.dialogVisible = false
    },

    // 重置成绩
    resetScore (row) {
      this.curRow = row
      this.dialogReset = true
    },

    // 确认重置
    async resetConfirm () {
      const res = await this.PUT_RESET_SCORE({
        teamProgress: this.activeType,
        teamNo: this.curRow.teamNo
      })
      this.$message.success(res.msg)
      this.dialogReset = false
      this.getData()
    }

  }
}
</script>

<style lang="scss" scoped>
.works_list_container {
  .operation_contain {
    display: flex;
    align-items: center;
    flex-direction: column;
  }
  .filter_contain{
    display: flex;
    margin-bottom: 30px;
    p {
      width: 20%;
      color: #333333;
      font-size: 16px;
    }
  }
  .clickable {
    font-size: 14px;
    color: #409eff;
    cursor: pointer;
  }
  .down_list {
    margin-bottom: 20px;
  }
  .el-icon-view {
    margin-right: 5px;

    color: #dc1e32;
   &:hover {
      color: #333;
}
  }
}
</style>

<style lang="scss">
  .nowrap {
    width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .el-table .cell {
    white-space: nowrap;
  }
  .el-pagination {
    margin-top: 20px;
    text-align: right;
  }
  span.el-tooltip {
    width: 100%;
    overflow: hidden;
    display: inline-block;
    text-overflow: ellipsis;
    white-space: nowrap;
    cursor: pointer;
    color: #409EFF;
  }
  .filter_contain {
    p {
      .el-input--mini, .el-select--mini {
        width: 70%;
        .el-input--mini {
          width: 100%;
        }
      }
    }
  }
</style>
