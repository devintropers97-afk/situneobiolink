<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Home Page
 * 26 Layanan Digital Lengkap
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

session_start();
date_default_timezone_set('Asia/Jakarta');

// Multi-language content
 $text = [
    'id' => [
        'page_title' => 'SITUNEO DIGITAL - Solusi Digital Terlengkap',
        'page_subtitle' => '26 Layanan Digital Profesional untuk Mengembangkan Bisnis Anda',
        'hero_title' => 'Digital Harmony for a Modern World',
        'hero_subtitle' => 'Solusi digital terlengkap untuk mengembangkan bisnis Anda di era modern',
        'btn_get_started' => 'Mulai Sekarang',
        'btn_view_services' => 'Lihat Layanan',
        'btn_login' => 'Masuk',
        'btn_register' => 'Daftar',
        'section_services_title' => 'Layanan Unggulan',
        'section_services_subtitle' => 'Pilih dari 26 layanan digital profesional kami',
        'section_portfolio_title' => 'Portfolio Kami',
        'section_portfolio_subtitle' => 'Hasil kerja kami untuk klien-klien terbaik',
        'section_testimonial_title' => 'Testimoni Klien',
        'section_testimonial_subtitle' => 'Apa kata klien tentang layanan kami',
        'section_pricing_title' => 'Paket Harga',
        'section_pricing_subtitle' => 'Pilih paket yang sesuai dengan kebutuhan Anda',
        'section_contact_title' => 'Hubungi Kami',
        'section_contact_subtitle' => 'Siap membantu mengembangkan bisnis digital Anda',
        'footer_about' => 'SITUNEO DIGITAL adalah penyedia layanan digital terlengkap di Indonesia. Kami menyediakan 26 layanan digital profesional untuk membantu mengembangkan bisnis Anda.',
        'footer_services' => 'Layanan',
        'footer_company' => 'Perusahaan',
        'footer_support' => 'Dukungan',
        'footer_copyright' => '© 2023 SITUNEO DIGITAL. All rights reserved.',
        'nav_home' => 'Beranda',
        'nav_about' => 'Tentang Kami',
        'nav_services' => 'Layanan',
        'nav_portfolio' => 'Portfolio',
        'nav_pricing' => 'Harga',
        'nav_contact' => 'Hubungi',
        'nav_calculator' => 'Hitung Harga',
        'nav_dashboard' => 'Dashboard',
        'nav_logout' => 'Keluar',
    ],
    'en' => [
        'page_title' => 'SITUNEO DIGITAL - Complete Digital Solutions',
        'page_subtitle' => '26 Professional Digital Services to Grow Your Business',
        'hero_title' => 'Digital Harmony for a Modern World',
        'hero_subtitle' => 'Complete digital solutions to grow your business in the modern era',
        'btn_get_started' => 'Get Started',
        'btn_view_services' => 'View Services',
        'btn_login' => 'Login',
        'btn_register' => 'Register',
        'section_services_title' => 'Featured Services',
        'section_services_subtitle' => 'Choose from our 26 professional digital services',
        'section_portfolio_title' => 'Our Portfolio',
        'section_portfolio_subtitle' => 'Our work for the best clients',
        'section_testimonial_title' => 'Client Testimonials',
        'section_testimonial_subtitle' => 'What our clients say about our services',
        'section_pricing_title' => 'Pricing Plans',
        'section_pricing_subtitle' => 'Choose the plan that suits your needs',
        'section_contact_title' => 'Contact Us',
        'section_contact_subtitle' => 'Ready to help develop your digital business',
        'footer_about' => 'SITUNEO DIGITAL is the most complete digital service provider in Indonesia. We provide 26 professional digital services to help grow your business.',
        'footer_services' => 'Services',
        'footer_company' => 'Company',
        'footer_support' => 'Support',
        'footer_copyright' => '© 2023 SITUNEO DIGITAL. All rights reserved.',
        'nav_home' => 'Home',
        'nav_about' => 'About Us',
        'nav_services' => 'Services',
        'nav_portfolio' => 'Portfolio',
        'nav_pricing' => 'Pricing',
        'nav_contact' => 'Contact',
        'nav_calculator' => 'Price Calculator',
        'nav_dashboard' => 'Dashboard',
        'nav_logout' => 'Logout',
    ]
];

 $lang = $_GET['lang'] ?? $_SESSION['lang'] ?? 'id';
 $_SESSION['lang'] = $lang;
 $t = $text[$lang];

// Check if user is logged in
 $isLoggedIn = isset($_SESSION['user_id']);
 $userName = $isLoggedIn ? $_SESSION['user_name'] : '';
 $userRole = $isLoggedIn ? $_SESSION['user_role'] : 0;
?>

