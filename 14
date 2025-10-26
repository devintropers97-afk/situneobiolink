<?php
/**
 * ========================================
 * SITUNEO DIGITAL - User Dashboard
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once 'config.php';

// Require login
requireLogin();

// Get current user
 $user = getCurrentUser();

// Check for unauthorized access
if (isset($_GET['error']) && $_GET['error'] === 'unauthorized') {
    $error = 'Anda tidak memiliki izin untuk mengakses halaman tersebut.';
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard - SITUNEO DIGITAL</title>
    <meta name="description" content="Dashboard pengguna SITUNEO DIGITAL">
    
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
        
        /* Dashboard Cards */
        .dashboard-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 25px;
            height: 100%;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .dashboard-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        .card-icon {
            font-size: 2.5rem;
            color: var(--gold);
            margin-bottom: 15px;
        }
        
        .card-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.2rem;
            font-weight: 700;
            margin-bottom: 10px;
        }
        
        .card-value {
            font-size: 2rem;
            font-weight: 800;
            color: var(--bright-gold);
        }
        
        /* User Profile */
        .user-profile {
            display: flex;
            align-items: center;
            padding: 15px 20px;
            border-bottom: 1px solid rgba(255, 180, 0, 0.2);
            margin-bottom: 20px;
        }
        
        .user-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: var(--gradient-gold);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            color: var(--dark-blue);
            margin-right: 15px;
        }
        
        .user-info h5 {
            margin: 0;
            font-weight: 700;
        }
        
        .user-info p {
            margin: 0;
            font-size: 0.85rem;
            color: var(--text-light);
        }
        
        /* Recent Activity */
        .activity-item {
            display: flex;
            align-items: start;
            padding: 15px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .activity-item:last-child {
            border-bottom: none;
        }
        
        .activity-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(255, 180, 0, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            flex-shrink: 0;
        }
        
        .activity-icon i {
            color: var(--gold);
        }
        
        .activity-content {
            flex: 1;
        }
        
        .activity-title {
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .activity-time {
            font-size: 0.85rem;
            color: var(--text-light);
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
                <?= strtoupper(substr($user['name'], 0, 1)) ?>
            </div>
            <div class="user-info">
                <h5><?= htmlspecialchars($user['name']) ?></h5>
                <p><?= htmlspecialchars($user['email']) ?></p>
            </div>
        </div>
        
        <a href="dashboard.php" class="sidebar-item active">
            <i class="bi bi-speedometer2"></i> Dashboard
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
            <?php if (isset($error)): ?>
                <div class="alert alert-danger">
                    <i class="bi bi-exclamation-triangle-fill me-2"></i><?= $error ?>
                </div>
            <?php endif; ?>
            
            <?php if (isset($_GET['message']) && $_GET['message'] === 'logout_success'): ?>
                <div class="alert alert-success">
                    <i class="bi bi-check-circle-fill me-2"></i>Anda telah berhasil keluar dari sistem.
                </div>
            <?php endif; ?>
            
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h1 class="mb-0">Dashboard</h1>
                <div>
                    <span class="badge bg-success">Online</span>
                </div>
            </div>
            
            <!-- Dashboard Stats -->
            <div class="row mb-4">
                <div class="col-lg-3 col-md-6 mb-4" data-aos="fade-up" data-aos-delay="100">
                    <div class="dashboard-card">
                        <div class="card-icon">
                            <i class="bi bi-cart-check"></i>
                        </div>
                        <div class="card-title">Total Pesanan</div>
                        <div class="card-value">12</div>
                    </div>
                </div>
                <div class="col-lg-3 col-md-6 mb-4" data-aos="fade-up" data-aos-delay="200">
                    <div class="dashboard-card">
                        <div class="card-icon">
                            <i class="bi bi-clock-history"></i>
                        </div>
                        <div class="card-title">Sedang Diproses</div>
                        <div class="card-value">3</div>
                    </div>
                </div>
                <div class="col-lg-3 col-md-6 mb-4" data-aos="fade-up" data-aos-delay="300">
                    <div class="dashboard-card">
                        <div class="card-icon">
                            <i class="bi bi-check-circle"></i>
                        </div>
                        <div class="card-title">Selesai</div>
                        <div class="card-value">9</div>
                    </div>
                </div>
                <div class="col-lg-3 col-md-6 mb-4" data-aos="fade-up" data-aos-delay="400">
                    <div class="dashboard-card">
                        <div class="card-icon">
                            <i class="bi bi-wallet2"></i>
                        </div>
                        <div class="card-title">Total Pengeluaran</div>
                        <div class="card-value">Rp 5.2M</div>
                    </div>
                </div>
            </div>
            
            <!-- Recent Activity & Quick Actions -->
            <div class="row">
                <div class="col-lg-8 mb-4" data-aos="fade-up" data-aos-delay="500">
                    <div class="dashboard-card">
                        <h4 class="mb-4">Aktivitas Terbaru</h4>
                        <div class="activity-list">
                            <div class="activity-item">
                                <div class="activity-icon">
                                    <i class="bi bi-cart-plus"></i>
                                </div>
                                <div class="activity-content">
                                    <div class="activity-title">Pesanan baru: Website Profesional</div>
                                    <div class="activity-time">2 jam yang lalu</div>
                                </div>
                            </div>
                            <div class="activity-item">
                                <div class="activity-icon">
                                    <i class="bi bi-check-circle"></i>
                                </div>
                                <div class="activity-content">
                                    <div class="activity-title">Pesanan selesai: SEO Friendly (Basic)</div>
                                    <div class="activity-time">1 hari yang lalu</div>
                                </div>
                            </div>
                            <div class="activity-item">
                                <div class="activity-icon">
                                    <i class="bi bi-credit-card"></i>
                                </div>
                                <div class="activity-content">
                                    <div class="activity-title">Pembayaran berhasil untuk invoice #INV-2023-001</div>
                                    <div class="activity-time">3 hari yang lalu</div>
                                </div>
                            </div>
                            <div class="activity-item">
                                <div class="activity-icon">
                                    <i class="bi bi-envelope"></i>
                                </div>
                                <div class="activity-content">
                                    <div class="activity-title">Pesan baru dari tim support</div>
                                    <div class="activity-time">5 hari yang lalu</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4 mb-4" data-aos="fade-up" data-aos-delay="600">
                    <div class="dashboard-card">
                        <h4 class="mb-4">Aksi Cepat</h4>
                        <div class="d-grid gap-2">
                            <a href="../services.php" class="btn btn-outline-warning">
                                <i class="bi bi-grid me-2"></i>Lihat Semua Layanan
                            </a>
                            <a href="orders.php?action=new" class="btn btn-outline-warning">
                                <i class="bi bi-plus-circle me-2"></i>Buat Pesanan Baru
                            </a>
                            <a href="calculator.php" class="btn btn-outline-warning">
                                <i class="bi bi-calculator me-2"></i>Hitung Harga
                            </a>
                            <a href="support.php" class="btn btn-outline-warning">
                                <i class="bi bi-headset me-2"></i>Hubungi Support
                            </a>
                        </div>
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
    </script>
</body>
</html>
