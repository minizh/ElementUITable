<template>
  <div>
    <el-table ref="table" :data="tableData" style="width: 100%" row-key="id" border lazy :load="load" :tree-props="{children: 'childs', hasChildren: 'hasChild'}" @row-click="rowClick" @expand-change="expandChageFn">
      <el-table-column prop="date" label="日期" width="180">
      </el-table-column>
      <el-table-column prop="name" label="姓名" width="180">
        <template slot-scope="scope">
          <el-button type="primary" @click="add(scope.row, $event)">添加</el-button>
        </template>
      </el-table-column>
      <el-table-column prop="address" label="地址">
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  data () {
    return {
      event: {},
      tableData: [{
        id: 1,
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄',
        expanded: false,
        loaded: false
      }, {
        id: 2,
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄',
        expanded: false,
        loaded: false,
      }, {
        id: 3,
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1519 弄',
        expanded: false,
        hasChild: true,
        loaded: false,
      }, {
        id: 4,
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄',
        expanded: false,
        loaded: false,
      }]
    }
  },
  methods: {
    expandChageFn (row, expanded) {
      console.log(row, expanded)
      row.expanded = expanded
      // this.$refs.table.toggleRowExpansion(row, !expanded)
    },
    rowClick (row, column, event) {
      console.log(row, column, event)
    },
    add (row, event) {
      console.log(event)
      const temp = [
        {
          id: new Date().getTime(),
          date: '11111',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄',
          hasChild: true,
          expanded: false,
          loaded: false,
          childs: []
        }
      ]
      if (row.loaded) {
        this.$refs.table.toggleRowExpansion(row)
        return
      }
      setTimeout(() => {
        this.$set(row, 'loaded', true)
        this.$set(row, 'expanded', true)
        this.$set(row, 'hasChild', true)
        this.$set(row, 'childs', temp)
        this.$nextTick(() => {
          this.$refs.table.toggleRowExpansion(row, true)
          // if () {

          // }
          event.target.offsetParent.previousElementSibling.querySelector('.el-table__expand-icon').click()
          this.$refs.table.doLayout()
        })
      }, 400);
    },
    load (tree, treeNode, resolve) {
      if (tree.loaded) {
        resolve(tree.childs)
        return
      }
      setTimeout(() => {
        tree.loaded = true
        resolve([
          {
            id: Math.floor(Math.random() * 1000000),
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1519 弄',
            hasChild: false,
            expanded: false,
            loaded: false
          }, {
            id: Math.floor(Math.random() * 1000000) + new Date().getTime(),
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1519 弄',
            hasChild: true,
            expanded: false,
            loaded: false
          }
        ])
      }, 300)
    }
  },
}
</script>
