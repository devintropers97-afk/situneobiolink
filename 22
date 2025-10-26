<?php
/**
 * Calculator Page
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
    <title>Hitung Harga - <?= $settings['company_name'] ?? 'SITUNEO DIGITAL' ?></title>
    <meta name="description" content="Kalkulator harga untuk estimasi biaya pembuatan website dan layanan digital. Hitung sendiri harga yang sesuai dengan kebutuhan Anda.">
    <meta name="keywords" content="kalkulator harga, hitung harga, biaya website, estimasi harga, kalkulator digital">
    
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
        .hero-calculator {
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

        /* Calculator Section */
        .calculator-section {
            padding: 80px 0;
        }

        .calculator-container {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255, 180, 0, 0.2);
            border-radius: 25px;
            padding: 3rem;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
        }

        /* Service Selection */
        .service-selection {
            margin-bottom: 3rem;
        }

        .service-tabs {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
            margin-bottom: 2rem;
        }

        .service-tab {
            padding: 10px 20px;
            border-radius: 50px;
            background: rgba(255,180,0,0.1);
            border: 2px solid rgba(255,180,0,0.3);
            color: var(--text-light);
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        .service-tab:hover, .service-tab.active {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: var(--gold);
        }

        /* Service Cards */
        .services-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 1.5rem;
            margin-bottom: 3rem;
        }

        .service-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 2px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 1.5rem;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
        }

        .service-card:hover {
            transform: translateY(-5px);
            border-color: var(--gold);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.3);
        }

        .service-card.selected {
            border-color: var(--gold);
            background: linear-gradient(135deg, rgba(255, 180, 0, 0.2) 0%, rgba(15, 48, 87, 0.3) 100%);
        }

        .service-header {
            display: flex;
            align-items: center;
            margin-bottom: 1rem;
        }

        .service-icon {
            width: 50px;
            height: 50px;
            border-radius: 15px;
            background: var(--gradient-gold);
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 1rem;
            color: var(--dark-blue);
            font-size: 1.5rem;
        }

        .service-title {
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--gold);
            margin: 0;
        }

        .service-price {
            font-size: 1.1rem;
            color: var(--text-light);
            margin: 0;
        }

        .service-description {
            color: var(--text-light);
            font-size: 0.9rem;
            margin-top: 0.5rem;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
        }

        .service-checkbox {
            position: absolute;
            top: 1rem;
            right: 1rem;
            width: 24px;
            height: 24px;
            background: var(--gradient-gold);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--dark-blue);
            font-weight: 700;
            opacity: 0;
            transition: all 0.3s;
        }

        .service-card.selected .service-checkbox {
            opacity: 1;
        }

        /* Cart Section */
        .cart-section {
            background: linear-gradient(135deg, rgba(255, 180, 0, 0.1) 0%, rgba(255, 180, 0, 0.05) 100%);
            border: 2px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 2rem;
            margin-bottom: 3rem;
        }

        .cart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
        }

        .cart-title {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--gold);
        }

        .cart-clear {
            background: transparent;
            border: 2px solid var(--gold);
            color: var(--gold);
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        .cart-clear:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: transparent;
        }

        .cart-items {
            margin-bottom: 1.5rem;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem 0;
            border-bottom: 1px solid rgba(255, 180, 0, 0.1);
        }

        .cart-item:last-child {
            border-bottom: none;
        }

        .cart-item-info {
            flex: 1;
        }

        .cart-item-name {
            font-weight: 600;
            color: var(--white);
            margin-bottom: 0.25rem;
        }

        .cart-item-price {
            color: var(--text-light);
            font-size: 0.9rem;
        }

        .cart-item-quantity {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .quantity-btn {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            color: var(--white);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        .quantity-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        .quantity-value {
            width: 40px;
            text-align: center;
            color: var(--white);
            font-weight: 600;
        }

        .cart-item-remove {
            background: transparent;
            border: none;
            color: #e74c3c;
            cursor: pointer;
            transition: all 0.3s;
        }

        .cart-item-remove:hover {
            color: #ff6b6b;
        }

        .cart-summary {
            background: rgba(15, 48, 87, 0.7);
            border-radius: 15px;
            padding: 1.5rem;
            margin-bottom: 1.5rem;
        }

        .summary-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 0.5rem;
        }

        .summary-label {
            color: var(--text-light);
        }

        .summary-value {
            color: var(--white);
            font-weight: 600;
        }

        .summary-total {
            display: flex;
            justify-content: space-between;
            margin-top: 1rem;
            padding-top: 1rem;
            border-top: 1px solid rgba(255, 180, 0, 0.1);
        }

        .total-label {
            color: var(--gold);
            font-size: 1.2rem;
            font-weight: 700;
        }

        .total-value {
            color: var(--gold);
            font-size: 1.5rem;
            font-weight: 800;
        }

        .discount-badge {
            background: linear-gradient(135deg, #FF0000 0%, #FF6B00 100%);
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: 700;
            font-size: 0.9rem;
            margin-bottom: 1.5rem;
            text-align: center;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { box-shadow: 0 0 0 0 rgba(255, 0, 0, 0.7); }
            50% { box-shadow: 0 0 0 10px rgba(255, 0, 0, 0); }
        }

        /* Cart Actions */
        .cart-actions {
            display: flex;
            gap: 1rem;
            justify-content: center;
        }

        .btn-cart {
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
            display: inline-flex;
            align-items: center;
            gap: 10px;
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.3);
        }

        .btn-cart:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255, 180, 0, 0.6);
            color: var(--dark-blue);
        }

        .btn-share {
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
            display: inline-flex;
            align-items: center;
            gap: 10px;
        }

        .btn-share:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: transparent;
        }

        /* Empty Cart */
        .empty-cart {
            text-align: center;
            padding: 3rem 0;
        }

        .empty-cart-icon {
            font-size: 4rem;
            color: var(--gold);
            opacity: 0.5;
            margin-bottom: 1rem;
        }

        .empty-cart-text {
            color: var(--text-light);
            font-size: 1.2rem;
            margin-bottom: 1.5rem;
        }

        .empty-cart-description {
            color: var(--text-light);
            font-size: 1rem;
            margin-bottom: 2rem;
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
            .services-grid {
                grid-template-columns: 1fr;
            }
            
            .cart-actions {
                flex-direction: column;
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
                    <li class="nav-item"><a class="nav-link" href="/pricing">Harga Paket</a></li>
                    <li class="nav-item"><a class="nav-link" href="/contact">Hubungi</a></li>
                    <li class="nav-item"><a class="nav-link active" href="/calculator">Hitung Harga</a></li>
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
    <section class="hero-calculator">
        <div class="container">
            <div class="text-center" data-aos="fade-up">
                <h1 class="section-title">Hitung Harga</h1>
                <p class="section-subtitle">Kalkulasi estimasi biaya untuk kebutuhan digital bisnis Anda</p>
                <div style="display: inline-block; background: linear-gradient(135deg, #FF0000 0%, #FF6B00 100%); border: 2px solid #FFD700; padding: 12px 25px; border-radius: 50px; animation: pulse 2s infinite; box-shadow: 0 10px 30px rgba(255,0,0,0.5); margin-bottom: 2rem;">
                    <i class="bi bi-calculator" style="color: #FFD700; font-size: 1.3rem;"></i>
                    <span style="color: white; font-weight: 800; margin-left: 8px; font-size: 1.1rem;">Hitung Sendiri!</span>
                </div>
            </div>
        </div>
    </section>

    <!-- Calculator Section -->
    <section class="calculator-section">
        <div class="container">
            <div class="calculator-container" data-aos="fade-up">
                <!-- Service Selection -->
                <div class="service-selection">
                    <div class="service-tabs">
                        <button class="service-tab active" onclick="filterServices('all')">Semua Layanan</button>
                        <?php foreach($categories as $category): ?>
                        <button class="service-tab" onclick="filterServices('<?= $category ?>')"><?= $category ?></button>
                        <?php endforeach; ?>
                    </div>
                </div>
                
                <!-- Services Grid -->
                <div class="services-grid" id="servicesGrid">
                    <?php foreach($services as $index => $service): ?>
                    <div class="service-item" data-category="<?= $service['category'] ?>" data-id="<?= $service['id'] ?>" data-price="<?= $service['price_start'] ?>" data-name="<?= htmlspecialchars($service['name']) ?>" onclick="toggleService(<?= $service['id'] ?>)">
                        <div class="service-header">
                            <div class="service-icon">
                                <i class="bi bi-<?= $service['icon'] ?>"></i>
                            </div>
                            <div>
                                <h5 class="service-title"><?= htmlspecialchars($service['name']) ?></h5>
                                <p class="service-price">Rp <?= number_format($service['price_start'], 0, ',', '.') ?> / <?= htmlspecialchars($service['price_unit']) ?></p>
                                <p class="service-description"><?= htmlspecialchars($service['description']) ?></p>
                            </div>
                        </div>
                        <div class="service-checkbox">
                            <i class="bi bi-check-lg"></i>
                        </div>
                    </div>
                    <?php endforeach; ?>
                </div>
                
                <!-- Cart Section -->
                <div class="cart-section">
                    <div class="cart-header">
                        <h3 class="cart-title">Keranjang Belanja</h3>
                        <button class="cart-clear" onclick="clearCart()">
                            <i class="bi bi-trash"></i> Kosongkan
                        </button>
                    </div>
                    
                    <div class="cart-items" id="cartItems">
                        <!-- Cart items will be dynamically added here -->
                    </div>
                    
                    <div class="empty-cart" id="emptyCart">
                        <div class="empty-cart-icon">
                            <i class="bi bi-cart-x"></i>
                        </div>
                        <h3>Keranjang Belanja Kosong</h3>
                        <p class="empty-cart-description">Pilih layanan yang Anda butuhkan untuk mulai menghitung harga</p>
                    </div>
                    
                    <div class="cart-summary" id="cartSummary" style="display: none;">
                        <div class="summary-row">
                            <span class="summary-label">Subtotal:</span>
                            <span class="summary-value" id="subtotal">Rp 0</span>
                        </div>
                        <div class="summary-row">
                            <span class="summary-label">Diskon:</span>
                            <span class="summary-value" id="discount">Rp 0</span>
                        </div>
                        <div class="discount-badge" id="discountBadge" style="display: none;">
                            <i class="bi bi-tag-fill"></i>
                            <span id="discountText">Diskon 10%</span>
                        </div>
                        <div class="summary-total">
                            <span class="total-label">Total:</span>
                            <span class="total-value" id="total">Rp 0</span>
                        </div>
                    </div>
                    
                    <div class="cart-actions">
                        <a href="#" class="btn-cart" onclick="shareToWhatsApp()">
                            <i class="bi bi-whatsapp"></i> Bagikan ke WhatsApp
                        </a>
                        <a href="#" class="btn-share" onclick="saveCalculation()">
                            <i class="bi bi-save"></i> Simpan
                        </a>
                    </div>
                </div>
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
                            <span>Apakah harga yang ditampilkan sudah final?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Harga yang ditampilkan adalah harga awal. Ada kemungkinan biaya tambahan untuk fitur khusus yang Anda minta. Konsultasikan dengan tim kami untuk harga final yang sesuai kebutuhan Anda.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="200">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah saya bisa mendapatkan diskon?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Ya! Kami memberikan diskon otomatis untuk pembelian 3+ layanan (10%) atau 5+ layanan (15%). Diskon akan otomatis dihitung saat Anda menambahkan lebih dari batas minimum.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="300">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah hasil kalkulasi bisa disimpan?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Ya! Anda bisa menyimpan hasil kalkulasi dengan tombol "Simpan". Hasil akan tersimpan di browser Anda dan bisa diakses kapan saja.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="400">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah ada biaya tambahan untuk maintenance?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Ya, ada biaya maintenance tahunan untuk setiap paket. Biaya maintenance sudah termasuk dalam harga paket untuk tahun pertama. Untuk tahun berikutnya, ada biaya renewal yang lebih terjangkau.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="500">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah saya bisa membayar dengan cicilan?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Ya! Kami bekerja sama dengan berbagai platform cicilan seperti Kredivo, Akulaku, dan lainnya. Hubungi kami untuk informasi lebih lanjut.</p>
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
                        Siap untuk Memulai Bisnis Digital Anda?
                    </h2>
                    <p style="color: var(--text-light); font-size: 1.1rem; margin: 0;">
                        Hitung estimasi biaya sekarang juga dan dapatkan penawaran gratis dari tim digital expert kami!
                    </p>
                </div>
                <div class="col-lg-4 text-lg-end mt-3 mt-lg-0">
                    <a href="https://wa.me/6283173868915?text=Halo%20Situneo,%20saya%20mau%20konsultasi%20gratis%20tentang%20kebutuhan%20digital%20saya" 
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
    <a href="https://wa.me/6283173868915?text=Halo%20Situneo,%20saya%20mau%20konsultasi%20gratis%20tentang%20kebutuhan%20digital%20saya" 
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
        
        // Cart functionality
        let cart = [];
        
        // Toggle FAQ
        function toggleFaq(element) {
            const answer = element.nextElementSibling;
            const icon = element.querySelector('.faq-icon');
            
            answer.classList.toggle('show');
            icon.classList.toggle('rotate');
        }
        
        // Filter Services
        function filterServices(category) {
            const items = document.querySelectorAll('.service-item');
            const filterBtns = document.querySelectorAll('.service-tab');
            
            // Update active button
            filterBtns.forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            // Filter items
            items.forEach(item => {
                if (category === 'all' || item.dataset.category === category) {
                    item.style.display = 'block';
                } else {
                    item.style.display = 'none';
                }
            });
        }
        
        // Toggle Service Selection
        function toggleService(serviceId) {
            const serviceCard = document.querySelector(`.service-item[data-id="${serviceId}"]`);
            const checkbox = serviceCard.querySelector('.service-checkbox');
            const service = {
                id: serviceId,
                name: serviceCard.dataset.name,
                price: parseInt(serviceCard.dataset.price),
                quantity: 1
            };
            
            // Check if service already in cart
            const existingIndex = cart.findIndex(item => item.id === serviceId);
            
            if (existingIndex !== -1) {
                // Remove from cart
                cart.splice(existingIndex, 1);
                serviceCard.classList.remove('selected');
            } else {
                // Add to cart
                cart.push(service);
                serviceCard.classList.add('selected');
            }
            
            updateCart();
        }
        
        // Update Cart Display
        function updateCart() {
            const cartItems = document.getElementById('cartItems');
            const emptyCart = document.getElementById('emptyCart');
            const cartSummary = document.getElementById('cartSummary');
            
            if (cart.length === 0) {
                cartItems.innerHTML = '';
                emptyCart.style.display = 'block';
                cartSummary.style.display = 'none';
            } else {
                emptyCart.style.display = 'none';
                cartSummary.style.display = 'block';
                
                let cartHTML = '';
                let subtotal = 0;
                
                cart.forEach(item => {
                    subtotal += item.price * item.quantity;
                    
                    cartHTML += `
                        <div class="cart-item">
                            <div class="cart-item-info">
                                <h6 class="cart-item-name">${item.name}</h6>
                                <p class="cart-item-price">Rp ${number_format(item.price, 0, ',', '.')} / ${item.quantity} unit</p>
                            </div>
                            <div class="cart-item-quantity">
                                <button class="quantity-btn" onclick="updateQuantity(${item.id}, -1)">
                                    <i class="bi bi-dash"></i>
                                </button>
                                <div class="quantity-value">${item.quantity}</div>
                                <button class="quantity-btn" onclick="updateQuantity(${item.id}, 1)">
                                    <i class="bi bi-plus"></i>
                                </button>
                                <button class="cart-item-remove" onclick="removeFromCart(${item.id})">
                                    <i class="bi bi-trash"></i>
                                </button>
                            </div>
                        </div>
                    `;
                });
                
                cartItems.innerHTML = cartHTML;
                
                // Calculate discount
                let discount = 0;
                if (cart.length >= 5) {
                    discount = 15;
                } else if (cart.length >= 3) {
                    discount = 10;
                }
                
                const discountAmount = subtotal * (discount / 100);
                const total = subtotal - discountAmount;
                
                // Update summary
                document.getElementById('subtotal').textContent = `Rp ${number_format(subtotal, 0, ',', '.')}`;
                document.getElementById('discount').textContent = `Rp ${number_format(discountAmount, 0, ',', '.')}`;
                document.getElementById('total').textContent = `Rp ${number_format(total, 0, ',', '.')}`;
                
                // Show/hide discount badge
                const discountBadge = document.getElementById('discountBadge');
                if (discount > 0) {
                    discountBadge.style.display = 'block';
                    document.getElementById('discountText').textContent = `Diskon ${discount}%`;
                } else {
                    discountBadge.style.display = 'none';
                }
            }
        }
        
        // Update Quantity
        function updateQuantity(serviceId, change) {
            const itemIndex = cart.findIndex(item => item.id === serviceId);
            
            if (itemIndex !== -1) {
                cart[itemIndex].quantity += change;
                
                // Ensure quantity doesn't go below 1
                if (cart[itemIndex].quantity < 1) {
                    cart[itemIndex].quantity = 1;
                }
                
                updateCart();
            }
        }
        
        // Remove from Cart
        function removeFromCart(serviceId) {
            const itemIndex = cart.findIndex(item => item.id === serviceId);
            
            if (itemIndex !== -1) {
                cart.splice(itemIndex, 1);
                
                // Remove selected class from service card
                const serviceCard = document.querySelector(`.service-item[data-id="${serviceId}"]`);
                if (serviceCard) {
                    serviceCard.classList.remove('selected');
                }
                
                updateCart();
            }
        }
        
        // Clear Cart
        function clearCart() {
            cart = [];
            
            // Remove selected class from all service cards
            document.querySelectorAll('.service-item.selected').forEach(card => {
                card.classList.remove('saveCalculation');
            });
            
            updateCart();
        }
        
        // Save Calculation
        function saveCalculation() {
            if (cart.length === 0) {
                alert('Tidak ada item di keranjang belanja untuk disimpan!');
                return;
            }
            
            // Create calculation data
            const calculationData = {
                date: new Date().toISOString(),
                services: cart,
                subtotal: document.getElementById('subtotal').textContent,
                discount: document.getElementById('discount').textContent,
                total: document.getElementById('total').textContent,
                settings: <?= json_encode($settings) ?>
            };
            
            // Save to localStorage
            localStorage.setItem('situneo_calculation', JSON.stringify(calculationData));
            
            // Show success message
            alert('Perhitungan berhasil disimpan!');
        }
        
        // Load Saved Calculation
        function loadCalculation() {
            const savedCalculation = localStorage.getItem('situneo_calculation');
            
            if (savedCalculation) {
                const calculationData = JSON.parse(savedCalculation);
                
                // Clear current cart
                clearCart();
                
                // Restore cart from saved data
                cart = calculationData.services || [];
                
                // Add selected class to service cards
                cart.forEach(item => {
                    const serviceCard = document.querySelector(`.service-item[data-id="${item.id}"]`);
                    if (serviceCard) {
                        serviceCard.classList.add('saveCalculation');
                    }
                });
                
                updateCart();
            }
        }
        
        // Share to WhatsApp
        function shareToWhatsApp() {
            if (cart.length === 0) {
                alert('Tidak ada item di keranjang belanja untuk dibagikan!');
                return;
            }
            
            let message = "Halo Situneo Digital! Saya tertarik dengan perhitungan harga berikut:\n\n";
            
            cart.forEach((item, index) => {
                message += `${index + 1}. ${item.name}\n`;
                message += `   Harga: Rp ${number_format(item.price * item.quantity, 0, ',', '.')}\n`;
                message += `   Jumlah: ${item.quantity} unit\n`;
                message += `   Subtotal: Rp ${number_format(item.price * item.quantity, 0, ',', '.')}\n\n`;
            });
            
            const subtotal = document.getElementById('subtotal').textContent;
            const discount = document.getElementById('discount').textContent;
            const total = document.getElementById('total').textContent;
            
            message += `Subtotal: ${subtotal}\n`;
            
            if (discount !== 'Rp 0') {
                message += `Diskon: ${discount}\n`;
            }
            
            message += `Total: ${total}\n\n`;
            message += `Mohon informasi lebih lanjut!`;
            
            const whatsappURL = `https://wa.me/6283173868915?text=${encodeURIComponent(message)}`;
            window.open(whatsappURL, '_blank');
        }
        
        // Load saved calculation on page load
        window.addEventListener('DOMContentLoaded', function() {
            loadCalculation();
        });
    </script>
</body>
</html>
