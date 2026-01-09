<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const currentYear = new Date().getFullYear()


// Customer Logos (placeholder - you can replace with actual logos)
const customerLogos = [
  { name: 'Blantyre Waste Services', logo: 'https://via.placeholder.com/150x80/29a329/ffffff?text=BWS' },
  { name: 'Lilongwe Clean City', logo: 'https://via.placeholder.com/150x80/32d296/ffffff?text=LCC' },
  { name: 'Mzuzu Environmental', logo: 'https://via.placeholder.com/150x80/3b82f6/ffffff?text=MES' },
  { name: 'Zomba Waste Management', logo: 'https://via.placeholder.com/150x80/ec4899/ffffff?text=ZWM' },
  { name: 'Kasungu Clean Solutions', logo: 'https://via.placeholder.com/150x80/29a329/ffffff?text=KCS' },
  { name: 'Mangochi Waste Co', logo: 'https://via.placeholder.com/150x80/32d296/ffffff?text=MWC' }
]



// Scroll to top functionality
const showScrollTop = ref(false)
const isLoading = ref(true)

// Cookie consent
const showCookieBanner = ref(false)
const showCookieSettings = ref(false)
const cookieConsent = ref({
  necessary: true, // Always true, cannot be disabled
  analytics: false,
  marketing: false
})

const handleScroll = () => {
  showScrollTop.value = window.scrollY > 300
}

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

const scrollToSection = (sectionId, closeOffcanvas = false) => {
  const element = document.getElementById(sectionId)
  if (element) {
    const headerOffset = 80 // Account for fixed navigation height
    const elementPosition = element.offsetTop
    const offsetPosition = elementPosition - headerOffset

    // Close offcanvas immediately before scrolling to prevent interference
    if (closeOffcanvas) {
      const offcanvas = document.getElementById('offcanvas-nav')
      if (offcanvas && offcanvas.classList.contains('uk-open')) {
        offcanvas.classList.remove('uk-open')
        document.body.classList.remove('uk-offcanvas-page')
        // Trigger UIKit's hide event to clean up
        const event = new Event('hidden')
        offcanvas.dispatchEvent(event)
      }
    }

    // Small delay to ensure offcanvas is fully closed before scrolling
    setTimeout(() => {
      window.scrollTo({
        top: offsetPosition,
        behavior: 'smooth'
      })
    }, closeOffcanvas ? 50 : 0)
  }
}

const checkCookieConsent = () => {
  const consent = localStorage.getItem('cookieConsent')
  if (!consent) {
    showCookieBanner.value = true
  } else {
    try {
      cookieConsent.value = { ...cookieConsent.value, ...JSON.parse(consent) }
    } catch (e) {
      showCookieBanner.value = true
    }
  }
}

const acceptAllCookies = () => {
  cookieConsent.value = {
    necessary: true,
    analytics: true,
    marketing: true
  }
  saveCookieConsent()
  showCookieBanner.value = false
}

const acceptEssentialCookies = () => {
  cookieConsent.value = {
    necessary: true,
    analytics: false,
    marketing: false
  }
  saveCookieConsent()
  showCookieBanner.value = false
}

const saveCookieConsent = () => {
  localStorage.setItem('cookieConsent', JSON.stringify(cookieConsent.value))
  localStorage.setItem('cookieConsentDate', new Date().toISOString())
  // Here you would initialize analytics/marketing tools based on consent
  if (cookieConsent.value.analytics) {
    // Initialize analytics (e.g., Google Analytics)
  }
  if (cookieConsent.value.marketing) {
    // Initialize marketing tools
  }
}

const openCookieSettings = () => {
  showCookieSettings.value = true
  showCookieBanner.value = false
}

const closeCookieSettings = () => {
  showCookieSettings.value = false
  if (!localStorage.getItem('cookieConsent')) {
    showCookieBanner.value = true
  }
}

const saveCookiePreferences = () => {
  saveCookieConsent()
  showCookieSettings.value = false
}

const toggleCookieCategory = (category) => {
  if (category !== 'necessary') {
    cookieConsent.value[category] = !cookieConsent.value[category]
  }
}


onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  // Simulate loading completion
  setTimeout(() => {
    isLoading.value = false
  }, 500)
  // Check cookie consent after a short delay
  setTimeout(() => {
    checkCookieConsent()
  }, 1000)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})

