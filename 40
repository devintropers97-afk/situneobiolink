<?php
/**
 * ========================================
 * SITUNEO DIGITAL - User Profile Page
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once 'config.php';

// Require login
requireLogin();

// Get current user
 $user = getCurrentUser();

 $errors = [];
 $success = '';

// Process profile update form
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $name = trim($_POST['name']);
    $phone = trim($_POST['phone']);
    $address = trim($_POST['address']);
    
    // Validation
    if (empty($name)) {
        $errors['name'] = 'Nama lengkap harus diisi';
    }
    
    if (empty($phone)) {
        $errors['phone'] = 'Nomor telepon harus diisi';
    }
    
    // If no errors, update profile
    if (empty($errors)) {
        $stmt = $conn->prepare("UPDATE users SET name = ?, phone = ?, address = ? WHERE id = ?");
        $stmt->bind_param("sssi", $name, $phone, $address, $user['id']);
        
        if ($stmt->execute()) {
            $success = 'Profil berhasil diperbarui!';
            
            // Update session data
            $_SESSION['user_name'] = $name;
            
            // Log activity
            logActivity($user['id'], 'Profile updated');
            
            // Refresh user data
            $user = getCurrentUser();
        } else {
            $errors['general'] = 'Terjadi kesalahan. Silakan coba lagi.';
        }
    }
}

// Process password change form
if (isset($_POST['change_password'])) {
    $current_password = $_POST['current_password'];
    $new_password = $_POST['new_password'];
    $confirm_password = $_POST['confirm_password'];
    
    // Validation
    if (empty($current_password)) {
        $errors['current_password'] = 'Password saat ini harus diisi';
    } elseif (!verifyPassword($current_password, $user['password'])) {
        $errors['current_password'] = 'Password saat ini salah';
    }
    
    if (empty($new_password)) {
        $errors['new_password'] = 'Password baru harus diisi';
    } elseif (strlen($new_password) < MIN_PASSWORD_LENGTH) {
        $errors['new_password'] = 'Password minimal ' . MIN_PASSWORD_LENGTH . ' karakter';
    }
    
    if ($new_password !== $confirm_password) {
        $errors['confirm_password'] = 'Konfirmasi password tidak cocok';
    }
    
    // If no errors, change password
    if (empty($errors)) {
        $hashed_password = hashPassword($new_password);
        
        $stmt = $conn->prepare("UPDATE users SET password = ? WHERE id = ?");
        $stmt->bind_param("si", $hashed_password, $user['id']);
        
        if ($stmt->execute()) {
            $success = 'Password berhasil diubah!';
            
            // Log activity
            logActivity($user['id'], 'Password changed');
            
            // Refresh user data
            $user = getCurrentUser();
        } else {
            $errors['general'] = 'Terjadi kesalahan. Silakan coba lagi.';
        }
    }
}

// Process avatar upload
if (isset($_FILES['avatar']) && $_FILES['avatar']['error'] === UPLOAD_ERR_OK) {
    $avatar = $_FILES['avatar'];
    $avatar_name = $avatar['name'];
    $avatar_tmp = $avatar['tmp_name'];
    $avatar_size = $avatar['size'];
    $avatar_type = $avatar['type'];
    
    // Validation
    $allowed_types = ['image/jpeg', 'image/png', 'image/gif'];
    $max_size = 2 * 1024 * 1024; // 2MB
    
    if (!in_array($avatar_type, $allowed_types)) {
        $errors['avatar'] = 'Format avatar harus JPEG, PNG, atau GIF';
    } elseif ($avatar_size > $max_size) {
        $errors['avatar'] = 'Ukuran avatar maksimal 2MB';
    } else {
        // Generate unique filename
        $avatar_ext = pathinfo($avatar_name, PATHINFO_EXTENSION);
        $avatar_new_name = 'avatar_' . $user['id'] . '_' . time() . '.' . $avatar_ext;
        $avatar_path = '../uploads/avatars/' . $avatar_new_name;
        
        // Create directory if not exists
        if (!is_dir('../uploads/avatars/')) {
            mkdir('../uploads/avatars/', 0777, true);
        }
        
        // Upload file
        if (move_uploaded_file($avatar_tmp, $avatar_path)) {
            // Update user avatar in database
            $stmt = $conn->prepare("UPDATE users SET avatar = ? WHERE id = ?");
            $stmt->bind_param("si", $avatar_new_name, $user['id']);
            
            if ($stmt->execute()) {
                $success = 'Avatar berhasil diperbarui!';
                
                // Log activity
                logActivity($user['id'], 'Avatar updated');
                
                // Refresh user data
                $user = getCurrentUser();
            } else {
                $errors['general'] = 'Terjadi kesalahan saat memperbarui avatar.';
            }
        } else {
            $errors['avatar'] = 'Terjadi kesalahan saat mengupload avatar.';
        }
    }
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Profil Saya - SITUNEO DIGITAL</title>
    <meta name="description" content="Kelola profil pengguna SITUNEO DIGITAL">
    
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
        
        /* Profile Card */
        .profile-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 30px;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .profile-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        .avatar-container {
            position: relative;
            width: 150px;
            height: 150px;
            margin: 0 auto 20px;
        }
        
        .avatar {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            object-fit: cover;
            border: 4px solid var(--gold);
        }
        
        .avatar-placeholder {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            background: var(--gradient-gold);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
            font-weight: 700;
            color: var(--dark-blue);
            border: 4px solid var(--gold);
        }
        
        .avatar-upload {
            position: absolute;
            bottom: 0;
            right: 0;
            background: var(--gradient-gold);
            color: var(--dark-blue);
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }
        
        .avatar-upload:hover {
            transform: scale(1.1);
        }
        
        .profile-name {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.8rem;
            font-weight: 800;
            text-align: center;
            margin-bottom: 10px;
            background: var(--gradient-gold);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .profile-email {
            text-align: center;
            color: var(--text-light);
            margin-bottom: 20px;
        }
        
        .profile-stats {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
            padding-top: 20px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .stat-item {
            text-align: center;
        }
        
        .stat-value {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--gold);
        }
        
        .stat-label {
            font-size: 0.9rem;
            color: var(--text-light);
        }
        
        /* Form Card */
        .form-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 30px;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .form-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        .form-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 20px;
            color: var(--gold);
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
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.3);
        }
        
        .btn-gold:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255, 180, 0, 0.6);
            color: var(--dark-blue);
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
        
        .invalid-feedback {
            color: #ff6b6b;
            font-size: 0.875rem;
            margin-top: 0.25rem;
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
            <a class="navbar-brand d-flex align-items-center" href="../index.php" style="text-decoration: none;">
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
                        <li><a class="dropdown-item" href="profile.php"><i class="bi bi-person me-2"></i> Profil Saya</a></li>
                        <li><a class="dropdown-item" href="settings.php"><i class="bi bi-gear me-2"></i> Pengaturan</a></li>
                        <li><hr class="dropdown-divider"></li>
                        <li><a class="dropdown-item" href="logout.php"><i class="bi bi-box-arrow-right me-2"></i> Keluar</a></li>
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
                    <img src="../uploads/avatars/<?= htmlspecialchars($user['avatar']) ?>" alt="<?= htmlspecialchars($user['name']) ?>">
                <?php else: ?>
                    <?= strtoupper(substr($user['name'], 0, 1)) ?>
                <?php endif; ?>
            </div>
            <div class="user-info">
                <h5><?= htmlspecialchars($user['name']) ?></h5>
                <p><?= htmlspecialchars($user['email']) ?></p>
            </div>
        </div>
        
        <a href="dashboard.php" class="sidebar-item">
            <i class="bi bi-speedometer2"></i> Dashboard
        </a>
        <a href="profile.php" class="sidebar-item active">
            <i class="bi bi-person"></i> Profil Saya
        </a>
        <a href="orders.php" class="sidebar-item">
            <i class="bi bi-cart-check"></i> Pesanan Saya
        </a>
        <a href="services.php" class="sidebar-item">
            <i class="bi bi-grid"></i> Layanan
        </a>
        <a href="invoices.php" class="sidebar-item">
            <i class="bi bi-file-earmark-text"></i> Invoice
        </a>
        <a href="support.php" class="sidebar-item">
            <i class="bi bi-headset"></i> Bantuan
        </a>
        
        <?php if (hasRole(ROLE_ADMIN)): ?>
            <hr class="my-3" style="border-color: rgba(255, 180, 0, 0.2);">
            <h6 class="px-3 text-uppercase small text-muted">Admin</h6>
            <a href="admin/users.php" class="sidebar-item">
                <i class="bi bi-people"></i> Kelola Pengguna
            </a>
            <a href="admin/orders.php" class="sidebar-item">
                <i class="bi bi-receipt"></i> Semua Pesanan
            </a>
            <a href="admin/services.php" class="sidebar-item">
                <i class="bi bi-gear"></i> Kelola Layanan
            </a>
            <a href="admin/reports.php" class="sidebar-item">
                <i class="bi bi-graph-up"></i> Laporan
            </a>
        <?php endif; ?>
    </div>
    
    <!-- Main Content -->
    <div class="main-content">
        <div class="container-fluid">
            <?php if (!empty($success)): ?>
                <div class="alert alert-success">
                    <i class="bi bi-check-circle-fill me-2"></i><?= $success ?>
                </div>
            <?php endif; ?>
            
            <?php if (!empty($errors['general'])): ?>
                <div class="alert alert-danger">
                    <i class="bi bi-exclamation-triangle-fill me-2"></i><?= $errors['general'] ?>
                </div>
            <?php endif; ?>
            
            <div class="row">
                <div class="col-lg-4 mb-4" data-aos="fade-up" data-aos-delay="100">
                    <div class="profile-card text-center">
                        <div class="avatar-container">
                            <?php if ($user['avatar']): ?>
                                <img src="../uploads/avatars/<?= htmlspecialchars($user['avatar']) ?>" alt="<?= htmlspecialchars($user['name']) ?>" class="avatar">
                            <?php else: ?>
                                <div class="avatar-placeholder"><?= strtoupper(substr($user['name'], 0, 1)) ?></div>
                            <?php endif; ?>
                            <label for="avatar" class="avatar-upload">
                                <i class="bi bi-camera-fill"></i>
                            </label>
                            <input type="file" id="avatar" name="avatar" accept="image/*" style="display: none;">
                        </div>
                        <h3 class="profile-name"><?= htmlspecialchars($user['name']) ?></h3>
                        <p class="profile-email"><?= htmlspecialchars($user['email']) ?></p>
                        <div class="profile-stats">
                            <div class="stat-item">
                                <div class="stat-value"><?= getUserOrders($user['id']) ? count(getUserOrders($user['id'])) : 0 ?></div>
                                <div class="stat-label">Pesanan</div>
                            </div>
                            <div class="stat-item">
                                <div class="stat-value"><?= $user['role'] == 3 ? 'Admin' : ($user['role'] == 2 ? 'Staff' : 'User') ?></div>
                                <div class="stat-label">Role</div>
                            </div>
                            <div class="stat-item">
                                <div class="stat-value"><?= date('d M Y', strtotime($user['created_at'])) ?></div>
                                <div class="stat-label">Bergabung</div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="col-lg-8 mb-4" data-aos="fade-up" data-aos-delay="200">
                    <div class="form-card">
                        <h4 class="form-title">Informasi Profil</h4>
                        <form method="post" action="">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label for="name" class="form-label">Nama Lengkap</label>
                                        <input type="text" class="form-control" id="name" name="name" value="<?= htmlspecialchars($user['name']) ?>">
                                        <?php if (!empty($errors['name'])): ?>
                                            <div class="invalid-feedback"><?= $errors['name'] ?></div>
                                        <?php endif; ?>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label for="email" class="form-label">Email</label>
                                        <input type="email" class="form-control" id="email" name="email" value="<?= htmlspecialchars($user['email']) ?>" readonly>
                                        <small class="form-text text-muted">Email tidak dapat diubah. Hubungi admin untuk mengubah email.</small>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label for="phone" class="form-label">Nomor Telepon</label>
                                        <input type="tel" class="form-control" id="phone" name="phone" value="<?= htmlspecialchars($user['phone']) ?>">
                                        <?php if (!empty($errors['phone'])): ?>
                                            <div class="invalid-feedback"><?= $errors['phone'] ?></div>
                                        <?php endif; ?>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label for="referral_code" class="form-label">Kode Referral</label>
                                        <input type="text" class="form-control" id="referral_code" value="<?= htmlspecialchars($user['referral_code']) ?>" readonly>
                                        <small class="form-text text-muted">Bagikan kode ini kepada teman Anda.</small>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="form-group">
                                <label for="address" class="form-label">Alamat</label>
                                <textarea class="form-control" id="address" name="address" rows="3"><?= htmlspecialchars($user['address']) ?></textarea>
                            </div>
                            
                            <button type="submit" class="btn btn-gold">Simpan Perubahan</button>
                        </form>
                    </div>
                </div>
            </div>
            
            <div class="row">
                <div class="col-lg-12 mb-4" data-aos="fade-up" data-aos-delay="300">
                    <div class="form-card">
                        <h4 class="form-title">Ubah Password</h4>
                        <form method="post" action="">
                            <div class="row">
                                <div class="col-md-4">
                                    <div class="form-group">
                                        <label for="current_password" class="form-label">Password Saat Ini</label>
                                        <input type="password" class="form-control" id="current_password" name="current_password">
                                        <?php if (!empty($errors['current_password'])): ?>
                                            <div class="invalid-feedback"><?= $errors['current_password'] ?></div>
                                        <?php endif; ?>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="form-group">
                                        <label for="new_password" class="form-label">Password Baru</label>
                                        <input type="password" class="form-control" id="new_password" name="new_password">
                                        <?php if (!empty($errors['new_password'])): ?>
                                            <div class="invalid-feedback"><?= $errors['new_password'] ?></div>
                                        <?php endif; ?>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="form-group">
                                        <label for="confirm_password" class="form-label">Konfirmasi Password Baru</label>
                                        <input type="password" class="form-control" id="confirm_password" name="confirm_password">
                                        <?php if (!empty($errors['confirm_password'])): ?>
                                            <div class="invalid-feedback"><?= $errors['confirm_password'] ?></div>
                                        <?php endif; ?>
                                    </div>
                                </div>
                            </div>
                            
                            <button type="submit" name="change_password" class="btn btn-gold">Ubah Password</button>
                        </form>
                    </div>
                </div>
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
        document.getElementById('sidebarToggle').addEventListener('click', function() {
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
        
        // Avatar upload
        document.getElementById('avatar').addEventListener('change', function() {
            const form = document.createElement('form');
            form.method = 'post';
            form.enctype = 'multipart/form-data';
            
            const avatarInput = document.createElement('input');
            avatarInput.type = 'file';
            avatarInput.name = 'avatar';
            avatarInput.files = this.files;
            
            form.appendChild(avatarInput);
            document.body.appendChild(form);
            form.submit();
        });
        
        // Form validation
        document.querySelector('form').addEventListener('submit', function(e) {
            const name = document.getElementById('name').value;
            const phone = document.getElementById('phone').value;
            
            if (!name) {
                e.preventDefault();
                document.getElementById('name').focus();
                return false;
            }
            
            if (!phone) {
                e.preventDefault();
                document.getElementById('phone').focus();
                return false;
            }
        });
        
        // Password change form validation
        document.querySelector('form[name="change_password"]').addEventListener('submit', function(e) {
            const currentPassword = document.getElementById('current_password').value;
            const newPassword = document.getElementById('new_password').value;
            const confirmPassword = document.getElementById('confirm_password').value;
            
            if (!currentPassword || !newPassword || !confirmPassword) {
                e.preventDefault();
                alert('Semua field password harus diisi!');
                return false;
            }
            
            if (newPassword !== confirmPassword) {
                e.preventDefault();
                alert('Password baru dan konfirmasi password tidak cocok!');
                return false;
            }
            
            if (newPassword.length < 8) {
                e.preventDefault();
                alert('Password baru minimal 8 karakter!');
                return false;
            }
        });
    </script>
</body>
</html>