<!DOCTYPE html>
<html lang="<?= $lang ?>">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?= $t['page_title'] ?></title>
    <meta name="description" content="<?= $t['page_subtitle'] ?>">
    <meta name="keywords" content="digital services, web development, SEO, digital marketing, SITUNEO DIGITAL">
    
    <!-- Favicon -->
    <link rel="icon" type="image/png" href="https://situneo.my.id/logo">
    
    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=Plus+Jakarta+Sans:wght@400;600;700;800;900&display=swap" rel="stylesheet">
    
    <!-- CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css" rel="stylesheet">
    <link rel="stylesheet" href="https://unpkg.com/aos@2.3.1/dist/aos.css">
    
    <style>
        :root {
            --primary-blue: #1E5C99;
            --dark-blue: #0F3057;
            --gold: #FFB400;
            --bright-gold: #FFD700;
            --white: #ffffff;
            --text-light: #e9ecef;
            --gradient-primary: linear-gradient(135deg, #1E5C99 0%, #0F3057 100%);
            --gradient-gold: linear-gradient(135deg, #FFD700 0%, #FFB400 100%);
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background: var(--dark-blue);
            color: var(--white);
            overflow-x: hidden;
        }
        
        /* Network Background */
        .network-bg {
            position: fixed;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            z-index: -2;
            overflow: hidden;
            opacity: 0.3;
        }
        
        .network-bg canvas {
            width: 100%;
            height: 100%;
        }
        
        /* Circuit Pattern */
        .circuit-pattern {
            position: fixed;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            z-index: -1;
            opacity: 0.05;
            background-image: 
                linear-gradient(90deg, var(--gold) 1px, transparent 1px),
                linear-gradient(180deg, var(--gold) 1px, transparent 1px);
            background-size: 50px 50px;
            animation: circuit-move 60s linear infinite;
        }
        
        @keyframes circuit-move {
            0% { transform: translate(0, 0); }
            100% { transform: translate(50px, 50px); }
        }
        
        /* Navbar */
        .navbar-premium {
            background: rgba(15, 48, 87, 0.95);
            backdrop-filter: blur(20px);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            padding: 1rem 0;
            position: fixed;
            width: 100%;
            top: 0;
            z-index: 998;
            border-bottom: 1px solid rgba(255, 180, 0, 0.2);
            transition: all 0.3s ease;
        }
        
        .navbar-premium.scrolled {
            padding: 0.5rem 0;
            background: rgba(15, 48, 87, 0.98);
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
        }
        
        .nav-link {
            color: var(--white) !important;
            font-weight: 500;
            margin: 0 10px;
            transition: all 0.3s;
            position: relative;
        }
        
        .nav-link::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 50%;
            width: 0;
            height: 2px;
            background: var(--gold);
            transition: all 0.3s;
            transform: translateX(-50%);
        }
        
        .nav-link:hover::after {
            width: 100%;
        }
        
        .nav-link:hover {
            color: var(--gold) !important;
            transform: translateY(-2px);
        }
        
        .nav-link.active {
            color: var(--gold) !important;
        }
        
        .nav-link.active::after {
            width: 100%;
        }
        
        /* Language Switcher */
        .lang-switcher {
            display: flex;
            gap: 5px;
        }

        .lang-btn {
            padding: 5px 12px;
            border-radius: 20px;
            background: rgba(255,180,0,0.1);
            border: 1px solid rgba(255,180,0,0.3);
            color: var(--text-light);
            text-decoration: none;
            font-size: 0.85rem;
            transition: all 0.3s;
        }

        .lang-btn:hover, .lang-btn.active {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: var(--gold);
            font-weight: 700;
        }
        
        /* Hero Section */
        .hero {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 150px 20px 80px;
            position: relative;
            background: radial-gradient(ellipse at top, rgba(255, 180, 0, 0.1) 0%, transparent 50%);
        }
        
        .hero-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: clamp(2.5rem, 5vw, 4rem);
            font-weight: 800;
            margin-bottom: 1.5rem;
            background: var(--gradient-gold);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .hero-subtitle {
            font-size: 1.3rem;
            color: var(--text-light);
            margin-bottom: 2rem;
            max-width: 700px;
        }
        
        .btn-gold {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 15px 35px;
            font-weight: 700;
            border-radius: 50px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            transition: all 0.3s;
            text-decoration: none;
            display: inline-block;
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.3);
            margin-right: 15px;
            margin-bottom: 15px;
        }
        
        .btn-gold:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255, 180, 0, 0.6);
            color: var(--dark-blue);
        }
        
        .btn-outline-gold {
            background: transparent;
            color: var(--gold);
            border: 2px solid var(--gold);
            padding: 15px 35px;
            font-weight: 700;
            border-radius: 50px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            transition: all 0.3s;
            text-decoration: none;
            display: inline-block;
            margin-right: 15px;
            margin-bottom: 15px;
        }
        
        .btn-outline-gold:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255, 180, 0, 0.6);
        }
        
        /* Section Styles */
        .section {
            padding: 80px 0;
            position: relative;
        }
        
        .section-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: clamp(2rem, 4vw, 3rem);
            font-weight: 800;
            text-align: center;
            margin-bottom: 1rem;
            background: var(--gradient-gold);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .section-subtitle {
            text-align: center;
            color: var(--text-light);
            max-width: 700px;
            margin: 0 auto 3rem;
        }
        
        /* Service Card */
        .service-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 30px;
            height: 100%;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .service-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(255, 180, 0, 0.3);
            border-color: var(--gold);
        }
        
        .service-icon {
            font-size: 3rem;
            color: var(--gold);
            margin-bottom: 20px;
        }
        
        .service-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.3rem;
            font-weight: 700;
            margin-bottom: 15px;
        }
        
        .service-description {
            color: var(--text-light);
            margin-bottom: 20px;
        }
        
        /* Portfolio Card */
        .portfolio-card {
            position: relative;
            border-radius: 15px;
            overflow: hidden;
            height: 300px;
            margin-bottom: 30px;
            transition: all 0.4s;
        }
        
        .portfolio-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
        }
        
        .portfolio-image {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s;
        }
        
        .portfolio-card:hover .portfolio-image {
            transform: scale(1.1);
        }
        
        .portfolio-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to top, rgba(0, 0, 0, 0.8) 0%, rgba(0, 0, 0, 0.2) 50%, transparent 100%);
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            padding: 20px;
            transition: all 0.3s;
        }
        
        .portfolio-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.3rem;
            font-weight: 700;
            margin-bottom: 5px;
        }
        
        .portfolio-category {
            color: var(--gold);
            font-size: 0.9rem;
            margin-bottom: 10px;
        }
        
        .portfolio-link {
            color: var(--white);
            text-decoration: none;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            transition: all 0.3s;
        }
        
        .portfolio-link:hover {
            color: var(--gold);
            transform: translateX(5px);
        }
        
        /* Testimonial Card */
        .testimonial-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 30px;
            height: 100%;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .testimonial-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(255, 180, 0, 0.3);
            border-color: var(--gold);
        }
        
        .testimonial-content {
            font-style: italic;
            color: var(--text-light);
            margin-bottom: 20px;
            position: relative;
        }
        
        .testimonial-content::before {
            content: '"';
            font-size: 4rem;
            color: var(--gold);
            position: absolute;
            top: -20px;
            left: -10px;
            opacity: 0.3;
        }
        
        .testimonial-author {
            display: flex;
            align-items: center;
        }
        
        .testimonial-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            margin-right: 15px;
            object-fit: cover;
        }
        
        .testimonial-name {
            font-weight: 700;
            margin-bottom: 5px;
        }
        
        .testimonial-position {
            color: var(--gold);
            font-size: 0.9rem;
        }
        
        /* Pricing Card */
        .pricing-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 30px;
            height: 100%;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
            position: relative;
        }
        
        .pricing-card.featured {
            border-color: var(--gold);
            transform: scale(1.05);
        }
        
        .pricing-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(255, 180, 0, 0.3);
            border-color: var(--gold);
        }
        
        .pricing-card.featured:hover {
            transform: scale(1.05) translateY(-10px);
        }
        
        .pricing-badge {
            position: absolute;
            top: -15px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--gradient-gold);
            color: var(--dark-blue);
            padding: 5px 20px;
            border-radius: 50px;
            font-weight: 700;
            font-size: 0.8rem;
        }
        
        .pricing-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 15px;
            text-align: center;
        }
        
        .pricing-price {
            font-size: 3rem;
            font-weight: 800;
            color: var(--gold);
            text-align: center;
            margin-bottom: 5px;
        }
        
        .pricing-period {
            text-align: center;
            color: var(--text-light);
            margin-bottom: 20px;
        }
        
        .pricing-features {
            list-style: none;
            padding: 0;
            margin-bottom: 30px;
        }
        
        .pricing-features li {
            padding: 10px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            display: flex;
            align-items: center;
        }
        
        .pricing-features li:last-child {
            border-bottom: none;
        }
        
        .pricing-features i {
            color: var(--gold);
            margin-right: 10px;
        }
        
        /* Contact Form */
        .contact-form {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 30px;
            backdrop-filter: blur(10px);
        }
        
        .form-control {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            padding: 12px 15px;
            color: var(--white);
            transition: all 0.3s;
        }
        
        .form-control:focus {
            background: rgba(255, 255, 255, 0.15);
            border-color: var(--gold);
            box-shadow: 0 0 0 0.25rem rgba(255, 180, 0, 0.25);
            color: var(--white);
        }
        
        .form-control::placeholder {
            color: rgba(255, 255, 255, 0.5);
        }
        
        /* Footer */
        footer {
            background: rgba(15, 48, 87, 0.9);
            padding: 50px 0 20px;
            border-top: 1px solid rgba(255, 180, 0, 0.2);
        }
        
        .footer-logo {
            width: 150px;
            height: 150px;
            border-radius: 20px;
            margin-bottom: 20px;
        }
        
        .footer-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 20px;
            color: var(--gold);
        }
        
        .footer-links {
            list-style: none;
            padding: 0;
        }
        
        .footer-links li {
            margin-bottom: 10px;
        }
        
        .footer-links a {
            color: var(--text-light);
            text-decoration: none;
            transition: all 0.3s;
        }
        
        .footer-links a:hover {
            color: var(--gold);
            transform: translateX(5px);
        }
        
        .social-links {
            display: flex;
            gap: 15px;
            margin-top: 20px;
        }
        
        .social-link {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(255, 180, 0, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--gold);
            transition: all 0.3s;
        }
        
        .social-link:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            transform: translateY(-5px);
        }
        
        .copyright {
            text-align: center;
            padding-top: 30px;
            margin-top: 30px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            color: var(--text-light);
        }
        
        /* Floating WhatsApp */
        .floating-wa {
            position: fixed;
            bottom: 30px;
            right: 30px;
            z-index: 999;
            background: #25D366;
            color: white;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            box-shadow: 0 8px 20px rgba(37,211,102,0.4);
            animation: pulse-wa 2s infinite;
            text-decoration: none;
        }

        @keyframes pulse-wa {
            0%, 100% { 
                transform: scale(1); 
                box-shadow: 0 8px 20px rgba(37,211,102,0.4);
            }
            50% { 
                transform: scale(1.1); 
                box-shadow: 0 12px 30px rgba(37,211,102,0.6);
            }
        }
        
        /* Back to Top */
        #backToTop {
            position: fixed;
            bottom: 100px;
            right: 30px;
            z-index: 998;
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: none;
            box-shadow: 0 5px 15px rgba(255,180,0,0.4);
            transition: all 0.3s;
            cursor: pointer;
        }

        #backToTop:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 20px rgba(255,180,0,0.6);
        }
        
        /* Responsive */
        @media (max-width: 768px) {
            .hero-title {
                font-size: 2rem;
            }
            
            .hero-subtitle {
                font-size: 1.1rem;
            }
            
            .btn-gold, .btn-outline-gold {
                padding: 12px 25px;
                font-size: 0.9rem;
            }
            
            .section {
                padding: 60px 0;
            }
            
            .pricing-card.featured {
                transform: scale(1);
            }
        }
    </style>
