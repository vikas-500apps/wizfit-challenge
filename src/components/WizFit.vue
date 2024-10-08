<template>
  <div class="flex flex-col min-h-screen bg-white">
    <!-- Start of banner image -->
    <header>
      <img src="/image/banner.png" alt="WizFit Challenge Banner" class="banner" />
    </header>
    <!-- End of banner image -->

    <!-- Start of WizFit Challenge main section -->
    <main class="main-content">
      <div class="text-center">
        <h2 class="title">What is WizFit Challenge?</h2>
        <button class="watch-video">Watch Video</button>
      </div>
      <div class="content">
        <img src="/image/player.png" alt="Harlem Wizards Player" class="player-image" />
        <!-- Start of card section -->
        <div class="card">
          <div class="card-content">
            <h3 class="card-title">Are you ready to take the challenge?</h3>
            <p>Download Harlem Wizards App</p>
            <!-- Start of app link -->
            <div class="app-links">
              <a href="https://play.google.com/store/apps/details?id=com.harlemwizards">
                <img src="/image/goolge-store.png" alt="Get it on Google Play" class="app-badge" />
              </a>
              <a href="https://apps.apple.com/us/app/harlem-wizards/id1659863735">
                <img src="/image/apple-store.png" alt="Download on the App Store" class="app-badge" />
              </a>
            </div>
            <!-- End of app link -->
            <p>
              <span class="line"></span>
              <span>or you can sign up right now</span>
              <span class="line"></span>
            </p>

            <!-- Start of seach input -->
            <input type="text" placeholder="Search Campaign Here" v-model="searchQuery" class="search-input"
              @input="handleSearch" />
            <!-- End of seach input -->

            <div v-if="error" class="error-message">
              <p>{{ error }}</p>
            </div>

            <div v-if="filteredSchools.length === 0 && searchQuery.length >= 3" class="no-results-message">
              <p>No schools match your search criteria. Please try another term.</p>
            </div>

            <!-- Start of seach list -->
            <div v-if="filteredSchools.length > 0" class="search-results">
              <ul class="results-list">
                <li v-for="school in filteredSchools" :key="school.id" class="result-item">
                  <span class="school-info">
                    <img :src="school.logo_url" alt="School Logo" class="school-logo" />
                    {{ school.school_name }}
                  </span>
                  <button class="join-button">Join Campaign</button>
                </li>
              </ul>
            </div>
            <!-- End of seach list -->

          </div>
        </div>
        <!-- End of card section -->
      </div>
    </main>
    <!-- End of WizFit Challenge main section -->

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const searchQuery = ref('');
const schools = ref([]);
const error = ref('');
const filteredSchools = ref([]);


const getSchoolDetails = async () => {
  try {
    const response = await fetch('http://api.devharlemwizardsinabox.com/campaign/campaign_school_list/');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    schools.value = data.school_list || [];
    filteredSchools.value = schools.value; // Initialize filtered schools with all schools
    error.value = '';
  } catch (err) {
    error.value = 'Error fetching schools: ' + err.message;
  }
};

const handleSearch = () => {
  if (searchQuery.value.length >= 3) {
    const query = searchQuery.value.toLowerCase();
    filteredSchools.value = schools.value.filter(school =>
    school.school_name.toLowerCase().includes(query)
    );
  } else {
    filteredSchools.value = []; // Clear results if less than 3 characters
  }
};

// Fetch schools on component mount
onMounted(getSchoolDetails);
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: white;
}

.banner {
  width: 100%;
  height: 200px;
}

.error-message,
.no-results-message {
  color: red;
  text-align: center;
}

.main-content {
  flex-grow: 1;
  padding: 32px 16px;
  text-align: center;
}

.line {
  display: inline-block;
  width: 100px;
  height: 1px;
  background-color: #151515;
  vertical-align: middle;
  margin: 0 10px; /* Space between the text and the line */
}

.title {
  font-size: 2.5rem;
  color: #c62828; /* Red color */
}

.watch-video {
  background-color: #c62828; /* Red color */
  color: white;
  padding: 12px 24px;
  font-size: 1.25rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.player-image {
  width: 300px;
  height: 200px;
}

.card {
  width: 100%;
  max-width: 600px;
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  padding: 32px;
}

.card-title {
  font-size: 1.5rem;
  color: #c62828;
  margin-bottom: 16px;
}

.result-item {
  display: flex;
  align-items: center;
  background-color: #f7f7f7;
  padding: 12px;
  border-radius: 4px;
  margin-bottom: 8px;
}

.school-info {
  display: flex;
  align-items: center;
}

.school-logo {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 8px;
  object-fit: cover;
}


.app-links {
  display: flex;
  justify-content: center;
  gap: 16px;
  margin-bottom: 16px;
}

.text-center {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
}

.app-badge {
  width: 135px;
  height: 40px;
}

.search-input {
  max-width: 300px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.search-results {
  margin-top: 32px;
}

.results-title {
  font-size: 1.25rem;
  font-weight: bold;
}

.results-list {
  list-style: none;
  padding: 0;
}

.result-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #f7f7f7;
  padding: 12px;
  border-radius: 4px;
  margin-bottom: 8px;
}

.join-button {
  background-color: transparent;
  color: #c62828;
  border: 1px solid #c62828;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
}
</style>
