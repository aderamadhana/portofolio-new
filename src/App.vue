<template>
  <div class="min-vh-100 bg-light">
    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm fixed-top">
      <div class="container">
        <a class="navbar-brand fw-bold fs-4" href="#">Ade Ramadhana Pratama</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse justify-content-end" id="navbarNav">
          <ul class="navbar-nav align-items-center gap-3">
            <li class="nav-item">
              <a class="nav-link" href="#home">{{ t.home }}</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#skills">{{ t.skills }}</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#projects">{{ t.projects }}</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#contact">{{ t.contact }}</a>
            </li>
            <li class="nav-item">
              <button @click="toggleLanguage" class="btn btn-gradient btn-sm px-4 fw-semibold">
                {{ lang === 'id' ? 'EN' : 'ID' }}
              </button>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <!-- Hero Section -->
    <section id="home" class="py-5" style="margin-top: 80px">
      <div class="container py-5">
        <div class="row align-items-center g-5">
          <div class="col-lg-6">
            <h1 class="fw-bold mb-3 display-3 display-md-2 display-lg-1">
              {{ t.greeting }}<br />
              <span class="gradient-text">{{ t.title }}</span>
            </h1>
            <p class="text-secondary mb-4 fs-5 fs-md-4">{{ t.subtitle }}</p>
            <p class="text-secondary mb-4 lh-lg fs-6">{{ t.description }}</p>

            <div class="d-flex gap-3 flex-wrap mb-4">
              <a href="#projects" class="btn btn-gradient btn-lg px-4 fw-semibold">
                {{ t.viewProjects }}
              </a>
              <a href="#contact" class="btn btn-outline-gradient btn-lg px-4 fw-semibold">
                {{ t.contactMe }}
              </a>
            </div>

            <!-- Social Links -->
            <div class="d-flex gap-3 align-items-center">
              <a
                href="https://github.com/aderamadhana"
                target="_blank"
                class="btn btn-outline-secondary rounded-circle d-flex align-items-center justify-content-center"
                style="width: 50px; height: 50px"
                aria-label="GitHub"
              >
                <i class="bi bi-github fs-5"></i>
              </a>
              <a
                href="https://www.linkedin.com/in/ade-ramadhana-p-abb489196/"
                target="_blank"
                class="btn btn-outline-secondary rounded-circle d-flex align-items-center justify-content-center"
                style="width: 50px; height: 50px"
                aria-label="LinkedIn"
              >
                <i class="bi bi-linkedin fs-5"></i>
              </a>
            </div>
          </div>

          <div class="col-lg-6">
            <div class="position-relative">
              <div class="hero-image-wrapper overflow-hidden">
                <img
                  :src="foto_profil || 'https://via.placeholder.com/600x600'"
                  alt="Profile Photo"
                  class="w-100 h-100"
                  style="object-fit: cover"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Skills Section -->
    <section id="skills" class="py-5 bg-white">
      <div class="container py-5">
        <div class="text-center mb-5">
          <h2 class="fw-bold display-5 display-md-4 mb-3">{{ t.whatIDo }}</h2>
          <p class="text-secondary fs-6 fs-md-5">{{ t.expertise }}</p>
        </div>

        <div class="row g-4">
          <div v-for="(skill, index) in skills" :key="index" class="col-md-6 col-lg-3">
            <div class="card skill-card card-hover h-100 border-0 shadow-sm">
              <div class="card-body p-4 text-center">
                <div class="fs-1 mb-3">{{ skill.icon }}</div>
                <h3 class="h5 fw-bold mb-3">{{ skill.title }}</h3>
                <p class="text-secondary small lh-lg mb-0">{{ skill.desc }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Projects Section -->
    <section id="projects" class="py-5">
      <div class="container py-5">
        <div class="text-center mb-5">
          <h2 class="fw-bold display-5 display-md-4 mb-3">{{ t.featuredProjects }}</h2>
          <p class="text-secondary fs-6 fs-md-5">{{ t.recentWork }}</p>
        </div>

        <div v-if="isLoading" class="text-center py-5">
          <div class="spinner-border text-primary" role="status">
            <span class="visually-hidden">Loading...</span>
          </div>
          <p class="text-muted mt-3">{{ t.loadingProjects }}</p>
        </div>
        <div v-else-if="projects.length === 0" class="text-center py-5">
          <p class="text-muted mt-3">{{ t.emptyProjects }}</p>
        </div>

        <div v-else class="row g-4">
          <div v-for="(project, index) in projects" :key="index" class="col-md-6 col-lg-4">
            <div
              class="card card-hover h-100 border-0 shadow-sm"
              @click="selectedProject = project"
              data-bs-toggle="modal"
              data-bs-target="#projectModal"
              style="cursor: pointer"
            >
              <div class="project-img-wrapper">
                <img
                  :src="project.icon_menu_url || 'https://via.placeholder.com/600x400'"
                  :alt="project.nama_menu[lang]"
                />
                <div class="project-overlay d-flex align-items-center justify-content-center">
                  <span class="text-white fw-bold fs-5">{{ t.viewDetails }}</span>
                </div>
              </div>
              <div class="card-body p-4">
                <h3 class="h5 fw-bold mb-3">{{ project.nama_menu[lang] }}</h3>
                <p class="text-secondary mb-4 lh-lg small" v-html="project.deskripsi[lang]"></p>
                <div class="d-flex flex-wrap gap-2">
                  <span
                    v-for="(tag, i) in project.tech_stack"
                    :key="i"
                    class="badge tech-badge rounded-pill px-3 py-2 fw-semibold small"
                  >
                    {{ tag }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Project Modal -->
    <div class="modal fade" id="projectModal" tabindex="-1" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered modal-lg modal-dialog-scrollable">
        <div class="modal-content border-0 shadow-lg" v-if="selectedProject">
          <div class="modal-header border-0 pb-0">
            <h5 class="modal-title fw-bold fs-4">{{ selectedProject.nama_menu[lang] }}</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body p-4">
            <img
              :src="selectedProject.icon_menu_url || 'https://via.placeholder.com/800x400'"
              :alt="selectedProject.nama_menu[lang]"
              class="w-100 rounded-3 mb-4"
              style="max-height: 400px; object-fit: cover"
            />

            <div class="mb-4">
              <h6 class="fw-bold mb-3 fs-6 text-uppercase text-primary">{{ t.projectOverview }}</h6>
              <p class="text-secondary lh-lg" v-html="selectedProject.deskripsi[lang]"></p>
            </div>

            <div class="mb-4" v-if="selectedProject?.fitur?.[lang]">
              <h6 class="fw-bold mb-3 fs-6 text-uppercase text-primary">{{ t.keyFeatures }}</h6>
              <div class="text-secondary lh-lg" v-html="selectedProject.fitur[lang]"></div>
            </div>

            <div class="mb-4">
              <h6 class="fw-bold mb-3 fs-6 text-uppercase text-primary">{{ t.technologies }}</h6>
              <div class="d-flex flex-wrap gap-2">
                <span
                  v-for="(tag, i) in selectedProject.tech_stack"
                  :key="i"
                  class="badge tech-badge rounded-pill px-3 py-2 fw-semibold"
                >
                  {{ tag }}
                </span>
              </div>
            </div>

            <div v-if="selectedProject.token_akses" class="d-grid gap-2">
              <a
                :href="selectedProject.token_akses"
                target="_blank"
                class="btn btn-gradient btn-lg fw-semibold"
              >
                <i class="bi bi-box-arrow-up-right me-2"></i>{{ t.visitWebsite }}
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Contact Section -->
    <section id="contact" class="py-5 position-relative overflow-hidden">
      <div class="contact-bg-shapes">
        <div class="shape shape-1"></div>
        <div class="shape shape-2"></div>
        <div class="shape shape-3"></div>
      </div>

      <div class="container py-5 position-relative">
        <div class="text-center mb-5">
          <h2 class="fw-bold display-4 mb-3">{{ t.workTogether }}</h2>
          <p class="text-muted fs-5">{{ t.workTogetherDesc }}</p>
        </div>

        <div class="row justify-content-center g-4">
          <div class="col-lg-10">
            <div class="contact-card">
              <div class="contact-item">
                <div class="icon email">
                  <i class="bi bi-envelope-fill"></i>
                </div>
                <h5>Email</h5>
                <p>sanade2034@gmail.com</p>
              </div>

              <div class="contact-item">
                <div class="icon linkedin">
                  <i class="bi bi-linkedin"></i>
                </div>
                <h5>LinkedIn</h5>
                <p>Connect with me</p>
              </div>

              <div class="contact-item">
                <div class="icon time">
                  <i class="bi bi-clock-fill"></i>
                </div>
                <h5>Waktu Respon</h5>
                <p>Dalam 24 jam</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="py-5 bg-white border-top">
      <div class="container">
        <div class="row align-items-center">
          <div class="col-md-6 text-center text-md-start mb-3 mb-md-0">
            <p class="text-secondary mb-0">{{ t.footer }}</p>
          </div>
          <div class="col-md-6 text-center text-md-end">
            <div class="d-flex gap-3 justify-content-center justify-content-md-end">
              <a
                href="https://github.com/aderamadhana"
                target="_blank"
                class="text-secondary text-decoration-none"
              >
                <i class="bi bi-github fs-5"></i>
              </a>
              <a
                href="https://www.linkedin.com/in/ade-ramadhana-p-abb489196/"
                target="_blank"
                class="text-secondary text-decoration-none"
              >
                <i class="bi bi-linkedin fs-5"></i>
              </a>
              <a href="mailto:sanade2034@gmail.com" class="text-secondary text-decoration-none">
                <i class="bi bi-envelope fs-5"></i>
              </a>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  data() {
    return {
      api_url: import.meta.env.VITE_API_URL,
      lang: 'id',
      selectedProject: null,
      isSubmitting: false,
      formData: {
        name: '',
        email: '',
        message: '',
      },
      foto_profil: null,
      projects: [],
      translations: {
        id: {
          home: 'Beranda',
          skills: 'Keahlian',
          projects: 'Projek',
          contact: 'Kontak',
          greeting: 'Halo, Saya',
          title: 'Senior Fullstack Web Developer',
          subtitle: 'Enterprise Systems • Technical Leadership • Multi-Project Delivery',
          description:
            'Senior Fullstack Developer dengan pengalaman lebih dari 4 tahun mengembangkan dan memimpin aplikasi web untuk enterprise dan internal system. Berpengalaman menangani multi-project secara paralel untuk institusi besar seperti BUMN dan perbankan.',
          viewProjects: 'Lihat Projek',
          contactMe: 'Hubungi Saya',
          whatIDo: 'Keahlian Teknis',
          expertise: 'Area spesialisasi dan teknologi yang saya kuasai',
          frontendTitle: 'Frontend Development',
          frontendDesc:
            'Mengembangkan antarmuka pengguna yang responsif dan interaktif menggunakan Vue.js, JavaScript, dan framework modern untuk aplikasi enterprise.',
          backendTitle: 'Backend Development',
          backendDesc:
            'Membangun sistem backend yang robust dengan PHP (Laravel, CodeIgniter 3/4), ASP.NET, PostgreSQL, MySQL, Redis, dan Laravel Queue untuk aplikasi berskala enterprise.',
          techLeadTitle: 'Technical Leadership',
          techLeadDesc:
            'Memimpin diskusi teknis, menentukan arsitektur sistem, melakukan code review, dan memastikan standar kualitas kode pada multi-project enterprise.',
          systemIntegrationTitle: 'System Integration & API',
          systemIntegrationDesc:
            'Merancang dan mengimplementasikan REST API, integrasi antar sistem, dan memastikan komunikasi yang seamless antara berbagai platform.',
          featuredProjects: 'Projek Enterprise',
          recentWork: 'Beberapa projek yang telah saya kerjakan',
          loadingProjects: 'Memuat projek...',
          emptyProjects: 'Tidak ada projek...',
          viewDetails: 'Lihat Detail',
          projectOverview: 'Ringkasan Projek',
          keyFeatures: 'Fitur Utama',
          technologies: 'Teknologi yang Digunakan',
          visitWebsite: 'Kunjungi Website',
          workTogether: 'Mari Bekerja Sama',
          workTogetherDesc: 'Mari buat sesuatu yang menakjubkan bersama',
          yourName: 'Nama Anda',
          emailAddress: 'Alamat Email',
          yourMessage: 'Pesan Anda',
          sendMessage: 'Kirim Pesan',
          sending: 'Mengirim...',
          responseTime: 'Waktu Respon',
          responseTimeDesc: 'Dalam 24 jam',
          footer: '© 2026 Ade Ramadhana Pratama. Dibuat dengan passion dan code.',
          successMessage: 'Terima kasih! Pesan Anda telah terkirim.',
        },
        en: {
          home: 'Home',
          skills: 'Skills',
          projects: 'Projects',
          contact: 'Contact',
          greeting: "Hi, I'm",
          title: 'Senior Fullstack Web Developer',
          subtitle: 'Enterprise Systems • Technical Leadership • Multi-Project Delivery',
          description:
            'Senior Fullstack Developer with over 4 years of experience developing and leading web applications for enterprise and internal systems. Experienced in handling multi-projects in parallel for large institutions such as SOEs and banking.',
          viewProjects: 'View Projects',
          contactMe: 'Contact Me',
          whatIDo: 'Technical Expertise',
          expertise: 'Areas of specialization and technologies I master',
          frontendTitle: 'Frontend Development',
          frontendDesc:
            'Developing responsive and interactive user interfaces using Vue.js, JavaScript, and modern frameworks for enterprise applications.',
          backendTitle: 'Backend Development',
          backendDesc:
            'Building robust backend systems with PHP (Laravel, CodeIgniter 3/4), ASP.NET, PostgreSQL, MySQL, Redis, and Laravel Queue for enterprise-scale applications.',
          techLeadTitle: 'Technical Leadership',
          techLeadDesc:
            'Leading technical discussions, determining system architecture, conducting code reviews, and ensuring code quality standards across multi-project enterprises.',
          systemIntegrationTitle: 'System Integration & API',
          systemIntegrationDesc:
            'Designing and implementing REST APIs, inter-system integration, and ensuring seamless communication between various platforms.',
          featuredProjects: 'Enterprise Projects',
          recentWork: 'Some projects I have worked on',
          loadingProjects: 'Loading projects...',
          emptyProjects: 'No projects...',
          viewDetails: 'View Details',
          projectOverview: 'Project Overview',
          keyFeatures: 'Key Features',
          technologies: 'Technologies Used',
          visitWebsite: 'Visit Website',
          workTogether: "Let's Work Together",
          workTogetherDesc: "Let's create something amazing together",
          yourName: 'Your Name',
          emailAddress: 'Email Address',
          yourMessage: 'Your Message',
          sendMessage: 'Send Message',
          sending: 'Sending...',
          responseTime: 'Response Time',
          responseTimeDesc: 'Within 24 hours',
          footer: '© 2026 Ade Ramadhana Pratama. Built with passion and code.',
          successMessage: 'Thank you! Your message has been sent.',
        },
      },

      isLoading: false,
    }
  },
  computed: {
    t() {
      return this.translations[this.lang]
    },
    skills() {
      return [
        { icon: '💻', title: this.t.frontendTitle, desc: this.t.frontendDesc },
        { icon: '⚙️', title: this.t.backendTitle, desc: this.t.backendDesc },
        { icon: '👨‍💼', title: this.t.techLeadTitle, desc: this.t.techLeadDesc },
        { icon: '🔗', title: this.t.systemIntegrationTitle, desc: this.t.systemIntegrationDesc },
      ]
    },
  },
  mounted() {
    this.getUserProfil()
    this.getProjects()
  },
  methods: {
    async getUserProfil() {
      try {
        const response = await axios.get(this.api_url + 'get_portofolio?lang=' + this.lang)
        this.foto_profil = response.data.data.profil_foto_url
      } catch (error) {
        console.error('Error loading profile:', error)
      }
    },
    async getProjects() {
      this.isLoading = true
      try {
        const response = await axios.get(this.api_url + 'get_projects?lang=' + this.lang)
        this.projects = response.data.data
        this.isLoading = false
      } catch (error) {
        console.error('Error loading projects:', error)
      }
    },
    toggleLanguage() {
      this.lang = this.lang === 'id' ? 'en' : 'id'
      this.getProjects()
    },
    async handleSubmit() {
      if (this.formData.name && this.formData.email && this.formData.message) {
        this.isSubmitting = true

        // Simulate API call
        await new Promise((resolve) => setTimeout(resolve, 1500))

        alert(this.t.successMessage)
        this.formData = {
          name: '',
          email: '',
          message: '',
        }
        this.isSubmitting = false
      }
    },
  },
}
</script>

<style scoped>
/* Styles sudah ada di main.css */
</style>
