<template>
  <div>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
        <el-breadcrumb-item>内容管理</el-breadcrumb-item>
        <el-breadcrumb-item>文章管理</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <el-link href="/admin/editor" :underline="false" target="_blank" class="add-link">
      <el-button type="success">写文章</el-button>
    </el-link>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="articles"
        stripe
        style="width: 100%"
        :max-height="tableHeight">
        <el-table-column
          type="selection"
          width="55">
        </el-table-column>
        <el-table-column type="expand">
          <template slot-scope="props">
            <el-form label-position="left" inline>
              <el-form-item>
                <span>{{ props.row.articleAbstract }}</span>
              </el-form-item>
            </el-form>
          </template>
        </el-table-column>
        <el-table-column
          prop="articleTitle"
          label="题目（展开查看摘要）"
          fit>
        </el-table-column>
        <el-table-column
          prop="articleDate"
          label="发布日期"
          width="200">
        </el-table-column>
        <el-table-column
          fixed="right"
          label="操作"
          width="180">
          <template slot-scope="scope">
            <el-button
              @click.native.prevent="viewArticle(scope.row.id)"
              type="text"
              size="small">
              查看
            </el-button>
            <el-button
              @click.native.prevent="editArticle(scope.row)"
              type="text"
              size="small">
              编辑
            </el-button>
            <el-button
              @click.native.prevent=""
              type="text"
              size="small">
              移除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
      <div style="margin: 20px 0 20px 0;float: left">
        <el-button>取消选择</el-button>
        <el-button>批量删除</el-button>
      </div>
    </el-card>
  </div>
</template>

<script>
  export default {
    name: 'ArticleManagement',
    data () {
      return {
        articles: []
      }
    },
    mounted () {
      this.loadArticles()
    },
    computed: {
      tableHeight () {
        return window.innerHeight - 320
      }
    },
    methods: {
      loadArticles () {
        var _this = this
        this.$axios.get('/article').then(resp => {
          if (resp && resp.status === 200) {
            _this.articles = resp.data.content
          }
        })
      },
      viewArticle (id) {
        let articleUrl = this.$router.resolve(
          {
            path: '../../jotter/article',
            query: {
              id: id
            }
          }
        )
        window.open(articleUrl.href, '_blank')
      },
      editArticle (article) {
        this.$router.push(
          {
            name: 'Editor',
            params: {
              article: article
            }
          }
        )
      }
    }
  }
</script>

<style scoped>
  .add-link {
    margin: 18px 0 15px 10px;
    float: left;
  }
</style>
