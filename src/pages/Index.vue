<template>
  <Layout>
    <div class="container sm:pxi-0 mx-auto overflow-x-hidden">
      <div class="flex flex-wrap with-large pt-16 pb-16 mx-4 sm:-mx-4">
        <PostListItem v-for="edge in $page.entries.edges" :key="edge.node.id" :record="edge.node" />
      </div>
    </div>
  </Layout>
</template>

<page-query>

query($page:Int) {

  entries: allBlog(sortBy: "created", order: DESC, perPage: 9, page: $page) @paginate {
    totalCount
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        title
        excerpt
        image(width:800)
        path
        timeToRead
        humanTime : created(format:"DD MMM YYYY")
        datetime : created
        category {
          id
          title
        }
        author {
          id
          name
          image(width:64, height:64, fit:inside)
          path
        }
      }
    }
  }
}

</page-query>

<script>

import PostListItem from '~/components/PostListItem.vue';

export default {
  metaInfo: {
    title: "Hello, world!"
  },
  components: {
    PostListItem
  }
};
</script>

<style>
a, a:hover {
  color: inherit!important;
  text-decoration: none!important;
}

@media (min-width: 640px){
  .sm\:-mx-4 {
    margin-left: 1rem!important;
    margin-right: 1rem!important;
  }
}

@media (min-width: 640px){
  .sm\:px-4 {
    padding-left: 1rem!important;
    padding-right: 1rem!important; 
  }
}

ul{
  margin-bottom: 0rem!important;
}
</style>
