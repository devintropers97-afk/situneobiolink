<?php
/**
 * Services Page
 */

// Get company settings from database
 $settings = [];
 $result = $conn->query("SELECT setting_key, setting_value FROM settings");
if ($result) {
    while ($row = $result->fetch_assoc()) {
        $settings[$row['setting_key']] = $row['setting_value'];
    }
}

// Get services from database
 $services = [];
 $result = $conn->query("SELECT * FROM services WHERE status = 'active' ORDER BY category, id");
if ($result) {
    while ($row = $result->fetch_assoc()) {
        // Decode JSON features
        $row['features'] = json_decode($row['features'], true) ?: [];
        $services[] = $row;
    }
}

// Get unique categories
 $categories = [];
foreach ($services as $service) {
    if (!in_array($service['category'], $categories)) {
        $categories[] = $service['category'];
    }
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>26 Layanan Digital Profesional - <?= $settings['company_name'] ?? 'SITUNEO DIGITAL' ?></title>
    <meta name="description" content="26 layanan digital profesional mulai Rp 200rb. Website, SEO, Ads, Chatbot AI, Design, dan masih banyak lagi. Solusi lengkap untuk bisnis online.">
    <meta name="keywords" content="jasa website, seo, google ads, facebook ads, chatbot, design logo, toko online, digital marketing">
    
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
        .hero-services {
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

        /* Filter Buttons */
        .filter-container {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            justify-content: center;
            margin-bottom: 3rem;
        }

        .filter-btn {
            padding: 10px 25px;
            border-radius: 50px;
            background: rgba(255,180,0,0.1);
            border: 2px solid rgba(255,180,0,0.3);
            color: var(--text-light);
            font-weight: 600;
            transition: all 0.3s;
            cursor: pointer;
        }

        .filter-btn:hover, .filter-btn.active {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: var(--gold);
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(255,180,0,0.4);
        }

        /* Service Card */
        .service-card-full {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            overflow: hidden;
            height: 100%;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }

        .service-card-full:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(255,180,0,0.3);
            border-color: var(--gold);
        }

        .service-image-full {
            position: relative;
            height: 200px;
            overflow: hidden;
        }

        .service-image-full img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s;
        }

        .service-card-full:hover .service-image-full img {
            transform: scale(1.1);
        }

        .service-icon-badge {
            position: absolute;
            top: 15px;
            left: 15px;
            width: 60px;
            height: 60px;
            background: var(--gradient-gold);
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        .price-badge {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(255,0,0,0.9);
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: 700;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { box-shadow: 0 0 0 0 rgba(255, 0, 0, 0.7); }
            50% { box-shadow: 0 0 0 10px rgba(255, 0, 0, 0); }
        }

        .service-content {
            padding: 1.5rem;
        }

        .category-badge {
            display: inline-block;
            background: rgba(255,180,0,0.2);
            border: 1px solid var(--gold);
            color: var(--gold);
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            margin-bottom: 1rem;
        }

        .btn-gold {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 12px 30px;
            font-weight: 700;
            border-radius: 50px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            transition: all 0.3s;
            text-decoration: none;
            display: inline-block;
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.3);
        }

        .btn-gold:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255, 180, 0, 0.6);
            color: var(--dark-blue);
        }

        /* Feature List */
        .feature-list {
            list-style: none;
            padding: 0;
            margin: 1rem 0;
        }

        .feature-list li {
            padding: 8px 0;
            color: var(--text-light);
            font-size: 0.9rem;
            display: flex;
            align-items: start;
        }

        .feature-list li i {
            color: var(--gold);
            margin-right: 10px;
            margin-top: 3px;
            flex-shrink: 0;
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

        /* Responsive */
        @media (max-width: 768px) {
            .filter-btn {
                padding: 8px 15px;
                font-size: 0.85rem;
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
                    <li class="nav-item"><a class="nav-link active" href="/services">Layanan</a></li>
                    <li class="nav-item"><a class="nav-link" href="/portfolio">Demo</a></li>
                    <li class="nav-item"><a class="nav-link" href="/pricing">Harga Paket</a></li>
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
    <section class="hero-services">
        <div class="container">
            <div class="text-center" data-aos="fade-up">
                <h1 class="section-title">26 Layanan Digital Profesional</h1>
                <p class="section-subtitle">Solusi lengkap untuk semua kebutuhan digital bisnis kamu</p>
                <div style="display: inline-block; background: linear-gradient(135deg, #FF0000 0%, #FF6B00 100%); border: 2px solid #FFD700; padding: 12px 25px; border-radius: 50px; animation: pulse 2s infinite; box-shadow: 0 10px 30px rgba(255,0,0,0.5); margin-bottom: 3rem;">
                    <i class="bi bi-fire" style="color: #FFD700; font-size: 1.3rem;"></i>
                    <span style="color: white; font-weight: 800; margin-left: 8px; font-size: 1.1rem;">Mulai dari Rp 200rb aja!</span>
                </div>
            </div>
        </div>
    </section>

    <!-- Filter Section -->
    <section class="py-3" style="background: rgba(30,92,153,0.1);">
        <div class="container">
            <div class="filter-container" data-aos="fade-up">
                <button class="filter-btn active" onclick="filterServices('all')">Semua Layanan (26)</button>
                <?php foreach($categories as $category): ?>
                <button class="filter-btn" onclick="filterServices('<?= $category ?>')"><?= $category ?></button>
                <?php endforeach; ?>
            </div>
        </div>
    </section>

    <!-- Services Grid -->
    <section class="py-5">
        <div class="container">
            <div class="row g-4" id="servicesGrid">
                <?php foreach($services as $index => $service): ?>
                <div class="col-lg-4 col-md-6 service-item" data-category="<?= $service['category'] ?>" data-aos="zoom-in" data-aos-delay="<?= ($index % 3) * 100 ?>">
                    <div class="service-card-full">
                        <!-- Image -->
                        <div class="service-image-full">
                            <img src="<?= htmlspecialchars($service['image']) ?>" 
                                 alt="<?= htmlspecialchars($service['name']) ?>"
                                 loading="lazy">
                            <div class="service-icon-badge">
                                <i class="bi bi-<?= htmlspecialchars($service['icon']) ?>" style="font-size: 2rem; color: var(--dark-blue);"></i>
                            </div>
                            <div class="price-badge">
                                Rp <?= number_format($service['price_start'], 0, ',', '.') ?>
                            </div>
                        </div>
                        
                        <!-- Content -->
                        <div class="service-content">
                            <span class="category-badge"><?= htmlspecialchars($service['category']) ?></span>
                            
                            <h4 style="color: var(--gold); font-weight: 700; margin-bottom: 1rem;">
                                <?= htmlspecialchars($service['name']) ?>
                            </h4>
                            
                            <div class="mb-3">
                                <span style="color: var(--gold); font-weight: 700; font-size: 1.3rem;">
                                    Mulai dari Rp <?= number_format($service['price_start'], 0, ',', '.') ?>
                                </span>
                                <small style="color: var(--text-light); display: block; font-size: 0.85rem;">
                                    <?= htmlspecialchars($service['price_unit']) ?>
                                </small>
                            </div>
                            
                            <p style="color: var(--text-light); line-height: 1.7; margin-bottom: 1rem;">
                                <?= htmlspecialchars($service['description']) ?>
                            </p>
                            
                            <!-- Features (show first 5) -->
                            <ul class="feature-list">
                                <?php 
                                $feature_count = 0;
                                foreach($service['features'] as $feature): 
                                    if (++$feature_count > 5) break;
                                ?>
                                <li>
                                    <i class="bi bi-check-circle-fill"></i>
                                    <span><?= htmlspecialchars($feature) ?></span>
                                </li>
                                <?php endforeach; ?>
                                <?php if (count($service['features']) > 5): ?>
                                <li style="color: var(--gold); font-weight: 600;">
                                    <i class="bi bi-plus-circle-fill"></i>
                                    <span>Dan <?= count($service['features']) - 5 ?> fitur lainnya...</span>
                                </li>
                                <?php endif; ?>
                            </ul>
                            
                            <div style="border-top: 1px solid rgba(255,180,0,0.2); padding-top: 1rem; margin-top: 1rem;">
                                <div style="display: flex; gap: 5px; margin-bottom: 0.5rem;">
                                    <i class="bi bi-clock" style="color: var(--gold);"></i>
                                    <small style="color: var(--text-light);">Selesai: <?= htmlspecialchars($service['delivery_time']) ?></small>
                                </div>
                                <div style="display: flex; gap: 5px;">
                                    <i class="bi bi-star-fill" style="color: var(--gold);"></i>
                                    <small style="color: var(--text-light);">Perfect for: <?= htmlspecialchars($service['perfect_for']) ?></small>
                                </div>
                            </div>
                            
                            <!-- CTA Buttons -->
                            <div class="d-grid gap-2 mt-3">
                                <a href="https://wa.me/6283173868915?text=Halo, saya mau pesan layanan <?= urlencode($service['name']) ?>" 
                                   class="btn-gold" target="_blank">
                                    <i class="bi bi-whatsapp"></i> PESAN SEKARANG
                                </a>
                                <a href="/calculator" 
                                   class="btn btn-outline-warning" 
                                   style="border-radius: 50px; padding: 12px; font-weight: 700; border-width: 2px;">
                                    <i class="bi bi-calculator"></i> HITUNG HARGA
                                </a>
                            </div>
                        </div>
                    </div>
                </div>
                <?php endforeach; ?>
            </div>
            
            <!-- No Results Message -->
            <div id="noResults" style="display: none; text-align: center; padding: 3rem;">
                <i class="bi bi-search" style="font-size: 4rem; color: var(--gold); opacity: 0.5;"></i>
                <h3 style="color: var(--gold); margin-top: 1rem;">Gak ada layanan di kategori ini</h3>
                <p style="color: var(--text-light);">Coba pilih kategori lain atau lihat semua layanan</p>
            </div>
        </div>
    </section>

    <!-- CTA Section -->
    <section class="cta-section">
        <div class="container">
            <div class="row align-items-center" data-aos="fade-up">
                <div class="col-lg-8">
                    <h2 style="color: var(--gold); font-weight: 800; margin-bottom: 1rem; font-size: 2rem;">
                        Gak Nemu Layanan yang Kamu Cari?
                    </h2>
                    <p style="color: var(--text-light); font-size: 1.1rem; margin: 0;">
                        Chat aja langsung! Kami bisa custom layanan sesuai kebutuhan bisnis kamu. <strong style="color: var(--gold);">FREE Konsultasi!</strong>
                    </p>
                </div>
                <div class="col-lg-4 text-lg-end mt-3 mt-lg-0">
                    <a href="https://wa.me/6283173868915?text=Halo, saya mau konsultasi tentang layanan custom" 
                       class="btn-gold btn-lg" target="_blank" style="font-size: 1.1rem;">
                        <i class="bi bi-whatsapp"></i> CHAT SEKARANG
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
    <a href="https://wa.me/6283173868915?text=Halo%20Situneo,%20saya%20mau%20tanya%20tentang%20layanan" 
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
        
        // Filter Services Function
        function filterServices(category) {
            const items = document.querySelectorAll('.service-item');
            const noResults = document.getElementById('noResults');
            const filterBtns = document.querySelectorAll('.filter-btn');
            let visibleCount = 0;
            
            // Update active button
            filterBtns.forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            // Filter items
            items.forEach(item => {
                if (category === 'all' || item.dataset.category === category) {
                    item.style.display = 'block';
                    visibleCount++;
                } else {
                    item.style.display = 'none';
                }
            });
            
            // Show/hide no results message
            if (visibleCount === 0) {
                noResults.style.display = 'block';
            } else {
                noResults.style.display = 'none';
            }
            
            // Re-initialize AOS for filtered items
            AOS.refresh();
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
