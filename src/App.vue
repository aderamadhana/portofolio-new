<template>
  <div class="min-vh-100 bg-light">
    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm fixed-top">
      <div class="container">
        <a class="navbar-brand fw-bold fs-4" href="#">DevPortfolio</a>
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
            <div class="d-flex gap-3 flex-wrap">
              <a href="#projects" class="btn btn-gradient btn-lg px-4 fw-semibold">
                {{ t.viewProjects }}
              </a>
              <a href="#contact" class="btn btn-outline-gradient btn-lg px-4 fw-semibold">
                {{ t.contactMe }}
              </a>
            </div>
          </div>

          <div class="col-lg-6">
            <div class="position-relative">
              <!-- Ganti URL di src dengan foto Anda -->
              <div class="hero-image-wrapper overflow-hidden">
                <img
                  src="https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=600&h=800&fit=crop"
                  alt="Profile Photo"
                  class="w-100 h-100"
                  style="object-fit: cover"
                />
              </div>

              <!-- <div
                class="floating floating-card position-absolute d-none d-md-block"
                style="top: 50px; left: -30px"
              >
                <div class="fs-1 mb-2">💼</div>
                <div class="small fw-semibold">{{ t.experience }}</div>
              </div>

              <div
                class="floating-delayed floating-card position-absolute d-none d-md-block"
                style="bottom: 80px; right: -30px"
              >
                <div class="fs-1 mb-2">✨</div>
                <div class="small fw-semibold">{{ t.projectsDone }}</div>
              </div> -->
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Skills Section -->
    <section id="skills" class="py-5 bg-white">
      <div class="container py-5">
        <h2 class="fw-bold text-center mb-3 display-5 display-md-4">{{ t.whatIDo }}</h2>
        <p class="text-center text-secondary mb-5 fs-6 fs-md-5">{{ t.expertise }}</p>

        <div class="row g-4">
          <div v-for="(skill, index) in skills" :key="index" class="col-md-6 col-lg-3">
            <div class="card skill-card card-hover h-100 border">
              <div class="card-body p-4">
                <div class="fs-1 mb-4">{{ skill.icon }}</div>
                <h3 class="h5 h4-md fw-bold mb-3">{{ skill.title }}</h3>
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
        <h2 class="fw-bold text-center mb-3 display-5 display-md-4">{{ t.featuredProjects }}</h2>
        <p class="text-center text-secondary mb-5 fs-6 fs-md-5">{{ t.recentWork }}</p>

        <div class="row g-4">
          <div v-for="(project, index) in projects" :key="index" class="col-md-6 col-lg-4">
            <div
              class="card card-hover h-100 border"
              @click="selectedProject = project"
              data-bs-toggle="modal"
              data-bs-target="#projectModal"
              style="cursor: pointer"
            >
              <div class="project-img-wrapper">
                <img :src="project.image" :alt="project.title" />
                <div class="project-overlay d-flex align-items-center justify-content-center">
                  <span class="text-white fw-bold fs-5">{{ t.viewDetails }}</span>
                </div>
              </div>
              <div class="card-body p-4">
                <h3 class="h5 h4-md fw-bold mb-3">{{ project.title }}</h3>
                <p class="text-secondary mb-4 lh-lg small">{{ project.description }}</p>
                <div class="d-flex flex-wrap gap-2">
                  <span
                    v-for="(tag, i) in project.tags"
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
      <div class="modal-dialog modal-dialog-centered modal-lg">
        <div class="modal-content" v-if="selectedProject">
          <div class="modal-header border-0">
            <h5 class="modal-title fw-bold fs-4 fs-md-3">{{ selectedProject.title }}</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body p-3 p-md-4">
            <img
              :src="selectedProject.image"
              :alt="selectedProject.title"
              class="w-100 rounded mb-4"
              style="max-height: 300px; object-fit: cover"
            />

            <div class="mb-4">
              <h6 class="fw-bold mb-3 fs-6 fs-md-5">{{ t.projectOverview }}</h6>
              <p class="text-secondary lh-lg small">{{ selectedProject.fullDescription }}</p>
            </div>

            <div class="mb-4">
              <h6 class="fw-bold mb-3 fs-6 fs-md-5">{{ t.keyFeatures }}</h6>
              <ul class="list-unstyled">
                <li v-for="(feature, index) in selectedProject.features" :key="index" class="mb-2">
                  <i class="bi bi-check-circle-fill text-success me-2"></i>
                  <span class="text-secondary small">{{ feature }}</span>
                </li>
              </ul>
            </div>

            <div class="mb-4">
              <h6 class="fw-bold mb-3 fs-6 fs-md-5">{{ t.technologies }}</h6>
              <div class="d-flex flex-wrap gap-2">
                <span
                  v-for="(tag, i) in selectedProject.tags"
                  :key="i"
                  class="badge tech-badge rounded-pill px-3 py-2 fw-semibold small"
                >
                  {{ tag }}
                </span>
              </div>
            </div>

            <div v-if="selectedProject.link" class="d-grid">
              <a
                :href="selectedProject.link"
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
    <section id="contact" class="py-5 bg-white">
      <div class="container py-5">
        <div class="row justify-content-center">
          <div class="col-lg-8">
            <div class="card border shadow-sm">
              <div class="card-body p-4 p-md-5">
                <h2 class="fw-bold mb-4 display-6 display-md-5">{{ t.workTogether }}</h2>

                <div class="mb-4">
                  <label class="form-label fw-semibold small">{{ t.yourName }}</label>
                  <input
                    type="text"
                    v-model="formData.name"
                    :placeholder="t.namePlaceholder"
                    class="form-control form-control-lg"
                  />
                </div>

                <div class="mb-4">
                  <label class="form-label fw-semibold small">{{ t.emailAddress }}</label>
                  <input
                    type="email"
                    v-model="formData.email"
                    :placeholder="t.emailPlaceholder"
                    class="form-control form-control-lg"
                  />
                </div>

                <div class="mb-4">
                  <label class="form-label fw-semibold small">{{ t.yourMessage }}</label>
                  <textarea
                    v-model="formData.message"
                    :placeholder="t.messagePlaceholder"
                    rows="5"
                    class="form-control form-control-lg"
                  ></textarea>
                </div>

                <button @click="handleSubmit" class="btn btn-gradient btn-lg w-100 fw-semibold">
                  {{ t.sendMessage }}
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="py-4 border-top">
      <div class="container">
        <p class="text-center text-secondary small mb-0">{{ t.footer }}</p>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      lang: 'id',
      selectedProject: null,
      formData: {
        name: '',
        email: '',
        message: '',
      },
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
            'Senior Fullstack Developer dengan pengalaman lebih dari 3 tahun mengembangkan dan memimpin aplikasi web untuk enterprise dan internal system. Berpengalaman menangani multi-project secara paralel untuk institusi besar seperti BUMN dan perbankan.',
          viewProjects: 'Lihat Projek',
          contactMe: 'Hubungi Saya',
          photoLabel: 'Foto Anda Di Sini',
          experience: '3+ Tahun Pengalaman',
          projectsDone: 'Enterprise Projects',
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
          viewDetails: 'Lihat Detail',
          projectOverview: 'Ringkasan Projek',
          keyFeatures: 'Fitur Utama',
          technologies: 'Teknologi yang Digunakan',
          visitWebsite: 'Kunjungi Website',
          ecommerceTitle: 'Platform E-Commerce',
          ecommerceDesc:
            'Toko online dengan fitur lengkap meliputi integrasi pembayaran, manajemen produk, dan tracking inventori real-time.',
          ecommerceFullDesc:
            'Platform e-commerce modern yang dibangun dengan teknologi terkini untuk memberikan pengalaman belanja online yang seamless. Sistem ini dilengkapi dengan dashboard admin yang komprehensif untuk mengelola produk, pesanan, dan pelanggan dengan mudah.',
          ecommerceFeatures: [
            'Sistem pembayaran terintegrasi dengan berbagai metode (Stripe, PayPal, Transfer Bank)',
            'Real-time inventory management dengan notifikasi stok rendah',
            'Dashboard analytics untuk tracking penjualan dan performa produk',
            'Sistem review dan rating produk',
            'Responsive design untuk semua perangkat',
            'Keranjang belanja dengan auto-save',
            'Email notification untuk order tracking',
          ],
          dashboardTitle: 'Dashboard Analitik',
          dashboardDesc:
            'Dashboard analitik real-time dengan chart interaktif, visualisasi data, dan pelaporan otomatis.',
          dashboardFullDesc:
            'Dashboard analitik yang powerful untuk visualisasi data bisnis secara real-time. Dilengkapi dengan berbagai chart interaktif, filter dinamis, dan sistem reporting otomatis yang dapat dikustomisasi sesuai kebutuhan.',
          dashboardFeatures: [
            'Real-time data visualization dengan Chart.js dan D3.js',
            'Customizable dashboard dengan drag & drop widgets',
            'Export data ke PDF, Excel, dan CSV',
            'Automated daily/weekly/monthly reports via email',
            'Multi-user access dengan role-based permissions',
            'Dark mode dan light mode',
            'API integration untuk berbagai data sources',
          ],
          corporateTitle: 'Website Korporat',
          corporateDesc:
            'Website korporat profesional dengan integrasi CMS, sistem blog, dan formulir kontak.',
          corporateFullDesc:
            'Website korporat yang elegant dan profesional dengan CMS yang user-friendly. Dilengkapi dengan sistem blog, portfolio showcase, dan formulir kontak yang terintegrasi dengan email notification.',
          corporateFeatures: [
            'Content Management System (CMS) yang mudah digunakan',
            'Blog system dengan kategori dan tags',
            'Portfolio/Case studies showcase',
            'Team member profiles dengan bio dan social links',
            'Contact form dengan spam protection',
            'SEO optimized dengan meta tags dinamis',
            'Multi-language support (EN/ID)',
          ],
          workTogether: 'Mari Bekerja Sama',
          yourName: 'Nama Anda',
          emailAddress: 'Alamat Email',
          yourMessage: 'Pesan Anda',
          namePlaceholder: 'Nama Anda',
          emailPlaceholder: 'email@anda.com',
          messagePlaceholder: 'Ceritakan tentang projek Anda...',
          sendMessage: 'Kirim Pesan',
          footer: '© 2024 DevPortfolio. Dibuat dengan passion dan code.',
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
            'Senior Fullstack Developer with over 3 years of experience developing and leading web applications for enterprise and internal systems. Experienced in handling multi-projects in parallel for large institutions such as SOEs and banking.',
          viewProjects: 'View Projects',
          contactMe: 'Contact Me',
          photoLabel: 'Your Photo Here',
          experience: '3+ Years Experience',
          projectsDone: 'Enterprise Projects',
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
          viewDetails: 'View Details',
          projectOverview: 'Project Overview',
          keyFeatures: 'Key Features',
          technologies: 'Technologies Used',
          visitWebsite: 'Visit Website',
          ecommerceTitle: 'E-Commerce Platform',
          ecommerceDesc:
            'A full-featured online store with payment integration, product management, and real-time inventory tracking.',
          ecommerceFullDesc:
            'A modern e-commerce platform built with cutting-edge technologies to provide a seamless online shopping experience. The system includes a comprehensive admin dashboard for easy product, order, and customer management.',
          ecommerceFeatures: [
            'Integrated payment system with multiple methods (Stripe, PayPal, Bank Transfer)',
            'Real-time inventory management with low stock notifications',
            'Analytics dashboard for sales tracking and product performance',
            'Product review and rating system',
            'Responsive design for all devices',
            'Shopping cart with auto-save functionality',
            'Email notifications for order tracking',
          ],
          dashboardTitle: 'Analytics Dashboard',
          dashboardDesc:
            'Real-time analytics dashboard with interactive charts, data visualization, and automated reporting.',
          dashboardFullDesc:
            'A powerful analytics dashboard for real-time business data visualization. Features interactive charts, dynamic filters, and customizable automated reporting system.',
          dashboardFeatures: [
            'Real-time data visualization with Chart.js and D3.js',
            'Customizable dashboard with drag & drop widgets',
            'Export data to PDF, Excel, and CSV',
            'Automated daily/weekly/monthly reports via email',
            'Multi-user access with role-based permissions',
            'Dark mode and light mode support',
            'API integration for various data sources',
          ],
          corporateTitle: 'Corporate Website',
          corporateDesc:
            'Professional corporate website with CMS integration, blog system, and contact forms.',
          corporateFullDesc:
            'An elegant and professional corporate website with a user-friendly CMS. Includes blog system, portfolio showcase, and contact forms with email notification integration.',
          corporateFeatures: [
            'User-friendly Content Management System (CMS)',
            'Blog system with categories and tags',
            'Portfolio/Case studies showcase',
            'Team member profiles with bio and social links',
            'Contact form with spam protection',
            'SEO optimized with dynamic meta tags',
            'Multi-language support (EN/ID)',
          ],
          workTogether: "Let's Work Together",
          yourName: 'Your Name',
          emailAddress: 'Email Address',
          yourMessage: 'Your Message',
          namePlaceholder: 'John Doe',
          emailPlaceholder: 'john@example.com',
          messagePlaceholder: 'Tell me about your project...',
          sendMessage: 'Send Message',
          footer: '© 2024 DevPortfolio. Built with passion and code.',
          successMessage: 'Thank you! Your message has been sent.',
        },
      },
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
    projects() {
      return [
        {
          image: 'https://images.unsplash.com/photo-1557821552-17105176677c?w=600&h=400&fit=crop',
          title: this.t.ecommerceTitle,
          description: this.t.ecommerceDesc,
          fullDescription: this.t.ecommerceFullDesc,
          features: this.t.ecommerceFeatures,
          tags: ['React', 'Node.js', 'MongoDB', 'Stripe'],
          link: 'https://example-ecommerce.com',
        },
        {
          image: 'https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=600&h=400&fit=crop',
          title: this.t.dashboardTitle,
          description: this.t.dashboardDesc,
          fullDescription: this.t.dashboardFullDesc,
          features: this.t.dashboardFeatures,
          tags: ['Vue.js', 'D3.js', 'Express', 'PostgreSQL'],
          link: 'https://example-dashboard.com',
        },
        {
          image:
            'https://images.unsplash.com/photo-1460925895917-afdab827c52f?w=600&h=400&fit=crop',
          title: this.t.corporateTitle,
          description: this.t.corporateDesc,
          fullDescription: this.t.corporateFullDesc,
          features: this.t.corporateFeatures,
          tags: ['Next.js', 'TypeScript', 'Tailwind'],
          link: 'https://example-corporate.com',
        },
      ]
    },
  },
  methods: {
    toggleLanguage() {
      this.lang = this.lang === 'id' ? 'en' : 'id'
    },
    openProjectModal(project) {
      this.selectedProject = project
      this.$nextTick(() => {
        const modalElement = document.getElementById('projectModal')
        if (modalElement) {
          // Menggunakan Bootstrap Modal API
          const modal = window.bootstrap?.Modal?.getOrCreateInstance(modalElement)
          modal?.show()
        }
      })
    },
    handleSubmit() {
      if (this.formData.name && this.formData.email && this.formData.message) {
        alert(this.t.successMessage)
        this.formData = {
          name: '',
          email: '',
          message: '',
        }
      }
    },
  },
}
</script>

<style scoped>
/* Styles sudah ada di main.css */
</style>
