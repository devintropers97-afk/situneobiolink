<?php
/**
 * Portfolio Page
 */

// Get company settings from database
 $settings = [];
 $result = $conn->query("SELECT setting_key, setting_value FROM settings");
if ($result) {
    while ($row = $result->fetch_assoc()) {
        $settings[$row['setting_key']] = $row['setting_value'];
    }
}

// Get portfolios from database
 $portfolios = [];
 $result = $conn->query("SELECT * FROM portfolios WHERE status = 'active' ORDER BY id DESC");
if ($result) {
    while ($row = $result->fetch_assoc()) {
        $portfolios[] = $row;
    }
}

// Get unique categories
 $categories = [];
foreach ($portfolios as $portfolio) {
    if (!in_array($portfolio['category'], $categories)) {
        $categories[] = $portfolio['category'];
    }
}

// Count portfolios per category
 $category_count = [];
foreach ($categories as $cat) {
    $category_count[$cat] = 0;
    foreach ($portfolios as $portfolio) {
        if ($portfolio['category'] === $cat) {
            $category_count[$cat]++;
        }
    }
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>50 Demo Website - <?= $settings['company_name'] ?? 'SITUNEO DIGITAL' ?></title>
    <meta name="description" content="50+ demo website berkualitas dari berbagai industri. E-Commerce, F&B, Corporate, Healthcare, Education, dan masih banyak lagi.">
    <meta name="keywords" content="demo website, portfolio, e-commerce, company profile, toko online, website klinik">
    
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
        .hero-portfolio {
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

        /* Search Section */
        .search-section {
            background: rgba(30,92,153,0.1);
            padding: 2rem 0;
            margin-bottom: 3rem;
        }

        .search-box {
            position: relative;
            max-width: 600px;
            margin: 0 auto;
        }

        .search-input {
            width: 100%;
            padding: 15px 50px 15px 20px;
            border-radius: 50px;
            border: 2px solid rgba(255,180,0,0.3);
            background: rgba(15, 48, 87, 0.7);
            color: var(--white);
            font-size: 1rem;
            transition: all 0.3s;
        }

        .search-input:focus {
            outline: none;
            border-color: var(--gold);
            box-shadow: 0 0 15px rgba(255,180,0,0.3);
        }

        .search-input::placeholder {
            color: var(--text-light);
        }

        .search-btn {
            position: absolute;
            right: 5px;
            top: 50%;
            transform: translateY(-50%);
            background: var(--gradient-gold);
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            color: var(--dark-blue);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        .search-btn:hover {
            transform: translateY(-50%) scale(1.1);
        }

        /* Filter Section */
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

        /* Portfolio Card */
        .portfolio-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            overflow: hidden;
            height: 100%;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
            cursor: pointer;
        }

        .portfolio-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(255,180,0,0.3);
            border-color: var(--gold);
        }

        .portfolio-image {
            position: relative;
            height: 220px;
            overflow: hidden;
        }

        .portfolio-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s;
        }

        .portfolio-card:hover .portfolio-image img {
            transform: scale(1.1);
        }

        .portfolio-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to bottom, transparent 0%, rgba(0,0,0,0.7) 100%);
            display: flex;
            align-items: flex-end;
            padding: 1.5rem;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .portfolio-card:hover .portfolio-overlay {
            opacity: 1;
        }

        .portfolio-category {
            display: inline-block;
            background: var(--gradient-gold);
            color: var(--dark-blue);
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            margin-bottom: 0.5rem;
        }

        .portfolio-views {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(0,0,0,0.7);
            color: var(--white);
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .portfolio-content {
            padding: 1.5rem;
        }

        .portfolio-title {
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--gold);
            margin-bottom: 0.5rem;
            line-height: 1.3;
        }

        .portfolio-description {
            color: var(--text-light);
            font-size: 0.9rem;
            line-height: 1.6;
            margin-bottom: 1rem;
        }

        .portfolio-tags {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            margin-bottom: 1rem;
        }

        .portfolio-tag {
            background: rgba(255,180,0,0.1);
            border: 1px solid rgba(255,180,0,0.3);
            color: var(--gold);
            padding: 3px 10px;
            border-radius: 15px;
            font-size: 0.75rem;
        }

        .portfolio-actions {
            display: flex;
            gap: 10px;
        }

        .btn-view {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: 600;
            font-size: 0.85rem;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            gap: 5px;
            transition: all 0.3s;
        }

        .btn-view:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(255,180,0,0.4);
            color: var(--dark-blue);
        }

        .btn-order {
            background: transparent;
            color: var(--gold);
            border: 2px solid var(--gold);
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: 600;
            font-size: 0.85rem;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            gap: 5px;
            transition: all 0.3s;
        }

        .btn-order:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: transparent;
        }

        /* Modal */
        .portfolio-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 1000;
            overflow-y: auto;
        }

        .modal-content {
            background: var(--dark-blue);
            border: 2px solid var(--gold);
            border-radius: 20px;
            max-width: 900px;
            margin: 50px auto;
            position: relative;
            overflow: hidden;
        }

        .modal-close {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(255,180,0,0.2);
            border: 1px solid var(--gold);
            color: var(--gold);
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            z-index: 1001;
            transition: all 0.3s;
        }

        .modal-close:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
        }

        .modal-image {
            width: 100%;
            height: 400px;
            object-fit: cover;
        }

        .modal-body {
            padding: 2rem;
        }

        .modal-title {
            font-size: 2rem;
            font-weight: 800;
            color: var(--gold);
            margin-bottom: 1rem;
        }

        .modal-category {
            display: inline-block;
            background: var(--gradient-gold);
            color: var(--dark-blue);
            padding: 8px 20px;
            border-radius: 20px;
            font-weight: 700;
            text-transform: uppercase;
            margin-bottom: 1rem;
        }

        .modal-description {
            color: var(--text-light);
            line-height: 1.8;
            margin-bottom: 2rem;
        }

        .modal-actions {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
        }

        .btn-modal {
            padding: 12px 30px;
            border-radius: 50px;
            font-weight: 700;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            gap: 10px;
            transition: all 0.3s;
        }

        .btn-modal-primary {
            background: var(--gradient-gold);
            color: var(--dark-blue);
        }

        .btn-modal-primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255,180,0,0.6);
            color: var(--dark-blue);
        }

        .btn-modal-secondary {
            background: transparent;
            color: var(--gold);
            border: 2px solid var(--gold);
        }

        .btn-modal-secondary:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: transparent;
        }

        /* No Results */
        .no-results {
            text-align: center;
            padding: 3rem;
            display: none;
        }

        .no-results i {
            font-size: 4rem;
            color: var(--gold);
            opacity: 0.5;
            margin-bottom: 1rem;
        }

        .no-results h3 {
            color: var(--gold);
            margin-bottom: 1rem;
        }

        .no-results p {
            color: var(--text-light);
        }

        /* Stats Section */
        .stats-section {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.2) 0%, rgba(15, 48, 87, 0.3) 100%);
            padding: 60px 0;
        }

        .stat-card {
            background: rgba(0,0,0,0.3);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255,180,0,0.2);
            border-radius: 20px;
            padding: 2rem;
            text-align: center;
            transition: all 0.3s;
            height: 100%;
        }

        .stat-card:hover {
            transform: translateY(-10px);
            border-color: var(--gold);
            box-shadow: 0 15px 40px rgba(255,180,0,0.3);
        }

        .stat-icon {
            font-size: 3.5rem;
            margin-bottom: 1rem;
            display: inline-block;
            filter: drop-shadow(0 5px 15px rgba(255,180,0,0.5));
        }

        .stat-number {
            font-size: 3rem;
            font-weight: 900;
            color: var(--gold);
            line-height: 1;
            margin-bottom: 0.5rem;
            text-shadow: 0 5px 20px rgba(255,180,0,0.5);
        }

        .stat-label {
            color: var(--text-light);
            font-size: 1.1rem;
            font-weight: 600;
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
            .filter-btn {
                padding: 8px 15px;
                font-size: 0.85rem;
            }
            
            .modal-content {
                margin: 20px;
                max-width: calc(100% - 40px);
            }
            
            .modal-image {
                height: 250px;
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
                    <li class="nav-item"><a class="nav-link active" href="/portfolio">Demo</a></li>
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
    <section class="hero-portfolio">
        <div class="container">
            <div class="text-center" data-aos="fade-up">
                <h1 class="section-title">50+ Demo Website Berkualitas</h1>
                <p class="section-subtitle">Lihat hasil kerja kami untuk berbagai industri. Klik untuk detail!</p>
                <div style="display: inline-block; background: linear-gradient(135deg, #FF0000 0%, #FF6B00 100%); border: 2px solid #FFD700; padding: 12px 25px; border-radius: 50px; animation: pulse 2s infinite; box-shadow: 0 10px 30px rgba(255,0,0,0.5); margin-bottom: 2rem;">
                    <i class="bi bi-eye" style="color: #FFD700; font-size: 1.3rem;"></i>
                    <span style="color: white; font-weight: 800; margin-left: 8px; font-size: 1.1rem;">Lihat Demo Langsung!</span>
                </div>
            </div>
        </div>
    </section>

    <!-- Search Section -->
    <section class="search-section">
        <div class="container">
            <div class="search-box" data-aos="fade-up">
                <input type="text" class="search-input" id="searchInput" placeholder="Cari demo website...">
                <button class="search-btn" id="searchBtn">
                    <i class="bi bi-search"></i>
                </button>
            </div>
        </div>
    </section>

    <!-- Filter Section -->
    <section class="py-3">
        <div class="container">
            <div class="filter-container" data-aos="fade-up">
                <button class="filter-btn active" onclick="filterPortfolios('all')">Semua (<?= count($portfolios) ?>)</button>
                <?php foreach($categories as $category): ?>
                <button class="filter-btn" onclick="filterPortfolios('<?= $category ?>')"><?= $category ?> (<?= $category_count[$category] ?>)</button>
                <?php endforeach; ?>
            </div>
        </div>
    </section>

    <!-- Portfolio Grid -->
    <section class="py-5">
        <div class="container">
            <div class="row g-4" id="portfolioGrid">
                <?php foreach($portfolios as $index => $portfolio): ?>
                <div class="col-lg-4 col-md-6 portfolio-item" data-category="<?= $portfolio['category'] ?>" data-title="<?= strtolower($portfolio['title']) ?>" data-aos="zoom-in" data-aos-delay="<?= ($index % 3) * 100 ?>">
                    <div class="portfolio-card" onclick="openModal(<?= $portfolio['id'] ?>)">
                        <div class="portfolio-image">
                            <img src="<?= htmlspecialchars($portfolio['image']) ?>" 
                                 alt="<?= htmlspecialchars($portfolio['title']) ?>"
                                 loading="lazy">
                            <div class="portfolio-overlay">
                                <div>
                                    <span class="portfolio-category"><?= htmlspecialchars($portfolio['category']) ?></span>
                                    <h5 class="text-white"><?= htmlspecialchars($portfolio['title']) ?></h5>
                                </div>
                            </div>
                            <div class="portfolio-views">
                                <i class="bi bi-eye"></i>
                                <span><?= number_format($portfolio['views']) ?></span>
                            </div>
                        </div>
                        <div class="portfolio-content">
                            <h5 class="portfolio-title"><?= htmlspecialchars($portfolio['title']) ?></h5>
                            <p class="portfolio-description"><?= substr(htmlspecialchars($portfolio['description']), 0, 100) ?>...</p>
                            <div class="portfolio-tags">
                                <?php 
                                $tags = explode(', ', $portfolio['tags']);
                                foreach(array_slice($tags, 0, 3) as $tag): 
                                ?>
                                <span class="portfolio-tag"><?= htmlspecialchars($tag) ?></span>
                                <?php endforeach; ?>
                            </div>
                            <div class="portfolio-actions">
                                <a href="<?= htmlspecialchars($portfolio['url']) ?>" target="_blank" class="btn-view" onclick="event.stopPropagation()">
                                    <i class="bi bi-eye"></i> Lihat Demo
                                </a>
                                <a href="https://wa.me/6283173868915?text=Halo, saya mau pesan website seperti <?= urlencode($portfolio['title']) ?>" target="_blank" class="btn-order" onclick="event.stopPropagation()">
                                    <i class="bi bi-whatsapp"></i> Pesan
                                </a>
                            </div>
                        </div>
                    </div>
                </div>
                <?php endforeach; ?>
            </div>
            
            <!-- No Results Message -->
            <div id="noResults" class="no-results">
                <i class="bi bi-search"></i>
                <h3>Tidak ada demo yang ditemukan</h3>
                <p>Coba kata kunci lain atau lihat semua demo</p>
            </div>
        </div>
    </section>

    <!-- Stats Section -->
    <section class="stats-section">
        <div class="container">
            <div class="row g-4">
                <div class="col-md-3 col-6" data-aos="zoom-in" data-aos-delay="100">
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="bi bi-laptop" style="color: var(--gold);"></i>
                        </div>
                        <div class="stat-number">50+</div>
                        <div class="stat-label">Demo Website</div>
                    </div>
                </div>
                <div class="col-md-3 col-6" data-aos="zoom-in" data-aos-delay="200">
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="bi bi-grid-3x3-gap" style="color: var(--gold);"></i>
                        </div>
                        <div class="stat-number">11</div>
                        <div class="stat-label">Kategori</div>
                    </div>
                </div>
                <div class="col-md-3 col-6" data-aos="zoom-in" data-aos-delay="300">
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="bi bi-eye" style="color: var(--gold);"></i>
                        </div>
                        <div class="stat-number">10K+</div>
                        <div class="stat-label">Total Views</div>
                    </div>
                </div>
                <div class="col-md-3 col-6" data-aos="zoom-in" data-aos-delay="400">
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="bi bi-star-fill" style="color: var(--gold);"></i>
                        </div>
                        <div class="stat-number">4.9/5</div>
                        <div class="stat-label">Rating</div>
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
                        Tertarik dengan Website Kami?
                    </h2>
                    <p style="color: var(--text-light); font-size: 1.1rem; margin: 0;">
                        Kami bisa buatkan website custom sesuai kebutuhan bisnis Anda. <strong style="color: var(--gold);">FREE Demo 24 Jam!</strong>
                    </p>
                </div>
                <div class="col-lg-4 text-lg-end mt-3 mt-lg-0">
                    <a href="/contact" class="btn-gold btn-lg" style="font-size: 1.1rem;">
                        <i class="bi bi-whatsapp"></i> KONSULTASI GRATIS
                    </a>
                </div>
            </div>
        </div>
    </section>

    <!-- Portfolio Modal -->
    <div id="portfolioModal" class="portfolio-modal">
        <div class="modal-content">
            <div class="modal-close" onclick="closeModal()">
                <i class="bi bi-x-lg"></i>
            </div>
            <img id="modalImage" class="modal-image" src="" alt="">
            <div class="modal-body">
                <span id="modalCategory" class="modal-category"></span>
                <h2 id="modalTitle" class="modal-title"></h2>
                <p id="modalDescription" class="modal-description"></p>
                <div class="modal-actions">
                    <a id="modalViewBtn" href="" target="_blank" class="btn-modal btn-modal-primary">
                        <i class="bi bi-eye"></i> Lihat Demo
                    </a>
                    <a id="modalOrderBtn" href="" target="_blank" class="btn-modal btn-modal-secondary">
                        <i class="bi bi-whatsapp"></i> Pesan Sekarang
                    </a>
                </div>
            </div>
        </div>
    </div>

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
    <a href="https://wa.me/6283173868915?text=Halo%20Situneo,%20saya%20mau%20tanya%20tentang%20portfolio" 
       class="floating-wa" 
       target="_blank">
        <i class="bi bi-whatsapp"></i>
    </a>
    
    <!-- JavaScript Libraries -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
    
    <script>
        // Portfolio data
        const portfolios = <?= json_encode($portfolios) ?>;
        
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
        
        // Filter Portfolios Function
        function filterPortfolios(category) {
            const items = document.querySelectorAll('.portfolio-item');
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
        
        // Search Functionality
        document.getElementById('searchBtn').addEventListener('click', function() {
            const searchTerm = document.getElementById('searchInput').value.toLowerCase();
            const items = document.querySelectorAll('.portfolio-item');
            const noResults = document.getElementById('noResults');
            let visibleCount = 0;
            
            items.forEach(item => {
                const title = item.dataset.title;
                const category = item.dataset.category;
                
                if (title.includes(searchTerm) || category.toLowerCase().includes(searchTerm)) {
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
        });
        
        // Search on Enter key
        document.getElementById('searchInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                document.getElementById('searchBtn').click();
            }
        });
        
        // Modal Functions
        function openModal(portfolioId) {
            const portfolio = portfolios.find(p => p.id == portfolioId);
            
            if (portfolio) {
                document.getElementById('modalImage').src = portfolio.image;
                document.getElementById('modalCategory').textContent = portfolio.category;
                document.getElementById('modalTitle').textContent = portfolio.title;
                document.getElementById('modalDescription').textContent = portfolio.description;
                document.getElementById('modalViewBtn').href = portfolio.url;
                document.getElementById('modalOrderBtn').href = `https://wa.me/6283173868915?text=Halo, saya mau pesan website seperti ${encodeURIComponent(portfolio.title)}`;
                
                document.getElementById('portfolioModal').style.display = 'block';
                document.body.style.overflow = 'hidden';
                
                // Increment view count
                incrementViewCount(portfolioId);
            }
        }
        
        function closeModal() {
            document.getElementById('portfolioModal').style.display = 'none';
            document.body.style.overflow = 'auto';
        }
        
        // Close modal when clicking outside
        window.addEventListener('click', function(e) {
            const modal = document.getElementById('portfolioModal');
            if (e.target === modal) {
                closeModal();
            }
        });
        
        // Increment View Count (simulated)
        function incrementViewCount(portfolioId) {
            // In a real application, this would make an AJAX call to update the database
            // For now, we'll just update the display
            const portfolio = portfolios.find(p => p.id == portfolioId);
            if (portfolio) {
                portfolio.views++;
                
                // Update the view count display
                const viewElements = document.querySelectorAll(`[data-category="${portfolio.category}"][data-title="${portfolio.title.toLowerCase()}"] .portfolio-views span`);
                viewElements.forEach(element => {
                    element.textContent = portfolio.views.toLocaleString();
                });
            }
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
