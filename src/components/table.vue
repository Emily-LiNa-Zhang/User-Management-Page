<template>
  <div class="btnTable">
    <!-- 折叠面板 -->
    <div class="btnAndFold" :style="{ height: blockHeight }">
      <!-- 上面的按钮 -->
      <div class="buttons">
        <el-row class="btns">
          <el-button
            type="primary"
            icon="el-icon-document-add"
            size="small"
            class="btn"
            >创建用户</el-button
          >
          <el-button icon="el-icon-upload2" size="small" class="btn"
            >批量导入</el-button
          >
          <el-button icon="el-icon-download" size="small" class="btn"
            >下载模板</el-button
          >
          <el-button icon="el-icon-download" size="small" class="btn"
            >批量导出</el-button
          >
          <el-button size="small" class="btn"
            >批量管理
            <li class="el-icon-arrow-down" style="color: black"></li
          ></el-button>
          <!-- 旁边的折叠面板按钮 -->
          <el-button
            size="small"
            @click="moreDown"
            class="foldBtn"
            autofocus:false
            >筛选查询<i :class="caseClass"></i
          ></el-button>
        </el-row>
      </div>
      <!-- 下面的筛选框 -->
      <div class="searchs">
        <el-row class="search">
          <el-col class="blank"> </el-col>
          <el-col class="sch">
            <!-- 帐号状态 -->
            <el-select
              v-model="selectValue1"
              clearable
              placeholder="请选择账号状态"
              size="small"
            >
              <el-option
                v-for="item in options1"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
              </el-option>
            </el-select>
            <!-- 账号角色 -->
            <el-select
              v-model="selectValue2"
              clearable
              collapse-tags
              style="margin-left: 20px"
              placeholder="请选择账号角色"
              size="small"
            >
              <el-option
                v-for="item in options2"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
              </el-option>
            </el-select>
            <!-- 搜索框 -->
            <el-input
              type="search"
              v-model="search"
              placeholder="搜索用户姓名/账号名称"
              suffix-icon="el-icon-search"
              class="searchName"
              size="small"
            ></el-input>
          </el-col>
        </el-row>
      </div>
    </div>

    <!-- 表格 -->
    <!-- <div class="table"></div> -->
    <div class="table">
      <el-table :data="tables" style="width: 98%" max-height="500">
        <!-- 多选框 -->
        <el-table-column type="selection"> </el-table-column>
        <!-- 用户姓名 -->
        <el-table-column label="用户姓名">
          <template slot-scope="scope">
            <span class="col-cont" v-html="showDate(scope.row.name)"></span>
          </template>
        </el-table-column>
        <!-- 账号名称 -->
        <el-table-column label="账号名称" show-overflow-tooltip>
          <template slot-scope="scope">
            <span class="col-cont" v-html="showDate(scope.row.account)"></span>
          </template>
        </el-table-column>
        <!-- 编号 -->
        <el-table-column label="编号" show-overflow-tooltip>
          <template slot-scope="scope">
            <span class="col-cont" v-html="showDate(scope.row.number)"></span>
          </template>
        </el-table-column>
        <!-- 账号角色 -->
        <el-table-column
          label="账号角色"
          show-overflow-tooltip
          column-key="role"
        >
          <template slot-scope="scope">
            <span class="col-cont" v-html="showDate(scope.row.role)"></span>
          </template>
        </el-table-column>
        <!-- 账号状态 -->
        <el-table-column label="账号状态" column-key="tag">
          <template slot-scope="scope">
            <el-tag :type="scope.row.state === '启用' ? 'success' : 'warning'">
              {{ scope.row.state }}
            </el-tag>
          </template>
        </el-table-column>
        <!-- 账号过期时间 -->
        <el-table-column label="有效时间" show-overflow-tooltip width="95">
          <template slot-scope="scope">
            <span
              class="col-cont"
              v-html="showDate(scope.row.effectiveTime)"
            ></span>
          </template>
        </el-table-column>
        <!-- 操作 -->
        <el-table-column fixed="right" label="操作" min-width="150%">
          <template slot-scope="scope">
            <el-link type="primary" :underline="false" @click="edit(scope)"
              >编辑</el-link
            >
            <el-link
              :type="scope.row.state === '启用' ? 'warning' : 'success'"
              :underline="false"
              @click="changState(scope)"
              >{{ scope.row.state === "启用" ? "禁用" : "启用" }}</el-link
            >
            <el-link type="primary" :underline="false">更多操作</el-link>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      moreValue: 0,
      blockHeight: 50,
      caseClass: "",
      search: "",
      state: "",
      tableData: [
        {
          name: "钱大",
          account: "aa",
          number: "USR-1",
          role: "超级管理员",
          state: "启用",
          effectiveTime: "永久有效",
        },
        {
          name: "孙二",
          account: "bb",
          number: "USR-2",
          role: "中心管理员",
          state: "启用",
          effectiveTime: "永久有效",
        },
        {
          name: "张三",
          account: "cc",
          number: "USR-3",
          role: "中心管理员",
          state: "启用",
          effectiveTime: "永久有效",
        },
        {
          name: "李四",
          account: "dd",
          number: "USR-4",
          role: "中心管理员",
          state: "禁用",
          effectiveTime: "永久有效",
        },
        {
          name: "王五",
          account: "ee",
          number: "USR-5",
          role: "综合演练靶场管理员",
          state: "启用",
          effectiveTime: "永久有效",
        },
        {
          name: "赵六",
          account: "ff",
          number: "USR-6",
          role: "中心管理员",
          state: "启用",
          effectiveTime: "永久有效",
        },
        {
          name: "周七",
          account: "gg",
          number: "USR-7",
          role: "综合演练靶场管理员",
          state: "启用",
          effectiveTime: "永久有效",
        },
        {
          name: "吴八",
          account: "hh",
          number: "USR-8",
          role: "中心管理员",
          state: "启用",
          effectiveTime: "永久有效",
        },
        {
          name: "郑九",
          account: "ii",
          number: "USR-9",
          role: "综合演练靶场管理员",
          state: "启用",
          effectiveTime: "永久有效",
        },
        {
          name: "杨十",
          account: "jj",
          number: "USR-10",
          role: "中心管理员",
          state: "启用",
          effectiveTime: "永久有效",
        },
      ],
      options1: [
        {
          value: "启用",
          label: "启用",
        },
        {
          value: "禁用",
          label: "禁用",
        },
      ],
      options2: [
        {
          value: "中心管理员",
          label: "中心管理员",
        },
        {
          value: "超级管理员",
          label: "超级管理员",
        },
        {
          value: "综合演练靶场管理员",
          label: "综合演练靶场管理员",
        },
      ],
      selectValue1: "",
      selectValue2: "",
    };
  },
  components: {},
  computed: {
    // 实时监听表格
    tables: function () {
      const search = this.search;
      if (search) {
        return this.tableData.filter((dataNews) => {
          return Object.keys(dataNews).some((key) => {
            return String(dataNews[key]).toLowerCase().indexOf(search) > -1;
          });
        });
      }
      return this.tableData;
    },
  },
  mounted() {},
  methods: {
    // 筛选变色
    showDate(val) {
      val = val + "";
      if (val.indexOf(this.search) !== -1 && this.search !== "") {
        return val.replace(
          this.search,
          this.search
          // '<font color="#409EFF">' + this.search + "</font>"
        );
      } else {
        return val;
      }
    },
    moreDown() {
      this.moreValue++;
      if (this.moreValue % 2 != 1) {
        this.blockHeight = "110px";
        this.caseClass = "el-icon-arrow-down";
      } else {
        this.blockHeight = "50px";
        this.caseClass = "el-icon-arrow-up";
      }
    },
    changState(scope) {
      let item = this.tableData[scope.$index];
      let order = item["state"] === "启用" ? "禁用" : "启用";
      this.$confirm(`确定要将此项${order}吗？`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        dangerouslyUseHTMLString: true,
        type: "warning",
      })
        .then(() => {
          this.$message({
            type: "success",
            message: "操作成功!",
          });
          if (item["state"] === "启用") {
            this.$set(item, "state", "禁用");
          } else {
            this.$set(item, "state", "启用");
          }
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消操作",
          });
        });
    },
    edit(scope) {
      console.log("编辑输出");
    },
    moreActions() {
      console.log("更多操作。。。");
    },
  },
};
</script>
<style scoped lang="scss">
.btnTable {
  .btnAndFold {
    overflow: hidden;
    .btns {
      display: flex;
      .btn {
        flex: 6;
      }
      .foldBtn {
        flex: 1;
        margin-left: 34px;
      }
      .foldBtn {
        background: #fff;
        border-color: #cbcbcd;
        color: #505255;
      }
      .foldBtn :hover {
        background: #ecf5ff;
        border-color: #c6efff;
        color: #40a9ff;
      }
    }

    .searchs {
      margin-top: 25px;
      margin-bottom: 15px;
      .search {
        display: flex;
        justify-content: space-between;
        .blank {
          flex: 1.1;
        }
        .sch {
          flex: 3.9;
          display: flex;
          justify-content: space-between;
          .el-select {
            width: 140px;
          }
          .searchName {
            width: 195px;
            margin-left: 20px;
          }
        }
      }
    }
  }
  .table {
    height: 500px;
    border: 1px solid rgb(215, 215, 215);
    border-radius: 5px;
    padding: 10px;
    flex: 1;
    position: relative;
    .el-table {
      position: absolute;
      font-size: 13px;
    }
    .el-link {
      margin-right: 5px;
      font-size: 13px;
    }
    /* 去掉表格下面多出来的一小节 */
    .el-table::before {
      /* 将高度修改为0 */
      height: 0;
    }
  }
}
</style>