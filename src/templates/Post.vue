<template>
  <Layout>
    <br />
    <g-link to="/" class="link"> &larr; Go Back</g-link>

    <div class="post-title">
      <h1 v-html="$page.post.title"></h1>
      <p class="post-date">
        {{ $page.post.date }} | {{ $page.post.timeToRead }} min read
      </p>
    </div>

    <div class="post-content">
      <div v-html="$page.post.content" />
    </div>
  </Layout>
</template>

<page-query>
query Post ($path: String!) {
   post: post (path: $path) {
    id
    title
    content
    date (format: "D MMMM YYYY")
    timeToRead
  }
}
</page-query>

<style>
.post-title {
  text-align: center;
  font-size: 25px;
  font-family: "Stylish";
}

.post-date {
  font-size: 16px;
  font-weight: 400;
}

.post-content {
  font-size: 20px;
}

table td {
  padding: 5px;
}

table tr {
  border: 1px solid #ddd;
}

table tr:nth-child(even) {
  background-color: #f2f2f2;
}

table th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
}
</style>
