<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Admin Orders Management
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once '../config.php';

// Require admin role
requireRole(ROLE_ADMIN);

// Get current user
 $user = getCurrentUser();

// Get filter parameters
 $status = $_GET['status'] ?? 'all';
 $search = $_GET['search'] ?? '';
 $page = isset($_GET['page']) ? (int)$_GET['page'] : 1;
 $limit = 10;
 $offset = ($page - 1) * $limit;

// Build query based on filters
 $whereClause = "WHERE 1=1";
 $params = [];
 $types = "";

if ($status !== 'all') {
    $whereClause .= " AND o.status = ?";
    $params[] = $status;
    $types .= "s";
}

if (!empty($search)) {
    $whereClause .= " AND (o.order_number LIKE ? OR u.name LIKE ? OR u.email LIKE ?)";
    $searchParam = "%$search%";
    $params[] = $searchParam;
    $params[] = $searchParam;
    $params[] = $searchParam;
    $types .= "sss";
}

// Get total orders count
 $countQuery = "SELECT COUNT(*) as total FROM orders o LEFT JOIN users u ON o.user_id = u.id $whereClause";
 $stmt = $conn->prepare($countQuery);
if (!empty($params)) {
    $stmt->bind_param($types, ...$params);
}
 $stmt->execute();
 $result = $stmt->get_result();
 $totalOrders = $result->fetch_assoc()['total'];
 $totalPages = ceil($totalOrders / $limit);

// Get orders with pagination
 $query = "SELECT o.*, u.name as user_name, u.email as user_email, s.name as service_name, s.image as service_image 
          FROM orders o 
          LEFT JOIN users u ON o.user_id = u.id 
          LEFT JOIN services s ON o.service_id = s.id 
          $whereClause 
          ORDER BY o.created_at DESC 
          LIMIT $limit OFFSET $offset";
 $stmt = $conn->prepare($query);
if (!empty($params)) {
    $stmt->bind_param($types, ...$params);
}
 $stmt->execute();
 $orders = $stmt->get_result();

// Get order details if requested
 $orderDetails = null;
