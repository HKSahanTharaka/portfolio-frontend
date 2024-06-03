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

    <h2>Create Project</h2>
    <form @submit.prevent="createProject" class="form">
      <div class="form-group">
        <label for="createName">Project Name:</label>
        <input type="text" v-model="createName" id="createName" required>
      </div>
      <div class="form-group">
        <label for="createDescription">Description:</label>
        <textarea v-model="createDescription" id="createDescription" required></textarea>
      </div>
      <button type="submit" class="btn">Create Project</button>
    </form>

    <h2>Update Project</h2>
    <form @submit.prevent="updateProject" class="form">
      <div class="form-group">
        <label for="updateId">Project ID:</label>
        <select v-model="updateId" id="updateId" required>
          <option v-for="project in projects" :key="project._id" :value="project._id">{{ project._id }}</option>
        </select>
      </div>
      <div class="form-group">
        <label for="updateName">Project Name:</label>
        <input type="text" v-model="updateName" id="updateName">
      </div>
      <div class="form-group">
        <label for="updateDescription">Description:</label>
        <textarea v-model="updateDescription" id="updateDescription"></textarea>
      </div>
      <button type="submit" class="btn">Update Project</button>
    </form>

    <h2>Delete Project</h2>
    <form @submit.prevent="deleteProject" class="form">
      <div class="form-group">
        <label for="deleteId">Project ID:</label>
        <select v-model="deleteId" id="deleteId" required>
          <option v-for="project in projects" :key="project._id" :value="project._id">{{ project._id }}</option>
        </select>
      </div>
      <button type="submit" class="btn">Delete Project</button>
    </form>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const projects = ref([])
const blogs = ref([])
const createName = ref('')
const createDescription = ref('')
const updateId = ref('')
const updateName = ref('')
const updateDescription = ref('')
const deleteId = ref('')

const fetchProjects = async () => {
  try {
    const response = await fetch('http://localhost:5000/projects')
    const data = await response.json()
    projects.value = data
  } catch (error) {
    console.error('Error fetching projects:', error)
  }
}

const fetchBlogs = async () => {
  try {
    const response = await fetch('http://localhost:5000/blogs')
    const data = await response.json()
    blogs.value = data
  } catch (error) {
    console.error('Error fetching blogs:', error)
  }
}

const createProject = async () => {
  try {
    const response = await fetch('http://localhost:5000/projects', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ name: createName.value, description: createDescription.value })
    })

    if (!response.ok) {
      throw new Error('Failed to create project')
    }

    // Clear form fields
    createName.value = ''
    createDescription.value = ''

    fetchProjects()
    alert('Project created successfully!')
  } catch (error) {
    console.error('Error creating project:', error)
    alert('Error creating project')
  }
}

const updateProject = async () => {
  try {
    const response = await fetch(`http://localhost:5000/projects/${updateId.value}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ name: updateName.value, description: updateDescription.value })
    })

    if (!response.ok) {
      throw new Error('Failed to update project')
    }

    // Clear form fields
    updateId.value = ''
    updateName.value = ''
    updateDescription.value = ''

    fetchProjects()
    alert('Project updated successfully!')
  } catch (error) {
    console.error('Error updating project:', error)
    alert('Error updating project')
  }
}

const deleteProject = async () => {
  try {
    const response = await fetch(`http://localhost:5000/projects/${deleteId.value}`, {
      method: 'DELETE'
    })

    if (!response.ok) {
      throw new Error('Failed to delete project')
    }

    // Clear form field
    deleteId.value = ''

    fetchProjects()
    alert('Project deleted successfully!')
  } catch (error) {
    console.error('Error deleting project:', error)
    alert('Error deleting project')
  }
}

onMounted(() => {
  fetchProjects()
  fetchBlogs()
})
</script>

<style>
/* Basic styling for forms */
.form {
  margin: 20px 0;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  max-width: 400px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
}

.form-group input,
.form-group textarea,
.form-group select {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}

.form-group textarea {
  resize: vertical;
}

.btn {
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn:hover {
  background-color: #0056b3;
}
</style>
