<template>
  <div class="report">
    <h1>实验报告</h1>
    <el-divider></el-divider>
    <div class="info">
      班级:
      <el-input v-model="class" style="width: 50px" ></el-input>
      &nbsp;&nbsp; 姓名:
      <el-input v-model="name" style="width: 100px"></el-input>
      &nbsp;&nbsp;学号:
      <el-input v-model="id" style="width: 120px"></el-input>
    </div>
    <el-divider></el-divider>

    <el-table :data="tableData" style="width: 100%" ref="table">

      <el-table-column prop="column1" label=""></el-table-column>

      <el-table-column prop="column2" label="">
        <template #default="{row}">
          <el-input v-if="row.column1=='工作量单价$'" v-model="row.column2" ></el-input>
        </template>
      </el-table-column>

      <el-table-column prop="column3" label="">
        <template #default="{row}">
          <el-input v-if="row.column3 != '子项内容'&&row.column3 != '设备原价'&&
          row.column3 != '维护费'&&row.column3 != '工具实际使用时间'&&row.column3 != '租借工具成本'
          &&row.column3 != '软件测试人工工作量TW'&&row.column3 != '产品说明评审SR'
          &&row.column3 != '用户文档集评审DR'" v-model="row.column3" ></el-input>
        </template>
      </el-table-column>

      <el-table-column prop="column4" label="">
        <template #default="{row}">
          <el-input v-if="row.column4!='计数'&&row.column4!='采用IFPUG计算'" v-model="row.column4" ></el-input>
        </template>
      </el-table-column>

    </el-table>

    <el-button class="download-btn" type="primary" @click="downloadPDF">下载报告</el-button>
    <el-button class="download-btn" type="success" @click="clearAll">清空数据</el-button>

  </div>
</template>

<script>
  import jsPDF from "jspdf";
  import html2canvas from "html2canvas";
  export default {
    data() {
      return {
        class: '',
        name: '',
        id: '',
        tableData: [
          {
            column1: '直接成本',
            column2: '测试环境成本',
            column3: '子项内容',
            column4: '计数'
          },
          {
            column1: '',
            column2: '测试工具成本',
            column3: '设备原价',
            column4: ''
          },
          {
            column1: '',
            column2: ' ',
            column3: '维护费',
            column4: ''
          },
          {
            column1: '',
            column2: ' ',
            column3: '工具实际使用时间',
            column4: ''
          },
          {
            column1: '',
            column2: ' ',
            column3: '租借工具成本',
            column4: ''
          },
          {
            column1: '',
            column2: '测试人工成本',
            column3: '软件测试人工工作量TW',
            column4: '采用IFPUG计算'
          },
          {
            column1: '',
            column2: ' ',
            column3: '产品说明评审SR',
            column4: ''
          },
          {
            column1: '',
            column2: ' ',
            column3: '用户文档集评审DR',
            column4: ''
          },
          {
            column1: '间接成本',
            column2: '办公成本',
            column3: '',
            column4: ''
          },
          {
            column1: '',
            column2: '管理成本',
            column3: '',
            column4: ''
          },
          {
            column1: '测试成本调整因子',
            column2: '软件复杂性调整因子C',
            column3: '',
            column4: ''
          },
          {
            column1: '',
            column2: '软件完整性调整因子I',
            column3: '',
            column4: ''
          },
          {
            column1: '',
            column2: '测试风险调整因子R',
            column3: '',
            column4: ''
          },
          {
            column1: '',
            column2: '回归测试调整因子Tr',
            column3: '',
            column4: ''
          },
          {
            column1: '',
            column2: '现场测试调整因子X',
            column3: '',
            column4: ''
          },
          {
            column1: '',
            column2: '加急测试调整因子U',
            column3: '',
            column4: ''
          },
          {
            column1: '',
            column2: '评测机构资质调整因子A',
            column3: '',
            column4: ''
          },
          {
            column1: '',
            column2: '回归测试次数n',
            column3: '',
            column4: ''
          },
          {
            column1: '工作量单价$',
            column2: '',
            column3: '',
            column4: ''
          },
        ],
      };
    },
    methods: {
      async downloadPDF() {
        const pdf = new jsPDF();
        const table = this.$refs.table.$el;
        const canvas = await html2canvas(table);
        const imgData = canvas.toDataURL('image/png');
        pdf.addImage(imgData, 'PNG', 0, 0, 210, 0);
        pdf.save('实验报告.pdf');
      },
      clearAll(){
        console.log("666")
        this.tableData.forEach(item => {
          if(item.column4!='计数'&&item.column4!='采用IFPUG计算')
            item.column4=''
          if(item.column1=='工作量单价$'){
            item.column2=''
            item.column3=''
          }
          if(item.column2!=''&&item.column2!='测试环境成本'&&item.column2!='测试工具成本'
                  &&item.column2!='测试人工成本'&&item.column3!='产品说明评审SR'
                  &&item.column3!='用户文档集评审DR')
            item.column3=''
        });
      }
    }
  };
</script>

<style scoped>
  .report {
    margin: 20px auto;
    max-width: 800px;
    padding: 20px;
  }

  .info {
    font-size: 16px;
    margin-bottom: 20px;
  }

  .item {
    display: flex;
    align-items: center;
  }

  .item span {
    margin-right: 10px;
  }

  .download-btn {
    margin-top: 20px;
  }
</style>
