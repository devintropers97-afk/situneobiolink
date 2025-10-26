<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Admin Users Management
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once '../config.php';

// Require admin role
requireRole(ROLE_ADMIN);

// Get current user
 $user = getCurrentUser();

// Get filter parameters
 $role = $_GET['role'] ?? 'all';
 $status = $_GET['status'] ?? 'all';
 $search = $_GET['search'] ?? '';
 $page = isset($_GET['page']) ? (int)$_GET['page'] : 1;
 $limit = 10;
 $offset = ($page - 1) * $limit;

// Build query based on filters
 $whereClause = "WHERE 1=1";
 $params = [];
 $types = "";

if ($role !== 'all') {
    $whereClause .= " AND u.role = ?";
    $params[] = $role;
    $types .= "i";
}

if ($status !== 'all') {
    $whereClause .= " AND u.status = ?";
    $params[] = $status;
    $types .= "s";
}

if (!empty($search)) {
    $whereClause .= " AND (u.name LIKE ? OR u.email LIKE ?)";
    $searchParam = "%$search%";
    $params[] = $searchParam;
    $params[] = $searchParam;
    $types .= "ss";
}

// Get total users count
 $countQuery = "SELECT COUNT(*) as total FROM users u $whereClause";
 $stmt = $conn->prepare($countQuery);
if (!empty($params)) {
    $stmt->bind_param($types, ...$params);
}
 $stmt->execute();
 $result = $stmt->get_result();
 $totalUsers = $result->fetch_assoc()['total'];
 $totalPages = ceil($totalUsers / $limit);

// Get users with pagination
 $query = "SELECT u.*, 
          (SELECT COUNT(*) FROM orders WHERE user_id = u.id) as order_count,
          (SELECT COUNT(*) FROM orders WHERE user_id = u.id AND status = 'completed') as completed_orders
          FROM users u 
          $whereClause 
          ORDER BY u.created_at DESC 
          LIMIT $limit OFFSET $offset";
 $stmt = $conn->prepare($query);
if (!empty($params)) {
    $stmt->bind_param($types, ...$params);
}
 $stmt->execute();
 $users = $stmt->get_result();

// Get user details if requested
 $userDetails = null;
if (isset($_GET['id'])) {
    $userId = $_GET['id'];
    
    $stmt = $conn->prepare("SELECT u.*, 
                          (SELECT COUNT(*) FROM orders WHERE user_id = u.id) as order_count,
                          (SELECT COUNT(*) FROM orders WHERE user_id = u.id AND status = 'completed') as completed_orders,
                          (SELECT SUM(total_amount) FROM orders WHERE user_id = u.id) as total_spent
                          FROM users u 
                          WHERE u.id = ?");
    $stmt->bind_param("i", $userId);
    $stmt->execute();
    $result = $stmt->get_result();
    
    if ($result->num_rows > 0) {
        $userDetails = $result->fetch_assoc();
    }
}

// Process user status update
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['update_status'])) {
    $userId = $_POST['user_id'];
    $status = $_POST['status'];
    
    $stmt = $conn->prepare("UPDATE users SET status = ? WHERE id = ?");
    $stmt->bind_param("si", $status, $userId);
    
    if ($stmt->execute()) {
        // Log activity
        logActivity($user['id'], "Updated user status to $status for user ID: $userId");
        
        header("Location: users.php?success=status_updated");
        exit;
    } else {
        $error = "Terjadi kesalahan saat memperbarui status pengguna.";
    }
}

