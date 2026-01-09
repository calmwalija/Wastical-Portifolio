# Wastical - Waste Intelligence Platform

[![Vue 3](https://img.shields.io/badge/Vue-3.5.24-4FC08D?style=flat&logo=vue.js)](https://vuejs.org/)
[![Vite](https://img.shields.io/badge/Vite-7.2.4-646CFF?style=flat&logo=vite)](https://vitejs.dev/)
[![UIKit](https://img.shields.io/badge/UIKit-3.25.4-2396F3?style=flat)](https://getuikit.com/)

A modern, responsive portfolio website for Wastical - the comprehensive waste intelligence platform that transforms waste management operations through unified billing, collections, and field operations.

## Features

### Core Capabilities
- **Unified Operations Dashboard** - Single platform for billing, collections, and field operations
- **Real-time Analytics** - Live revenue tracking, payment timelines, and KPI dashboards
- **Multi-role Support** - Tailored interfaces for administrators, cashiers, and field operators
- **Payment Processing** - Support for mobile money (Airtel Money, TNM Mpamba) and bank transfers
- **Offline Capability** - Sync center for offline data synchronization
- **Advanced Reporting** - Comprehensive analytics with custom date ranges

### User Roles
- **Company Administrators** - Full control over operations, clients, and revenue
- **Cashiers** - Field-ready tools for recording collections and managing payments
- **Field Operators** - Mobile-optimized interface for waste collection management

### Mobile-First Design
- Responsive design optimized for all devices
- Mobile navigation with smooth scrolling
- Progressive Web App capabilities
- Native mobile app availability (iOS/Android)

## Live Demo

Visit the live portfolio at: **[wastical.app](https://wastical.app)**

## Tech Stack

- **Frontend Framework**: Vue 3 with Composition API
- **Build Tool**: Vite
- **UI Framework**: UIKit 3
- **Icons**: Font Awesome 6
- **Deployment**: Docker + Caddy web server

## Installation

### Prerequisites
- Node.js 18+ and npm
- Git

### Setup
```bash
# Clone the repository
git clone https://github.com/calmwalija/Wastical-Portifolio.git
cd Wastical-Portifolio

# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Deployment

This project includes automated deployment scripts for VPS hosting. See [DEPLOYMENT.md](./DEPLOYMENT.md) for detailed deployment instructions.

### Quick Deploy (One-liner)
```bash
ssh root@YOUR_VPS_IP "cd /root/wastical-portfolio && git pull origin main && npm install && npm run build && rm -rf /var/www/html/portfolio/* && cp -r dist/* /var/www/html/portfolio/ && docker restart caddy"
```

## Project Structure

```
wastical-portifolio/
├── src/
│   ├── assets/          # Static assets (images, icons)
│   ├── components/      # Vue components
│   ├── App.vue         # Main application component
│   ├── main.js         # Application entry point
│   └── style.css       # Global styles
├── public/             # Public static files
├── dist/               # Production build output
├── DEPLOYMENT.md       # Deployment documentation
└── README.md          # This file
```

## Key Sections

### Hero Section
- Compelling headline and value proposition
- Clear call-to-action buttons
- Mobile-optimized layout

### Features Overview
- Product capabilities showcase
- User role demonstrations
- Visual feature highlights

### Pricing Information
- Transparent pricing structure
- Free trial offer (2 months, no credit card required)
- Flexible cancellation policy

### FAQ Section
- Comprehensive answers for operations, finance, and IT
- Payment processing details
- Security and compliance information

### Contact & Support
- Direct email support: support@wastical.app
- Mobile app download links
- Legal documentation links

## Security & Privacy

- Enterprise-grade security with encrypted data transmission
- Secure cloud storage with regular backups
- Compliance with industry-standard security protocols
- Transparent privacy policy and terms of service

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

## License

© 2025 Wastical. All rights reserved.

## Support

- **Email**: support@wastical.app
- **Website**: [wastical.app](https://wastical.app)
- **Privacy Policy**: [legal.wastical.app/privacy.html](https://legal.wastical.app/privacy.html)
- **Terms of Service**: [legal.wastical.app/terms.html](https://legal.wastical.app/terms.html)

## About Wastical

Wastical is revolutionizing waste management by providing cities and waste companies with intelligent software solutions that streamline billing, improve collections, and provide real-time visibility into operations. Our platform helps waste management companies spend less time chasing payments and more time running cleaner cities.

---

Built with Vue 3, Vite, and modern web technologies.
