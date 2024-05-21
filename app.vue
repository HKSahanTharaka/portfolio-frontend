<template>
  <div>
    <h1>Projects</h1>
    <ul>
      <li v-for="project in projects" :key="project._id">{{ project.name }} - {{ project.description }}</li>
    </ul>
    
    <h1>Blogs</h1>
    <ul>
      <li v-for="blog in blogs" :key="blog._id">{{ blog.title }} - {{ blog.content }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      projects: [],
      blogs: []
    }
  },
  async mounted() {
    try {
      const projectResponse = await fetch('http://localhost:5000/projects')
      const projects = await projectResponse.json()
      console.log('Projects:', projects) // Log the projects data
      const blogResponse = await fetch('http://localhost:5000/blogs')
      const blogs = await blogResponse.json()
      console.log('Blogs:', blogs) // Log the blogs data
      this.projects = projects
      this.blogs = blogs
    } catch (error) {
      console.error('Error fetching data:', error)
    }
  }
}
</script>