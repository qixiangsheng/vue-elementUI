<template>
  <div class="roles-container">
     <!--面包屑导航-->
    <el-breadcrumb separator="/">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>角色列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card class="box-card">
      <el-button type="primary">添加角色</el-button>
      <!--表格-->
        <el-table
          :data="rolesList"
          border
          style="width: 100%">
           <el-table-column
            type="expand">
            <template slot-scope="scope">
                <!--{{scope.row}}-->
                <!--第一层 循环渲染 scope.row.children 当前渲染的是第一层权限-->
                <el-row v-for="(first, index1) in scope.row.children" :key="first.id" :style="{'border-top':'1px solid #ccc','border-bottom':index1 === scope.row.children.length-1 ? '1px solid #ccc':''}">
                  <!--第一个 col 放的是 一级权限-->
                  <el-col :span="4">
                    <el-tag closable @close="removeTag(scope,first.id)"> {{first.authName}}</el-tag><i class="el-icon-caret-right"></i>
                  </el-col>
                  <!--第二层 放的是 二级权限-->
                  <el-col :span="20">
                    <el-row v-for="(second, index2) in first.children" :key="second.id" :style="{'border-top': index2 === 0 ? '': '1px solid #ccc'}">
                      <el-col :span="4">
                        <el-tag closable type="success" @close="removeTag(scope,second.id)">
                            {{second.authName}}
                        </el-tag><i class="el-icon-caret-right"></i>
                      </el-col>
                      <!--第三层  放的是 三级权限-->
                      <el-col :span="20">
                        <el-tag closable type="warning" v-for="(third) in second.children" :key="third.id" @close="removeTag(scope,third.id)">
                          {{third.authName}}
                        </el-tag>
                      </el-col>
                    </el-row>
                  </el-col>
                </el-row>
            </template>
          </el-table-column>
          <el-table-column
            type="index"
            width="50">
          </el-table-column>
          <el-table-column
            prop="roleName"
            label="角色名称"
            width="180">
          </el-table-column>
          <el-table-column
            prop="roleDesc"
            label="描述">
          </el-table-column>
          <el-table-column
            label="操作"
            width="300">
            <template slot-scope="scope">
              <el-button type="primary" icon="el-icon-edit" size="mini">编辑</el-button>
              <el-button type="danger" icon="el-icon-delete" size="mini">删除</el-button>
              <el-button type="warning" icon="el-icon-setting" size="mini" @click="showSetRoleDialog(scope)">分配权限</el-button>
            </template>
          </el-table-column>
        </el-table>
    </el-card>

    <!--分配权限的对话框-->
    <el-dialog
      title="分配权限"
      :visible.sync="setRoleDialogVisible"
      width="50%"
      @close="setRoleDialogClosed">
      <el-tree
         ref='tree'
        :data="treeData"
        :props="treeProps"
        show-checkbox
        node-key="id"
        :default-expand-all="true"
        :default-checked-keys="defaultCheckedKeys"
        >
      </el-tree>
      <span slot="footer" class="dialog-footer">
        <el-button @click="setRoleDialogClosed">取 消</el-button>
        <el-button type="primary" @click="saveRights">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import mix from './Roles-mixins.js'
export default {
  mixins: [mix]
}
</script>

<style lang="less" scoped>
.el-tag{
  margin:10px 5px;
}
</style>