const features = [
  {
    title: 'For Companies',
    description: 'Complete control over operations, clients, and revenue with powerful administrative tools.',
    role: 'Company',
    icon: 'fa-solid fa-briefcase',
    details: [
      'Manage multiple locations, zones, and service areas',
      'Verify proof of payment submissions from clients',
      'Track upfront payments, collected payments, and unpaid clients',
      'Send SMS notifications and manage payment reminders',
      'View payment timeline, history, and generate detailed reports',
      'Configure payment methods (mobile money & bank transfers)',
      'Set up payment plans, billing dates, and VAT configuration',
      'Sync center for offline data synchronization',
      'Real-time revenue analytics and KPI dashboards'
    ]
  },
  {
    title: 'For Clients',
    description: 'Simple payment experience with proof of payment and instant receipts.',
    role: 'Client',
    icon: 'fa-solid fa-user',
    details: [
      'View outstanding invoices and payment history',
      'Submit proof of payment with screenshots (mobile money/bank)',
      'Submit proof of payment for multiple months in advance',
      'Receive instant payment receipts via SMS and email',
      'Track payment verification status in real-time',
      'Support for Airtel Money, TNM Mpamba, and major banks',
      'Offline proof of payment recording with automatic sync',
      'Receive automated payment reminders and company updates'
    ]
  },
  {
    title: 'For Cashiers',
    description: 'Field-ready tools for recording cash collections and managing client payments.',
    role: 'Cashier',
    icon: 'fa-solid fa-money-bill-transfer',
    details: [
      'Browse and search clients by assigned locations',
      'Record cash payments directly in the field',
      'View client payment history and outstanding balances',
      'Offline-first design with automatic data sync',
      'Track daily collection timelines and targets',
      'Real-time status updates visible to company managers',
      'Simple interface optimized for mobile devices'
    ]
  }
]

const pricing = [
  {
    name: 'Starter',
    price: '85000',
    priceFormatted: '85,000',
    currency: 'MK',
    duration: 'per month',
    target: 'Small startups',
    features: [
      'Up to 1,000 Clients',
      '3 Staff Seats',
      'Expense Tracking',
      'Digital Receipts',
      'Custom Branding (Sender ID)',
      'Priority Email Support'
    ],
    cta: 'Start 2-Month Free Trial',
    highlighted: false,
    lossAversion: 'Most companies outgrow this plan within 3-6 months'
  },
  {
    name: 'Growth',
    price: '147000',
    priceFormatted: '147,000',
    currency: 'MK',
    duration: 'per month',
    target: 'Growing companies',
    features: [
      'Up to 5,000 Clients',
      '10 Staff Seats',
      'Expense Tracking',
      'Digital Receipts',
      'Custom Branding (Sender ID)',
      'Priority Support (Email & Phone)'
    ],
    cta: 'Start 2-Month Free Trial',
    highlighted: true,
    lossAversion: 'Save MK 373,000/year vs Ultimate • Avoid costly upgrades later'
  },
  {
    name: 'Ultimate',
    price: '420000',
    priceFormatted: '420,000',
    currency: 'MK',
    duration: 'per month',
    target: 'Established enterprises',
    features: [
      'Up to 12,000 Clients',
      '25 Staff Seats',
      'Expense Tracking',
      'Digital Receipts',
      'Custom Branding (Sender ID)',
      'Priority Support (Email & Phone)'
    ],
    cta: 'Contact Sales',
    highlighted: false,
    lossAversion: 'You may be paying for features you don\'t need yet'
  }
]

const faqs = [
  {
    question: 'How long is the trial period?',
    answer: 'We offer a generous 2-month free trial period for new companies to explore all the features of Wastical without any commitment. No credit card required. You can cancel anytime during the trial.',
    category: 'Pricing',
    icon: 'fa-solid fa-calendar-check'
  },
  {
    question: 'Does the app work offline?',
    answer: 'Yes! The Company, Cashier, and Client modules are all designed to work offline. Payments, proof of payment submissions, and administrative data are stored locally and automatically sync when connectivity is restored. The sync center shows real-time sync status, so you never lose data.',
    category: 'Features',
    icon: 'fa-solid fa-wifi'
  },
  {
    question: 'What payment methods are supported?',
    answer: 'Wastical records proof of payment for mobile money (Airtel Money, TNM Mpamba) and bank transfers through Eco Bank, FDH Bank, First Capital Bank, Inde Bank, NBS Bank, National Bank, and Standard Bank. Clients submit proof of payment screenshots (transaction receipts) through the app for verification. The app does not process payments directly—it records and verifies proof of payment.',
    category: 'Payments',
    icon: 'fa-solid fa-credit-card'
  },
  {
    question: 'How does proof of payment verification work?',
    answer: 'Clients can submit payment screenshots (mobile money or bank transfers) directly through the app. Company administrators receive instant notifications and can verify payments, approve or reject them, with automatic status updates sent to clients via SMS. The verification process is streamlined and secure.',
    category: 'Payments',
    icon: 'fa-solid fa-circle-check'
  },
  {
    question: 'What happens if I exceed my plan limits?',
    answer: 'If you approach your plan limits (clients or staff seats), you\'ll receive notifications in advance. You can upgrade to a higher tier at any time, and the change takes effect immediately. We also offer flexible add-ons for temporary capacity increases.',
    category: 'Pricing',
    icon: 'fa-solid fa-chart-line'
  },
  {
    question: 'How do SMS notifications work?',
    answer: 'SMS notifications are sent automatically for payment reminders, collection schedules, payment confirmations, and important updates. You can customize message templates, schedule notifications, and track delivery status. SMS credits are included in your plan based on your tier.',
    category: 'Features',
    icon: 'fa-solid fa-message'
  },
  {
    question: 'Is my data secure?',
    answer: 'Absolutely. Wastical uses enterprise-grade security with encrypted data transmission, secure cloud storage, and regular backups. Your financial data and client information are protected with industry-standard security measures and compliance protocols.',
    category: 'Security',
    icon: 'fa-solid fa-lock'
  },
  {
    question: 'What kind of reports and analytics are available?',
    answer: 'Wastical provides comprehensive reporting including revenue analytics, payment timelines, client payment status, collection performance by location, staff productivity metrics, and financial summaries. Reports can be generated for daily, weekly, monthly, or custom date ranges.',
    category: 'Analytics',
    icon: 'fa-solid fa-chart-pie'
  },
  {
    question: 'Can clients pay multiple months in advance?',
    answer: 'Yes! Clients can submit proof of payment for multiple months in advance through the app. The system tracks "months covered" based on verified proof of payment submissions and automatically applies them to future billing cycles. This feature helps clients stay ahead and reduces administrative overhead.',
    category: 'Payments',
    icon: 'fa-solid fa-calendar-days'
  },
]
</script>

