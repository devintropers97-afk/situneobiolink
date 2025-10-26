<?php
/**
 * About Page
 */

// Get company settings from database
 $settings = [];
 $result = $conn->query("SELECT setting_key, setting_value FROM settings");
if ($result) {
    while ($row = $result->fetch_assoc()) {
        $settings[$row['setting_key']] = $row['setting_value'];
    }
}

// Team data
 $team = [
    [
        'name' => 'Devin Prasetyo Hermawan',
        'position' => 'CEO & Founder',
        'photo' => 'https://ui-avatars.com/api/?name=Devin+Prasetyo&size=150&background=FFB400&color=0F3057&bold=true',
        'bio' => 'Entrepreneur digital dengan passion membantu UMKM go digital. Memiliki pengalaman 8+ tahun di industri teknologi dan digital marketing.',
        'social' => [
            'linkedin' => 'https://linkedin.com/in/devin-prasetyo',
            'instagram' => 'https://instagram.com/situneodigital',
            'facebook' => 'https://facebook.com/situneodigital'
        ]
    ],
    [
        'name' => 'Budi Santoso',
        'position' => 'Chief Technology Officer',
        'photo' => 'https://ui-avatars.com/api/?name=Budi+Santoso&size=150&background=FFB400&color=0F3057&bold=true',
        'bio' => 'Full-stack developer berpengalaman. Expert dalam PHP, Node.js, dan Cloud Architecture.',
        'social' => [
            'linkedin' => '#',
            'instagram' => '#',
            'facebook' => '#'
        ]
    ],
    [
        'name' => 'Sarah Wijaya',
        'position' => 'Creative Director & Head of Design',
        'photo' => 'https://ui-avatars.com/api/?name=Sarah+Wijaya&size=150&background=FFB400&color=0F3057&bold=true',
        'bio' => 'UI/UX designer dengan portfolio 200+ project. Spesialis dalam user experience yang intuitif.',
        'social' => [
            'linkedin' => '#',
            'instagram' => '#',
            'facebook' => '#'
        ]
    ],
    [
        'name' => 'Maya Putri',
        'position' => 'Head of Digital Marketing',
        'photo' => 'https://ui-avatars.com/api/?name=Maya+Putri&size=150&background=FFB400&color=0F3057&bold=true',
        'bio' => 'Digital marketing strategist dengan expertise di SEO, Google Ads, dan Social Media Marketing.',
        'social' => [
            'linkedin' => '#',
            'instagram' => '#',
            'facebook' => '#'
        ]
    ]
];
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tentang Kami - <?= $settings['company_name'] ?? 'SITUNEO DIGITAL' ?></title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.0/font/bootstrap-icons.css" rel="stylesheet">
    <link href="https://unpkg.com/aos@2.3.1/dist/aos.css" rel="stylesheet">
    
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
            font-family: 'Inter', 'Segoe UI', Arial, sans-serif;
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
        .hero-section {
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

        /* Story Section */
        .story-content {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.2) 0%, rgba(15, 48, 87, 0.3) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255,180,0,0.3);
            border-radius: 25px;
            padding: 3rem;
        }

        /* Mission/Vision Cards */
        .mission-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.3) 0%, rgba(15, 48, 87, 0.4) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255,180,0,0.3);
            border-radius: 25px;
            padding: 2.5rem;
            height: 100%;
            position: relative;
            overflow: hidden;
            transition: all 0.4s;
        }
        
        .mission-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,180,0,0.1) 0%, transparent 70%);
            animation: rotate 20s linear infinite;
        }
        
        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }
        
        .mission-card:hover {
            transform: translateY(-10px);
            border-color: var(--gold);
            box-shadow: 0 20px 50px rgba(255,180,0,0.4);
        }
        
        .mission-icon {
            font-size: 4rem;
            color: var(--gold);
            margin-bottom: 1.5rem;
            filter: drop-shadow(0 5px 15px rgba(255,180,0,0.5));
            position: relative;
            z-index: 1;
        }
        
        .mission-title {
            font-size: 2rem;
            font-weight: 800;
            color: var(--gold);
            margin-bottom: 1rem;
            position: relative;
            z-index: 1;
        }
        
        .mission-desc {
            color: var(--text-light);
            font-size: 1.15rem;
            line-height: 1.8;
            position: relative;
            z-index: 1;
        }

        /* Values Grid */
        .value-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.2) 0%, rgba(15, 48, 87, 0.3) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255,180,0,0.2);
            border-radius: 20px;
            padding: 2rem;
            text-align: center;
            transition: all 0.3s;
            height: 100%;
        }
        
        .value-card:hover {
            transform: translateY(-10px);
            border-color: var(--gold);
            box-shadow: 0 15px 40px rgba(255,180,0,0.3);
        }
        
        .value-icon {
            width: 80px;
            height: 80px;
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            color: white;
            margin: 0 auto 1.5rem;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        
        .value-title {
            font-size: 1.4rem;
            font-weight: 700;
            color: var(--gold);
            margin-bottom: 1rem;
        }
        
        .value-desc {
            color: var(--text-light);
            line-height: 1.7;
        }

        /* Team Cards */
        .team-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.2) 0%, rgba(15, 48, 87, 0.3) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255,180,0,0.2);
            border-radius: 20px;
            padding: 2rem;
            text-align: center;
            transition: all 0.3s;
            height: 100%;
        }
        
        .team-card:hover {
            transform: translateY(-10px);
            border-color: var(--gold);
            box-shadow: 0 15px 40px rgba(255,180,0,0.3);
        }
        
        .team-avatar {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            margin: 0 auto 1.5rem;
            border: 3px solid var(--gold);
            box-shadow: 0 10px 20px rgba(0,0,0,0.3);
        }
        
        .team-name {
            font-size: 1.3rem;
            font-weight: 700;
            color: var(--gold);
            margin-bottom: 0.5rem;
        }
        
        .team-position {
            color: var(--text-light);
            font-size: 1rem;
            margin-bottom: 1rem;
        }
        
        .team-bio {
            color: var(--text-light);
            line-height: 1.6;
            margin-bottom: 1.5rem;
        }
        
        .team-social {
            display: flex;
            justify-content: center;
            gap: 1rem;
        }
        
        .team-social a {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(255,180,0,0.2);
            border: 1px solid var(--gold);
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--gold);
            transition: all 0.3s;
        }
        
        .team-social a:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            transform: translateY(-3px);
        }

        /* Timeline */
        .timeline {
            position: relative;
            padding: 2rem 0;
        }
        
        .timeline::before {
            content: '';
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 2px;
            height: 100%;
            background: var(--gold);
        }
        
        .timeline-item {
            position: relative;
            margin-bottom: 3rem;
        }
        
        .timeline-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: var(--gold);
            border: 4px solid var(--dark-blue);
            z-index: 1;
        }
        
        .timeline-content {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.2) 0%, rgba(15, 48, 87, 0.3) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255,180,0,0.2);
            border-radius: 20px;
            padding: 2rem;
            width: 45%;
        }
        
        .timeline-item:nth-child(odd) .timeline-content {
            margin-left: auto;
        }
        
        .timeline-year {
            font-size: 1.5rem;
            font-weight: 800;
            color: var(--gold);
            margin-bottom: 0.5rem;
        }
        
        .timeline-title {
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--white);
            margin-bottom: 1rem;
        }
        
        .timeline-desc {
            color: var(--text-light);
            line-height: 1.6;
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

        /* Why Choose Us */
        .why-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.2) 0%, rgba(15, 48, 87, 0.3) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255,180,0,0.2);
            border-radius: 20px;
            padding: 2rem;
            text-align: center;
            transition: all 0.3s;
            height: 100%;
        }
        
        .why-card:hover {
            transform: translateY(-10px);
            border-color: var(--gold);
            box-shadow: 0 15px 40px rgba(255,180,0,0.3);
        }
        
        .why-icon {
            width: 80px;
            height: 80px;
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            color: white;
            margin: 0 auto 1.5rem;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        
        .why-title {
            font-size: 1.3rem;
            font-weight: 700;
            color: var(--gold);
            margin-bottom: 1rem;
        }
        
        .why-desc {
            color: var(--text-light);
            line-height: 1.6;
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
            .timeline::before {
                left: 30px;
            }
            
            .timeline-item::before {
                left: 30px;
            }
            
            .timeline-content {
                width: calc(100% - 80px);
                margin-left: 60px !important;
            }
            
            .timeline-item:nth-child(odd) .timeline-content {
                margin-left: 60px !important;
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
                    <li class="nav-item"><a class="nav-link active" href="/about">Tentang Kami</a></li>
                    <li class="nav-item"><a class="nav-link" href="/services">Layanan</a></li>
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
    <section class="hero-section">
        <div class="container">
            <div class="text-center" data-aos="fade-up">
                <h1 class="section-title">Tentang Kami</h1>
                <p class="section-subtitle">Menjadi partner digital terpercaya untuk kesuksesan bisnis Anda</p>
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
                            <i class="bi bi-people-fill" style="color: var(--gold);"></i>
                        </div>
                        <div class="stat-number">500+</div>
                        <div class="stat-label">Happy Clients</div>
                    </div>
                </div>
                <div class="col-md-3 col-6" data-aos="zoom-in" data-aos-delay="200">
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="bi bi-briefcase-fill" style="color: var(--gold);"></i>
                        </div>
                        <div class="stat-number">1200+</div>
                        <div class="stat-label">Projects Done</div>
                    </div>
                </div>
                <div class="col-md-3 col-6" data-aos="zoom-in" data-aos-delay="300">
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="bi bi-star-fill" style="color: var(--gold);"></i>
                        </div>
                        <div class="stat-number">4.9/5.0</div>
                        <div class="stat-label">Satisfaction Rate</div>
                    </div>
                </div>
                <div class="col-md-3 col-6" data-aos="zoom-in" data-aos-delay="400">
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="bi bi-clock-fill" style="color: var(--gold);"></i>
                        </div>
                        <div class="stat-number">5+ Tahun</div>
                        <div class="stat-label">Experience</div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Story Section -->
    <section class="py-5">
        <div class="container">
            <div class="row">
                <div class="col-lg-12" data-aos="fade-up">
                    <div class="story-content">
                        <h2 class="section-title">Cerita Kami</h2>
                        <p class="lead" style="color: var(--text-light); line-height: 1.8; margin-bottom: 1.5rem;">
                            <?= $settings['company_name'] ?? 'SITUNEO DIGITAL' ?> adalah perusahaan digital yang berfokus pada penyediaan solusi digital lengkap untuk bisnis Anda. Kami menyediakan berbagai layanan mulai dari pembuatan website, SEO, digital marketing, hingga pengembangan aplikasi khusus.
                        </p>
                        <p class="lead" style="color: var(--text-light); line-height: 1.8; margin-bottom: 1.5rem;">
                            Didirikan pada tahun 2020, kami telah membantu lebih dari 500 klien dari berbagai industri untuk transformasi digital. Dengan tim yang berpengalaman dan profesional, kami siap membantu bisnis Anda berkembang di era digital. Kami percaya bahwa setiap bisnis memiliki potensi untuk sukses jika didukung oleh solusi digital yang tepat.
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Mission & Vision Section -->
    <section class="py-5">
        <div class="container">
            <div class="text-center mb-5" data-aos="fade-up">
                <h2 class="section-title">Visi & Misi</h2>
                <p class="section-subtitle">Arah dan tujuan kami untuk mendukung kesuksesan Anda</p>
            </div>
            <div class="row g-4">
                <div class="col-lg-6" data-aos="fade-right">
                    <div class="mission-card">
                        <div class="mission-icon">
                            <i class="bi bi-eye-fill"></i>
                        </div>
                        <h3 class="mission-title">Visi Kami</h3>
                        <p class="mission-desc">
                            Menjadi digital agency terdepan di Indonesia yang memberdayakan UMKM dan brand lokal untuk bersaing secara global melalui solusi digital inovatif dan teknologi terkini.
                        </p>
                    </div>
                </div>
                <div class="col-lg-6" data-aos="fade-left">
                    <div class="mission-card">
                        <div class="mission-icon">
                            <i class="bi bi-bullseye"></i>
                        </div>
                        <h3 class="mission-title">Misi Kami</h3>
                        <p class="mission-desc">
                            <ul style="list-style: none; padding-left: 0;">
                                <li style="margin-bottom: 0.8rem;"><i class="bi bi-check-circle-fill text-warning me-2"></i> Menyediakan solusi digital berkualitas tinggi dengan harga terjangkau untuk UMKM Indonesia</li>
                                <li style="margin-bottom: 0.8rem;"><i class="bi bi-check-circle-fill text-warning me-2"></i> Membantu bisnis lokal go digital dengan strategi dan teknologi yang tepat</li>
                                <li style="margin-bottom: 0.8rem;"><i class="bi bi-check-circle-fill text-warning me-2"></i> Memberikan training dan edukasi digital untuk meningkatkan kapabilitas tim client</li>
                                <li style="margin-bottom: 0.8rem;"><i class="bi bi-check-circle-fill text-warning me-2"></i> Membangun ekosistem kerja sama dengan freelancer berbakat melalui sistem komisi yang adil</li>
                                <li><i class="bi bi-check-circle-fill text-warning me-2"></i> Menjaga kepuasan client melalui support 24/7 dan garansi kepuasan 100%</li>
                            </ul>
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Values Section -->
    <section class="py-5">
        <div class="container">
            <div class="text-center mb-5" data-aos="fade-up">
                <h2 class="section-title">Nilai-Nilai Kami</h2>
                <p class="section-subtitle">Prinsip yang memandu setiap langkah kami</p>
            </div>
            <div class="row g-4">
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="100">
                    <div class="value-card">
                        <div class="value-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-shield-check" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="value-title">Integritas</h5>
                        <p class="value-desc">
                            Kami berkomitmen untuk selalu jujur, transparan, dan dapat dipercaya dalam setiap project yang kami kerjakan.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="200">
                    <div class="value-card">
                        <div class="value-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-lightbulb" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="value-title">Inovasi</h5>
                        <p class="value-desc">
                            Kami terus berinovasi menggunakan teknologi terkini untuk memberikan solusi terbaik bagi klien kami.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="300">
                    <div class="value-card">
                        <div class="value-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-heart" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="value-title">Dedikasi</h5>
                        <p class="value-desc">
                            Kesuksesan klien adalah prioritas utama kami. Kami dedikasikan waktu dan keahlian untuk hasil maksimal.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="400">
                    <div class="value-card">
                        <div class="value-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-star" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="value-title">Kualitas</h5>
                        <p class="value-desc">
                            Kami tidak pernah kompromi dengan kualitas. Setiap project dikerjakan dengan standar tertinggi.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="500">
                    <div class="value-card">
                        <div class="value-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-cash-stack" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="value-title">Harga Terjangkau</h5>
                        <p class="value-desc">
                            Kami percaya solusi digital berkualitas harus accessible untuk semua bisnis, besar maupun kecil.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="600">
                    <div class="value-card">
                        <div class="value-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-people" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="value-title">Kolaborasi</h5>
                        <p class="value-desc">
                            Kami bekerja sebagai partner, bukan vendor. Tim kami siap mendengar dan berkolaborasi dengan Anda.
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Team Section -->
    <section class="py-5">
        <div class="container">
            <div class="text-center mb-5" data-aos="fade-up">
                <h2 class="section-title">Tim Kami</h2>
                <p class="section-subtitle">Profesional berpengalaman yang siap membantu kesuksesan Anda</p>
            </div>
            <div class="row g-4">
                <?php foreach($team as $index => $member): ?>
                <div class="col-lg-3 col-md-6" data-aos="zoom-in" data-aos-delay="<?= ($index + 1) * 100 ?>">
                    <div class="team-card">
                        <img src="<?= $member['photo'] ?>" class="team-avatar" alt="<?= $member['name'] ?>">
                        <h5 class="team-name"><?= $member['name'] ?></h5>
                        <p class="team-position"><?= $member['position'] ?></p>
                        <p class="team-bio"><?= $member['bio'] ?></p>
                        <div class="team-social">
                            <a href="<?= $member['social']['linkedin'] ?>" target="_blank">
                                <i class="bi bi-linkedin"></i>
                            </a>
                            <a href="<?= $member['social']['instagram'] ?>" target="_blank">
                                <i class="bi bi-instagram"></i>
                            </a>
                            <a href="<?= $member['social']['facebook'] ?>" target="_blank">
                                <i class="bi bi-facebook"></i>
                            </a>
                        </div>
                    </div>
                </div>
                <?php endforeach; ?>
            </div>
        </div>
    </section>

    <!-- Timeline Section -->
    <section class="py-5">
        <div class="container">
            <div class="text-center mb-5" data-aos="fade-up">
                <h2 class="section-title">Perjalanan Kami</h2>
                <p class="section-subtitle">Milestone penting dalam perjalanan Situneo Digital</p>
            </div>
            <div class="timeline">
                <div class="timeline-item" data-aos="fade-right">
                    <div class="timeline-content">
                        <div class="timeline-year">2020</div>
                        <h4 class="timeline-title">Berdiri</h4>
                        <p class="timeline-desc">
                            Situneo Digital resmi berdiri dengan 3 orang tim dan visi besar membantu UMKM go digital.
                        </p>
                    </div>
                </div>
                <div class="timeline-item" data-aos="fade-left">
                    <div class="timeline-content">
                        <div class="timeline-year">2021</div>
                        <h4 class="timeline-title">100 Klien</h4>
                        <p class="timeline-desc">
                            Mencapai milestone 100 klien happy dalam 1 tahun pertama dengan repeat order 70%.
                        </p>
                    </div>
                </div>
                <div class="timeline-item" data-aos="fade-right">
                    <div class="timeline-content">
                        <div class="timeline-year">2022</div>
                        <h4 class="timeline-title">Ekspansi Tim</h4>
                        <p class="timeline-desc">
                            Tim berkembang menjadi 15 orang profesional di berbagai bidang digital.
                        </p>
                    </div>
                </div>
                <div class="timeline-item" data-aos="fade-left">
                    <div class="timeline-content">
                        <div class="timeline-year">2023</div>
                        <h4 class="timeline-title">500+ Projects</h4>
                        <p class="timeline-desc">
                            Berhasil menyelesaikan 500+ project dengan satisfaction rate 98%.
                        </p>
                    </div>
                </div>
                <div class="timeline-item" data-aos="fade-right">
                    <div class="timeline-content">
                        <div class="timeline-year">2024</div>
                        <h4 class="timeline-title">NIB Resmi</h4>
                        <p class="timeline-desc">
                            Mendapat NIB resmi dan sertifikasi sebagai perusahaan digital agency terpercaya.
                        </p>
                    </div>
                </div>
                <div class="timeline-item" data-aos="fade-left">
                    <div class="timeline-content">
                        <div class="timeline-year">2025</div>
                        <h4 class="timeline-title">Going National</h4>
                        <p class="timeline-desc">
                            Melayani klien dari seluruh Indonesia dengan target 1000+ projects di tahun ini.
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Why Choose Us Section -->
    <section class="py-5">
        <div class="container">
            <div class="text-center mb-5" data-aos="fade-up">
                <h2 class="section-title">Kenapa Pilih Kami</h2>
                <p class="section-subtitle">Keunggulan yang membuat kami berbeda</p>
            </div>
            <div class="row g-4">
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="100">
                    <div class="why-card">
                        <div class="why-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-award" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="why-title">500+ Happy Clients</h5>
                        <p class="why-desc">
                            Dipercaya oleh ratusan bisnis dari berbagai industri di seluruh Indonesia.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="200">
                    <div class="why-card">
                        <div class="why-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-cash-coin" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="why-title">Harga Terjangkau</h5>
                        <p class="why-desc">
                            Kualitas premium dengan harga yang ramah di kantong UMKM hingga enterprise.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="300">
                    <div class="why-card">
                        <div class="why-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-lightning-charge" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="why-title">Fast Response</h5>
                        <p class="why-desc">
                            Tim support kami siap membantu 24/7 dengan response time 1-2 jam.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="400">
                    <div class="why-card">
                        <div class="why-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-shield-check" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="why-title">Garansi Kepuasan</h5>
                        <p class="why-desc">
                            Kami garansi 100% kepuasan atau revisi unlimited hingga Anda puas.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="500">
                    <div class="why-card">
                        <div class="why-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-tools" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="why-title">Teknologi Terkini</h5>
                        <p class="why-desc">
                            Menggunakan tech stack terbaru dan best practices dalam development.
                        </p>
                    </div>
                </div>
                <div class="col-lg-4 col-md-6" data-aos="zoom-in" data-aos-delay="600">
                    <div class="why-card">
                        <div class="why-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-graph-up-arrow" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="why-title">Proven Results</h5>
                        <p class="why-desc">
                            Track record 98% satisfaction rate dan banyak klien dengan repeat order.
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- CTA Section -->
    <section class="py-5" style="background: var(--gradient-primary);">
        <div class="container">
            <div class="row align-items-center" data-aos="fade-up">
                <div class="col-lg-8">
                    <h2 style="color: var(--gold); font-weight: 800; margin-bottom: 1rem; font-size: 2rem;">
                        Siap Bekerja Sama dengan Kami?
                    </h2>
                    <p style="color: var(--text-light); font-size: 1.1rem; margin: 0;">
                        Mari wujudkan kesuksesan digital bisnis Anda bersama kami. <strong style="color: var(--gold);">Konsultasi gratis!</strong>
                    </p>
                </div>
                <div class="col-lg-4 text-lg-end mt-3 mt-lg-0">
                    <a href="/contact" class="btn-gold btn-lg" style="font-size: 1.1rem;">
                        <i class="bi bi-whatsapp"></i> HUBUNGI KAMI
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
    
    <!-- JavaScript Libraries -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
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