</head>
<body>
    <!-- Network Background -->
    <div class="network-bg" id="networkBg"></div>
    
    <!-- Circuit Pattern -->
    <div class="circuit-pattern"></div>
    
    <!-- Navbar -->
    <nav class="navbar navbar-expand-lg navbar-premium">
        <div class="container">
            <a class="navbar-brand d-flex align-items-center" href="index.php" style="text-decoration: none;">
                <img src="https://situneo.my.id/logo" 
                     alt="Situneo" width="50" height="50" 
                     style="margin-right: 15px; border-radius: 10px; box-shadow: 0 5px 15px rgba(255,180,0,0.4);">
                <div>
                    <span style="font-family: 'Plus Jakarta Sans', sans-serif; font-size: 1.8rem; font-weight: 800; color: var(--gold);">SITUNEO</span>
                    <small style="display: block; font-size: 0.7rem; color: var(--text-light); margin-top: -5px;">Digital Harmony</small>
                </div>
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" 
                    style="border-color: var(--gold);">
                <span class="navbar-toggler-icon" style="filter: invert(1);"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto align-items-center">
                    <li class="nav-item"><a class="nav-link active" href="index.php"><?= $t['nav_home'] ?></a></li>
                    <li class="nav-item"><a class="nav-link" href="about.php"><?= $t['nav_about'] ?></a></li>
                    <li class="nav-item"><a class="nav-link" href="services.php"><?= $t['nav_services'] ?></a></li>
                    <li class="nav-item"><a class="nav-link" href="portfolio.php"><?= $t['nav_portfolio'] ?></a></li>
                    <li class="nav-item"><a class="nav-link" href="pricing.php"><?= $t['nav_pricing'] ?></a></li>
                    <li class="nav-item"><a class="nav-link" href="contact.php"><?= $t['nav_contact'] ?></a></li>
                    <li class="nav-item"><a class="nav-link" href="calculator.php"><?= $t['nav_calculator'] ?></a></li>
                    
                    <?php if ($isLoggedIn): ?>
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="userDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                                <i class="bi bi-person-circle me-1"></i> <?= htmlspecialchars($userName) ?>
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="userDropdown">
                                <li><a class="dropdown-item" href="auth/dashboard.php"><i class="bi bi-speedometer2 me-2"></i><?= $t['nav_dashboard'] ?></a></li>
                                <li><a class="dropdown-item" href="auth/profile.php"><i class="bi bi-person me-2"></i>Profil</a></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="auth/logout.php"><i class="bi bi-box-arrow-right me-2"></i><?= $t['nav_logout'] ?></a></li>
                            </ul>
                        </li>
                    <?php else: ?>
                        <li class="nav-item ms-3">
                            <a href="auth/login.php" class="btn btn-outline-warning btn-sm" style="border-radius: 50px; padding: 8px 20px;">
                                <i class="bi bi-box-arrow-in-right"></i> <?= $t['btn_login'] ?>
                            </a>
                        </li>
                        <li class="nav-item ms-2">
                            <a href="auth/register.php" class="btn btn-warning btn-sm" style="border-radius: 50px; padding: 8px 20px;">
                                <i class="bi bi-person-plus"></i> <?= $t['btn_register'] ?>
                            </a>
                        </li>
                    <?php endif; ?>
                    
                    <li class="nav-item ms-2">
                        <div class="lang-switcher">
                            <a href="?lang=id" class="lang-btn <?= $lang == 'id' ? 'active' : '' ?>">🇮🇩 ID</a>
                            <a href="?lang=en" class="lang-btn <?= $lang == 'en' ? 'active' : '' ?>">🇺🇸 EN</a>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    
    <!-- Hero Section -->
    <section class="hero">
        <div class="container">
            <div class="row align-items-center">
                <div class="col-lg-6" data-aos="fade-up">
                    <h1 class="hero-title"><?= $t['hero_title'] ?></h1>
                    <p class="hero-subtitle"><?= $t['hero_subtitle'] ?></p>
                    <div class="hero-buttons">
                        <?php if ($isLoggedIn): ?>
                            <a href="auth/dashboard.php" class="btn-gold"><?= $t['nav_dashboard'] ?></a>
                            <a href="services.php" class="btn-outline-gold"><?= $t['btn_view_services'] ?></a>
                        <?php else: ?>
                            <a href="auth/register.php" class="btn-gold"><?= $t['btn_get_started'] ?></a>
                            <a href="services.php" class="btn-outline-gold"><?= $t['btn_view_services'] ?></a>
                        <?php endif; ?>
                    </div>
                </div>
                <div class="col-lg-6" data-aos="fade-up" data-aos-delay="200">
                    <img src="https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=800&h=600&fit=crop&q=80" alt="Digital Services" class="img-fluid rounded-4">
                </div>
            </div>
        </div>
    </section>
    
    <!-- Services Section -->
    <section class="section" id="services">
        <div class="container">
            <h2 class="section-title" data-aos="fade-up"><?= $t['section_services_title'] ?></h2>
            <p class="section-subtitle" data-aos="fade-up" data-aos-delay="100"><?= $t['section_services_subtitle'] ?></p>
            
            <div class="row g-4">
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="200">
                    <div class="service-card">
                        <div class="service-icon">
                            <i class="bi bi-globe"></i>
                        </div>
                        <h3 class="service-title">Website Development</h3>
                        <p class="service-description">Website profesional yang responsif dan SEO-friendly untuk meningkatkan kehadiran online bisnis Anda.</p>
                        <a href="services.php" class="btn btn-outline-warning">Pelajari Lebih Lanjut</a>
                    </div>
                </div>
                
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="300">
                    <div class="service-card">
                        <div class="service-icon">
                            <i class="bi bi-search"></i>
                        </div>
                        <h3 class="service-title">SEO & Marketing</h3>
                        <p class="service-description">Optimasi mesin pencari dan strategi pemasaran digital untuk meningkatkan visibilitas dan traffic.</p>
                        <a href="services.php" class="btn btn-outline-warning">Pelajari Lebih Lanjut</a>
                    </div>
                </div>
                
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="400">
                    <div class="service-card">
                        <div class="service-icon">
                            <i class="bi bi-robot"></i>
                        </div>
                        <h3 class="service-title">Automation & AI</h3>
                        <p class="service-description">Solusi otomasi dan kecerdasan buatan untuk meningkatkan efisiensi operasional bisnis.</p>
                        <a href="services.php" class="btn btn-outline-warning">Pelajari Lebih Lanjut</a>
                    </div>
                </div>
            </div>
            
            <div class="text-center mt-5" data-aos="fade-up" data-aos-delay="500">
                <a href="services.php" class="btn btn-gold">Lihat Semua Layanan</a>
            </div>
        </div>
    </section>
    
    <!-- Portfolio Section -->
    <section class="section" id="portfolio">
        <div class="container">
            <h2 class="section-title" data-aos="fade-up"><?= $t['section_portfolio_title'] ?></h2>
            <p class="section-subtitle" data-aos="fade-up" data-aos-delay="100"><?= $t['section_portfolio_subtitle'] ?></p>
            
            <div class="row g-4">
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="200">
                    <div class="portfolio-card">
                        <img src="https://images.unsplash.com/photo-1441986300917-64674bd600d8?w=600&h=400&fit=crop&q=80" alt="E-Commerce" class="portfolio-image">
                        <div class="portfolio-overlay">
                            <h3 class="portfolio-title">Toko Fashion "Bolaya"</h3>
                            <p class="portfolio-category">E-Commerce</p>
                            <a href="portfolio.php" class="portfolio-link">Lihat Detail <i class="bi bi-arrow-right"></i></a>
                        </div>
                    </div>
                </div>
                
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="300">
                    <div class="portfolio-card">
                        <img src="https://images.unsplash.com/photo-1576091160399-112ba8d25d1d?w=600&h=400&fit=crop&q=80" alt="Healthcare" class="portfolio-image">
                        <div class="portfolio-overlay">
                            <h3 class="portfolio-title">Klinik Kecantikan "Beauty Clinic"</h3>
                            <p class="portfolio-category">Healthcare</p>
                            <a href="portfolio.php" class="portfolio-link">Lihat Detail <i class="bi bi-arrow-right"></i></a>
                        </div>
                    </div>
                </div>
                
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="400">
                    <div class="portfolio-card">
                        <img src="https://images.unsplash.com/photo-1517248135467-4c7edcad34c4?w=600&h=400&fit=crop&q=80" alt="Restaurant" class="portfolio-image">
                        <div class="portfolio-overlay">
                            <h3 class="portfolio-title">Restoran "Rasa Nusantara"</h3>
                            <p class="portfolio-category">F&B</p>
                            <a href="portfolio.php" class="portfolio-link">Lihat Detail <i class="bi bi-arrow-right"></i></a>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="text-center mt-5" data-aos="fade-up" data-aos-delay="500">
                <a href="portfolio.php" class="btn btn-gold">Lihat Semua Portfolio</a>
            </div>
        </div>
    </section>
    
    <!-- Testimonial Section -->
    <section class="section" id="testimonial">
        <div class="container">
            <h2 class="section-title" data-aos="fade-up"><?= $t['section_testimonial_title'] ?></h2>
            <p class="section-subtitle" data-aos="fade-up" data-aos-delay="100"><?= $t['section_testimonial_subtitle'] ?></p>
            
            <div class="row g-4">
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="200">
                    <div class="testimonial-card">
                        <p class="testimonial-content">Tim SITUNEO sangat profesional dan responsif. Website e-commerce yang mereka buat untuk bisnis saya meningkatkan penjualan hingga 200% dalam 3 bulan pertama.</p>
                        <div class="testimonial-author">
                            <img src="https://images.unsplash.com/photo-1494790108755-2616b612b786?w=100&h=100&fit=crop&q=80" alt="Client" class="testimonial-avatar">
                            <div>
                                <h4 class="testimonial-name">Andi Wijaya</h4>
                                <p class="testimonial-position">CEO, Bolaya Fashion</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="300">
                    <div class="testimonial-card">
                        <p class="testimonial-content">Sistem booking online yang dikembangkan oleh SITUNEO sangat membantu operasional klinik kami. Pasien lebih mudah membuat janji dan staf lebih efisien mengelola jadwal.</p>
                        <div class="testimonial-author">
                            <img src="https://images.unsplash.com/photo-1580489944761-15a19d654956?w=100&h=100&fit=crop&q=80" alt="Client" class="testimonial-avatar">
                            <div>
                                <h4 class="testimonial-name">Dr. Sarah Putri</h4>
                                <p class="testimonial-position">Owner, Beauty Clinic</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="400">
                    <div class="testimonial-card">
                        <p class="testimonial-content">Layanan SEO dari SITUNEO membantu restoran kami mendapatkan peringkat pertama di Google untuk kata kunci "restoran Indonesia di Jakarta". Traffic organik meningkat drastis!</p>
                        <div class="testimonial-author">
                            <img src="https://images.unsplash.com/photo-1507003211169-0cfed4f6a45d?w=100&h=100&fit=crop&q=80" alt="Client" class="testimonial-avatar">
                            <div>
                                <h4 class="testimonial-name">Budi Santoso</h4>
                                <p class="testimonial-position">Manager, Rasa Nusantara</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    
    <!-- Pricing Section -->
    <section class="section" id="pricing">
        <div class="container">
            <h2 class="section-title" data-aos="fade-up"><?= $t['section_pricing_title'] ?></h2>
            <p class="section-subtitle" data-aos="fade-up" data-aos-delay="100"><?= $t['section_pricing_subtitle'] ?></p>
            
            <div class="row g-4 align-items-center">
                <div class="col-lg-4" data-aos="fade-up" data-aos-delay="200">
                    <div class="pricing-card">
                        <h3 class="pricing-title">Starter</h3>
                        <div class="pricing-price">Rp 2.5jt</div>
                        <p class="pricing-period">per bulan</p>
                        <ul class="pricing-features">
                            <li><i class="bi bi-check-circle-fill"></i> Website Basic (5 halaman)</li>
                            <li><i class="bi bi-check-circle-fill"></i> SEO Basic</li>
                            <li><i class="bi bi-check-circle-fill"></i> 1 Revisi/bulan</li>
                            <li><i class="bi bi-check-circle-fill"></i> Email Support</li>
                            <li><i class="bi bi-check-circle-fill"></i> Hosting 1GB</li>
                        </ul>
                        <a href="pricing.php" class="btn btn-outline-warning w-100">Pilih Paket</a>
                    </div>
                </div>
                
                <div class="col-lg-4" data-aos="fade-up" data-aos-delay="300">
                    <div class="pricing-card featured">
                        <div class="pricing-badge">Paling Populer</div>
                        <h3 class="pricing-title">Professional</h3>
                        <div class="pricing-price">Rp 5jt</div>
                        <p class="pricing-period">per bulan</p>
                        <ul class="pricing-features">
                            <li><i class="bi bi-check-circle-fill"></i> Website Advanced (15 halaman)</li>
                            <li><i class="bi bi-check-circle-fill"></i> SEO Advanced</li>
                            <li><i class="bi bi-check-circle-fill"></i> 3 Revisi/bulan</li>
                            <li><i class="bi bi-check-circle-fill"></i> Priority Support</li>
                            <li><i class="bi bi-check-circle-fill"></i> Hosting 5GB</li>
                            <li><i class="bi bi-check-circle-fill"></i> Google Ads Management</li>
                        </ul>
                        <a href="pricing.php" class="btn btn-warning w-100">Pilih Paket</a>
                    </div>
                </div>
                
                <div class="col-lg-4" data-aos="fade-up" data-aos-delay="400">
                    <div class="pricing-card">
                        <h3 class="pricing-title">Enterprise</h3>
                        <div class="pricing-price">Rp 10jt</div>
                        <p class="pricing-period">per bulan</p>
                        <ul class="pricing-features">
                            <li><i class="bi bi-check-circle-fill"></i> Website Unlimited</li>
                            <li><i class="bi bi-check-circle-fill"></i> SEO Premium</li>
                            <li><i class="bi bi-check-circle-fill"></i> Unlimited Revisi</li>
                            <li><i class="bi bi-check-circle-fill"></i> 24/7 Support</li>
                            <li><i class="bi bi-check-circle-fill"></i> Unlimited Hosting</li>
                            <li><i class="bi bi-check-circle-fill"></i> Full Digital Marketing</li>
                        </ul>
                        <a href="pricing.php" class="btn btn-outline-warning w-100">Pilih Paket</a>
                    </div>
                </div>
            </div>
        </div>
    </section>
    
    <!-- Contact Section -->
    <section class="section" id="contact">
        <div class="container">
            <h2 class="section-title" data-aos="fade-up"><?= $t['section_contact_title'] ?></h2>
            <p class="section-subtitle" data-aos="fade-up" data-aos-delay="100"><?= $t['section_contact_subtitle'] ?></p>
            
            <div class="row">
                <div class="col-lg-6" data-aos="fade-up" data-aos-delay="200">
                    <div class="contact-form">
                        <form id="contactForm">
                            <div class="row">
                                <div class="col-md-6 mb-3">
                                    <label for="name" class="form-label">Nama Lengkap</label>
                                    <input type="text" class="form-control" id="name" name="name" required>
                                </div>
                                <div class="col-md-6 mb-3">
                                    <label for="email" class="form-label">Email</label>
                                    <input type="email" class="form-control" id="email" name="email" required>
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="subject" class="form-label">Subjek</label>
                                <input type="text" class="form-control" id="subject" name="subject" required>
                            </div>
                            <div class="mb-3">
                                <label for="message" class="form-label">Pesan</label>
                                <textarea class="form-control" id="message" name="message" rows="5" required></textarea>
                            </div>
                            <button type="submit" class="btn btn-gold">Kirim Pesan</button>
                        </form>
                    </div>
                </div>
                
                <div class="col-lg-6" data-aos="fade-up" data-aos-delay="300">
                    <div class="ps-lg-5">
                        <div class="mb-4">
                            <h4 class="text-warning mb-3">Informasi Kontak</h4>
                            <div class="d-flex align-items-center mb-3">
                                <div class="flex-shrink-0">
                                    <i class="bi bi-geo-alt-fill text-warning fs-4"></i>
                                </div>
                                <div class="ms-3">
                                    <p class="mb-0">Jl. Bekasi Timur IX Dalam No. 27, RT 002/RW 003, Kel. Rawa Bunga, Kec. Jatinegara, Jakarta Timur 13450, DKI Jakarta</p>
                                </div>
                            </div>
                            <div class="d-flex align-items-center mb-3">
                                <div class="flex-shrink-0">
                                    <i class="bi bi-telephone-fill text-warning fs-4"></i>
                                </div>
                                <div class="ms-3">
                                    <p class="mb-0">+62 831-7386-8915</p>
                                </div>
                            </div>
                            <div class="d-flex align-items-center mb-3">
                                <div class="flex-shrink-0">
                                    <i class="bi bi-envelope-fill text-warning fs-4"></i>
                                </div>
                                <div class="ms-3">
                                    <p class="mb-0">info@situneo.my.id</p>
                                </div>
                            </div>
                        </div>
                        
                        <div class="mb-4">
                            <h4 class="text-warning mb-3">Jam Operasional</h4>
                            <div class="d-flex justify-content-between mb-2">
                                <span>Senin - Jumat:</span>
                                <span>09:00 - 18:00</span>
                            </div>
                            <div class="d-flex justify-content-between mb-2">
                                <span>Sabtu:</span>
                                <span>10:00 - 16:00</span>
                            </div>
                            <div class="d-flex justify-content-between">
                                <span>Minggu:</span>
                                <span>Tutup</span>
                            </div>
                        </div>
                        
                        <div>
                            <h4 class="text-warning mb-3">Ikuti Kami</h4>
                            <div class="social-links">
                                <a href="#" class="social-link"><i class="bi bi-facebook"></i></a>
                                <a href="#" class="social-link"><i class="bi bi-instagram"></i></a>
                                <a href="#" class="social-link"><i class="bi bi-twitter"></i></a>
                                <a href="#" class="social-link"><i class="bi bi-linkedin"></i></a>
                                <a href="#" class="social-link"><i class="bi bi-youtube"></i></a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    
    <!-- Footer -->
    <footer>
        <div class="container">
            <div class="row">
                <div class="col-lg-4 mb-4">
                    <img src="https://situneo.my.id/logo" alt="Situneo" class="footer-logo">
                    <p><?= $t['footer_about'] ?></p>
                    <div class="social-links">
                        <a href="#" class="social-link"><i class="bi bi-facebook"></i></a>
                        <a href="#" class="social-link"><i class="bi bi-instagram"></i></a>
                        <a href="#" class="social-link"><i class="bi bi-twitter"></i></a>
                        <a href="#" class="social-link"><i class="bi bi-linkedin"></i></a>
                        <a href="#" class="social-link"><i class="bi bi-youtube"></i></a>
                    </div>
                </div>
                
                <div class="col-lg-2 col-md-6 mb-4">
                    <h4 class="footer-title"><?= $t['footer_services'] ?></h4>
                    <ul class="footer-links">
                        <li><a href="services.php">Website Development</a></li>
                        <li><a href="services.php">SEO & Marketing</a></li>
                        <li><a href="services.php">E-Commerce</a></li>
                        <li><a href="services.php">Mobile Apps</a></li>
                        <li><a href="services.php">Digital Advertising</a></li>
                    </ul>
                </div>
                
                <div class="col-lg-2 col-md-6 mb-4">
                    <h4 class="footer-title"><?= $t['footer_company'] ?></h4>
                    <ul class="footer-links">
                        <li><a href="about.php">Tentang Kami</a></li>
                        <li><a href="portfolio.php">Portfolio</a></li>
                        <li><a href="team.php">Tim Kami</a></li>
                        <li><a href="careers.php">Karir</a></li>
                        <li><a href="blog.php">Blog</a></li>
                    </ul>
                </div>
                
                <div class="col-lg-2 col-md-6 mb-4">
                    <h4 class="footer-title"><?= $t['footer_support'] ?></h4>
                    <ul class="footer-links">
                        <li><a href="contact.php">Hubungi Kami</a></li>
                        <li><a href="faq.php">FAQ</a></li>
                        <li><a href="terms.php">Syarat & Ketentuan</a></li>
                        <li><a href="privacy.php">Kebijakan Privasi</a></li>
                        <li><a href="calculator.php">Hitung Harga</a></li>
                    </ul>
                </div>
                
                <div class="col-lg-2 col-md-6 mb-4">
                    <h4 class="footer-title">Newsletter</h4>
                    <p>Dapatkan tips dan penawaran terbaru dari kami</p>
                    <form id="newsletterForm">
                        <div class="input-group mb-3">
                            <input type="email" class="form-control" placeholder="Email Anda" required>
                            <button class="btn btn-warning" type="submit">
                                <i class="bi bi-send"></i>
                            </button>
                        </div>
                    </form>
                </div>
            </div>
            
            <div class="copyright">
                <p><?= $t['footer_copyright'] ?></p>
            </div>
        </div>
    </footer>
    
    <!-- Floating WhatsApp -->
    <a href="https://wa.me/6283173868915?text=Halo%20SITUNEO%20DIGITAL,%20saya%20tertarik%20dengan%20layanan%20Anda" 
       class="floating-wa" target="_blank">
        <i class="bi bi-whatsapp"></i>
    </a>
    
    <!-- Back to Top -->
    <button id="backToTop">
        <i class="bi bi-arrow-up"></i>
    </button>
    
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
    <script>
        // Initialize AOS
        AOS.init({
            duration: 1000,
            once: true
        });
        
        // Network Background Animation
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        const networkBg = document.getElementById('networkBg');
        networkBg.appendChild(canvas);
        
        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        
        resizeCanvas();
        window.addEventListener('resize', resizeCanvas);
        
        const nodes = [];
        const nodeCount = 50;
        const connectionDistance = 150;
        
        class Node {
            constructor() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height;
                this.vx = (Math.random() - 0.5) * 0.5;
                this.vy = (Math.random() - 0.5) * 0.5;
                this.radius = Math.random() * 2 + 1;
            }
            
            update() {
                this.x += this.vx;
                this.y += this.vy;
                
                if (this.x < 0 || this.x > canvas.width) this.vx = -this.vx;
                if (this.y < 0 || this.y > canvas.height) this.vy = -this.vy;
            }
            
            draw() {
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                ctx.fillStyle = 'rgba(255, 180, 0, 0.7)';
                ctx.fill();
            }
        }
        
        for (let i = 0; i < nodeCount; i++) {
            nodes.push(new Node());
        }
        
        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Update and draw nodes
            nodes.forEach(node => {
                node.update();
                node.draw();
            });
            
            // Draw connections
            for (let i = 0; i < nodes.length; i++) {
                for (let j = i + 1; j < nodes.length; j++) {
                    const dx = nodes[i].x - nodes[j].x;
                    const dy = nodes[i].y - nodes[j].y;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance < connectionDistance) {
                        ctx.beginPath();
                        ctx.moveTo(nodes[i].x, nodes[i].y);
                        ctx.lineTo(nodes[j].x, nodes[j].y);
                        ctx.strokeStyle = `rgba(255, 180, 0, ${0.2 * (1 - distance / connectionDistance)})`;
                        ctx.stroke();
                    }
                }
            }
            
            requestAnimationFrame(animate);
        }
        
        animate();
        
        // Navbar scroll effect
        window.addEventListener('scroll', function() {
            const navbar = document.querySelector('.navbar-premium');
            if (window.scrollY > 50) {
                navbar.classList.add('scrolled');
            } else {
                navbar.classList.remove('scrolled');
            }
        });
        
        // Back to top button
        const backToTopButton = document.getElementById('backToTop');
        
        window.addEventListener('scroll', function() {
            if (window.scrollY > 300) {
                backToTopButton.style.display = 'flex';
            } else {
                backToTopButton.style.display = 'none';
            }
        });
        
        backToTopButton.addEventListener('click', function() {
            window.scrollTo({
                top: 0,
                behavior: 'smooth'
            });
        });
        
        // Contact form submission
        document.getElementById('contactForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Get form data
            const formData = new FormData(this);
            const name = formData.get('name');
            const email = formData.get('email');
            const subject = formData.get('subject');
            const message = formData.get('message');
            
            // Simple validation
            if (!name || !email || !subject || !message) {
                alert('Mohon lengkapi semua field!');
                return;
            }
            
            // Here you would normally send the data to a server
            // For demo purposes, we'll just show a success message
            alert('Pesan Anda telah terkirim! Kami akan segera menghubungi Anda.');
            this.reset();
        });
        
        // Newsletter form submission
        document.getElementById('newsletterForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const email = this.querySelector('input[type="email"]').value;
            
            if (!email) {
                alert('Mohon masukkan email Anda!');
                return;
            }
            
            // Here you would normally send the data to a server
            // For demo purposes, we'll just show a success message
            alert('Terima kasih telah berlangganan newsletter kami!');
            this.reset();
        });
    </script>
</body>
</html>