<template>
  <div class="page-shell uk-offcanvas-content">
    <!-- Navigation -->
    <header
      class="site-header"
      uk-sticky="sel-target: .uk-navbar-container; cls-active: uk-navbar-sticky"
    >
      <nav class="uk-navbar-container site-navbar" uk-navbar>
        <div class="uk-navbar-left">
          <a class="uk-navbar-item uk-logo logo-mark" @click.prevent="scrollToSection('hero')" href="#hero">
            <span class="logo-icon">
              <img src="./assets/logo.svg" alt="Wastical Logo" class="logo-svg">
            </span>
            <span class="logo-text">
              <span class="logo-primary">Wastical</span>
              <span class="logo-sub">Waste Intelligence Platform</span>
            </span>
          </a>
        </div>

        <div class="uk-navbar-right">
          <ul class="uk-navbar-nav uk-visible@m">
            <li><a @click.prevent="scrollToSection('features')" href="#features"><i class="fa-solid fa-cube uk-margin-small-right"></i>Product</a></li>
            <li><a @click.prevent="scrollToSection('how-it-works')" href="#how-it-works"><i class="fa-solid fa-gear uk-margin-small-right"></i>How it works</a></li>
            <li><a @click.prevent="scrollToSection('pricing')" href="#pricing"><i class="fa-solid fa-tag uk-margin-small-right"></i>Pricing</a></li>
            <li><a @click.prevent="scrollToSection('faq')" href="#faq"><i class="fa-solid fa-circle-question uk-margin-small-right"></i>FAQ</a></li>
          </ul>
          <a
            class="uk-navbar-toggle uk-hidden@m"
            uk-navbar-toggle-icon
            href="#offcanvas-nav"
            uk-toggle
          ></a>
        </div>
      </nav>
    </header>

    <!-- Mobile Nav -->
    <div id="offcanvas-nav" uk-offcanvas="overlay: true">
      <div class="uk-offcanvas-bar">
        <div class="offcanvas-header">
          <a
            class="offcanvas-logo"
            href="#hero"
            uk-toggle="target: #offcanvas-nav"
          >
            <span class="logo-icon">
              <img src="./assets/logo.svg" alt="Wastical Logo" class="logo-svg">
            </span>
            <span class="logo-text">
              <span class="logo-primary">Wastical</span>
              <span class="logo-sub">Waste Intelligence Platform</span>
            </span>
          </a>
          <button class="offcanvas-close" type="button" uk-close aria-label="Close navigation"></button>
        </div>

        <nav class="offcanvas-nav">
          <ul class="uk-nav uk-nav-default">
            <li>
              <a @click.prevent="scrollToSection('hero', true)" href="#hero">
                <i class="fa-solid fa-home uk-margin-small-right"></i>
                Home
              </a>
            </li>
            <li>
              <a @click.prevent="scrollToSection('features', true)" href="#features">
                <i class="fa-solid fa-cube uk-margin-small-right"></i>
                Product
              </a>
            </li>
            <li>
              <a @click.prevent="scrollToSection('how-it-works', true)" href="#how-it-works">
                <i class="fa-solid fa-gear uk-margin-small-right"></i>
                How it works
              </a>
            </li>
            <li>
              <a @click.prevent="scrollToSection('pricing', true)" href="#pricing">
                <i class="fa-solid fa-tag uk-margin-small-right"></i>
                Pricing
              </a>
            </li>
            <li>
              <a @click.prevent="scrollToSection('faq', true)" href="#faq">
                <i class="fa-solid fa-circle-question uk-margin-small-right"></i>
                FAQ
              </a>
            </li>
          </ul>

          <div class="offcanvas-cta">
            <div class="offcanvas-cta-buttons">
              <a
                @click.prevent="scrollToSection('contact', true)"
                href="#contact"
                class="uk-button uk-button-primary offcanvas-cta-primary"
              >
                <i class="fa-solid fa-play uk-margin-small-right"></i>
                Start free trial
              </a>
              <a
                @click.prevent="scrollToSection('pricing', true)"
                href="#pricing"
                class="uk-button uk-button-default offcanvas-cta-secondary"
              >
                <i class="fa-solid fa-tag uk-margin-small-right"></i>
                View pricing
              </a>
            </div>
          </div>
          <div class="offcanvas-footer">
            <p class="offcanvas-footer-copy">
              © {{ currentYear }} Wastical. All rights reserved.
            </p>
            <div class="offcanvas-footer-links">
              <a href="https://legal.wastical.app/privacy.html" target="_blank" rel="noopener noreferrer">
                Privacy
              </a>
              <span class="offcanvas-footer-separator">·</span>
              <a href="https://legal.wastical.app/terms.html" target="_blank" rel="noopener noreferrer">
                Terms
              </a>
            </div>
          </div>
        </nav>
      </div>
    </div>

    <!-- Hero -->
    <main>
      <section id="hero" class="hero-section uk-section">
        <div class="hero-background-shape"></div>
        <div class="uk-container">
          <div
            class="uk-grid-large uk-flex-middle hero-layout"
            uk-grid
          >
            <div class="uk-width-1-2@m">
              <div class="hero-eyebrow">
                <span class="hero-pill">
                  <i class="fa-solid fa-bolt"></i>
                  2‑Month free pilot for new cities
                </span>
              </div>
              <h1 class="hero-title">
                Operational excellence
                <span class="uk-text-primary">for modern waste teams</span>
              </h1>
              <p class="hero-subtitle">
                Wastical unifies billing, collections, and field operations into one
                clean dashboard—so you spend less time chasing payments and more time
                running a cleaner city.
              </p>

              <div
                class="uk-flex uk-flex-middle uk-flex-wrap hero-actions"
                uk-grid
              >
                <div>
                  <a
                    @click.prevent="scrollToSection('contact')"
                    href="#contact"
                    class="uk-button uk-button-primary uk-button-large"
                  >
                    Start free trial
                  </a>
                </div>
                <div>
                  <a
                    @click.prevent="scrollToSection('features')"
                    href="#features"
                    class="uk-button uk-button-default uk-button-large"
                  >
                    View product tour
                  </a>
                </div>
              </div>

              <div class="hero-meta uk-flex uk-flex-middle uk-flex-wrap">
                <div class="hero-meta-item">
                  <i class="fa-solid fa-circle-check"></i>
                  No credit card required
                </div>
                <div class="hero-meta-item">
                  <i class="fa-solid fa-users"></i>
                  Built for companies, clients & cashiers
                </div>
              </div>
            </div>

            <div class="uk-width-1-2@m">
              <div class="app-screenshot">
                <div class="screenshot-card">
                  <img
                    src="./assets/image/Artboard 1@2x.jpg"
                    alt="Wastical App Screenshot"
                    class="screenshot-image"
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Metrics strip -->
      <section class="metrics-strip">
        <div class="uk-container">
          <div class="metrics-grid">
            <div class="metric-card">
              <div class="metric-icon">
                <i class="fa-solid fa-bolt"></i>
              </div>
              <div class="metric-content">
                <p class="metric-number">3x</p>
                <p class="metric-label">Faster revenue reconciliation</p>
              </div>
            </div>
            <div class="metric-card">
              <div class="metric-icon">
                <i class="fa-solid fa-arrow-trend-down"></i>
              </div>
              <div class="metric-content">
                <p class="metric-number">40%</p>
                <p class="metric-label">Average reduction in missed pickups</p>
              </div>
            </div>
            <div class="metric-card">
              <div class="metric-icon">
                <i class="fa-solid fa-eye"></i>
              </div>
              <div class="metric-content">
                <p class="metric-number">24/7</p>
                <p class="metric-label">Visibility for management & field teams</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Product roles -->
      <section id="features" class="section-muted uk-section">
        <div class="uk-container">
          <div class="section-heading uk-text-center">
            <p class="section-eyebrow">Built for your whole operation</p>
            <h2 class="uk-h1 uk-text-bold">One platform, three powerful workspaces</h2>
            <p class="uk-text-lead uk-text-muted">
              Wastical adapts to each role in your organization while keeping everyone
              connected to the same live data.
            </p>
          </div>

          <div class="features-grid">
            <div
              v-for="(feature, index) in features"
              :key="feature.role"
              class="feature-card-modern"
              :class="`feature-card-${index + 1}`"
            >
              <div class="feature-card-header">
                <div class="feature-icon-wrapper">
                  <i :class="feature.icon"></i>
                </div>
                <div class="feature-badge">{{ feature.role }}</div>
              </div>
              <div class="feature-card-content">
                <h3 class="feature-title">{{ feature.title }}</h3>
                <p class="feature-description">{{ feature.description }}</p>
                <ul class="feature-list-modern">
                  <li
                    v-for="detail in feature.details"
                    :key="detail"
                    class="feature-list-item"
                  >
                    <i class="fa-solid fa-check"></i>
                    <span>{{ detail }}</span>
                  </li>
                </ul>
              </div>
            </div>
          </div>
          <br>
          <br>
        </div>
      </section>

      <!-- How it works -->
      <section id="how-it-works" class="uk-section how-it-works-section">
        <div class="uk-container">
          <div class="section-heading uk-text-center">
            <p class="section-eyebrow">From signup to first collection</p>
            <h2 class="uk-h1 uk-text-bold">Go live in days, not months</h2>
            <p class="uk-text-lead uk-text-muted">
              We've simplified rollout so you can pilot Wastical in one zone before
              scaling across your entire city.
            </p>
          </div>

          <div class="steps-container">
            <div class="step-item">
              <div class="step-content">
                <div class="step-icon">
                  <i class="fa-solid fa-gear"></i>
                </div>
                <h3 class="step-title">Configure your company</h3>
                <p class="step-text">
                  Define locations, zones, services, and tariffs with guided onboarding
                  from our implementation team.
                </p>
              </div>
            </div>

            <div class="step-item">
              <div class="step-content">
                <div class="step-icon">
                  <i class="fa-solid fa-users"></i>
                </div>
                <h3 class="step-title">Invite staff & import clients</h3>
                <p class="step-text">
                  Create admin and cashier accounts, then import clients from
                  Excel or your existing system. Configure payment methods and billing plans.
                </p>
              </div>
            </div>

            <div class="step-item">
              <div class="step-content">
                <div class="step-icon">
                  <i class="fa-solid fa-chart-line"></i>
                </div>
                <h3 class="step-title">Start tracking real results</h3>
                <p class="step-text">
                  Cashiers record payments in the field while managers monitor revenue,
                  collections, and SMS engagement in real‑time.
                </p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Pricing -->
      <section id="pricing" class="uk-section uk-section-secondary pricing-section">
        <div class="uk-container">
          <div class="pricing-top uk-text-center">
            <div class="trial-badge">2‑MONTH PILOT INCLUDED</div>
            <h2 class="uk-h1 uk-text-bold">Choose the velocity that fits</h2>
            <p class="uk-text-lead">
              Three clear tiers for teams at different stages. Everything stays aligned in one row,
              with identical heights so comparisons are easy.
            </p>
          </div>

          <div class="pricing-row">
            <div
              v-for="plan in pricing"
              :key="plan.name"
              class="pricing-card-horizontal"
              :class="{ 'pricing-card-highlighted': plan.highlighted }"
            >
              <div class="pricing-card-inner">
                <div class="pricing-card-head">
                  <div>
                    <p class="plan-kicker">Per company · SaaS</p>
                    <h3 class="pricing-plan-name">{{ plan.name }}</h3>
                    <p class="pricing-plan-target">{{ plan.target }}</p>
                  </div>
                  <div v-if="plan.highlighted" class="pricing-flag">
                    <i class="fa-solid fa-star"></i>
                    Most popular
                  </div>
                </div>

                <div class="pricing-card-body">
                  <div class="price-stack">
                    <div class="price-amount">
                      <span class="pricing-currency">{{ plan.currency }}</span>
                      <span class="pricing-number">{{ plan.priceFormatted }}</span>
                      <span class="pricing-duration">/{{ plan.duration.replace('per ', '') }}</span>
                    </div>
                    <p class="price-note">Includes pilot, onboarding, and exports.</p>
                  </div>

                  <ul class="feature-list">
                    <li v-for="f in plan.features" :key="f">
                      <i class="fa-solid fa-check"></i>
                      <span>{{ f }}</span>
                    </li>
                  </ul>
                </div>

                <div class="pricing-card-foot">
                  <!-- <div v-if="plan.lossAversion" class="loss-aversion-message"> -->
                    <!-- <i class="fa-solid fa-exclamation-triangle"></i> -->
                    <!-- <span>{{ plan.lossAversion }}</span> -->
                  <!-- </div> -->
                  <a
                    @click.prevent="scrollToSection('contact')"
                    href="#contact"
                    class="uk-button uk-button-large"
                    :class="plan.highlighted ? 'uk-button-primary' : 'uk-button-default'"
                  >
                    {{ plan.cta }}
                  </a>
                  <p class="footnote">
                    <i class="fa-solid fa-circle-info"></i>
                    Cancel anytime during pilot. Data is yours.
                  </p>
                </div>
              </div>
            </div>
          </div>

          <div class="pricing-footbar uk-light">
            <div class="pill small"><i class="fa-solid fa-shield-heart"></i> Security monitoring</div>
            <div class="pill small"><i class="fa-solid fa-headset"></i> Priority support</div>
            <div class="pill small"><i class="fa-solid fa-graduation-cap"></i> Guided onboarding</div>
            <div class="pill small"><i class="fa-solid fa-cloud-arrow-down"></i> Client Data import</div>
          </div>
        </div>
      </section>

      <!-- FAQ -->
      <section id="faq" class="uk-section section-muted faq-section">
        <div class="uk-container">
          <div class="section-heading uk-text-center">
            <p class="section-eyebrow">Answers for operations, finance & IT</p>
            <h2 class="uk-h1 uk-text-bold">Frequently asked questions</h2>
            <p class="uk-text-lead uk-margin-small-top">
              Everything you need to know about Wastical
            </p>
          </div>

          <div class="faq-container">
            <ul uk-accordion="collapsible: true; animation: true; duration: 300" class="faq-accordion">
              <li
                v-for="(faq, index) in faqs"
                :key="index"
                :class="{ 'uk-open': index === 0, [`faq-category-${faq.category.toLowerCase()}`]: true }"
                class="faq-item"
              >
                <a class="uk-accordion-title faq-question" href>
                  <div class="faq-question-content">
                    <i :class="`${faq.icon} faq-icon faq-icon-${faq.category.toLowerCase()}`"></i>
                    <span class="faq-question-text">{{ faq.question }}</span>
                  </div>
                  <i class="fa-solid fa-chevron-down faq-chevron"></i>
                </a>
                <div class="uk-accordion-content faq-answer">
                  <div class="faq-answer-content">
                    <span class="faq-category-badge" :class="`faq-badge-${faq.category.toLowerCase()}`">{{ faq.category }}</span>
                    <p>{{ faq.answer }}</p>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </section>

      <!-- Security & Privacy -->
      <section id="security" class="uk-section section-muted">
        <div class="uk-container">
          <div class="section-heading uk-text-center">
            <p class="section-eyebrow">Enterprise-grade security</p>
            <h2 class="uk-h1 uk-text-bold">Your data is protected</h2>
            <p class="uk-text-lead uk-text-muted">
              We take security seriously with industry-standard encryption and compliance protocols.
            </p>
          </div>

          <div class="security-grid">
            <div class="security-badge-card">
              <div class="security-icon-wrapper">
                <i class="fa-solid fa-lock"></i>
              </div>
              <h3 class="security-title">SSL Encryption</h3>
              <p class="security-description">All data transmitted over 256-bit SSL/TLS encryption</p>
            </div>

            <div class="security-badge-card">
              <div class="security-icon-wrapper">
                <i class="fa-solid fa-shield-halved"></i>
              </div>
              <h3 class="security-title">Data Encryption</h3>
              <p class="security-description">Data at rest encrypted with AES-256 encryption</p>
            </div>

            <div class="security-badge-card">
              <div class="security-icon-wrapper">
                <i class="fa-solid fa-database"></i>
              </div>
              <h3 class="security-title">Secure Backups</h3>
              <p class="security-description">Automated daily backups with 30-day retention</p>
            </div>

            <div class="security-badge-card">
              <div class="security-icon-wrapper">
                <i class="fa-solid fa-user-shield"></i>
              </div>
              <h3 class="security-title">Access Control</h3>
              <p class="security-description">Role-based permissions and multi-factor authentication</p>
            </div>

            <div class="security-badge-card">
              <div class="security-icon-wrapper">
                <i class="fa-solid fa-file-shield"></i>
              </div>
              <h3 class="security-title">Privacy Compliant</h3>
              <p class="security-description">GDPR compliant with transparent data handling</p>
            </div>

            <div class="security-badge-card">
              <div class="security-icon-wrapper">
                <i class="fa-solid fa-server"></i>
              </div>
              <h3 class="security-title">Secure Infrastructure</h3>
              <p class="security-description">Hosted on enterprise-grade cloud infrastructure</p>
            </div>
          </div>
        </div>
      </section>

      <!-- App Store Badges -->
      <section class="uk-section app-store-section">
        <div class="uk-container">
          <div class="app-store-content uk-text-center">
            <h3 class="app-store-title">Get Wastical on your mobile device</h3>
            <p class="app-store-description">Download our mobile app for iOS and Android</p>
            <div class="app-store-badges">
              <a 
                href="https://play.google.com/store/apps/details?id=net.techandgraphics.wastical" 
                target="_blank" 
                rel="noopener noreferrer"
                class="app-store-badge"
              >
                <div class="app-badge-custom app-badge-android">
                  <i class="fa-brands fa-google-play"></i>
                  <div class="app-badge-text">
                    <span class="app-badge-top">GET IT ON</span>
                    <span class="app-badge-bottom">Google Play</span>
                  </div>
                </div>
              </a>

              <div class="app-store-badge app-store-badge-coming-soon">
                <div class="app-badge-custom app-badge-ios">
                  <i class="fa-brands fa-apple"></i>
                  <div class="app-badge-text">
                    <span class="app-badge-top">Coming Soon</span>
                    <span class="app-badge-bottom">App Store</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Dedicated CTA Section -->
      <section class="uk-section cta-section">
        <div class="cta-background"></div>
        <div class="uk-container">
          <div class="cta-content">
            <div class="cta-badge">
              <i class="fa-solid fa-rocket"></i>
              Start your free 2-month pilot today
            </div>
            <h2 class="cta-title">
              Ready to transform your waste management operations?
            </h2>
            <p class="cta-description">
              Join leading waste management companies using Wastical to streamline billing, 
              improve collections, and gain real-time visibility into operations. 
              No credit card required. Cancel anytime.
            </p>
            <div class="cta-actions">
              <a @click.prevent="scrollToSection('contact')" href="#contact" class="uk-button uk-button-primary  cta-button-primary">
                <i class="fa-solid fa-play uk-margin-small-right"></i>
                Start Free Trial
              </a>
              <a @click.prevent="scrollToSection('pricing')" href="#pricing" class="uk-button uk-button-default cta-button-secondary">
                <i class="fa-solid fa-tag uk-margin-small-right"></i>
                View Pricing
              </a>
            </div>
            <div class="cta-trust-indicators">
              <div class="cta-trust-item">
                <i class="fa-solid fa-shield-check"></i>
                <span>No credit card required</span>
              </div>
              <div class="cta-trust-item">
                <i class="fa-solid fa-clock"></i>
                <span>2-month free pilot</span>
              </div>
              <div class="cta-trust-item">
                <i class="fa-solid fa-xmark-circle"></i>
                <span>Cancel anytime</span>
              </div>
              <div class="cta-trust-item">
                <i class="fa-solid fa-download"></i>
                <span>Export your data</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Contact / Footer Style -->
      <section id="contact" class="uk-section section-contact-footer">
        <div class="uk-container">
          <br>
          <br>
          <div class="contact-section-wrapper">
            <div class="contact-header uk-text-center">
              <div class="contact-badge">
                <i class="fa-solid fa-headset"></i>
                Get in touch
              </div>
              <h2 class="contact-main-title">Talk to our team</h2>
              <p class="contact-main-description">
                Have questions? Need support? Want to see a demo? We're here to help you succeed.
              </p>
            </div>

            <div class="contact-content-grid">
              <div class="contact-card">
                <div class="contact-card-icon">
                  <i class="fa-solid fa-envelope"></i>
                </div>
                <h3 class="contact-card-title">Email Us</h3>
                <p class="contact-card-description">Send us an email and we'll get back to you within 24 hours</p>
                <a href="mailto:support@wastical.app" class="contact-card-link">
                  support@wastical.app
                  <i class="fa-solid fa-arrow-right"></i>
                </a>
              </div>

              <div class="contact-card">
                <div class="contact-card-icon">
                  <i class="fa-solid fa-phone"></i>
                </div>
                <h3 class="contact-card-title">Call Us</h3>
                <p class="contact-card-description">Speak directly with our team during business hours</p>
                <a href="tel:+265980127007" class="contact-card-link">
                  +265 980 127 007
                  <i class="fa-solid fa-arrow-right"></i>
                </a>
              </div>

              <div class="contact-card">
                <div class="contact-card-icon">
                  <i class="fa-brands fa-whatsapp"></i>
                </div>
                <h3 class="contact-card-title">WhatsApp</h3>
                <p class="contact-card-description">Chat with us instantly via WhatsApp</p>
                <a href="https://wa.me/265980127007" target="_blank" rel="noopener noreferrer" class="contact-card-link">
                  +265 980 127 007
                  <i class="fa-solid fa-arrow-right"></i>
                </a>
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- Footer -->
    <footer class="site-footer uk-section uk-section-small">
      <div class="uk-container">
        <div class="uk-flex uk-flex-between uk-flex-middle uk-flex-wrap">
          <div>
            <p class="uk-margin-remove">
              © {{ currentYear }} Wastical. All rights reserved.
            </p>
          </div>
          <div class="footer-links">
            <a href="https://legal.wastical.app/privacy.html" target="_blank" rel="noopener noreferrer" class="uk-margin-small-right">Privacy Policy</a>
            <a href="https://legal.wastical.app/terms.html" target="_blank" rel="noopener noreferrer" class="">Terms of Service</a>
          </div>
        </div>
      </div>
    </footer>

    <!-- Scroll to Top Button -->
    <button
      v-if="showScrollTop"
      @click="scrollToTop"
      class="scroll-to-top"
      aria-label="Scroll to top"
    >
      <i class="fa-solid fa-arrow-up"></i>
    </button>

    <!-- Loading Overlay -->
    <div v-if="isLoading" class="loading-overlay">
      <div class="loading-content">
        <div class="loading-spinner"></div>
        <p class="loading-text">Loading Wastical...</p>
      </div>
    </div>

    <!-- Cookie Consent Banner -->
    <div v-if="showCookieBanner" class="cookie-consent-banner">
      <div class="cookie-consent-content">
        <div class="cookie-consent-text">
          <div class="cookie-consent-icon">
            <i class="fa-solid fa-cookie-bite"></i>
          </div>
          <div class="cookie-consent-message">
            <h4 class="cookie-consent-title">We value your privacy</h4>
            <p class="cookie-consent-description">
              We use cookies to enhance your browsing experience, analyze site traffic, and personalize content. 
              By clicking "Accept All", you consent to our use of cookies. You can also choose to accept only essential cookies 
              or manage your preferences. 
              <a href="https://legal.wastical.app/privacy.html" target="_blank" rel="noopener noreferrer" class="cookie-link">
                Learn more in our Privacy Policy
              </a>.
            </p>
          </div>
        </div>
        <div class="cookie-consent-actions">
          <button @click="acceptEssentialCookies" class="cookie-button cookie-button-secondary">
            Essential Only
          </button>
          <button @click="openCookieSettings" class="cookie-button cookie-button-settings">
            <i class="fa-solid fa-sliders"></i>
            Settings
          </button>
          <button @click="acceptAllCookies" class="cookie-button cookie-button-primary">
            Accept All
          </button>
        </div>
      </div>
    </div>

    <!-- Cookie Settings Modal -->
    <div v-if="showCookieSettings" class="cookie-settings-modal" @click.self="closeCookieSettings">
      <div class="cookie-settings-content">
        <div class="cookie-settings-header">
          <h3 class="cookie-settings-title">Cookie Preferences</h3>
          <button @click="closeCookieSettings" class="cookie-settings-close" aria-label="Close">
            <i class="fa-solid fa-times"></i>
          </button>
        </div>
        
        <div class="cookie-settings-body">
          <p class="cookie-settings-intro">
            We use cookies to improve your experience on our site. You can choose which cookies you want to accept. 
            <a href="https://legal.wastical.app/privacy.html" target="_blank" rel="noopener noreferrer" class="cookie-link">
              Read our Privacy Policy
            </a> for more information.
          </p>

          <!-- Necessary Cookies -->
          <div class="cookie-category">
            <div class="cookie-category-header">
              <div class="cookie-category-info">
                <h4 class="cookie-category-title">Necessary Cookies</h4>
                <p class="cookie-category-description">
                  These cookies are essential for the website to function properly. They cannot be disabled.
                </p>
              </div>
              <div class="cookie-toggle disabled">
                <input type="checkbox" checked disabled id="cookie-necessary">
                <label for="cookie-necessary" class="cookie-toggle-label"></label>
              </div>
            </div>
            <ul class="cookie-category-list">
              <li>Session management</li>
              <li>Security and authentication</li>
              <li>Load balancing</li>
            </ul>
          </div>

          <!-- Analytics Cookies -->
          <div class="cookie-category">
            <div class="cookie-category-header">
              <div class="cookie-category-info">
                <h4 class="cookie-category-title">Analytics Cookies</h4>
                <p class="cookie-category-description">
                  These cookies help us understand how visitors interact with our website by collecting and reporting information anonymously.
                </p>
              </div>
              <div class="cookie-toggle">
                <input 
                  type="checkbox" 
                  :checked="cookieConsent.analytics" 
                  @change="toggleCookieCategory('analytics')"
                  id="cookie-analytics"
                >
                <label for="cookie-analytics" class="cookie-toggle-label"></label>
              </div>
            </div>
            <ul class="cookie-category-list">
              <li>Page views and navigation</li>
              <li>User behavior analysis</li>
              <li>Performance metrics</li>
            </ul>
          </div>

          <!-- Marketing Cookies -->
          <div class="cookie-category">
            <div class="cookie-category-header">
              <div class="cookie-category-info">
                <h4 class="cookie-category-title">Marketing Cookies</h4>
                <p class="cookie-category-description">
                  These cookies are used to deliver personalized advertisements and track campaign performance.
                </p>
              </div>
              <div class="cookie-toggle">
                <input 
                  type="checkbox" 
                  :checked="cookieConsent.marketing" 
                  @change="toggleCookieCategory('marketing')"
                  id="cookie-marketing"
                >
                <label for="cookie-marketing" class="cookie-toggle-label"></label>
              </div>
            </div>
            <ul class="cookie-category-list">
              <li>Ad personalization</li>
              <li>Campaign tracking</li>
              <li>Conversion measurement</li>
            </ul>
          </div>
        </div>

        <div class="cookie-settings-footer">
          <button @click="closeCookieSettings" class="cookie-button cookie-button-secondary">
            Cancel
          </button>
          <button @click="saveCookiePreferences" class="cookie-button cookie-button-primary">
            Save Preferences
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Layout-specific overrides live in style.css */
</style>