if (isset($_GET['id'])) {
    $orderId = $_GET['id'];
    
    $stmt = $conn->prepare("SELECT o.*, u.name as user_name, u.email as user_email, u.phone as user_phone, u.address as user_address, 
                           s.name as service_name, s.description as service_description, s.image as service_image 
                           FROM orders o 
                           LEFT JOIN users u ON o.user_id = u.id 
                           LEFT JOIN services s ON o.service_id = s.id 
                           WHERE o.id = ?");
    $stmt->bind_param("i", $orderId);
    $stmt->execute();
    $result = $stmt->get_result();
    
    if ($result->num_rows > 0) {
        $orderDetails = $result->fetch_assoc();
    }
}

// Process order status update
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['update_status'])) {
    $orderId = $_POST['order_id'];
    $status = $_POST['status'];
    $notes = $_POST['notes'] ?? '';
    
    $stmt = $conn->prepare("UPDATE orders SET status = ?, notes = ?, updated_at = NOW() WHERE id = ?");
    $stmt->bind_param("ssi", $status, $notes, $orderId);
    
    if ($stmt->execute()) {
        // Log activity
        logActivity($user['id'], "Updated order status to $status for order ID: $orderId");
        
        header("Location: orders.php?success=status_updated");
        exit;
    } else {
        $error = "Terjadi kesalahan saat memperbarui status pesanan.";
    }
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kelola Pesanan - SITUNEO DIGITAL</title>
    <meta name="description" content="Kelola pesanan SITUNEO DIGITAL">
    
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
        
        /* Order Card */
        .order-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 25px;
            margin-bottom: 20px;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .order-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        .order-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .order-number {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--gold);
        }
        
        .order-status {
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
        }
        
        .status-pending {
            background: rgba(255, 193, 7, 0.2);
            color: #ffc107;
        }
        
        .status-processing {
            background: rgba(13, 202, 240, 0.2);
            color: #0dcaf0;
        }
        
        .status-completed {
            background: rgba(25, 135, 84, 0.2);
            color: #198754;
        }
        
        .status-cancelled {
            background: rgba(220, 53, 69, 0.2);
            color: #dc3545;
        }
        
        .order-service {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .service-image {
            width: 60px;
            height: 60px;
            border-radius: 10px;
            object-fit: cover;
            margin-right: 15px;
        }
        
        .service-name {
            font-weight: 600;
            font-size: 1.1rem;
        }
        
        .order-details {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
        }
        
        .order-date {
            color: var(--text-light);
            font-size: 0.9rem;
        }
        
        .order-amount {
            font-weight: 700;
            font-size: 1.1rem;
            color: var(--gold);
        }
        
        .order-customer {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
        }
        
        .customer-name {
            font-weight: 600;
        }
        
        .customer-email {
            color: var(--text-light);
            font-size: 0.9rem;
        }
        
        .order-actions {
            display: flex;
            gap: 10px;
        }
        
        .btn-outline-gold {
            background: transparent;
            color: var(--gold);
            border: 1px solid var(--gold);
            padding: 8px 15px;
            font-weight: 600;
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
        
        /* Filter Tabs */
        .filter-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }
        
        .filter-tab {
            padding: 10px 20px;
            border-radius: 50px;
            background: rgba(255, 180, 0, 0.1);
            border: 1px solid rgba(255, 180, 0, 0.3);
            color: var(--text-light);
            font-weight: 600;
            transition: all 0.3s;
            text-decoration: none;
        }
        
        .filter-tab:hover, .filter-tab.active {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: var(--gold);
        }
        
        /* Search Form */
        .search-form {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
        }
        
        .search-input {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 50px;
            padding: 10px 20px;
            color: var(--white);
            transition: all 0.3s;
            flex: 1;
        }
        
        .search-input:focus {
            background: rgba(255, 255, 255, 0.15);
            border-color: var(--gold);
            box-shadow: 0 0 0 0.25rem rgba(255, 180, 0, 0.25);
            color: var(--white);
            outline: none;
        }
        
        .search-input::placeholder {
            color: rgba(255, 255, 255, 0.5);
        }
        
        .search-btn {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 10px 20px;
            font-weight: 700;
            border-radius: 50px;
            transition: all 0.3s;
        }
        
        .search-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.4);
        }
        
        /* Pagination */
        .pagination {
            display: flex;
            justify-content: center;
            gap: 5px;
            margin-top: 30px;
        }
        
        .page-link {
            background: rgba(255, 180, 0, 0.1);
            border: 1px solid rgba(255, 180, 0, 0.3);
            color: var(--text-light);
            padding: 8px 15px;
            border-radius: 5px;
            transition: all 0.3s;
            text-decoration: none;
        }
        
        .page-link:hover, .page-link.active {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border-color: var(--gold);
        }
        
        /* Order Details Modal */
        .modal-content {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            backdrop-filter: blur(10px);
        }
        
        .modal-header {
            border-bottom: 1px solid rgba(255, 180, 0, 0.2);
        }
        
        .modal-footer {
            border-top: 1px solid rgba(255, 180, 0, 0.2);
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
        
        .form-label {
            font-weight: 600;
            margin-bottom: 8px;
            color: var(--text-light);
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
                        <li><a class="dropdown-item" href="../settings.php"><i class="bi bi-gear me-2"></i> Pengaturan</a></li>
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
        <a href="orders.php" class="sidebar-item active">
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
            <?php if (isset($_GET['success']) && $_GET['success'] === 'status_updated'): ?>
                <div class="alert alert-success">
                    <i class="bi bi-check-circle-fill me-2"></i>Status pesanan berhasil diperbarui!
                </div>
            <?php endif; ?>
            
            <?php if (isset($error)): ?>
                <div class="alert alert-danger">
                    <i class="bi bi-exclamation-triangle-fill me-2"></i><?= $error ?>
                </div>
            <?php endif; ?>
            
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h1 class="mb-0">Kelola Semua Pesanan</h1>
                <div class="d-flex gap-2">
                    <button class="btn btn-gold" data-bs-toggle="modal" data-bs-target="#addOrderModal">
                        <i class="bi bi-plus-circle me-2"></i>Tambah Pesanan
                    </button>
                    <a href="reports.php?export=orders" class="btn btn-outline-gold">
                        <i class="bi bi-download me-2"></i>Export
                    </a>
                </div>
            </div>
            
            <!-- Search Form -->
            <form class="search-form" method="get" action="">
                <input type="text" class="search-input" name="search" placeholder="Cari nomor pesanan, nama atau email..." value="<?= htmlspecialchars($search) ?>">
                <button type="submit" class="search-btn">
                    <i class="bi bi-search"></i>
                </button>
            </form>
            
            <!-- Filter Tabs -->
            <div class="filter-tabs">
                <span class="me-2">Status:</span>
                <a href="?status=all&search=<?= $search ?>" class="filter-tab <?= $status === 'all' ? 'active' : '' ?>">
                    Semua (<?= $totalOrders ?>)
                </a>
                <a href="?status=pending&search=<?= $search ?>" class="filter-tab <?= $status === 'pending' ? 'active' : '' ?>">
                    Menunggu
                </a>
                <a href="?status=processing&search=<?= $search ?>" class="filter-tab <?= $status === 'processing' ? 'active' : '' ?>">
                    Diproses
                </a>
                <a href="?status=completed&search=<?= $search ?>" class="filter-tab <?= $status === 'completed' ? 'active' : '' ?>">
                    Selesai
                </a>
                <a href="?status=cancelled&search=<?= $search ?>" class="filter-tab <?= $status === 'cancelled' ? 'active' : '' ?>">
                    Dibatalkan
                </a>
            </div>
            
            <!-- Orders List -->
            <?php if ($orders->num_rows > 0): ?>
                <div class="orders-list">
                    <?php while ($order = $orders->fetch_assoc()): ?>
                        <div class="order-card" data-aos="fade-up">
                            <div class="order-header">
                                <div class="order-number"><?= htmlspecialchars($order['order_number']) ?></div>
                                <div class="order-status status-<?= htmlspecialchars($order['status']) ?>">
                                    <?= ucfirst(htmlspecialchars($order['status'])) ?>
                                </div>
                            </div>
                            
                            <div class="order-service">
                                <?php if ($order['service_image']): ?>
                                    <img src="<?= htmlspecialchars($order['service_image']) ?>" alt="<?= htmlspecialchars($order['service_name']) ?>" class="service-image">
                                <?php else: ?>
                                    <div class="service-image d-flex align-items-center justify-content-center bg-secondary">
                                        <i class="bi bi-image fs-4"></i>
                                    </div>
                                <?php endif; ?>
                                <div>
                                    <div class="service-name"><?= htmlspecialchars($order['service_name']) ?></div>
                                    <div class="text-muted small">
                                        <?= date('d M Y, H:i', strtotime($order['created_at'])) ?>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="order-customer">
                                <div>
                                    <div class="customer-name"><?= htmlspecialchars($order['user_name']) ?></div>
                                    <div class="customer-email"><?= htmlspecialchars($order['user_email']) ?></div>
                                </div>
                                <div class="order-amount">
                                    Rp <?= number_format($order['total_amount'], 0, ',', '.') ?>
                                </div>
                            </div>
                            
                            <div class="order-actions">
                                <a href="?id=<?= $order['id'] ?>" class="btn-outline-gold">
                                    <i class="bi bi-eye me-1"></i> Detail
                                </a>
                                <button class="btn-outline-gold" data-bs-toggle="dropdown">
                                    <i class="bi bi-gear me-1"></i> Kelola
                                </button>
                                <ul class="dropdown-menu dropdown-menu-end">
                                    <li>
                                        <form method="post" action="">
                                            <input type="hidden" name="order_id" value="<?= $order['id'] ?>">
                                            <input type="hidden" name="update_status" value="1">
                                            
                                            <?php if ($order['status'] === 'pending'): ?>
                                                <button type="submit" name="status" value="processing" class="dropdown-item">
                                                    <i class="bi bi-play-circle me-2"></i>Proses
                                                </button>
                                                <button type="submit" name="status" value="completed" class="dropdown-item">
                                                    <i class="bi bi-check-circle me-2"></i>Selesaikan
                                                </button>
                                                <button type="submit" name="status" value="cancelled" class="dropdown-item">
                                                    <i class="bi bi-x-circle me-2"></i>Batalkan
                                                </button>
                                            <?php elseif ($order['status'] === 'processing'): ?>
                                                <button type="submit" name="status" value="completed" class="dropdown-item">
                                                    <i class="bi bi-check-circle me-2"></i>Selesaikan
                                                </button>
                                                <button type="submit" name="status" value="cancelled" class="dropdown-item">
                                                    <i class="bi bi-x-circle me-2"></i>Batalkan
                                                </button>
                                            <?php elseif ($order['status'] === 'completed'): ?>
                                                <button type="submit" name="status" value="processing" class="dropdown-item">
                                                    <i class="bi bi-arrow-clockwise me-2"></i>Kembali ke Proses
                                                </button>
                                                <button type="submit" name="status" value="cancelled" class="dropdown-item">
                                                    <i class="bi bi-x-circle me-2"></i>Batalkan
                                                </button>
                                            <?php else: ?>
                                                <button type="submit" name="status" value="pending" class="dropdown-item">
                                                    <i class="bi bi-arrow-counterclockwise me-2"></i>Kembali ke Menunggu
                                                </button>
                                                <button type="submit" name="status" value="processing" class="dropdown-item">
                                                    <i class="bi bi-play-circle me-2"></i>Proses
                                                </button>
                                                <button type="submit" name="status" value="completed" class="dropdown-item">
                                                    <i class="bi bi-check-circle me-2"></i>Selesaikan
                                                </button>
                                            <?php endif; ?>
                                        </form>
                                    </li>
                                </ul>
                            </div>
                        </div>
                    <?php endwhile; ?>
                </div>
                
                <!-- Pagination -->
                <?php if ($totalPages > 1): ?>
                    <div class="pagination">
                        <?php if ($page > 1): ?>
                            <a href="?status=<?= $status ?>&search=<?= $search ?>&page=<?= $page - 1 ?>" class="page-link">
                                <i class="bi bi-chevron-left"></i>
                            </a>
                        <?php endif; ?>
                        
                        <?php for ($i = 1; $i <= $totalPages; $i++): ?>
                            <?php if ($i == $page): ?>
                                <span class="page-link active"><?= $i ?></span>
                            <?php else: ?>
                                <a href="?status=<?= $status ?>&search=<?= $search ?>&page=<?= $i ?>" class="page-link"><?= $i ?></a>
                            <?php endif; ?>
                        <?php endfor; ?>
                        
                        <?php if ($page < $totalPages): ?>
                            <a href="?status=<?= $status ?>&search=<?= $search ?>&page=<?= $page + 1 ?>" class="page-link">
                                <i class="bi bi-chevron-right"></i>
                            </a>
                        <?php endif; ?>
                    </div>
                <?php endif; ?>
            <?php else: ?>
                <div class="text-center py-5">
                    <i class="bi bi-receipt fs-1 text-muted"></i>
                    <h4 class="mt-3">Tidak ada pesanan</h4>
                    <p class="text-muted">Tidak ada pesanan yang ditemukan dengan filter ini.</p>
                </div>
            <?php endif; ?>
        </div>
    </div>
    
    <!-- Order Details Modal -->
    <?php if ($orderDetails): ?>
        <div class="modal fade" id="orderDetailsModal" tabindex="-1" aria-labelledby="orderDetailsModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="orderDetailsModalLabel">Detail Pesanan</h5>
                        <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="row mb-3">
                            <div class="col-md-6">
                                <p class="text-muted">Nomor Pesanan</p>
                                <h6><?= htmlspecialchars($orderDetails['order_number']) ?></h6>
                            </div>
                            <div class="col-md-6">
                                <p class="text-muted">Status</p>
                                <div class="order-status status-<?= htmlspecialchars($orderDetails['status']) ?>">
                                    <?= ucfirst(htmlspecialchars($orderDetails['status'])) ?>
                                </div>
                            </div>
                        </div>
                        
                        <div class="row mb-3">
                            <div class="col-md-6">
                                <p class="text-muted">Tanggal Pesanan</p>
                                <h6><?= date('d M Y, H:i', strtotime($orderDetails['created_at'])) ?></h6>
                            </div>
                            <div class="col-md-6">
                                <p class="text-muted">Total Harga</p>
                                <h6 class="text-warning">Rp <?= number_format($orderDetails['total_amount'], 0, ',', '.') ?></h6>
                            </div>
                        </div>
                        
                        <div class="row mb-3">
                            <div class="col-md-6">
                                <p class="text-muted">Informasi Pelanggan</p>
                                <h6><?= htmlspecialchars($orderDetails['user_name']) ?></h6>
                                <p class="text-muted small"><?= htmlspecialchars($orderDetails['user_email']) ?></p>
                                <?php if (!empty($orderDetails['user_phone'])): ?>
                                    <p class="text-muted small"><?= htmlspecialchars($orderDetails['user_phone']) ?></p>
                                <?php endif; ?>
                            </div>
                            <div class="col-md-6">
                                <p class="text-muted">Layanan</p>
                                <div class="d-flex align-items-center">
                                    <?php if ($orderDetails['service_image']): ?>
                                        <img src="<?= htmlspecialchars($orderDetails['service_image']) ?>" alt="<?= htmlspecialchars($orderDetails['service_name']) ?>" class="service-image me-3">
                                    <?php else: ?>
                                        <div class="service-image me-3 d-flex align-items-center justify-content-center bg-secondary">
                                            <i class="bi bi-image fs-4"></i>
                                        </div>
                                    <?php endif; ?>
                                    <div>
                                        <h6><?= htmlspecialchars($orderDetails['service_name']) ?></h6>
                                        <p class="text-muted small mb-0"><?= htmlspecialchars($orderDetails['service_description']) ?></p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                        <?php if (!empty($orderDetails['requirements'])): ?>
                            <div class="mb-3">
                                <p class="text-muted">Keperluan</p>
                                <p><?= nl2br(htmlspecialchars($orderDetails['requirements'])) ?></p>
                            </div>
                        <?php endif; ?>
                        
                        <?php if (!empty($orderDetails['notes'])): ?>
                            <div class="mb-3">
                                <p class="text-muted">Catatan</p>
                                <p><?= nl2br(htmlspecialchars($orderDetails['notes'])) ?></p>
                            </div>
                        <?php endif; ?>
                        
                        <div class="mb-3">
                            <p class="text-muted">Update Status</p>
                            <form method="post" action="">
                                <input type="hidden" name="order_id" value="<?= $orderDetails['id'] ?>">
                                <input type="hidden" name="update_status" value="1">
                                
                                <div class="row">
                                    <div class="col-md-6 mb-3">
                                        <select class="form-select" name="status">
                                            <option value="pending" <?= $orderDetails['status'] === 'pending' ? 'selected' : '' ?>>Menunggu</option>
                                            <option value="processing" <?= $orderDetails['status'] === 'processing' ? 'selected' : '' ?>>Diproses</option>
                                            <option value="completed" <?= $orderDetails['status'] === 'completed' ? 'selected' : '' ?>>Selesai</option>
                                            <option value="cancelled" <?= $orderDetails['status'] === 'cancelled' ? 'selected' : '' ?>>Dibatalkan</option>
                                        </select>
                                    </div>
                                    <div class="col-md-6 mb-3">
                                        <input type="text" class="form-control" name="notes" placeholder="Catatan (opsional)" value="<?= htmlspecialchars($orderDetails['notes'] ?? '') ?>">
                                    </div>
                                </div>
                                
                                <button type="submit" class="btn btn-gold">Update Status</button>
                            </form>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                        <a href="../invoices.php?id=<?= $orderDetails['id'] ?>" class="btn btn-outline-gold" target="_blank">
                            <i class="bi bi-file-earmark-text me-2"></i>Lihat Invoice
                        </a>
                    </div>
                </div>
            </div>
        </div>
        
        <script>
            document.addEventListener('DOMContentLoaded', function() {
                const orderDetailsModal = new bootstrap.Modal(document.getElementById('orderDetailsModal'));
                orderDetailsModal.show();
            });
        </script>
    <?php endif; ?>
    
    <!-- Add Order Modal -->
    <div class="modal fade" id="addOrderModal" tabindex="-1" aria-labelledby="addOrderModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="addOrderModalLabel">Tambah Pesanan Baru</h5>
                    <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <form method="post" action="add_order.php">
                    <div class="modal-body">
                        <div class="mb-3">
                            <label for="user_id" class="form-label">Pelanggan</label>
                            <select class="form-select" id="user_id" name="user_id" required>
                                <option value="">Pilih Pelanggan</option>
                                <?php
                                $usersQuery = "SELECT id, name, email FROM users WHERE status = 'active' ORDER BY name";
                                $usersResult = $conn->query($usersQuery);
                                while ($userOption = $usersResult->fetch_assoc()) {
                                    echo '<option value="' . $userOption['id'] . '">' . htmlspecialchars($userOption['name']) . ' (' . htmlspecialchars($userOption['email']) . ')</option>';
                                }
                                ?>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label for="service_id" class="form-label">Layanan</label>
                            <select class="form-select" id="service_id" name="service_id" required>
                                <option value="">Pilih Layanan</option>
                                <?php
                                $servicesQuery = "SELECT id, name, price_start FROM services WHERE status = 'active' ORDER BY name";
                                $servicesResult = $conn->query($servicesQuery);
                                while ($serviceOption = $servicesResult->fetch_assoc()) {
                                    echo '<option value="' . $serviceOption['id'] . '">' . htmlspecialchars($serviceOption['name']) . ' - Rp ' . number_format($serviceOption['price_start'], 0, ',', '.') . '</option>';
                                }
                                ?>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label for="total_amount" class="form-label">Total Harga</label>
                            <div class="input-group">
                                <span class="input-group-text">Rp</span>
                                <input type="text" class="form-control" id="total_amount" name="total_amount" required>
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="requirements" class="form-label">Keperluan</label>
                            <textarea class="form-control" id="requirements" name="requirements" rows="4" placeholder="Jelaskan keperluan pelanggan..." required></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="status" class="form-label">Status</label>
                            <select class="form-select" id="status" name="status" required>
                                <option value="pending">Menunggu</option>
                                <option value="processing">Diproses</option>
                                <option value="completed">Selesai</option>
                            </select>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
                        <button type="submit" class="btn btn-gold">Tambah Pesanan</button>
                    </div>
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
        
        // Update total amount when service is selected
        document.getElementById('service_id').addEventListener('change', function() {
            const serviceId = this.value;
            if (serviceId) {
                // Fetch service price via AJAX
                fetch(`get_service_price.php?id=${serviceId}`)
                    .then(response => response.json())
                    .then(data => {
                        if (data.success) {
                            document.getElementById('total_amount').value = data.price.toLocaleString('id-ID');
                        }
                    })
                    .catch(error => console.error('Error:', error));
            } else {
                document.getElementById('total_amount').value = '';
            }
        });
    </script>
</body>
</html>
