<?php
/**
 * Pricing Page
 */

// Get company settings from database
 $settings = [];
 $result = $conn->query("SELECT setting_key, setting_value FROM settings");
if ($result) {
    while ($row = $result->fetch_assoc()) {
        $settings[$row['setting_key']] = $row['setting_value'];
    }
}

// Define packages
 $packages = [
    [
        'id' => 1,
        'name' => 'Starter',
        'price' => 1500000,
        'period' => 'tahun',
        'description' => 'Paket hemat untuk bisnis yang baru memulai digitalisasi',
        'popular' => false,
        'features' => [
            'Website 5 halaman',
            'Responsive Design',
            'Basic SEO Setup',
            'Contact Form',
            '1x Revisi',
            'SSL Certificate',
            'Free Domain (.my.id)',
            '1GB Hosting',
            'Email Support',
            '6 bulan maintenance'
        ],
        'not_included' => [
            'Custom Design',
            'Advanced SEO',
            'Content Creation',
            'Social Media Integration',
            'Analytics Setup'
        ],
        'button_text' => 'Pilih Paket',
        'button_link' => 'https://wa.me/6283173868915?text=Halo, saya mau pesan paket Starter'
    ],
    [
        'id' => 2,
        'name' => 'Professional',
        'price' => 3500000,
        'period' => 'tahun',
        'description' => 'Paket lengkap untuk bisnis yang serius dengan online presence',
        'popular' => true,
        'features' => [
            'Website 10 halaman',
            'Custom Design',
            'Advanced SEO Setup',
            'Contact Form + Quote Form',
            '3x Revisi',
            'SSL Certificate',
            'Free Domain (.com/.id)',
            '5GB Hosting',
            'Email + Phone Support',
            '1 tahun maintenance',
            'Google Analytics Setup',
            'Social Media Integration'
        ],
        'not_included' => [
            'E-Commerce Functionality',
            'Content Creation',
            'Monthly SEO Optimization',
            'PPC Advertising Setup'
        ],
        'button_text' => 'Pilih Paket',
        'button_link' => 'https://wa.me/6283173868915?text=Halo, saya mau pesan paket Professional'
    ],
    [
        'id' => 3,
        'name' => 'E-Commerce',
        'price' => 5000000,
        'period' => 'tahun',
        'description' => 'Paket khusus untuk toko online dengan fitur lengkap',
        'popular' => false,
        'features' => [
            'Toko Online Lengkap',
            'Payment Gateway Integration',
            'Product Management System',
            'Shopping Cart & Checkout',
            'Order Management',
            'Inventory Management',
            'Custom Design',
            'Advanced SEO Setup',
            'SSL Certificate',
            'Free Domain (.com/.id)',
            '10GB Hosting',
            'Email + Phone Support',
            '1 tahun maintenance',
            'Google Analytics Setup'
        ],
        'not_included' => [
            'Content Creation',
            'Monthly SEO Optimization',
            'PPC Advertising Setup',
            'Social Media Management'
        ],
        'button_text' => 'Pilih Paket',
        'button_link' => 'https://wa.me/6283173868915?text=Halo, saya mau pesan paket E-Commerce'
    ],
    [
        'id' => 4,
        'name' => 'Business',
        'price' => 7500000,
        'period' => 'tahun',
        'description' => 'Paket premium untuk bisnis dengan kebutuhan digital kompleks',
        'popular' => false,
        'features' => [
            'Custom Web Application',
            'Advanced Database System',
            'User Management System',
            'Payment Gateway Integration',
            'API Development',
            'Custom Design',
            'Advanced SEO Setup',
            'SSL Certificate',
            'Free Domain (.com/.id)',
            '20GB Hosting',
            'Priority Support',
            '1 tahun maintenance',
            'Google Analytics Setup',
            'Performance Optimization'
        ],
        'not_included' => [
            'Content Creation',
            'Monthly SEO Optimization',
            'PPC Advertising Setup',
            'Social Media Management'
        ],
        'button_text' => 'Pilih Paket',
        'button_link' => 'https://wa.me/6283173868915?text=Halo, saya mau pesan paket Business'
    ],
    [
        'id' => 5,
        'name' => 'Enterprise',
        'price' => 15000000,
        'period' => 'tahun',
        'description' => 'Paket enterprise untuk perusahaan besar dengan kebutuhan khusus',
        'popular' => false,
        'features' => [
            'Custom Enterprise Solution',
            'Advanced Database Architecture',
            'Multi-Role User System',
            'Advanced Analytics Dashboard',
            'API Integration (Multiple)',
            'Custom Design',
            'Advanced SEO Setup',
            'SSL Certificate',
            'Free Domain (.com/.id)',
            'Unlimited Hosting',
            'Dedicated Support',
            '2 tahun maintenance',
            'Google Analytics Setup',
            'Performance Optimization',
            'Security Audit',
            'Training & Documentation'
        ],
        'not_included' => [
            'Content Creation',
            'Monthly SEO Optimization',
            'PPC Advertising Setup',
            'Social Media Management'
        ],
        'button_text' => 'Hubungi Sales',
        'button_link' => 'https://wa.me/6283173868915?text=Halo, saya mau informasi paket Enterprise'
    ],
    [
        'id' => 6,
        'name' => 'Custom',
        'price' => 'Custom',
        'period' => 'sesuai kebutuhan',
        'description' => 'Paket custom yang disesuaikan sepenuhnya dengan kebutuhan bisnis Anda',
        'popular' => false,
        'features' => [
            'Konsultasi Gratis',
            'Analisis Kebutuhan Bisnis',
            'Solusi Custom Design',
            'Fitur Sesuai Permintaan',
            'Timeline Disesuaikan',
            'Budget Fleksibel',
            'Tim Ahli Berpengalaman',
            'Support Prioritas',
            'Garansi Kepuasan'
        ],
        'not_included' => [
            'Fitur yang tidak disepakati dalam konsultasi awal'
        ],
        'button_text' => 'Konsultasi Gratis',
        'button_link' => 'https://wa.me/6283173868915?text=Halo, saya mau konsultasi untuk paket custom'
    ]
];
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Harga Paket - <?= $settings['company_name'] ?? 'SITUNEO DIGITAL' ?></title>
    <meta name="description" content="Paket harga website dan layanan digital terjangkau. Starter, Professional, E-Commerce, Business, Enterprise, dan Custom.">
    <meta name="keywords" content="harga website, paket website, harga e-commerce, harga toko online, biaya pembuatan website">
    
    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=Plus+Jakarta+Sans:wght@400;600;700;800;900&display=swap" rel="stylesheet">
    
    <!-- CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css" rel="stylesheet">
    <link rel="stylesheet" href="https://unpkg.com/aos@2.3.1/dist/aos.css">
    
    <!-- Custom CSS -->
    <link rel="stylesheet" href="assets/css/style.css">
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

        /* Hero Section */
        .hero-pricing {
            min-height: 60vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 150px 20px 80px;
            position: relative;
            background: radial-gradient(ellipse at top, rgba(255, 180, 0, 0.1) 0%, transparent 50%);
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
            color: var(--text-light);
            text-align: center;
            font-size: 1.2rem;
            margin-bottom: 2rem;
        }

        /* Pricing Section */
        .pricing-section {
            padding: 80px 0;
        }

        .pricing-intro {
            text-align: center;
            margin-bottom: 4rem;
        }

        .pricing-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255, 180, 0, 0.2);
            border-radius: 25px;
            padding: 2.5rem;
            height: 100%;
            transition: all 0.4s;
            position: relative;
            overflow: hidden;
        }

        .pricing-card:hover {
            transform: translateY(-15px);
            box-shadow: 0 25px 50px rgba(255, 180, 0, 0.4);
            border-color: var(--gold);
        }

        .pricing-card.popular {
            border-color: var(--gold);
            transform: scale(1.05);
        }

        .pricing-card.popular:hover {
            transform: scale(1.05) translateY(-15px);
        }

        .popular-badge {
            position: absolute;
            top: -15px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--gradient-gold);
            color: var(--dark-blue);
            padding: 8px 25px;
            border-radius: 50px;
            font-weight: 700;
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.3);
        }

        .pricing-header {
            text-align: center;
            margin-bottom: 2rem;
        }

        .pricing-name {
            font-size: 1.8rem;
            font-weight: 800;
            color: var(--gold);
            margin-bottom: 0.5rem;
        }

        .pricing-description {
            color: var(--text-light);
            font-size: 1rem;
            margin-bottom: 1.5rem;
        }

        .pricing-price {
            text-align: center;
            margin-bottom: 2rem;
        }

        .price-amount {
            font-size: 3rem;
            font-weight: 900;
            color: var(--gold);
            line-height: 1;
        }

        .price-period {
            color: var(--text-light);
            font-size: 1rem;
            margin-top: 0.5rem;
        }

        .pricing-features {
            margin-bottom: 2rem;
        }

        .feature-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .feature-list li {
            padding: 0.75rem 0;
            color: var(--text-light);
            font-size: 0.95rem;
            display: flex;
            align-items: flex-start;
        }

        .feature-list li i {
            color: var(--gold);
            margin-right: 10px;
            margin-top: 3px;
            flex-shrink: 0;
        }

        .not-included-list {
            list-style: none;
            padding: 0;
            margin: 1.5rem 0 0;
            border-top: 1px solid rgba(255, 180, 0, 0.1);
            padding-top: 1.5rem;
        }

        .not-included-list li {
            padding: 0.75rem 0;
            color: var(--text-light);
            font-size: 0.9rem;
            display: flex;
            align-items: flex-start;
            opacity: 0.7;
        }

        .not-included-list li i {
            color: #e74c3c;
            margin-right: 10px;
            margin-top: 3px;
            flex-shrink: 0;
        }

        .pricing-action {
            text-align: center;
        }

        .btn-pricing {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 15px 30px;
            font-weight: 700;
            border-radius: 50px;
            text-transform: uppercase;
            letter-spacing: 1px;
            transition: all 0.3s;
            text-decoration: none;
            display: inline-block;
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.3);
        }

        .btn-pricing:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255, 180, 0, 0.6);
            color: var(--dark-blue);
        }

        .btn-pricing-outline {
            background: transparent;
            color: var(--gold);
            border: 2px solid var(--gold);
            padding: 15px 30px;
            font-weight: 700;
            border-radius: 50px;
            text-transform: uppercase;
            letter-spacing: 1px;
            transition: all 0.3s;
            text-decoration: none;
            display: inline-block;
        }

        .btn-pricing-outline:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: transparent;
        }

        /* Comparison Table */
        .comparison-section {
            padding: 80px 0;
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
        }

        .comparison-table {
            background: rgba(15, 48, 87, 0.7);
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
        }

        .comparison-table table {
            width: 100%;
            border-collapse: collapse;
        }

        .comparison-table th {
            background: var(--gradient-primary);
            color: var(--white);
            padding: 1rem;
            text-align: center;
            font-weight: 700;
        }

        .comparison-table td {
            padding: 1rem;
            text-align: center;
            border-bottom: 1px solid rgba(255, 180, 0, 0.1);
        }

        .comparison-table tr:last-child td {
            border-bottom: none;
        }

        .comparison-table .feature-name {
            text-align: left;
            font-weight: 600;
            color: var(--text-light);
        }

        .comparison-table .check {
            color: var(--gold);
            font-size: 1.2rem;
        }

        .comparison-table .cross {
            color: #e74c3c;
            font-size: 1.2rem;
        }

        /* FAQ Section */
        .faq-section {
            padding: 80px 0;
        }

        .faq-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 2rem;
            margin-bottom: 1.5rem;
        }

        .faq-question {
            color: var(--gold);
            font-weight: 700;
            font-size: 1.1rem;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0.5rem 0;
            transition: color 0.3s;
        }

        .faq-question:hover {
            color: var(--bright-gold);
        }

        .faq-icon {
            transition: transform 0.3s;
        }

        .faq-icon.rotate {
            transform: rotate(180deg);
        }

        .faq-answer {
            color: var(--text-light);
            line-height: 1.7;
            padding-top: 1rem;
            display: none;
        }

        .faq-answer.show {
            display: block;
        }

        /* CTA Section */
        .cta-section {
            background: var(--gradient-primary);
            padding: 60px 0;
        }

        /* Footer */
        footer {
            background: linear-gradient(135deg, #0F3057 0%, #000000 100%);
            border-top: 2px solid var(--gold);
            padding: 3rem 0 1rem;
        }

        footer a:hover {
            color: var(--gold) !important;
            padding-left: 5px;
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

        /* Responsive */
        @media (max-width: 768px) {
            .pricing-card {
                padding: 2rem;
                margin-bottom: 2rem;
            }
            
            .pricing-card.popular {
                transform: scale(1);
            }
            
            .pricing-card.popular:hover {
                transform: translateY(-15px);
            }
            
            .comparison-table {
                overflow-x: auto;
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
            <a class="navbar-brand d-flex align-items-center" href="/" style="text-decoration: none;">
                <img src="https://ui-avatars.com/api/?name=SITUNEO&size=50&background=FFB400&color=0F3057&bold=true" 
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
                    <li class="nav-item"><a class="nav-link" href="/">Beranda</a></li>
                    <li class="nav-item"><a class="nav-link" href="/about">Tentang Kami</a></li>
                    <li class="nav-item"><a class="nav-link" href="/services">Layanan</a></li>
                    <li class="nav-item"><a class="nav-link" href="/portfolio">Demo</a></li>
                    <li class="nav-item"><a class="nav-link active" href="/pricing">Harga Paket</a></li>
                    <li class="nav-item"><a class="nav-link" href="/contact">Hubungi</a></li>
                    <li class="nav-item"><a class="nav-link" href="/calculator">Hitung Harga</a></li>
                    <li class="nav-item ms-3">
                        <a href="/auth/login" class="btn btn-outline-warning btn-sm" style="border-radius: 50px; padding: 8px 20px;">
                            <i class="bi bi-box-arrow-in-right"></i> Masuk
                        </a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    
    <!-- Hero Section -->
    <section class="hero-pricing">
        <div class="container">
            <div class="text-center" data-aos="fade-up">
                <h1 class="section-title">Harga Paket</h1>
                <p class="section-subtitle">Solusi digital terjangkau untuk semua kebutuhan bisnis Anda</p>
                <div style="display: inline-block; background: linear-gradient(135deg, #FF0000 0%, #FF6B00 100%); border: 2px solid #FFD700; padding: 12px 25px; border-radius: 50px; animation: pulse 2s infinite; box-shadow: 0 10px 30px rgba(255,0,0,0.5); margin-bottom: 2rem;">
                    <i class="bi bi-tag" style="color: #FFD700; font-size: 1.3rem;"></i>
                    <span style="color: white; font-weight: 800; margin-left: 8px; font-size: 1.1rem;">Harga Mulai Rp 1.5Jt/Tahun</span>
                </div>
            </div>
        </div>
    </section>

    <!-- Pricing Section -->
    <section class="pricing-section">
        <div class="container">
            <div class="pricing-intro" data-aos="fade-up">
                <h2 class="section-title">Pilih Paket yang Sesuai Kebutuhan Anda</h2>
                <p class="section-subtitle">Semua paket sudah termasuk domain, hosting, SSL, dan maintenance</p>
            </div>
            
            <div class="row g-4 align-items-center">
                <?php foreach($packages as $index => $package): ?>
                <div class="col-lg-4 col-md-6" data-aos="fade-up" data-aos-delay="<?= $index * 100 ?>">
                    <div class="pricing-card <?= $package['popular'] ? 'popular' : '' ?>">
                        <?php if ($package['popular']): ?>
                        <div class="popular-badge">Paling Populer</div>
                        <?php endif; ?>
                        
                        <div class="pricing-header">
                            <h3 class="pricing-name"><?= $package['name'] ?></h3>
                            <p class="pricing-description"><?= $package['description'] ?></p>
                        </div>
                        
                        <div class="pricing-price">
                            <?php if ($package['price'] === 'Custom'): ?>
                            <div class="price-amount">Custom</div>
                            <div class="price-period"><?= $package['period'] ?></div>
                            <?php else: ?>
                            <div class="price-amount">Rp <?= number_format($package['price'], 0, ',', '.') ?></div>
                            <div class="price-period">/ <?= $package['period'] ?></div>
                            <?php endif; ?>
                        </div>
                        
                        <div class="pricing-features">
                            <ul class="feature-list">
                                <?php foreach($package['features'] as $feature): ?>
                                <li>
                                    <i class="bi bi-check-circle-fill"></i>
                                    <span><?= $feature ?></span>
                                </li>
                                <?php endforeach; ?>
                            </ul>
                            
                            <?php if (!empty($package['not_included'])): ?>
                            <ul class="not-included-list">
                                <?php foreach($package['not_included'] as $feature): ?>
                                <li>
                                    <i class="bi bi-x-circle"></i>
                                    <span><?= $feature ?></span>
                                </li>
                                <?php endforeach; ?>
                            </ul>
                            <?php endif; ?>
                        </div>
                        
                        <div class="pricing-action">
                            <a href="<?= $package['button_link'] ?>" target="_blank" class="<?= $package['popular'] ? 'btn-pricing' : 'btn-pricing-outline' ?>">
                                <?= $package['button_text'] ?>
                            </a>
                        </div>
                    </div>
                </div>
                <?php endforeach; ?>
            </div>
        </div>
    </section>

    <!-- Comparison Table Section -->
    <section class="comparison-section">
        <div class="container">
            <div class="text-center mb-5" data-aos="fade-up">
                <h2 class="section-title">Perbandingan Paket</h2>
                <p class="section-subtitle">Lihat perbedaan fitur setiap paket</p>
            </div>
            
            <div class="comparison-table" data-aos="fade-up">
                <table>
                    <thead>
                        <tr>
                            <th>Fitur</th>
                            <th>Starter</th>
                            <th>Professional</th>
                            <th>E-Commerce</th>
                            <th>Business</th>
                            <th>Enterprise</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td class="feature-name">Website Halaman</td>
                            <td>5</td>
                            <td>10</td>
                            <td>Unlimited</td>
                            <td>Custom</td>
                            <td>Custom</td>
                        </tr>
                        <tr>
                            <td class="feature-name">Custom Design</td>
                            <td><i class="bi bi-x"></i></td>
                            <td><i class="bi bi-check"></i></td>
                            <td><i class="bi bi-check"></i></td>
                            <td><i class="bi bi-check"></i></td>
                            <td><i class="bi bi-check"></i></td>
                        </tr>
                        <tr>
                            <td class="feature-name">E-Commerce</td>
                            <td><i class="bi bi-x"></i></td>
                            <td><i class="bi bi-x"></i></td>
                            <td><i class="bi bi-check"></i></td>
                            <td><i class="bi bi-x"></i></td>
                            <td><i class="bi bi-x"></i></td>
                        </tr>
                        <tr>
                            <td class="feature-name">Payment Gateway</td>
                            <td><i class="bi bi-x"></i></td>
                            <td><i class="bi bi-x"></i></td>
                            <td><i class="bi bi-check"></i></td>
                            <td><i class="bi bi-check"></i></td>
                            <td><i class="bi bi-check"></i></td>
                        </tr>
                        <tr>
                            <td class="feature-name">API Development</td>
                            <td><i class="bi bi-x"></i></td>
                            <td><i class="bi bi-x"></i></td>
                            <td><i class="bi bi-x"></i></td>
                            <td><i class="bi bi-check"></i></td>
                            <td><i class="bi bi-check"></i></td>
                        </tr>
                        <tr>
                            <td class="feature-name">Support</td>
                            <td>Email</td>
                            <td>Email + Phone</td>
                            <td>Email + Phone</td>
                            <td>Priority</td>
                            <td>Dedicated</td>
                        </tr>
                        <tr>
                            <td class="feature-name">Maintenance</td>
                            <td>6 bulan</td>
                            <td>1 tahun</td>
                            <td>1 tahun</td>
                            <td>1 tahun</td>
                            <td>2 tahun</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </section>

    <!-- FAQ Section -->
    <section class="faq-section">
        <div class="container">
            <div class="text-center mb-5" data-aos="fade-up">
                <h2 class="section-title">Pertanyaan Umum</h2>
                <p class="section-subtitle">Jawaban untuk pertanyaan yang sering ditanyakan</p>
            </div>
            <div class="row justify-content-center">
                <div class="col-lg-8">
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="100">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah saya bisa mengupgrade paket?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Tentu saja! Anda bisa mengupgrade paket kapan saja. Kami akan menghitung sisa biaya yang harus dibayar dan menyesuaikan fitur sesuai paket baru.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="200">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah ada biaya tambahan setelah periode berakhir?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Ya, ada biaya renewal sesuai paket yang Anda pilih. Namun kami memberikan diskon khusus untuk renewal sebelum periode berakhir.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="300">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah saya bisa request fitur tambahan?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Tentu saja! Untuk paket Business dan Enterprise, Anda bisa request fitur tambahan dengan biaya tambahan. Untuk paket lain, Anda bisa upgrade ke paket yang lebih tinggi.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="400">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Bagaimana cara pembayarannya?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Kami menerima pembayaran via transfer bank, e-wallet (OVO, GoPay, DANA), dan kartu kredit. Untuk pembayaran cicilan, kami bekerja sama dengan berbagai platform cicilan.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="500">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah ada garansi kepuasan?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Ya! Kami memberikan garansi 100% kepuasan. Jika Anda tidak puas dengan hasilnya, kami akan revisi sampai Anda puas atau refund 100%.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- CTA Section -->
    <section class="cta-section">
        <div class="container">
            <div class="row align-items-center" data-aos="fade-up">
                <div class="col-lg-8">
                    <h2 style="color: var(--gold); font-weight: 800; margin-bottom: 1rem; font-size: 2rem;">
                        Bingung dengan Paket yang Tepat?
                    </h2>
                    <p style="color: var(--text-light); font-size: 1.1rem; margin: 0;">
                        Konsultasi gratis dengan tim digital expert kami. <strong style="color: var(--gold);">Kami akan bantu pilih paket yang sesuai!</strong>
                    </p>
                </div>
                <div class="col-lg-4 text-lg-end mt-3 mt-lg-0">
                    <a href="https://wa.me/6283173868915?text=Halo%20Situneo,%20saya%20mau%20konsultasi%20gratis%20tentang%20paket" 
                       class="btn-gold btn-lg" target="_blank" style="font-size: 1.1rem;">
                        <i class="bi bi-whatsapp"></i> KONSULTASI GRATIS
                    </a>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer>
        <div class="container">
            <div class="row g-4">
                <!-- Brand Info -->
                <div class="col-lg-4">
                    <div class="d-flex align-items-center mb-3">
                        <img src="https://ui-avatars.com/api/?name=SITUNEO&size=60&background=FFB400&color=0F3057&bold=true" 
                             alt="Situneo" width="60" height="60" 
                             style="border-radius: 15px; margin-right: 15px;">
                        <div>
                            <h4 style="color: var(--gold); margin: 0; font-weight: 800;">SITUNEO DIGITAL</h4>
                            <small style="color: var(--text-light);">Digital Harmony</small>
                        </div>
                    </div>
                    <p style="color: var(--text-light); line-height: 1.8; margin-bottom: 1rem;">
                        Partner digital terpercaya sejak 2020. Udah bantu 500+ bisnis sukses online dengan harga paling terjangkau!
                    </p>
                    <div class="trust-badges">
                        <div style="background: rgba(255,180,0,0.1); border: 1px solid var(--gold); 
                             padding: 10px 20px; border-radius: 10px; display: inline-block; margin-bottom: 1rem;">
                            <strong style="color: var(--gold);">NIB:</strong>
                            <span style="color: var(--text-light); font-size: 0.9rem;"> 20250926145704515453</span>
                        </div>
                    </div>
                </div>
                
                <!-- Quick Links -->
                <div class="col-lg-2 col-md-4">
                    <h5 style="color: var(--gold); font-weight: 700; margin-bottom: 1.5rem;">Menu Cepat</h5>
                    <ul class="list-unstyled">
                        <li class="mb-2">
                            <a href="/" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-chevron-right" style="font-size: 0.8rem;"></i> Beranda
                            </a>
                        </li>
                        <li class="mb-2">
                            <a href="/about" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-chevron-right" style="font-size: 0.8rem;"></i> Tentang Kami
                            </a>
                        </li>
                        <li class="mb-2">
                            <a href="/services" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-chevron-right" style="font-size: 0.8rem;"></i> Layanan
                            </a>
                        </li>
                        <li class="mb-2">
                            <a href="/portfolio" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-chevron-right" style="font-size: 0.8rem;"></i> Demo Website
                            </a>
                        </li>
                        <li class="mb-2">
                            <a href="/pricing" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-chevron-right" style="font-size: 0.8rem;"></i> Harga Paket
                            </a>
                        </li>
                        <li class="mb-2">
                            <a href="/contact" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-chevron-right" style="font-size: 0.8rem;"></i> Hubungi
                            </a>
                        </li>
                    </ul>
                </div>
                
                <!-- Popular Services -->
                <div class="col-lg-3 col-md-4">
                    <h5 style="color: var(--gold); font-weight: 700; margin-bottom: 1.5rem;">Layanan Populer</h5>
                    <ul class="list-unstyled">
                        <li class="mb-2">
                            <a href="/services" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-check-circle" style="font-size: 0.9rem;"></i> Bikin Website
                            </a>
                        </li>
                        <li class="mb-2">
                            <a href="/services" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-check-circle" style="font-size: 0.9rem;"></i> Toko Online
                            </a>
                        </li>
                        <li class="mb-2">
                            <a href="/services" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-check-circle" style="font-size: 0.9rem;"></i> SEO Specialist
                            </a>
                        </li>
                        <li class="mb-2">
                            <a href="/services" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-check-circle" style="font-size: 0.9rem;"></i> Google Ads
                            </a>
                        </li>
                        <li class="mb-2">
                            <a href="/services" style="color: var(--text-light); text-decoration: none; transition: color 0.3s;">
                                <i class="bi bi-check-circle" style="font-size: 0.9rem;"></i> Chatbot AI
                            </a>
                        </li>
                    </ul>
                </div>
                
                <!-- Contact -->
                <div class="col-lg-3 col-md-4">
                    <h5 style="color: var(--gold); font-weight: 700; margin-bottom: 1.5rem;">Hubungi Kami</h5>
                    <div class="mb-3">
                        <i class="bi bi-whatsapp" style="color: var(--gold);"></i>
                        <a href="https://wa.me/6283173868915" target="_blank" style="color: var(--text-light); text-decoration: none; margin-left: 8px;">
                            +62 831-7386-8915
                        </a>
                    </div>
                    <div class="mb-3">
                        <i class="bi bi-envelope" style="color: var(--gold);"></i>
                        <a href="mailto:support@situneo.my.id" style="color: var(--text-light); text-decoration: none; margin-left: 8px;">
                            support@situneo.my.id
                        </a>
                    </div>
                    <div class="mb-3">
                        <i class="bi bi-geo-alt" style="color: var(--gold);"></i>
                        <span style="color: var(--text-light); margin-left: 8px; font-size: 0.9rem;">
                            Jakarta Timur, Indonesia
                        </span>
                    </div>
                </div>
            </div>
            
            <hr style="border-color: rgba(255,180,0,0.2); margin: 2rem 0;">
            
            <!-- Copyright -->
            <div class="row align-items-center">
                <div class="col-md-6 text-center text-md-start mb-3 mb-md-0">
                    <p style="color: var(--text-light); margin: 0; font-size: 0.95rem;">
                        &copy; <?= date('Y') ?> <strong style="color: var(--gold);">SITUNEO DIGITAL</strong>. All Rights Reserved.
                    </p>
                </div>
                <div class="col-md-6 text-center text-md-end">
                    <p style="color: var(--text-light); margin: 0; font-size: 0.95rem;">
                        Made with <i class="bi bi-heart-fill" style="color: #FF0000;"></i> in Jakarta, Indonesia
                    </p>
                </div>
            </div>
        </div>
    </footer>
    
    <!-- Floating WhatsApp Button -->
    <a href="https://wa.me/6283173868915?text=Halo%20Situneo,%20saya%20mau%20tanya%20tentang%20paket" 
       class="floating-wa" 
       target="_blank">
        <i class="bi bi-whatsapp"></i>
    </a>
    
    <!-- JavaScript Libraries -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
    
    <script>
        // Initialize AOS
        AOS.init({
            duration: 1000,
            once: true,
            offset: 100
        });
        
        // Navbar Scroll Effect
        window.addEventListener('scroll', function() {
            const navbar = document.querySelector('.navbar-premium');
            
            if (window.scrollY > 100) {
                navbar.classList.add('scrolled');
            } else {
                navbar.classList.remove('scrolled');
            }
        });
        
        // Toggle FAQ
        function toggleFaq(element) {
            const answer = element.nextElementSibling;
            const icon = element.querySelector('.faq-icon');
            
            answer.classList.toggle('show');
            icon.classList.toggle('rotate');
        }
        
        // Network Background Animation
        const canvas = document.createElement('canvas');
        const networkBg = document.getElementById('networkBg');
        networkBg.appendChild(canvas);
        const ctx = canvas.getContext('2d');
        
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        
        const particles = [];
        const particleCount = 80;
        
        class Particle {
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
                
                if (this.x < 0 || this.x > canvas.width) this.vx *= -1;
                if (this.y < 0 || this.y > canvas.height) this.vy *= -1;
            }
            
            draw() {
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                ctx.fillStyle = 'rgba(255, 180, 0, 0.5)';
                ctx.fill();
            }
        }
        
        for (let i = 0; i < particleCount; i++) {
            particles.push(new Particle());
        }
        
        function connectParticles() {
            for (let i = 0; i < particles.length; i++) {
                for (let j = i + 1; j < particles.length; j++) {
                    const dx = particles[i].x - particles[j].x;
                    const dy = particles[i].y - particles[j].y;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance < 150) {
                        ctx.beginPath();
                        ctx.strokeStyle = `rgba(255, 180, 0, ${0.2 * (1 - distance / 150)})`;
                        ctx.lineWidth = 0.5;
                        ctx.moveTo(particles[i].x, particles[i].y);
                        ctx.lineTo(particles[j].x, particles[j].y);
                        ctx.stroke();
                    }
                }
            }
        }
        
        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            particles.forEach(particle => {
                particle.update();
                particle.draw();
            });
            
            connectParticles();
            requestAnimationFrame(animate);
        }
        
        animate();
        
        window.addEventListener('resize', function() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });
    </script>
</body>
</html>