// Process user role update
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['update_role'])) {
    $userId = $_POST['user_id'];
    $role = $_POST['role'];
    
    $stmt = $conn->prepare("UPDATE users SET role = ? WHERE id = ?");
    $stmt->bind_param("ii", $role, $userId);
    
    if ($stmt->execute()) {
        // Log activity
        logActivity($user['id'], "Updated user role to $role for user ID: $userId");
        
        header("Location: users.php?success=role_updated");
        exit;
    } else {
        $error = "Terjadi kesalahan saat memperbarui role pengguna.";
    }
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kelola Pengguna - SITUNEO DIGITAL</title>
    <meta name="description" content="Kelola pengguna SITUNEO DIGITAL">
    
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
        
        /* User Card */
        .user-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 25px;
            margin-bottom: 20px;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .user-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        .user-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .user-info {
            display: flex;
            align-items: center;
        }
        
        .user-avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            object-fit: cover;
            margin-right: 15px;
        }
        
        .user-avatar-placeholder {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: var(--gradient-gold);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            color: var(--dark-blue);
            margin-right: 15px;
            font-size: 1.5rem;
        }
        
        .user-name {
            font-weight: 700;
            font-size: 1.1rem;
        }
        
        .user-email {
            color: var(--text-light);
            font-size: 0.9rem;
        }
        
        .user-status {
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
        }
        
        .status-active {
            background: rgba(25, 135, 84, 0.2);
            color: #198754;
        }
        
        .status-inactive {
            background: rgba(220, 53, 69, 0.2);
            color: #dc3545;
        }
        
        .status-suspended {
            background: rgba(255, 193, 7, 0.2);
            color: #ffc107;
        }
        
        .user-role {
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            margin-left: 10px;
        }
        
        .role-user {
            background: rgba(13, 202, 240, 0.2);
            color: #0dcaf0;
        }
        
        .role-admin {
            background: rgba(255, 180, 0, 0.2);
            color: var(--gold);
        }
        
        .role-super-admin {
            background: rgba(220, 53, 69, 0.2);
            color: #dc3545;
        }
        
        .user-details {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
        }
        
        .user-stat {
            text-align: center;
        }
        
        .user-stat-value {
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--gold);
        }
        
        .user-stat-label {
            font-size: 0.8rem;
            color: var(--text-light);
        }
        
        .user-actions {
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
        
        /* User Details Modal */
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
        <a href="users.php" class="sidebar-item active">
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
            <?php if (isset($_GET['success']) && $_GET['success'] === 'status_updated'): ?>
                <div class="alert alert-success">
                    <i class="bi bi-check-circle-fill me-2"></i>Status pengguna berhasil diperbarui!
                </div>
            <?php endif; ?>
            
            <?php if (isset($_GET['success']) && $_GET['success'] === 'role_updated'): ?>
                <div class="alert alert-success">
                    <i class="bi bi-check-circle-fill me-2"></i>Role pengguna berhasil diperbarui!
                </div>
            <?php endif; ?>
            
            <?php if (isset($error)): ?>
                <div class="alert alert-danger">
                    <i class="bi bi-exclamation-triangle-fill me-2"></i><?= $error ?>
                </div>
            <?php endif; ?>
            
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h1 class="mb-0">Kelola Pengguna</h1>
                <div class="d-flex gap-2">
                    <button class="btn btn-gold" data-bs-toggle="modal" data-bs-target="#addUserModal">
                        <i class="bi bi-plus-circle me-2"></i>Tambah Pengguna
                    </button>
                    <a href="reports.php?export=users" class="btn btn-outline-gold">
                        <i class="bi bi-download me-2"></i>Export
                    </a>
                </div>
            </div>
            
            <!-- Search Form -->
            <form class="search-form" method="get" action="">
                <input type="text" class="search-input" name="search" placeholder="Cari nama atau email..." value="<?= htmlspecialchars($search) ?>">
                <button type="submit" class="search-btn">
                    <i class="bi bi-search"></i>
                </button>
            </form>
            
            <!-- Filter Tabs -->
            <div class="filter-tabs">
                <div>
                    <span class="me-2">Role:</span>
                    <a href="?role=all&status=<?= $status ?>&search=<?= $search ?>" class="filter-tab <?= $role === 'all' ? 'active' : '' ?>">
                        Semua
                    </a>
                    <a href="?role=1&status=<?= $status ?>&search=<?= $search ?>" class="filter-tab <?= $role === '1' ? 'active' : '' ?>">
                        User
                    </a>
                    <a href="?role=2&status=<?= $status ?>&search=<?= $search ?>" class="filter-tab <?= $role === '2' ? 'active' : '' ?>">
                        Admin
                    </a>
                    <a href="?role=3&status=<?= $status ?>&search=<?= $search ?>" class="filter-tab <?= $role === '3' ? 'active' : '' ?>">
                        Super Admin
                    </a>
                </div>
                
                <div>
                    <span class="me-2">Status:</span>
                    <a href="?status=all&role=<?= $role ?>&search=<?= $search ?>" class="filter-tab <?= $status === 'all' ? 'active' : '' ?>">
                        Semua
                    </a>
                    <a href="?status=active&role=<?= $role ?>&search=<?= $search ?>" class="filter-tab <?= $status === 'active' ? 'active' : '' ?>">
                        Aktif
                    </a>
                    <a href="?status=inactive&role=<?= $role ?>&search=<?= $search ?>" class="filter-tab <?= $status === 'inactive' ? 'active' : '' ?>">
                        Tidak Aktif
                    </a>
                    <a href="?status=suspended&role=<?= $role ?>&search=<?= $search ?>" class="filter-tab <?= $status === 'suspended' ? 'active' : '' ?>">
                        Disuspend
                    </a>
                </div>
            </div>
            
            <!-- Users List -->
            <?php if ($users->num_rows > 0): ?>
                <div class="users-list">
                    <?php while ($userData = $users->fetch_assoc()): ?>
                        <div class="user-card" data-aos="fade-up">
                            <div class="user-header">
                                <div class="user-info">
                                    <?php if ($userData['avatar']): ?>
                                        <img src="../../uploads/avatars/<?= htmlspecialchars($userData['avatar']) ?>" alt="<?= htmlspecialchars($userData['name']) ?>" class="user-avatar">
                                    <?php else: ?>
                                        <div class="user-avatar-placeholder"><?= strtoupper(substr($userData['name'], 0, 1)) ?></div>
                                    <?php endif; ?>
                                    <div>
                                        <div class="user-name"><?= htmlspecialchars($userData['name']) ?></div>
                                        <div class="user-email"><?= htmlspecialchars($userData['email']) ?></div>
                                    </div>
                                </div>
                                <div>
                                    <span class="user-status status-<?= htmlspecialchars($userData['status']) ?>">
                                        <?= ucfirst(htmlspecialchars($userData['status'])) ?>
                                    </span>
                                    <span class="user-role role-<?= $userData['role'] == 1 ? 'user' : ($userData['role'] == 2 ? 'admin' : 'super-admin') ?>">
                                        <?= $userData['role'] == 1 ? 'User' : ($userData['role'] == 2 ? 'Admin' : 'Super Admin') ?>
                                    </span>
                                </div>
                            </div>
                            
                            <div class="user-details">
                                <div class="user-stat">
                                    <div class="user-stat-value"><?= $userData['order_count'] ?></div>
                                    <div class="user-stat-label">Total Pesanan</div>
                                </div>
                                <div class="user-stat">
                                    <div class="user-stat-value"><?= $userData['completed_orders'] ?></div>
                                    <div class="user-stat-label">Selesai</div>
                                </div>
                                <div class="user-stat">
                                    <div class="user-stat-value"><?= date('d M Y', strtotime($userData['created_at'])) ?></div>
                                    <div class="user-stat-label">Bergabung</div>
                                </div>
                            </div>
                            
                            <div class="user-actions">
                                <a href="?id=<?= $userData['id'] ?>" class="btn-outline-gold">
                                    <i class="bi bi-eye me-1"></i> Detail
                                </a>
                                <button class="btn-outline-gold" data-bs-toggle="dropdown">
                                    <i class="bi bi-gear me-1"></i> Kelola
                                </button>
                                <ul class="dropdown-menu dropdown-menu-end">
                                    <li>
                                        <form method="post" action="">
                                            <input type="hidden" name="user_id" value="<?= $userData['id'] ?>">
                                            <input type="hidden" name="update_status" value="1">
                                            
                                            <?php if ($userData['status'] === 'active'): ?>
                                                <button type="submit" name="status" value="inactive" class="dropdown-item">
                                                    <i class="bi bi-pause-circle me-2"></i>Nonaktifkan
                                                </button>
                                                <button type="submit" name="status" value="suspended" class="dropdown-item">
                                                    <i class="bi bi-exclamation-triangle me-2"></i>Suspend
                                                </button>
                                            <?php elseif ($userData['status'] === 'inactive'): ?>
                                                <button type="submit" name="status" value="active" class="dropdown-item">
                                                    <i class="bi bi-play-circle me-2"></i>Aktifkan
                                                </button>
                                                <button type="submit" name="status" value="suspended" class="dropdown-item">
                                                    <i class="bi bi-exclamation-triangle me-2"></i>Suspend
                                                </button>
                                            <?php else: ?>
                                                <button type="submit" name="status" value="active" class="dropdown-item">
                                                    <i class="bi bi-play-circle me-2"></i>Aktifkan
                                                </button>
                                                <button type="submit" name="status" value="inactive" class="dropdown-item">
                                                    <i class="bi bi-pause-circle me-2"></i>Nonaktifkan
                                                </button>
                                            <?php endif; ?>
                                        </form>
                                    </li>
                                    <li><hr class="dropdown-divider"></li>
                                    <li>
                                        <form method="post" action="">
                                            <input type="hidden" name="user_id" value="<?= $userData['id'] ?>">
                                            <input type="hidden" name="update_role" value="1">
                                            
                                            <?php if ($userData['role'] == 1): ?>
                                                <button type="submit" name="role" value="2" class="dropdown-item">
                                                    <i class="bi bi-shield-check me-2"></i>Jadikan Admin
                                                </button>
                                            <?php elseif ($userData['role'] == 2): ?>
                                                <button type="submit" name="role" value="1" class="dropdown-item">
                                                    <i class="bi bi-person me-2"></i>Jadikan User
                                                </button>
                                                <button type="submit" name="role" value="3" class="dropdown-item">
                                                    <i class="bi bi-shield-fill me-2"></i>Jadikan Super Admin
                                                </button>
                                            <?php else: ?>
                                                <button type="submit" name="role" value="2" class="dropdown-item">
                                                    <i class="bi bi-shield-check me-2"></i>Jadikan Admin
                                                </button>
                                                <button type="submit" name="role" value="1" class="dropdown-item">
                                                    <i class="bi bi-person me-2"></i>Jadikan User
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
                            <a href="?role=<?= $role ?>&status=<?= $status ?>&search=<?= $search ?>&page=<?= $page - 1 ?>" class="page-link">
                                <i class="bi bi-chevron-left"></i>
                            </a>
                        <?php endif; ?>
                        
                        <?php for ($i = 1; $i <= $totalPages; $i++): ?>
                            <?php if ($i == $page): ?>
                                <span class="page-link active"><?= $i ?></span>
                            <?php else: ?>
                                <a href="?role=<?= $role ?>&status=<?= $status ?>&search=<?= $search ?>&page=<?= $i ?>" class="page-link"><?= $i ?></a>
                            <?php endif; ?>
                        <?php endfor; ?>
                        
                        <?php if ($page < $totalPages): ?>
                            <a href="?role=<?= $role ?>&status=<?= $status ?>&search=<?= $search ?>&page=<?= $page + 1 ?>" class="page-link">
                                <i class="bi bi-chevron-right"></i>
                            </a>
                        <?php endif; ?>
                    </div>
                <?php endif; ?>
            <?php else: ?>
                <div class="text-center py-5">
                    <i class="bi bi-people fs-1 text-muted"></i>
                    <h4 class="mt-3">Tidak ada pengguna</h4>
                    <p class="text-muted">Tidak ada pengguna yang ditemukan dengan filter ini.</p>
                </div>
            <?php endif; ?>
        </div>
    </div>
    
    <!-- User Details Modal -->
    <?php if ($userDetails): ?>
        <div class="modal fade" id="userDetailsModal" tabindex="-1" aria-labelledby="userDetailsModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="userDetailsModalLabel">Detail Pengguna</h5>
                        <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="row mb-3">
                            <div class="col-md-4 text-center">
                                <?php if ($userDetails['avatar']): ?>
                                    <img src="../../uploads/avatars/<?= htmlspecialchars($userDetails['avatar']) ?>" alt="<?= htmlspecialchars($userDetails['name']) ?>" class="img-fluid rounded-circle mb-3" style="max-width: 150px;">
                                <?php else: ?>
                                    <div class="user-avatar-placeholder mx-auto mb-3" style="width: 150px; height: 150px; font-size: 3rem;">
                                        <?= strtoupper(substr($userDetails['name'], 0, 1)) ?>
                                    </div>
                                <?php endif; ?>
                                <h4><?= htmlspecialchars($userDetails['name']) ?></h4>
                                <p class="text-muted"><?= htmlspecialchars($userDetails['email']) ?></p>
                            </div>
                            <div class="col-md-8">
                                <div class="row mb-3">
                                    <div class="col-sm-4">
                                        <p class="text-muted">Status</p>
                                        <span class="user-status status-<?= htmlspecialchars($userDetails['status']) ?>">
                                            <?= ucfirst(htmlspecialchars($userDetails['status'])) ?>
                                        </span>
                                    </div>
                                    <div class="col-sm-4">
                                        <p class="text-muted">Role</p>
                                        <span class="user-role role-<?= $userDetails['role'] == 1 ? 'user' : ($userDetails['role'] == 2 ? 'admin' : 'super-admin') ?>">
                                            <?= $userDetails['role'] == 1 ? 'User' : ($userDetails['role'] == 2 ? 'Admin' : 'Super Admin') ?>
                                        </span>
                                    </div>
                                    <div class="col-sm-4">
                                        <p class="text-muted">Email Verified</p>
                                        <span class="badge <?= $userDetails['email_verified'] ? 'bg-success' : 'bg-danger' ?>">
                                            <?= $userDetails['email_verified'] ? 'Verified' : 'Not Verified' ?>
                                        </span>
                                    </div>
                                </div>
                                
                                <div class="row mb-3">
                                    <div class="col-sm-4">
                                        <p class="text-muted">Total Pesanan</p>
                                        <h5><?= $userDetails['order_count'] ?></h5>
                                    </div>
                                    <div class="col-sm-4">
                                        <p class="text-muted">Pesanan Selesai</p>
                                        <h5><?= $userDetails['completed_orders'] ?></h5>
                                    </div>
                                    <div class="col-sm-4">
                                        <p class="text-muted">Total Pengeluaran</p>
                                        <h5>Rp <?= number_format($userDetails['total_spent'] ?? 0, 0, ',', '.') ?></h5>
                                    </div>
                                </div>
                                
                                <div class="row mb-3">
                                    <div class="col-sm-6">
                                        <p class="text-muted">Tanggal Bergabung</p>
                                        <h5><?= date('d M Y, H:i', strtotime($userDetails['created_at'])) ?></h5>
                                    </div>
                                    <div class="col-sm-6">
                                        <p class="text-muted">Terakhir Update</p>
                                        <h5><?= date('d M Y, H:i', strtotime($userDetails['updated_at'])) ?></h5>
                                    </div>
                                </div>
                                
                                <?php if (!empty($userDetails['phone'])): ?>
                                    <div class="mb-3">
                                        <p class="text-muted">Nomor Telepon</p>
                                        <h5><?= htmlspecialchars($userDetails['phone']) ?></h5>
                                    </div>
                                <?php endif; ?>
                                
                                <?php if (!empty($userDetails['address'])): ?>
                                    <div class="mb-3">
                                        <p class="text-muted">Alamat</p>
                                        <h5><?= nl2br(htmlspecialchars($userDetails['address'])) ?></h5>
                                    </div>
                                <?php endif; ?>
                                
                                <?php if (!empty($userDetails['referral_code'])): ?>
                                    <div class="mb-3">
                                        <p class="text-muted">Kode Referral</p>
                                        <h5><?= htmlspecialchars($userDetails['referral_code']) ?></h5>
                                    </div>
                                <?php endif; ?>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                        <a href="orders.php?user=<?= $userDetails['id'] ?>" class="btn btn-outline-gold">
                            <i class="bi bi-receipt me-2"></i>Lihat Pesanan
                        </a>
                    </div>
                </div>
            </div>
        </div>
        
        <script>
            document.addEventListener('DOMContentLoaded', function() {
                const userDetailsModal = new bootstrap.Modal(document.getElementById('userDetailsModal'));
                userDetailsModal.show();
            });
        </script>
    <?php endif; ?>
    
    <!-- Add User Modal -->
    <div class="modal fade" id="addUserModal" tabindex="-1" aria-labelledby="addUserModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="addUserModalLabel">Tambah Pengguna Baru</h5>
                    <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <form method="post" action="add_user.php">
                    <div class="modal-body">
                        <div class="mb-3">
                            <label for="name" class="form-label">Nama Lengkap</label>
                            <input type="text" class="form-control" id="name" name="name" required>
                        </div>
                        <div class="mb-3">
                            <label for="email" class="form-label">Email</label>
                            <input type="email" class="form-control" id="email" name="email" required>
                        </div>
                        <div class="mb-3">
                            <label for="password" class="form-label">Password</label>
                            <input type="password" class="form-control" id="password" name="password" required>
                        </div>
                        <div class="mb-3">
                            <label for="phone" class="form-label">Nomor Telepon</label>
                            <input type="tel" class="form-control" id="phone" name="phone">
                        </div>
                        <div class="mb-3">
                            <label for="role" class="form-label">Role</label>
                            <select class="form-select" id="role" name="role" required>
                                <option value="1">User</option>
                                <option value="2">Admin</option>
                                <?php if ($user['role'] == 3): ?>
                                    <option value="3">Super Admin</option>
                                <?php endif; ?>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label for="status" class="form-label">Status</label>
                            <select class="form-select" id="status" name="status" required>
                                <option value="active">Aktif</option>
                                <option value="inactive">Tidak Aktif</option>
                            </select>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
                        <button type="submit" class="btn btn-gold">Tambah Pengguna</button>
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
    </script>
</body>
</html>
