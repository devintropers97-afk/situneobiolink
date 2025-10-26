<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Admin Settings Page
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once '../config.php';

// Require admin role
requireRole(ROLE_ADMIN);

// Get current user
 $user = getCurrentUser();

// Get settings from database
 $settings = [];
 $stmt = $conn->query("SELECT setting_key, setting_value FROM settings");
while ($row = $stmt->fetch_assoc()) {
    $settings[$row['setting_key']] = $settings[$row['setting_key']] ?? $row['setting_value'];
}

// Process settings update
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $companyName = $_POST['company_name'] ?? '';
    $companyTagline = $_POST['company_tagline'] ?? '';
    $companyEmail = $_POST['company_email'] ?? '';
    $supportEmail = $_POST['support_email'] ?? '';
    $companyPhone = $_POST['company_phone'] ?? '';
    $companyAddress = $_POST['company_address'] ?? '';
    $companyWebsite = $_POST['company_website'] ?? '';
    $whatsappNumber = $_POST['whatsapp_number'] ?? '';
    
    // Update settings in database
    $updateQueries = [
        "UPDATE settings SET setting_value = ? WHERE setting_key = 'company_name'",
        "UPDATE settings SET setting_value = ? WHERE setting_key = 'company_tagline'",
        "UPDATE settings SET setting_value = ? WHERE setting_key = 'company_email'",
        "UPDATE settings SET setting_value = ? WHERE setting_key = 'support_email'",
        "UPDATE settings SET setting_value = ? WHERE setting_key = 'company_phone'",
        "UPDATE settings SET setting_value = ? WHERE setting_key = 'company_address'",
        "UPDATE settings SET setting_value = ? WHERE setting_key = 'company_website'",
        "UPDATE settings SET setting_value = ? WHERE setting_key = 'whatsapp_number'"
    ];
    
    $updateParams = [
        [$companyName, 'company_name'],
        [$companyTagline, 'company_tagline'],
        [$companyEmail, 'company_email'],
        [$supportEmail, 'support_email'],
        [$companyPhone, 'company_phone'],
        [$companyAddress, 'company_address'],
        [$companyWebsite, 'company_website'],
        [$whatsappNumber, 'whatsapp_number']
    ];
    
    $success = true;
    foreach ($updateQueries as $index => $query) {
        $stmt = $conn->prepare($query);
        if (!$stmt->execute($updateParams[$index])) {
            $success = false;
        }
    }
    
    if ($success) {
        // Log activity
        logActivity($user['id'], "Updated system settings");
        
        header("Location: settings.php?success=settings_updated");
        exit;
    } else {
        $error = "Terjadi kesalahan saat memperbarui pengaturan.";
    }
}

