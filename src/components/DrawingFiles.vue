<template>
  <div class="container">
    <h1>List of Drawing Files</h1>
    <div v-if="loading" class="loading">
      <i class="fas fa-spinner fa-spin"></i> Loading...
    </div>
    <ul v-else class="file-list">
      <li v-for="file in files" :key="file.name" class="file-item">
        <span><i :class="fileIcon(file.name)"></i> {{ file.name }} ({{ file.size }} bytes)</span>
        <button @click="showFile(file.name)" class="view-button">View</button>
      </li>
    </ul>
    <div v-if="selectedFile" class="file-viewer">
      <h2>Viewing: {{ selectedFile }}</h2>
      <embed v-if="isPDF(selectedFile)" :src="fileUrl" width="600" height="400" type="application/pdf" />
      <img v-else :src="fileUrl" alt="Technical Drawing" width="600" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      files: [],
      selectedFile: null,
      fileUrl: "",
      loading: true,
    };
  },
  created() {
    this.fetchFiles();
  },
  methods: {
    async fetchFiles() {
      try {
        const response = await fetch('http://localhost:8200/v1/drawings');
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        const data = await response.json();
        this.files = data.drawings;
      } catch (error) {
        console.error('Error fetching files:', error);
      } finally {
        this.loading = false;
      }
    },
    async showFile(key) {
      this.selectedFile = key;
      this.fileUrl = `http://localhost:8200/v1/drawings/${key}`;
    },
    isPDF(file) {
      return file.endsWith('.pdf');
    },
    fileIcon(file) {
      const ext = file.split('.').pop();
      switch (ext) {
        case 'pdf':
          return 'fas fa-file-pdf';
        case 'jpg':
        case 'jpeg':
        case 'png':
        case 'gif':
          return 'fas fa-file-image';
        default:
          return 'fas fa-file';
      }
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

h1 {
  font-size: 2em;
  margin-bottom: 20px;
}

.loading {
  font-size: 1.2em;
}

.file-list {
  list-style: none;
  padding: 0;
}

.file-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
}

.file-item span {
  font-size: 1em;
}

.view-button {
  padding: 5px 10px;
  font-size: 0.9em;
  color: #fff;
  background-color: #007bff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.view-button:hover {
  background-color: #0056b3;
}

.file-viewer {
  margin-top: 20px;
}

.file-viewer h2 {
  font-size: 1.5em;
  margin-bottom: 10px;
}
</style>
