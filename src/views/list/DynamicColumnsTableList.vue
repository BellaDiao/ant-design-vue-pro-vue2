<template>
<!-- DynamicColumnsTableList  动态列的显示和隐藏  -->
  <a-card :bordered="false">

    <div class="filter-container">
      <a-checkbox-group v-model="checkboxVal">
        <a-checkbox value="name">
          name
        </a-checkbox>
        <a-checkbox value="age">
          age
        </a-checkbox>
        <a-checkbox value="address">
          address
        </a-checkbox>
      </a-checkbox-group>
    </div>
    <a-divider dashed />
    <a-table
      ref="table"
      size="middle"
      bordered
      rowKey="id"
      :columns="columns"
      :dataSource="dataSource"
      :pagination="ipagination"
      :loading="loading"
      :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
      @change="handleTableChange">

    </a-table>
  </a-card>
</template>
<script>
  const defaultFormThead = ['name', 'age']
  /*const columns = [
    {
      title: 'Name',
      dataIndex: 'name',
    },
    {
      title: 'Age',
      dataIndex: 'age',
    },
    {
      title: 'Address',
      dataIndex: 'address',
    },
  ];*/

  const data = [];
  for (let i = 0; i < 46; i++) {
    data.push({
      key: i,
      name: `Edward King ${i}`,
      age: 32,
      address: `London, Park Lane no. ${i}`,
    });
  }
  export default {
    data() {
      return {
        checkboxVal:defaultFormThead,
        formThead: defaultFormThead, // 默认表头 Default header
        key: 1, // table key
        formTheadOptions: ['name', 'age', 'address'],
        columns:[],
        queryParam: {},
        dataSource : data,
        ipagination:{
          current: 1,
          pageSize: 10,
          pageSizeOptions: ['10', '20', '30'],
          showTotal: (total, range) => {
            return range[0] + "-" + range[1] + " 共" + total + "条"
          },
          showQuickJumper: true,
          showSizeChanger: true,
          total: 0
        },
        /* table加载状态 */
        loading:false,
        /* table选中keys*/
        selectedRowKeys: [],
        /* table选中records*/
        selectionRows: [],
        isorter:{
          column: 'createTime',
          order: 'desc',
        },
      };
    },
    watch: {
      //监听用户点击上方的复选框
      checkboxVal(valArr) {
        console.log(valArr);
        this.formThead = this.formTheadOptions.filter(i => valArr.indexOf(i) >= 0)
        this.key = this.key + 1// 为了保证table 每次都会重渲 In order to ensure the table will be re-rendered each time
        this.createColumns();
      }
    },
    created() {
      this.createColumns();
    },
    methods:{
      createColumns(){
        this.columns = [];
        this.formThead.map((val,key)=>{
          let obj = {
              key:key,
              title: val,
              dataIndex: val,
          };
          this.columns.push(obj);
        })
      },
      loadData(arg) {
      },
      onSelectChange(selectedRowKeys, selectionRows) {
        this.selectedRowKeys = selectedRowKeys;
        this.selectionRows = selectionRows;
      },
      handleTableChange(pagination, filters, sorter) {
        //分页、排序、筛选变化时触发
        //TODO 筛选
        if (Object.keys(sorter).length > 0) {
          this.isorter.column = sorter.field;
          this.isorter.order = "ascend" == sorter.order ? "asc" : "desc"
        }
        this.ipagination = pagination;
        this.loadData();
      },
    },
  }
</script>
<style scoped>

</style>
