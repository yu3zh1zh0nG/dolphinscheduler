/*
 * Licensed to the Apache Software Foundation (ASF) under one or more
 * contributor license agreements.  See the NOTICE file distributed with
 * this work for additional information regarding copyright ownership.
 * The ASF licenses this file to You under the Apache License, Version 2.0
 * (the "License"); you may not use this file except in compliance with
 * the License.  You may obtain a copy of the License at
 *
 *    http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
<template>
  <div class="list-model">
    <div class="table-box">
      <el-table :data="list" size="mini" style="width: 100%" row-class-name="items">
        <el-table-column type="index" :label="$t('#')" width="50"></el-table-column>
        <el-table-column prop="userName" :label="$t('User')" class-name="userName"></el-table-column>
        <el-table-column prop="token" label="Token" width="300" class-name="token"></el-table-column>
        <el-table-column :label="$t('Expiration time')" min-width="120">
          <template slot-scope="scope">
            <span>{{scope.row.expireTime | formatDate}}</span>
          </template>
        </el-table-column>
        <el-table-column :label="$t('Create Time')" min-width="120">
          <template slot-scope="scope">
            <span>{{scope.row.createTime | formatDate}}</span>
          </template>
        </el-table-column>
        <el-table-column :label="$t('Update Time')" min-width="120">
          <template slot-scope="scope">
            <span>{{scope.row.updateTime | formatDate}}</span>
          </template>
        </el-table-column>
        <el-table-column :label="$t('Operation')" width="130">
          <template slot-scope="scope">
            <el-tooltip :content="$t('Edit')" placement="top" :enterable="false">
              <el-button type="primary" size="mini" icon="el-icon-edit-outline" @click="_edit(scope.row)" circle class="edit"></el-button>
            </el-tooltip>
            <el-tooltip :content="$t('Delete')" placement="top" :enterable="false">
              <el-popconfirm
                :confirmButtonText="$t('Confirm')"
                :cancelButtonText="$t('Cancel')"
                icon="el-icon-info"
                iconColor="red"
                :title="$t('Delete?')"
                @onConfirm="_delete(scope.row,scope.row.id)"
              >
                <el-button type="danger" size="mini" icon="el-icon-delete" circle slot="reference" class="delete"></el-button>
              </el-popconfirm>
            </el-tooltip>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>
<script>
  import { mapActions } from 'vuex'

  export default {
    name: 'token-list',
    data () {
      return {
        list: []
      }
    },
    props: {
      tokenList: Array,
      pageNo: Number,
      pageSize: Number
    },
    methods: {
      ...mapActions('user', ['deleteToken']),
      _delete (item, i) {
        this.deleteToken({
          id: item.id
        }).then(res => {
          this.$emit('on-update')
          this.$message.success(res.msg)
        }).catch(e => {
          this.$message.error(e.msg || '')
        })
      },
      _edit (item) {
        this.$emit('on-edit', item)
      }
    },
    watch: {
      tokenList (a) {
        this.list = []
        setTimeout(() => {
          this.list = a
        })
      }
    },
    created () {
      this.list = this.tokenList
    },
    mounted () {
    },
    components: { }
  }
</script>