// Process logo upload
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_FILES['logo'])) {
    $logo = $_FILES['logo'];
    
    // Check if file is an image
    $allowedTypes = ['image/jpeg', 'image/png', 'image/gif', 'image/webp'];
    $maxSize = 2 * 1024 * 1024; // 2MB
    
    if (in_array($logo['type'], $allowedTypes) && $logo['size'] <= $maxSize) {
        // Create directory if not exists
        if (!is_dir('../../uploads')) {
            mkdir('../../uploads', 0777, true);
        }
        
        // Generate unique filename
        $filename = 'logo_' . time() . '.' . pathinfo($logo['name'], PATHINFO_EXTENSION);
        $uploadPath = '../../uploads/' . $filename;
        
        // Upload file
        if (move_uploaded_file($logo['tmp_name'], $uploadPath)) {
            // Update logo setting in database
            $stmt = $conn->prepare("UPDATE settings SET setting_value = ? WHERE setting_key = 'company_logo'");
            $stmt->bind_param("s", $filename);
            
            if ($stmt->execute()) {
                // Log activity
                logActivity($user['id'], "Updated company logo");
                
                header("Location: settings.php?success=logo_updated");
                exit;
            } else {
                $error = "Terjadi kesalahan saat mengupload logo.";
            }
        } else {
            $error = "Terjadi kesalahan saat mengupload logo.";
        }
    } else {
        $error = "Format file tidak didukung atau ukuran file terlalu besar (maksimal 2MB).";
    }
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pengaturan - SITUNEO DIGITAL</title>
    <meta name="description" content="Pengaturan sistem SITUNEO DIGITAL">
    
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
            preview: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background: var(--dark-blue);
            color: var(--white);
            min-height: 100vh;
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
        
        /* Sidebar */
        .sidebar {
            position: fixed;
            top: 80px;
            left: 0;
            height: calc(100vh - 80px);
            width: 250px;
            background: rgba(15, 48, 87, 0.9);
            backdrop-filter: blur(10px);
            border-right: 1px solid rgba(255, 180, 0, 0.2);
            padding: 20px 0;
            z-index: 997;
            overflow-y: auto;
            transition: all 0.3s;
        }
        
        .sidebar-item {
            display: block;
            padding: 12px 20px;
            color: var(--text-light);
            text-decoration: none;
            transition: all 0.3s;
            border-left: 3px solid transparent;
        }
        
        .sidebar-item:hover {
            background: rgba(255, 180, 0, 0.1);
            color: var(--gold);
            border-left-color: var(--gold);
        }
        
        .sidebar-item.active {
            background: rgba(255, 180, 0, 0.2);
            color: var(--gold);
            border-left-color: var(--gold);
        }
        
        .sidebar-item i {
            margin-right: 10px;
            width: 20px;
            text-align: center;
        }
        
        /* Main Content */
        .main-content {
            margin-left: 250px;
            padding: 100px 20px 40px;
            min-height: 100vh;
        }
        
        /* Settings Card */
        .settings-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 20px;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .settings-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        .settings-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .settings-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--gold);
        }
        
        .settings-actions {
            display: flex;
            gap: 10px;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-label {
            font-weight: 600;
            margin-bottom: 8px;
            color: var(--text-light);
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
        
        .form-select {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            padding: 12px 15px;
            color: var(--white);
            transition: all 0.3s;
        }
        
        .form-select:focus {
            background: rgba(255, 255, 255, 0.15);
            border-color: var(--gold);
            box-shadow: 0 0 0 0.25rem rgba(255, 180, 0, 0.25);
            color: var(--white);
        }
        
        .btn-gold {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 10px 25px;
            font-weight: 700;
            border-radius: 50px;
            transition: all 0.3s;
        }
        
        .btn-gold:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.4);
        }
        
        .btn-outline-gold {
            background: transparent;
            color: var(--gold);
            border: 1px solid var(--gold);
            padding: 10px 25px;
            font-weight: 700;
            border-radius: 50px;
            transition: all 0.3s;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
        }
        
        .btn-outline-gold:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            transform: translateY(-2px);
        }
        
        /* Logo Upload */
        .logo-upload {
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 20px;
        }
        
        .current-logo {
            width: 120px;
            height: 120px;
            border-radius: 10px;
            object-fit: cover;
            border: 2px solid var(--gold);
        }
        
        .logo-upload-btn {
            background: rgba(255, 180, 0, 0.1);
            border: 1px solid rgba(255, 180, 0, 0.3);
            color: var(--white);
            padding: 10px 20px;
            font-weight: 600;
            border-radius: 50px;
            transition: all 0.3s;
            cursor: pointer;
        }
        
        .logo-upload-btn:hover {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            transform: translateY(-2px);
        }
        
        /* Alert */
        .alert {
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 20px;
        }
        
        .alert-danger {
            background: rgba(220, 53, 69, 0.2);
            border: 1px solid rgba(220, 53, 69, 0.3);
            color: #f8d7da;
        }
        
        .alert-success {
            background: rgba(25, 135, 84, 0.2);
            border: 1px solid rgba(25, 135, 84, 0.3);
            color: #d1e7dd;
        }
        
        /* Responsive */
        @media (max-width: 992px) {
            .sidebar {
                transform: translateX(-100%);
            }
            
            .sidebar.show {
                transform: translateX(0);
            }
            
            .main-content {
                margin-left: 0;
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
        <div class="container-fluid">
            <button class="navbar-toggler d-lg-none" type="button" id="sidebarToggle">
                <span class="navbar-toggler-icon" style="filter: invert(1);"></span>
            </button>
            <a class="navbar-brand d-flex align-items-center" href="../../index.php" style="text-decoration: none;">
                <img src="https://situneo.my.id/logo" 
                     alt="Situneo" width="40" height="40" 
                     style="margin-right: 15px; border-radius: 10px; box-shadow: 0 5px 15px rgba(255,180,0,0.4);">
                <div>
                    <span style="font-family: 'Plus Jakarta Sans', sans-serif; font-size: 1.5rem; font-weight: 800; color: var(--gold);">SITUNEO</span>
                    <small style="display: block; font-size: 0.7rem; color: var(--text-light); margin-top: -5px;">Digital Harmony</small>
                </div>
            </a>
            <div class="ms-auto d-flex align-items-center">
                <div class="dropdown">
                    <button class="btn btn-link text-white dropdown-toggle" type="button" id="userDropdown" data-bs-toggle="dropdown" aria-expanded="false">
                        <i class="bi bi-person-circle fs-4"></i>
                    </button>
                    <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="userDropdown">
                        <li><a class="dropdown-item" href="../profile.php"><i class="bi bi-person me-2"></i> Profil Saya</a></li>
                        <li><a class="dropdown-item" href="../settings.php" class="dropdown-item active"><i class="bi bi-gear me-2"></i> Pengaturan</a></li>
                        <li><hr class="dropdown-divider"></li>
                        <li><a class="dropdown-item" href="../logout.php"><i class="bi bi-box-arrow-right me-2"></i> Keluar</a></li>
                    </ul>
                </div>
            </div>
        </div>
    </nav>
    
    <!-- Sidebar -->
    <div class="sidebar" id="sidebar">
        <div class="user-profile">
            <div class="user-avatar">
                <?php if ($user['avatar']): ?>
                    <img src="../../uploads/avatars/<?= htmlspecialchars($user['avatar']) ?>" alt="<?= htmlspecialchars($user['name']) ?>">
                <?php else: ?>
                    <?= strtoupper(substr($user['name'], 0, 1)) ?>
                <?php endif; ?>
            </div>
            <div class="user-info">
                <h5><?= htmlspecialchars($user['name']) ?></h5>
                <p><?= htmlspecialchars($user['email']) ?></p>
            </div>
        </div>
        
        <a href="../dashboard.php" class="sidebar-item">
            <i class="bi bi-speedometer2"></i> Dashboard
        </a>
        <a href="../orders.php" class="sidebar-item">
            <i class="bi bi-cart-check"></i> Pesanan Saya
        </a>
        <a href="../services.php" class="sidebar-item">
            <i class="bi bi-grid"></i> Layanan
        </a>
        <a href="../invoices.php" class="sidebar-item">
            <i class="bi bi-file-earmark-text"></i> Invoice
        </a>
        <a href="../support.php" class="sidebar-item">
            <i class="bi bi-headset"></i> Bantuan
        </a>
        
        <hr class="my-3" style="border-color: rgba(255, 180, 0, 0.2);">
        <h6 class="px-3 text-uppercase small text-muted">Admin</h6>
        <a href="users.php" class="sidebar-item">
            <i class="bi bi-people"></i> Kelola Pengguna
        </a>
        <a href="orders.php" class="sidebar-item">
            <i class="bi bi-receipt"></i> Semua Pesanan
        </a>
        <a href="services.php" class="sidebar-item">
            <i class="bi bi-gear"></i> Kelola Layanan
        </a>
        <a href="reports.php" class="sidebar-item">
            <i class="bi bi-graph-up"></i> Laporan
        </a>
    </div>
    
    <!-- Main Content -->
    <div class="main-content">
        <div class="container-fluid">
            <?php if (isset($_GET['success']): ?>
                <?php if ($_GET['success'] === 'settings_updated'): ?>
                    <div class="alert alert-success">
                        <i class="bi bi-check-circle-fill me-2"></i>Pengaturan berhasil diperbarui!
                    </div>
                <?php elseif ($_GET['success'] === 'logo_updated'): ?>
                    <div class="alert alert-success">
                        <i class="bi bi-check-circle-fill me-2"></i>Logo berhasil diperbarui!
                    </div>
                <?php endif; ?>
            <?php endif; ?>
            
            <?php if (isset($error)): ?>
                <div class="alert alert-danger">
                    <i class="bi bi-exclamation-triangle-fill me-2"></i><?= $error ?>
                </div>
            <?php endif; ?>
            
            <div class="settings-header">
                <h1 class="settings-title">Pengaturan Sistem</h1>
                <div class="settings-actions">
                    <a href="reports.php" class="btn btn-outline-gold">
                        <i class="bi bi-graph-up me-2"></i>Lihat Laporan
                    </a>
                </div>
            </div>
            
            <!-- Company Information -->
            <div class="settings-card" data-aos="fade-up">
                <h5 class="mb-4">Informasi Perusahaan</h5>
                
                <form method="post" action="">
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="company_name" class="form-label">Nama Perusahaan</label>
                            <input type="text" class="form-control" id="company_name" name="company_name" value="<?= htmlspecialchars($settings['company_name']) ?>" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="company_tagline" class="form-label">Tagline</label>
                            <input type="text" class="form-control" id="company_tagline" name="company_tagline" value="<?= htmlspecialchars($settings['company_tagline']) ?>">
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="company_email" class="form-label">Email Perusahaan</label>
                            <input type="email" class="form-control" id="company_email" name="company_email" value="<?= htmlspecialchars($settings['company_email']) ?>" required>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="support_email" class="form-label">Email Support</label>
                            <input type="email" class="form-control" id="support_email" name="support_email" value="<?= htmlspecialchars($settings['support_email']) ?>">
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="company_phone" class="form-label">Telepon</label>
                            <input type="tel" class="form-control" id="company_phone" name="company_phone" value="<?= htmlspecialchars($settings['company_phone']) ?>">
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="company_address" class="form-label">Alamat</label>
                            <textarea class="form-control" id="company_address" name="company_address" rows="3"><?= htmlspecialchars($settings['company_address']) ?></textarea>
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="company_website" class="form-label">Website</label>
                            <input type="url" class="form-control" id="company_website" name="company_website" value="<?= htmlspecialchars($settings['company_website']) ?>">
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="whatsapp_number" class="form-label">WhatsApp</label>
                            <input type="tel" class="form-control" id="whatsapp_number" name="whatsapp_number" value="<?= htmlspecialchars($settings['whatsapp_number']) ?>">
                        </div>
                    </div>
                    
                    <button type="submit" class="btn btn-gold">
                        <i class="bi bi-check-circle me-2"></i>Simpan Perubahan
                    </button>
                </form>
            </div>
            
            <!-- Logo Management -->
            <div class="settings-card" data-aos="fade-up" data-aos-delay="100">
                <h5 class="mb-4">Logo Perusahaan</h5>
                
                <div class="logo-upload">
                    <?php if (!empty($settings['company_logo']): ?>
                        <img src="../../uploads/<?= htmlspecialchars($settings['company_logo']) ?>" alt="Company Logo" class="current-logo">
                    <?php else: ?>
                        <div class="current-logo d-flex align-items-center justify-content-center">
                            <i class="bi bi-image" style="font-size: 3rem;"></i>
                        </div>
                    <?php endif; ?>
                    
                    <form method="post" enctype="multipart/form-data" action="">
                        <div class="mb-3">
                            <label for="logo" class="form-label">Upload Logo Baru</label>
                            <input type="file" class="form-control" id="logo" name="logo" accept="image/*" required>
                            <small class="form-text text-muted">Format: JPG, PNG, GIF, atau WebP. Maksimal 2MB</small>
                        </div>
                        
                        <button type="submit" class="logo-upload-btn">
                            <i class="bi bi-upload"></i> Upload Logo Baru
                        </button>
                    </form>
                </div>
            </div>
            
            <!-- System Settings -->
            <div class="settings-card" data-aos="fade-up" data-aos-delay="200">
                <h5 class="mb-4">Pengaturan Sistem</h5>
                
                <form method="post" action="">
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="app_name" class="form-label">Nama Aplikasi</label>
                            <input type="text" class="form-control" id="app_name" name="app_name" value="<?= htmlspecialchars($settings['app_name'] ?? 'SITUNEO DIGITAL' ?>">
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="app_url" class="form-label">URL Aplikasi</label>
                            <input type="url" class="form-control" id="app_url" name="app_url" value="<?= htmlspecialchars($settings['app_url'] ?? 'https://situneo.my.id') ?>">
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="from_email" class="form-label">Email Pengirim</label>
                            <input type="email" class="form-control" id="from_email" name="from_email" value="<?= htmlspecialchars($settings['from_email'] ?? 'noreply@situneo.my.id') ?>">
                        </div>
                        <div class="from_name" class="form-label">Nama Pengirim</label>
                            <input type="text" class="form-control" id="from_name" name="from_name" value="<?= htmlspecialchars($settings['from_name'] ?? 'SITUNEO DIGITAL') ?>">
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="session_lifetime" class="session_lifetime" class="form-label">Session Lifetime (jam)</label>
                            <input type="number" class="form-control" id="session_lifetime" name="session_lifetime" value="<?= $settings['session_lifetime'] ?? 24 ?>">
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="remember_lifetime" class="remember_lifetime" class="form-label">Remember Me Lifetime (hari)</label>
                            <input type="number" class="form-control" id="remember_lifetime" name="remember_lifetime" value="<?= $settings['remember_lifetime'] ?? 30 ?>">
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="min_password_length" class="form-label">Minimal Password Length</label>
                            <input type="number" class="form-control" id="min_password_length" name="min_password_length" value="<?= $settings['min_password_length'] ?? 8 ?>">
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="max_upload_size" class="form-label">Max Upload Size (MB)</label>
                            <input type="number" class="form-control" id="max_upload_size" name="max_upload_size" value="<?= $settings['max_upload_size'] ?? 5 ?>">
                        </div>
                    </div>
                    
                    <button type="submit" class="btn btn-gold">
                        <i class="bi bi-check-circle me-2"></i>Simpan Perubahan
                    </button>
                </form>
            </div>
            
            <!-- Email Settings -->
            <div class="settings-card" data-aos="fade-up" data-aos-delay="300">
                <h5 class="mb-4">Pengaturan Email</h5>
                
                <form method="post" action="">
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="smtp_host" class="form-label">SMTP Host</label>
                            <input type="text" class="form-control" id="smtp_host" name="smtp_host" value="<?= $settings['smtp_host'] ?? 'smtp.example.com' ?>">
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="smtp_port" class="form-label">SMTP Port</label>
                            <input type="number" class="form-control" id="smtp_port" name="smtp_port" value="<?= $settings['smtp_port'] ?? '587' ?>">
                        </div>
                    </div>
                    
                    <div class="smtp-settings">
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label for="smtp_username" class="form-label">SMTP Username</label>
                                <input type="text" class="form-control" id="smtp_username" name="smtp_username" value="<?= $settings['smtp_username'] ?? 'user@example.com' ?>">
                            </div>
                            <div class="col-md-6 mb-3">
                                <label for="smtp_password" class="form-label">SMTP Password</label>
                                <input type="password" class="form-control" id="smtp_password" name="smtp_password">
                            <button type="button" class="btn btn-sm btn-outline-light text-white" type="button" onclick="togglePasswordVisibility('smtp_password')">
                                    <i class="bi bi-eye"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="email_protocol" class="form-label">Email Protocol</label>
                            <select class="form-select" id="email_protocol" name="email_protocol">
                                <option value="smtp">SMTP</option>
                                <option value="mail()">PHP Mail</option>
                            </select>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="encryption_type" class="form-label">Encryption</label>
                            <select class="form-select" id="encryption_type" name="encryption_type">
                                <option value="tls">TLS</option>
                                <option value="ssl">SSL</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-12">
                            <label for="test_email" class="send-test-email">
                                <div class="input-group">
                                    <span class="input-group-text">Test Email</span>
                                    <input type="email" class="form-control" id="test_email" placeholder="email@example.com">
                                    <button class="btn btn-outline-light text-white" type="button" onclick="testEmail()">
                                        <i class="bi bi-send"></i> Kirim Test Email
                                    </button>
                                </div>
                            </label>
                            <div id="test_result" class="alert alert alert-info mt-2" style="display: none;">
                                <i class="bi bi-info-circle-fill me-2"></i>
                                <span id="test_message"></span>
                            </div>
                        </div>
                        
                        <button type="submit" class="btn btn-gold">
                            <i class="bi bi-check-circle me-2"></i>Simpan Perubahan
                        </button>
                    </form>
                </div>
            </div>
            
            <!-- Backup Settings -->
            <div class="settings-card" data-aos="fade-up" data-aos-delay="400">
                <h5 class="mb-4">Backup & Maintenance</h5>
                
                <form method="post" action="">
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="backup_enabled" class="form-check form-switch">
                                <input type="checkbox" class="form-check-input" id="backup_enabled" name="backup_enabled" <?= !empty($settings['backup_enabled']) ? 'checked' : '' ?>>
                                <label class="form-check-label" for="backup_enabled">Aktifkan Backup Otomatis</label>
                            </div>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="backup_frequency" class="form-label">Frekuensi Backup</label>
                            <select class="form-select" id="backup_frequency" name="backup_frequency">
                                <option value="daily">Harianan</option>
                                <option value="weekly">Mingguan</option>
                                <option value="monthly">Bulanan</option>
                                <option value="quarterly">3 Bulan</option>
                                <option value="yearly">Tahunan</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="backup_retention" class="form-check form-switch">
                                <input type="checkbox" class="form-check-input" id="backup_retention" <?= !empty($settings['backup_retention']) ? 'checked' : '' ?>>
                                <label class="form-check-label" for="backup_retention">Simpan Backup (hari)</label>
                            </div>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="backup_location" class="form-label">Lokasi Backup</label>
                            <input type="text" class="form-control" id="backup_location" value="<?= $settings['backup_location'] ?? '../../backups' ?>">
                        </div>
                    </div>
                    
                    <button type="button" class="btn btn-gold">
                        <i class="bi bi-cloud-download me-2"></i>Backup Sekarang Ini
                    </button>
                </form>
            </div>
            
            <!-- Security Settings -->
            <div class="settings-card" data-aos="fade-up" data-aos-delay="500">
                <h5 class="mb-4">Keamanan</h5>
                
                <form method="post" action="">
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="captcha_enabled" class="form-check form-switch">
                                <input type="checkbox" class="form-check-input" id="captcha_enabled" <?= !empty($settings['captcha_enabled']) ? 'checked' : '' ?>>
                                <label class="form-check-label" for="captcha_enabled">Aktifkan Captcha</label>
                            </div>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="captcha_site_key" class="form-label">Site Key</label>
                            <input type="text" class="form-control" id="captcha_site_key" value="<?= $settings['captcha_site_key'] ?? '' ?>">
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="max_login_attempts" class="form-label">Maksimal Login Gagal</label>
                            <input type="number" class="form-control" id="max_login_attempts" value="<?= $settings['max_login_attempts'] ?? '5' ?>">
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="lockout_duration" class="form-label">Durasi Lockout (menit)</label>
                            <input type="number" class="form-control" id="lockout_duration" value="<?= $settings['lockout_duration'] ?? '30' ?>">
                        </div>
                    </div>
                    
                    <button type="submit" class="btn btn-gold">
                        <i class="bi bi-shield-check me-2"></i>Simpan Perubahan
                    </button>
                </form>
            </div>
            
            <!-- Notification Settings -->
            <div class="settings-card" data-aos="fade-up" data-aos-delay="600">
                <h5 class="mb-4">Notifikasi</h5>
                
                <form method="post" action="">
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label for="email_notifications" class="form-check form-switch">
                                <input type="checkbox" class="form-check-input" id="email_notifications" <?= !empty($settings['email_notifications']) ? 'checked' : '' ?>>
                                <label class="form-check-label" for="email_notifications">Notifikasi Email</label>
                            </div>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label for="sms_notifications" class="form-check form-switch">
                                <input type="checkbox" class="form-check-input" id="sms_notifications" <?= !empty($settings['sms_notifications']) ? 'checked' : '' ?>>
                                <label class="form-check-label" for="sms_notifications">Notifikasi SMS</label>
                            </div>
                        </div>
                    </div>
                    
                    <button type="submit" class="btn btn-gold">
                        <i class="bi bi-bell me-2"></i>Simpan Perubahan
                    </button>
                </form>
            </div>
        </div>
    </div>
    
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
        
        // Sidebar toggle for mobile
        document.getElementById('sidebarToggle').addEventListener('toggle', function() {
            document.getElementById('sidebar').classList.toggle('show');
        });
        
        // Navbar scroll effect
        window.addEventListener('scroll', function() {
            const navbar = document.querySelector('.navbar-premium');
            if (window.scrollY > 50) {
                navbar.classList.add('scrolled');
            } else {
                navbar.classList.remove('scrolled');
            }
        });
        
        // Toggle password visibility
        function togglePassword(fieldId) {
            const passwordField = document.getElementById(fieldId);
            const icon = passwordField.nextElementSibling.querySelector('i');
            
            if (passwordField.type === 'password') {
                passwordField.type = 'text';
                icon.classList.remove('bi-eye');
                icon.classList.add('bi-eye-slash');
            } else {
                passwordField.type = 'password';
                icon.classList.remove('bi-eye-slash');
            }
        }
        
        // Test email function
        function testEmail() {
            const email = document.getElementById('test_email').value;
            const testMessage = document.getElementById('test_message');
            const testResult = document.getElementById('test_result');
            
            if (!email) {
                testResult.classList.remove('alert-info');
                testResult.classList.add('alert-danger');
                testResult.style.display = 'block';
                testMessage.textContent = 'Mohon masukkan email untuk testing';
                return;
            }
            
            // Show loading state
            testResult.classList.remove('alert-info');
            testResult.classList.add('alert-info');
            testResult.style.display = 'block';
            testMessage.textContent = 'Mengirim...';
            
            // Send AJAX request
            fetch('test_email.php', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded',
                },
                body: new URLSearchParams({
                    'email': email
                })
            })
            .then(response => response.json())
            .then(data => {
                if (data.success) {
                    testResult.classList.remove('alert-info');
                    testResult.classList.add('alert-success');
                    testMessage.textContent = 'Email berhasil dikirim ke ' + email;
                } else {
                    testResult.classList.remove('alert-info');
                    testResult.classList.add('alert-danger');
                    testMessage.textContent = 'Gagal mengirim email: ' + data.message;
                }
            })
            .catch(error => {
                testResult.classList.remove('alert-info');
                testResult.classList.add('alert-danger');
                testMessage.textContent = 'Terjadi kesalahan: ' + error.message;
            })
            .finally(() => {
                testMessage.textContent = 'Mengirim...';
            });
        }
        
        // Form submission
        document.querySelectorAll('form').forEach(form => {
            form.addEventListener('submit', function(e) {
                const submitBtn = form.querySelector('button[type="submit"]');
                const originalText = submitBtn.innerHTML;
                
                // Show loading state
                submitBtn.disabled = true;
                submitBtn.innerHTML = '<span class="spinner-border spinner-border-sm me-2"></span> Menyimpan...';
                
                setTimeout(() => {
                    submitBtn.disabled = false;
                    submitBtn.innerHTML = originalText;
                }, 1000);
            });
        });
    </script>
</body>
</html>
