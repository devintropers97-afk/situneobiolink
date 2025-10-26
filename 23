<?php
/**
 * Contact Page
 */

// Get company settings from database
 $settings = [];
 $result = $conn->query("SELECT setting_key, setting_value FROM settings");
if ($result) {
    while ($row = $result->fetch_assoc()) {
        $settings[$row['setting_key']] = $row['setting_value'];
    }
}

// Process form submission
 $success = false;
 $error = '';
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Get form data
    $name = $_POST['name'] ?? '';
    $email = $_POST['email'] ?? '';
    $phone = $_POST['phone'] ?? '';
    $subject = $_POST['subject'] ?? '';
    $message = $_POST['message'] ?? '';
    
    // Validate form data
    if (empty($name) || empty($email) || empty($phone) || empty($subject) || empty($message)) {
        $error = 'Semua field harus diisi!';
    } elseif (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $error = 'Email tidak valid!';
    } else {
        // In a real application, you would send an email here
        // For now, we'll just simulate success
        $success = true;
        
        // You could also save to database
        // $stmt = $conn->prepare("INSERT INTO contact_messages (name, email, phone, subject, message, created_at) VALUES (?, ?, ?, ?, ?, NOW())");
        // $stmt->bind_param("sssss", $name, $email, $phone, $subject, $message);
        // $stmt->execute();
    }
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hubungi Kami - <?= $settings['company_name'] ?? 'SITUNEO DIGITAL' ?></title>
    <meta name="description" content="Hubungi Situneo Digital untuk konsultasi gratis. Kami siap membantu bisnis Anda sukses di era digital.">
    <meta name="keywords" content="hubungi kami, kontak, konsultasi, situneo digital, website, seo, digital marketing">
    
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
        .hero-contact {
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

        /* Contact Form Section */
        .contact-form-section {
            padding: 80px 0;
        }

        .contact-form-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255, 180, 0, 0.2);
            border-radius: 25px;
            padding: 3rem;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
        }

        .form-control, .form-select {
            background: rgba(15, 48, 87, 0.7);
            border: 2px solid rgba(255, 180, 0, 0.3);
            color: var(--white);
            border-radius: 15px;
            padding: 15px 20px;
            font-size: 1rem;
            transition: all 0.3s;
        }

        .form-control:focus, .form-select:focus {
            background: rgba(15, 48, 87, 0.9);
            border-color: var(--gold);
            box-shadow: 0 0 15px rgba(255, 180, 0, 0.3);
            color: var(--white);
            outline: none;
        }

        .form-control::placeholder {
            color: var(--text-light);
        }

        .form-label {
            color: var(--gold);
            font-weight: 600;
            margin-bottom: 0.5rem;
        }

        .btn-submit {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 15px 40px;
            font-weight: 700;
            border-radius: 50px;
            text-transform: uppercase;
            letter-spacing: 1px;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.3);
        }

        .btn-submit:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255, 180, 0, 0.6);
            color: var(--dark-blue);
        }

        /* Contact Info Section */
        .contact-info-section {
            padding: 80px 0;
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
        }

        .contact-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.2) 0%, rgba(15, 48, 87, 0.3) 100%);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 2.5rem;
            text-align: center;
            height: 100%;
            transition: all 0.3s;
        }

        .contact-card:hover {
            transform: translateY(-10px);
            border-color: var(--gold);
            box-shadow: 0 15px 40px rgba(255, 180, 0, 0.3);
        }

        .contact-icon {
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

        .contact-title {
            font-size: 1.3rem;
            font-weight: 700;
            color: var(--gold);
            margin-bottom: 1rem;
        }

        .contact-detail {
            color: var(--text-light);
            font-size: 1.1rem;
            margin-bottom: 0.5rem;
        }

        .contact-link {
            color: var(--text-light);
            text-decoration: none;
            transition: color 0.3s;
        }

        .contact-link:hover {
            color: var(--gold);
        }

        /* Map Section */
        .map-section {
            padding: 0;
            height: 450px;
            position: relative;
        }

        .map-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(15, 48, 87, 0.3);
            z-index: 1;
        }

        .map-container {
            width: 100%;
            height: 100%;
            position: relative;
            z-index: 0;
        }

        .map-container iframe {
            width: 100%;
            height: 100%;
            border: 0;
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

        /* Success/Error Messages */
        .alert-custom {
            border-radius: 15px;
            padding: 1rem 1.5rem;
            margin-bottom: 2rem;
            border: none;
        }

        .alert-success-custom {
            background: linear-gradient(135deg, rgba(39, 174, 96, 0.2) 0%, rgba(39, 174, 96, 0.1) 100%);
            border: 2px solid rgba(39, 174, 96, 0.3);
            color: #2ecc71;
        }

        .alert-error-custom {
            background: linear-gradient(135deg, rgba(231, 76, 60, 0.2) 0%, rgba(231, 76, 60, 0.1) 100%);
            border: 2px solid rgba(231, 76, 60, 0.3);
            color: #e74c3c;
        }

        /* Office Hours */
        .office-hours {
            background: linear-gradient(135deg, rgba(255, 180, 0, 0.1) 0%, rgba(255, 180, 0, 0.05) 100%);
            border: 2px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 2rem;
            margin-top: 2rem;
        }

        .office-hours-title {
            color: var(--gold);
            font-weight: 700;
            font-size: 1.2rem;
            margin-bottom: 1rem;
            text-align: center;
        }

        .office-hours-list {
            list-style: none;
            padding: 0;
        }

        .office-hours-list li {
            display: flex;
            justify-content: space-between;
            padding: 0.5rem 0;
            color: var(--text-light);
            border-bottom: 1px solid rgba(255, 180, 0, 0.1);
        }

        .office-hours-list li:last-child {
            border-bottom: none;
        }

        .day {
            font-weight: 600;
        }

        .time {
            color: var(--gold);
        }

        .closed {
            color: #e74c3c;
        }

        /* Social Media */
        .social-media {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin-top: 2rem;
        }

        .social-link {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: rgba(255, 180, 0, 0.1);
            border: 2px solid rgba(255, 180, 0, 0.3);
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--gold);
            font-size: 1.5rem;
            transition: all 0.3s;
            text-decoration: none;
        }

        .social-link:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            transform: translateY(-3px);
        }

        /* Trust Badge */
        .trust-badge {
            background: linear-gradient(135deg, rgba(255, 180, 0, 0.1) 0%, rgba(255, 180, 0, 0.05) 100%);
            border: 2px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 1.5rem;
            margin-top: 2rem;
            text-align: center;
        }

        .badge-title {
            color: var(--gold);
            font-weight: 700;
            font-size: 1.1rem;
            margin-bottom: 0.5rem;
        }

        .badge-text {
            color: var(--text-light);
            font-size: 0.9rem;
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
            .contact-form-card {
                padding: 2rem;
            }
            
            .contact-card {
                padding: 2rem;
            }
            
            .map-section {
                height: 350px;
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
                    <li class="nav-item"><a class="nav-link active" href="/contact">Hubungi</a></li>
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
    <section class="hero-contact">
        <div class="container">
            <div class="text-center" data-aos="fade-up">
                <h1 class="section-title">Hubungi Kami</h1>
                <p class="section-subtitle">Siap membantu bisnis Anda sukses di era digital</p>
            </div>
        </div>
    </section>

    <!-- Contact Form Section -->
    <section class="contact-form-section">
        <div class="container">
            <div class="row justify-content-center">
                <div class="col-lg-8" data-aos="fade-up">
                    <div class="contact-form-card">
                        <h2 class="text-center mb-4" style="color: var(--gold); font-weight: 800;">Kirim Pesan</h2>
                        
                        <?php if ($success): ?>
                        <div class="alert alert-success-custom alert-custom" role="alert">
                            <i class="bi bi-check-circle-fill me-2"></i>
                            Pesan Anda telah berhasil dikirim! Kami akan segera menghubungi Anda.
                        </div>
                        <?php endif; ?>
                        
                        <?php if ($error): ?>
                        <div class="alert alert-error-custom alert-custom" role="alert">
                            <i class="bi bi-exclamation-triangle-fill me-2"></i>
                            <?= $error ?>
                        </div>
                        <?php endif; ?>
                        
                        <form method="POST" action="">
                            <div class="row">
                                <div class="col-md-6 mb-3">
                                    <label for="name" class="form-label">Nama Lengkap</label>
                                    <input type="text" class="form-control" id="name" name="name" placeholder="Masukkan nama lengkap Anda" required>
                                </div>
                                <div class="col-md-6 mb-3">
                                    <label for="email" class="form-label">Email</label>
                                    <input type="email" class="form-control" id="email" name="email" placeholder="email@example.com" required>
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-6 mb-3">
                                    <label for="phone" class="form-label">Nomor WhatsApp</label>
                                    <input type="tel" class="form-control" id="phone" name="phone" placeholder="08xx-xxxx-xxxx" required>
                                </div>
                                <div class="col-md-6 mb-3">
                                    <label for="subject" class="form-label">Subjek</label>
                                    <select class="form-select" id="subject" name="subject" required>
                                        <option value="" selected disabled>Pilih subjek</option>
                                        <option value="Konsultasi Website">Konsultasi Website</option>
                                        <option value="Konsultasi SEO">Konsultasi SEO</option>
                                        <option value="Konsultasi Digital Marketing">Konsultasi Digital Marketing</option>
                                        <option value="Pesan Layanan">Pesan Layanan</option>
                                        <option value="Lainnya">Lainnya</option>
                                    </select>
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="message" class="form-label">Pesan</label>
                                <textarea class="form-control" id="message" name="message" rows="5" placeholder="Tuliskan pesan Anda di sini..." required></textarea>
                            </div>
                            <div class="text-center">
                                <button type="submit" class="btn btn-submit">
                                    <i class="bi bi-send-fill me-2"></i> Kirim Pesan
                                </button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Contact Info Section -->
    <section class="contact-info-section">
        <div class="container">
            <div class="text-center mb-5" data-aos="fade-up">
                <h2 class="section-title">Informasi Kontak</h2>
                <p class="section-subtitle">Hubungi kami melalui berbagai cara</p>
            </div>
            <div class="row g-4">
                <div class="col-md-4" data-aos="zoom-in" data-aos-delay="100">
                    <div class="contact-card">
                        <div class="contact-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-telephone-fill" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="contact-title">Telepon</h5>
                        <p class="contact-detail"><?= $settings['company_phone'] ?? '6283173868915' ?></p>
                        <a href="tel:<?= $settings['company_phone'] ?? '6283173868915' ?>" class="contact-link">
                            <i class="bi bi-telephone me-2"></i> Hubungi Sekarang
                        </a>
                    </div>
                </div>
                <div class="col-md-4" data-aos="zoom-in" data-aos-delay="200">
                    <div class="contact-card">
                        <div class="contact-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-envelope-fill" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="contact-title">Email</h5>
                        <p class="contact-detail"><?= $settings['support_email'] ?? 'support@situneo.my.id' ?></p>
                        <a href="mailto:<?= $settings['support_email'] ?? 'support@situneo.my.id' ?>" class="contact-link">
                            <i class="bi bi-envelope me-2"></i> Kirim Email
                        </a>
                    </div>
                </div>
                <div class="col-md-4" data-aos="zoom-in" data-aos-delay="300">
                    <div class="contact-card">
                        <div class="contact-icon" style="background: var(--gradient-gold);">
                            <i class="bi bi-geo-alt-fill" style="color: var(--dark-blue);"></i>
                        </div>
                        <h5 class="contact-title">Alamat</h5>
                        <p class="contact-detail"><?= $settings['company_address'] ?? 'Jakarta Timur, Indonesia' ?></p>
                        <a href="https://maps.google.com/?q=<?= urlencode($settings['company_address'] ?? 'Jakarta Timur, Indonesia') ?>" target="_blank" class="contact-link">
                            <i class="bi bi-map me-2"></i> Lihat di Maps
                        </a>
                    </div>
                </div>
            </div>
            
            <!-- Office Hours -->
            <div class="row justify-content-center">
                <div class="col-md-8" data-aos="fade-up">
                    <div class="office-hours">
                        <h5 class="office-hours-title">Jam Operasional</h5>
                        <ul class="office-hours-list">
                            <li>
                                <span class="day">Senin - Jumat</span>
                                <span class="time">09:00 - 18:00 WIB</span>
                            </li>
                            <li>
                                <span class="day">Sabtu</span>
                                <span class="time">09:00 - 15:00 WIB</span>
                            </li>
                            <li>
                                <span class="day">Minggu</span>
                                <span class="closed">Tutup</span>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
            
            <!-- Social Media -->
            <div class="text-center" data-aos="fade-up">
                <h5 class="office-hours-title">Ikuti Kami</h5>
                <div class="social-media">
                    <a href="https://instagram.com/situneodigital" target="_blank" class="social-link">
                        <i class="bi bi-instagram"></i>
                    </a>
                    <a href="https://facebook.com/situneodigital" target="_blank" class="social-link">
                        <i class="bi bi-facebook"></i>
                    </a>
                    <a href="https://linkedin.com/company/situneodigital" target="_blank" class="social-link">
                        <i class="bi bi-linkedin"></i>
                    </a>
                    <a href="https://tiktok.com/@situneodigital" target="_blank" class="social-link">
                        <i class="bi bi-tiktok"></i>
                    </a>
                </div>
            </div>
        </div>
    </section>

    <!-- Map Section -->
    <section class="map-section">
        <div class="map-overlay"></div>
        <div class="map-container">
            <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3966.4217268424!2d106.8753!3d-6.2388!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x2e69f3e945e34b9%3A0xf5c016e5b5b5b5b5!2sSituneo%20Digital!5e0!3m2!1sen!2sid!4v1654325678902!5m2!1sen!2sid" width="100%" height="100%" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
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
                            <span>Berapa lama proses pembuatan website?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Website standar 5-8 halaman kami kerjakan dalam 7-14 hari kerja. E-commerce: 14-21 hari. Custom system: 30-45 hari. Tergantung kompleksitas dan responsifnya klien memberikan feedback.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="200">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah ada jaminan website akan ranking di Google?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Kami tidak bisa menjamin ranking karena itu tergantung algoritma Google. Tapi kami pastikan website Anda SEO-friendly dengan on-page optimization, technical SEO, dan strategy konten yang tepat.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="300">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apa yang termasuk dalam paket website?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Paket sudah include domain, hosting, SSL, design responsive, basic SEO, contact form, dan maintenance selama periode kontrak. Lihat detail paket di halaman pricing.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="400">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Berapa biaya jika ingin custom features?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Tergantung kompleksitas fitur. Bisa mulai dari 500rb untuk integrasi sederhana hingga jutaan untuk custom development. Chat kami untuk konsultasi gratis.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="500">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apakah saya bisa update website sendiri?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Ya! Semua website kami dilengkapi admin panel yang mudah digunakan. Kami juga sediakan training gratis 2 jam untuk menggunakan CMS.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="600">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Apa itu free demo 24 jam?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Kami akan membuat preview website Anda dalam 24 jam tanpa charge. Jika Anda suka, lanjut ke proses development. Jika tidak, tidak ada biaya sama sekali.</p>
                        </div>
                    </div>
                    
                    <div class="faq-card" data-aos="fade-up" data-aos-delay="700">
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>Bagaimana support setelah website launch?</span>
                            <i class="bi bi-chevron-down faq-icon"></i>
                        </div>
                        <div class="faq-answer">
                            <p>Support 24/7 via WhatsApp. Semua paket sudah include maintenance selama periode kontrak. Setelah itu, bisa extend maintenance bulanan dengan harga terjangkau.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Trust Badge -->
    <section class="py-5">
        <div class="container">
            <div class="row justify-content-center">
                <div class="col-md-8" data-aos="fade-up">
                    <div class="trust-badge">
                        <h5 class="badge-title">Terpercaya & Resmi</h5>
                        <p class="badge-text">Perusahaan kami telah terdaftar resmi dengan NIB: 20250926145704515453</p>
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
                        Siap Memulai Transformasi Digital Bisnis Anda?
                    </h2>
                    <p style="color: var(--text-light); font-size: 1.1rem; margin: 0;">
                        Konsultasi gratis dengan tim digital expert kami. <strong style="color: var(--gold);">Tidak ada biaya tersembunyi!</strong>
                    </p>
                </div>
                <div class="col-lg-4 text-lg-end mt-3 mt-lg-0">
                    <a href="https://wa.me/6283173868915?text=Halo%20Situneo,%20saya%20mau%20konsultasi%20gratis" 
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
    <a href="https://wa.me/6283173868915?text=Halo%20Situneo,%20saya%20mau%20konsultasi%20gratis" 
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
