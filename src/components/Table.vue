<template>
  <div>
    <a-input-search
      v-model="search"
      placeholder="搜索 name"
      @search="handleSearch"
    />
    <a-input-search
      v-model="search2"
      placeholder="搜索 function"
      @search="handleSearch2"
    />
    <a-table
      :columns="columns"
      :data-source="data"
      :rowKey="(record, index) => record.id"
      :row-selection="rowSelection"
      :expanded-row-keys.sync="expandedRowKeys"
      childrenColumnName="child_menu"
    >
      <template slot="func" slot-scope="text, record">
        <a-checkbox-group @change="onChange" v-model="record.bind_list">
          <div v-for="f in record.function_list" :key="f.id">
            <a-checkbox :value="f.id" :key="f.id">{{f.name}}</a-checkbox>
          </div>
        </a-checkbox-group>
      </template>
    </a-table>
  </div>
</template>
<script>
const columns = [
  {
    title: 'Name',
    dataIndex: 'name',
    key: 'name',
  },
  {
    title: 'Function List',
    dataIndex: 'function_list',
    key: 'function_list',
    scopedSlots: { customRender: 'func'}
    // customRender: (text, record) => (
    //   <a-menu>
    //     <a-menu-item>
    //       <a-icon type="edit" />
    //       Edit
    //     </a-menu-item>
    //     <a-menu-item>
    //       <a-icon type="delete" />
    //       Delete
    //     </a-menu-item>
    //   </a-menu>
    // ),
  },
  
];

const data = [
    {
        "id": 12,
        "name": "name1",
        "child_menu": [
            {
                "id": 130,
                "name": "name2",
                "child_menu": [
            {
                "id": 131,
                "name": "name2",
                "bind_list": [
                    1,
                    2
                ],
                "function_list": [
                    {
                        "id": 1,
                        "name": "编辑"
                    },
                    {
                        "id": 2,
                        "name": "删除"
                    }
                ]
            },
            {
                "id": 101,
                "name": "name3",
                "bind_list": [
                    3
                ],
                "function_list": [
                    {
                        "id": 3,
                        "name": "编辑"
                    },
                    {
                        "id": 4,
                        "name": "删除"
                    }
                ]
            }
        ]
            },
            {
                "id": 102,
                "name": "name3",
                "bind_list": [
                    3
                ],
                "function_list": [
                    {
                        "id": 3,
                        "name": "编辑"
                    },
                    {
                        "id": 4,
                        "name": "删除"
                    }
                ]
            }
        ]
    },
    {
        "id": 13,
        "name": "1111",
        "bind_list": [
            1,
            2
        ],
        "function_list": [
            {
                "id": 1,
                "name": "编辑1"
            },
            {
                "id": 2,
                "name": "删除1"
            }
        ]
    },
    {
        "id": 14,
        "name": "2222",
        "bind_list": [
            2
        ],
        "function_list": [
            {
                "id": 1,
                "name": "编辑2"
            },
            {
                "id": 2,
                "name": "删除2"
            }
        ]
    },
    {
        "id": 15,
        "name": "3333",
        "bind_list": [
            1
        ],
        "function_list": [
            {
                "id": 1,
                "name": "编辑3"
            },
            {
                "id": 2,
                "name": "删除3"
            }
        ]
    }
]

const rowSelection = {
  onChange: (selectedRowKeys, selectedRows) => {
    console.log(`selectedRowKeys: ${selectedRowKeys}`, 'selectedRows: ', selectedRows);
  },
  onSelect: (record, selected, selectedRows) => {
    console.log(record, selected, selectedRows);
  },
  onSelectAll: (selected, selectedRows, changeRows) => {
    console.log(selected, selectedRows, changeRows);
  },
};

export default {
  data() {
    return {
      data,
      columns,
      rowSelection,
      expandedRowKeys: [],
      search: '',
      search2: '',
      dataBackup: null
    };
  },
  methods: {
    
    handleSearch() {
      if (this.dataBackup) {
        this.data = JSON.parse(this.dataBackup)
      } else {
        this.dataBackup = JSON.stringify(this.data)
      }

      const f = (d) => {
        return d.filter(item => {
          if (item.child_menu && item.child_menu.length > 0) {
            item.child_menu = f(item.child_menu)
          }
          if (item.child_menu && item.child_menu.length > 0) {
            return true
          }
          return item.name.indexOf(this.search) > -1
        });
      }
      this.data = f(this.data)
    },
    handleSearch2() {
      if (this.dataBackup) {
        this.data = JSON.parse(this.dataBackup)
      } else {
        this.dataBackup = JSON.stringify(this.data)
      }
      // const w = (d) => {
      //   d.forEach(item => {
      //     if (item.child_menu && item.child_menu.length > 0) {
      //       w(item.child_menu)
      //       return
      //     }
          
      //     if (item.function_list) {
      //       item.function_list = item.function_list.filter(f => {
      //         return f.name.indexOf(this.search2) > -1
      //       });
      //     }
      //   });
      // }
      const w = (d) => {
        return d.filter(item => {
          if (item.child_menu && item.child_menu.length > 0) {
            item.child_menu = w(item.child_menu)
            return item.child_menu.length > 0
          }
          
          if (item.function_list) {
            item.function_list = item.function_list.filter(f => {
              return f.name.indexOf(this.search2) > -1
            });
          }
          return item.function_list && item.function_list.length > 0
        });
      }
      this.data = w(this.data)
    },
    onChange() {}
  }
};
</script>
