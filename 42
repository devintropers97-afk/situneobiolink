<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Admin Reports
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once '../config.php';

// Require admin role
requireRole(ROLE_ADMIN);

// Get current user
 $user = getCurrentUser();

// Get report type
 $reportType = $_GET['type'] ?? 'overview';
 $export = $_GET['export'] ?? '';

// Process export requests
if (!empty($export)) {
    switch ($export) {
        case 'users':
            exportUsers();
            break;
        case 'orders':
            exportOrders();
            break;
        case 'services':
            exportServices();
            break;
        default:
            header('Location: reports.php');
            exit;
    }
}

// Get date range for reports
 $startDate = $_GET['start_date'] ?? date('Y-m-01');
 $endDate = $_GET['end_date'] ?? date('Y-m-t');

// Overview statistics
 $totalUsers = 0;
 $totalOrders = 0;
 $totalRevenue = 0;
 $activeServices = 0;
 $completedOrders = 0;
 $pendingOrders = 0;

// Get total users
 $stmt = $conn->query("SELECT COUNT(*) as count FROM users");
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    $totalUsers = $result->fetch_assoc()['count'];
}

// Get total orders
 $stmt = $conn->query("SELECT COUNT(*) as count FROM orders");
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    $totalOrders = $result->fetch_assoc()['count'];
}

// Get total revenue
 $stmt = $conn->query("SELECT SUM(total_amount) as total FROM orders WHERE status = 'completed'");
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    $totalRevenue = $result->fetch_assoc()['total'];
}

// Get active services
 $stmt = $conn->query("SELECT COUNT(*) as count FROM services WHERE status = 'active'");
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    $activeServices = $result->fetch_assoc()['count'];
}

// Get completed orders
 $stmt = $conn->query("SELECT COUNT(*) as count FROM orders WHERE status = 'completed'");
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    $completedOrders = $result->fetch_assoc()['count'];
}

// Get pending orders
 $stmt = $conn->query("SELECT COUNT(*) as count FROM orders WHERE status = 'pending'");
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    $pendingOrders = $result->fetch_assoc()['count'];
}

// Monthly revenue data
 $monthlyRevenue = [];
 $stmt = $conn->prepare("SELECT DATE_FORMAT(created_at, '%Y-%m') as month, SUM(total_amount) as revenue 
                          FROM orders 
                          WHERE status = 'completed' AND created_at BETWEEN ? AND ?
                          GROUP BY DATE_FORMAT(created_at, '%Y-%m')
                          ORDER BY month");
 $stmt->bind_param("ss", $startDate, $endDate);
 $stmt->execute();
 $result = $stmt->get_result();
while ($row = $result->fetch_assoc()) {
    $monthlyRevenue[] = [
        'month' => $row['month'],
        'revenue' => $row['revenue']
    ];
}

// Top services by revenue
 $topServices = [];
 $stmt = $conn->prepare("SELECT s.name, SUM(o.total_amount) as revenue 
                          FROM services s 
                          JOIN orders o ON s.id = o.service_id 
                          WHERE o.status = 'completed' AND o.created_at BETWEEN ? AND ?
                          GROUP BY s.id 
                          ORDER BY revenue DESC 
                          LIMIT 10");
 $stmt->bind_param("ss", $startDate, $endDate);
 $stmt->execute();
 $result = $stmt->get_result();
while ($row = $result->fetch_assoc()) {
    $topServices[] = [
        'name' => $row['name'],
        'revenue' => $row['revenue']
    ];
}

// Top customers by spending
 $topCustomers = [];
 $stmt = $conn->prepare("SELECT u.name, u.email, SUM(o.total_amount) as spending 
                          FROM users u 
                          JOIN orders o ON u.id = o.user_id 
                          WHERE o.status = 'completed' AND o.created_at BETWEEN ? AND ?
                          GROUP BY u.id 
                          ORDER BY spending DESC 
                          LIMIT 10");
 $stmt->bind_param("ss", $startDate, $endDate);
 $stmt->execute();
 $result = $stmt->get_result();
while ($row = $result->fetch_assoc()) {
    $topCustomers[] = [
        'name' => $row['name'],
        'email' => $row['email'],
        'spending' => $row['spending']
    ];
}

// Orders by status
 $ordersByStatus = [];
 $stmt = $conn->prepare("SELECT status, COUNT(*) as count 
                          FROM orders 
                          WHERE created_at BETWEEN ? AND ?
                          GROUP BY status");
 $stmt->bind_param("ss", $startDate, $endDate);
 $stmt->execute();
 $result = $stmt->get_result();
while ($row = $result->fetch_assoc()) {
    $ordersByStatus[] = [
        'status' => $row['status'],
        'count' => $row['count']
    ];
}

// Users by role
 $usersByRole = [];
 $stmt = $conn->query("SELECT role, COUNT(*) as count FROM users GROUP BY role");
 $result = $stmt->get_result();
while ($row = $result->fetch_assoc()) {
    $usersByRole[] = [
        'role' => $row['role'] == 1 ? 'User' : ($row['role'] == 2 ? 'Admin' : 'Super Admin'),
        'count' => $row['count']
    ];
}

// Services by category
 $servicesByCategory = [];
 $stmt = $conn->prepare("SELECT category, COUNT(*) as count 
                          FROM services 
                          WHERE status = 'active'
                          GROUP BY category");
 $stmt->execute();
 $result = $stmt->get_result();
while ($row = $result->fetch_assoc()) {
    $servicesByCategory[] = [
        'category' => $row['category'],
        'count' => $row['count']
    ];
}

function exportUsers() {
    global $conn;
    
    header('Content-Type: text/csv');
    header('Content-Disposition: attachment; filename="users_export_' . date('Y-m-d') . '.csv"');
    
    $output = fopen('php://output', 'w');
    
    // Header
    fputcsv($output, ['ID', 'Name', 'Email', 'Phone', 'Role', 'Status', 'Email Verified', 'Created At']);
    
    // Data
    $stmt = $conn->query("SELECT id, name, email, phone, role, status, email_verified, created_at FROM users ORDER BY created_at DESC");
    while ($row = $stmt->fetch_assoc()) {
        fputcsv($output, [
            $row['id'],
            $row['name'],
            $row['email'],
            $row['phone'],
            $row['role'] == 1 ? 'User' : ($row['role'] == 2 ? 'Admin' : 'Super Admin'),
            $row['status'],
            $row['email_verified'] ? 'Yes' : 'No',
            $row['created_at']
        ]);
    }
    
    fclose($output);
    exit;
}

function exportOrders() {
    global $conn;
    
    header('Content-Type: text/csv');
    header('Content-Disposition: attachment; filename="orders_export_' . date('Y-m-d') . '.csv"');
    
    $output = fopen('php://output', 'w');
    
    // Header
    fputcsv($output, ['ID', 'Order Number', 'User Name', 'User Email', 'Service Name', 'Total Amount', 'Status', 'Created At']);
    
    // Data
    $stmt = $conn->query("SELECT o.id, o.order_number, u.name as user_name, u.email as user_email, s.name as service_name, o.total_amount, o.status, o.created_at 
                          FROM orders o 
                          LEFT JOIN users u ON o.user_id = u.id 
                          LEFT JOIN services s ON o.service_id = s.id 
                          ORDER BY o.created_at DESC");
    while ($row = $stmt->fetch_assoc()) {
        fputcsv($output, [
            $row['id'],
            $row['order_number'],
            $row['user_name'],
            $row['user_email'],
            $row['service_name'],
            $row['total_amount'],
            $row['status'],
            $row['created_at']
        ]);
    }
    
    fclose($output);
    exit;
}

function exportServices() {
    global $conn;
    
    header('Content-Type: text/csv');
    header('Content-Disposition: attachment; filename="services_export_' . date('Y-m-d') . '.csv"');
    
    $output = fopen('php://output', 'w');
    
    // Header
    fputcsv($output, ['ID', 'Name', 'Category', 'Price Start', 'Price Unit', 'Status', 'Created At']);
    
    // Data
    $stmt = $conn->query("SELECT id, name, category, price_start, price_unit, status, created_at FROM services ORDER BY created_at DESC");
    while ($row = $stmt->fetch_assoc()) {
        fputcsv($output, [
            $row['id'],
            $row['name'],
            $row['category'],
            $row['price_start'],
            $row['price_unit'],
            $row['status'],
            $row['created_at']
        ]);
    }
    
    fclose($output);
    exit;
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Laporan - SITUNEO DIGITAL</title>
    <meta name="description" content="Laporan dan analisis SITUNEO DIGITAL">
    
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
        
        /* Report Card */
        .report-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 25px;
            margin-bottom: 20px;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .report-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        .report-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .report-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--gold);
        }
        
        .report-actions {
            display: flex;
            gap: 10px;
        }
        
        .stat-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 20px;
            text-align: center;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        .stat-icon {
            font-size: 2rem;
            color: var(--gold);
            margin-bottom: 10px;
        }
        
        .stat-value {
            font-size: 2rem;
            font-weight: 800;
            color: var(--gold);
        }
        
        .stat-label {
            color: var(--text-light);
            font-size: 0.9rem;
        }
        
        /* Chart Container */
        .chart-container {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 20px;
            height: 300px;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .chart-container:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        /* Table */
        .table-dark {
            background: rgba(15, 48, 87, 0.5);
            border-radius: 10px;
            overflow: hidden;
        }
        
        .table-dark th {
            background: rgba(30, 92, 153, 0.7);
            border-color: rgba(255, 180, 0, 0.2);
            color: var(--white);
            font-weight: 600;
        }
        
        .table-dark td {
            border-color: rgba(255, 255, 255, 0.1);
            color: var(--white);
        }
        
        .table-dark tbody tr:hover {
            background: rgba(255, 180, 0, 0.1);
        }
        
        /* Date Range Form */
        .date-range-form {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            align-items: center;
        }
        
        .date-input {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            padding: 10px 15px;
            color: var(--white);
            transition: all 0.3s;
        }
        
        .date-input:focus {
            background: rgba(255, 255, 255, 0.15);
            border-color: var(--gold);
            box-shadow: 0 0 0 0.25rem rgba(255, 180, 0, 0.25);
            color: var(--white);
            outline: none;
        }
        
        .btn-outline-gold {
            background: transparent;
            color: var(--gold);
            border: 1px solid var(--gold);
            padding: 10px 20px;
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
        
        .btn-gold {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 10px 20px;
            font-weight: 700;
            border-radius: 50px;
            transition: all 0.3s;
        }
        
        .btn-gold:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.4);
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
        <a href="orders.php" class="sidebar-item">
            <i class="bi bi-receipt"></i> Semua Pesanan
        </a>
        <a href="services.php" class="sidebar-item">
            <i class="bi bi-gear"></i> Kelola Layanan
        </a>
        <a href="reports.php" class="sidebar-item active">
            <i class="bi bi-graph-up"></i> Laporan
        </a>
    </div>
    
    <!-- Main Content -->
    <div class="main-content">
        <div class="container-fluid">
            <div class="report-header">
                <h1 class="report-title">Laporan</h1>
                <div class="report-actions">
                    <a href="?type=overview" class="btn-outline-gold <?= $reportType === 'overview' ? 'active' : '' ?>">
                        Ringkasan
                    </a>
                    <a href="?type=sales" class="btn-outline-gold <?= $reportType === 'sales' ? 'active' : '' ?>">
                        Penjualan
                    </a>
                    <a href="?type=users" class="btn-outline-gold <?= $reportType === 'users' ? 'active' : '' ?>">
                        Pengguna
                    </a>
                    <a href="?type=services" class="btn-outline-gold <?= $reportType === 'services' ? 'active' : '' ?>">
                        Layanan
                    </a>
                </div>
            </div>
            
            <!-- Date Range Form -->
            <form class="date-range-form" method="get" action="">
                <label for="start_date" class="me-2">Dari:</label>
                <input type="date" class="date-input" id="start_date" name="start_date" value="<?= $startDate ?>">
                
                <label for="end_date" class="me-2">Sampai:</label>
                <input type="date" class="date-input" id="end_date" name="end_date" value="<?= $endDate ?>">
                
                <button type="submit" class="btn-gold">
                    <i class="bi bi-calendar3 me-2"></i>Filter
                </button>
            </form>
            
            <?php if ($reportType === 'overview'): ?>
                <!-- Overview Report -->
                <div class="row g-4 mb-4" data-aos="fade-up">
                    <div class="col-md-3">
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="bi bi-people"></i>
                            </div>
                            <div class="stat-value"><?= number_format($totalUsers) ?></div>
                            <div class="stat-label">Total Pengguna</div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="bi bi-receipt"></i>
                            </div>
                            <div class="stat-value"><?= number_format($totalOrders) ?></div>
                            <div class="stat-label">Total Pesanan</div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="bi bi-currency-dollar"></i>
                            </div>
                            <div class="stat-value">Rp <?= number_format($totalRevenue, 0, ',', '.') ?></div>
                            <div class="stat-label">Total Pendapatan</div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="bi bi-gear"></i>
                            </div>
                            <div class="stat-value"><?= number_format($activeServices) ?></div>
                            <div class="stat-label">Layanan Aktif</div>
                        </div>
                    </div>
                </div>
                
                <div class="row g-4 mb-4" data-aos="fade-up" data-aos-delay="100">
                    <div class="col-md-6">
                        <div class="chart-container">
                            <h5 class="mb-3">Pendapatan Bulanan</h5>
                            <canvas id="revenueChart"></canvas>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="chart-container">
                            <h5 class="mb-3">Status Pesanan</h5>
                            <canvas id="ordersStatusChart"></canvas>
                        </div>
                    </div>
                </div>
                
                <div class="row g-4 mb-4" data-aos="fade-up" data-aos-delay="200">
                    <div class="col-md-6">
                        <div class="chart-container">
                            <h5 class="mb-3">Layanan Terpopuler</h5>
                            <canvas id="topServicesChart"></canvas>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="chart-container">
                            <h5 class="mb-3">Pelanggan Terbaik</h5>
                            <canvas id="topCustomersChart"></canvas>
                        </div>
                    </div>
                </div>
            <?php elseif ($reportType === 'sales'): ?>
                <!-- Sales Report -->
                <div class="report-card" data-aos="fade-up">
                    <h5 class="mb-3">Ringkasan Penjualan</h5>
                    <table class="table table-dark table-striped">
                        <thead>
                            <tr>
                                <th>Bulan</th>
                                <th>Pendapatan</th>
                                <th>Pesanan</th>
                            </tr>
                        </thead>
                        <tbody>
                            <?php foreach ($monthlyRevenue as $month): ?>
                                <tr>
                                    <td><?= date('F Y', strtotime($month['month'] . '-01')) ?></td>
                                    <td>Rp <?= number_format($month['revenue'], 0, ',', '.') ?></td>
                                    <td>
                                        <?php
                                        $stmt = $conn->prepare("SELECT COUNT(*) as count FROM orders WHERE DATE_FORMAT(created_at, '%Y-%m') = ? AND status = 'completed'");
                                        $stmt->bind_param("s", $month['month']);
                                        $stmt->execute();
                                        $result = $stmt->get_result();
                                        echo $result->fetch_assoc()['count'];
                                        ?>
                                    </td>
                                </tr>
                            <?php endforeach; ?>
                        </tbody>
                    </table>
                </div>
                
                <div class="report-card" data-aos="fade-up" data-aos-delay="100">
                    <h5 class="mb-3">Layanan Terlaris</h5>
                    <table class="table table-dark table-striped">
                        <thead>
                            <tr>
                                <th>Layanan</th>
                                <th>Pendapatan</th>
                                <th>Pesanan</th>
                            </tr>
                        </thead>
                        <tbody>
                            <?php foreach ($topServices as $service): ?>
                                <tr>
                                    <td><?= htmlspecialchars($service['name']) ?></td>
                                    <td>Rp <?= number_format($service['revenue'], 0, ',', '.') ?></td>
                                    <td>
                                        <?php
                                        $stmt = $conn->prepare("SELECT COUNT(*) as count FROM orders o JOIN services s ON o.service_id = s.id WHERE s.name = ? AND o.status = 'completed'");
                                        $stmt->bind_param("s", $service['name']);
                                        $stmt->execute();
                                        $result = $stmt->get_result();
                                        echo $result->fetch_assoc()['count'];
                                        ?>
                                    </td>
                                </tr>
                            <?php endforeach; ?>
                        </tbody>
                    </table>
                </div>
            <?php elseif ($reportType === 'users'): ?>
                <!-- Users Report -->
                <div class="report-card" data-aos="fade-up">
                    <h5 class="mb-3">Pengguna Berdasarkan Role</h5>
                    <table class="table table-dark table-striped">
                        <thead>
                            <tr>
                                <th>Role</th>
                                <th>Jumlah</th>
                                <th>Persentase</th>
                            </tr>
                        </thead>
                        <tbody>
                            <?php foreach ($usersByRole as $role): ?>
                                <tr>
                                    <td><?= htmlspecialchars($role['role']) ?></td>
                                    <td><?= number_format($role['count']) ?></td>
                                    <td><?= round(($role['count'] / $totalUsers) * 100, 1) ?>%</td>
                                </tr>
                            <?php endforeach; ?>
                        </tbody>
                    </table>
                </div>
                
                <div class="report-card" data-aos="fade-up" data-aos-delay="100">
                    <h5 class="mb-3">Pelanggan Terbaik</h5>
                    <table class="table table-dark table-striped">
                        <thead>
                            <tr>
                                <th>Nama</th>
                                <th>Email</th>
                                <th>Total Pengeluaran</th>
                            </tr>
                        </thead>
                        <tbody>
                            <?php foreach ($topCustomers as $customer): ?>
                                <tr>
                                    <td><?= htmlspecialchars($customer['name']) ?></td>
                                    <td><?= htmlspecialchars($customer['email']) ?></td>
                                    <td>Rp <?= number_format($customer['spending'], 0, ',', '.') ?></td>
                                </tr>
                            <?php endforeach; ?>
                        </tbody>
                    </table>
                </div>
            <?php elseif ($reportType === 'services'): ?>
                <!-- Services Report -->
                <div class="report-card" data-aos="fade-up">
                    <h5 class="mb-3">Layanan Berdasarkan Kategori</h5>
                    <table class="table table-dark table-striped">
                        <thead>
                            <tr>
                                <th>Kategori</th>
                                <th>Jumlah</th>
                                <th>Persentase</th>
                            </tr>
                        </thead>
                        <tbody>
                            <?php foreach ($servicesByCategory as $category): ?>
                                <tr>
                                    <td><?= htmlspecialchars($category['category']) ?></td>
                                    <td><?= number_format($category['count']) ?></td>
                                    <td><?= round(($category['count'] / $activeServices) * 100, 1) ?>%</td>
                                </tr>
                            <?php endforeach; ?>
                        </tbody>
                    </table>
                </div>
            <?php endif; ?>
        </div>
    </div>
    
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
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
        
        // Initialize charts based on report type
        <?php if ($reportType === 'overview'): ?>
            // Revenue Chart
            const revenueCtx = document.getElementById('revenueChart').getContext('2d');
            const revenueChart = new Chart(revenueCtx, {
                type: 'line',
                data: {
                    labels: <?php
                        $labels = [];
                        $data = [];
                        foreach ($monthlyRevenue as $month) {
                            $labels[] = date('M Y', strtotime($month['month'] . '-01'));
                            $data[] = $month['revenue'];
                        }
                        echo json_encode($labels);
                    ?>,
                    datasets: [{
                        label: 'Pendapatan',
                        data: <?php echo json_encode($data); ?>,
                        borderColor: '#FFB400',
                        backgroundColor: 'rgba(255, 180, 0, 0.1)',
                        tension: 0.4,
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                color: '#e9ecef'
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            }
                        },
                        x: {
                            ticks: {
                                color: '#e9ecef'
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            }
                        }
                    }
                }
            });
            
            // Orders Status Chart
            const ordersStatusCtx = document.getElementById('ordersStatusChart').getContext('2d');
            const ordersStatusChart = new Chart(ordersStatusCtx, {
                type: 'doughnut',
                data: {
                    labels: <?php
                        $labels = [];
                        $data = [];
                        foreach ($ordersByStatus as $status) {
                            $labels[] = ucfirst($status['status']);
                            $data[] = $status['count'];
                        }
                        echo json_encode($labels);
                    ?>,
                    datasets: [{
                        data: <?php echo json_encode($data); ?>,
                        backgroundColor: [
                            'rgba(255, 193, 7, 0.8)',
                            'rgba(13, 202, 240, 0.8)',
                            'rgba(25, 135, 84, 0.8)',
                            'rgba(220, 53, 69, 0.8)'
                        ],
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                color: '#e9ecef'
                            }
                        }
                    }
                }
            });
            
            // Top Services Chart
            const topServicesCtx = document.getElementById('topServicesChart').getContext('2d');
            const topServicesChart = new Chart(topServicesCtx, {
                type: 'bar',
                data: {
                    labels: <?php
                        $labels = [];
                        $data = [];
                        foreach ($topServices as $service) {
                            $labels[] = $service['name'];
                            $data[] = $service['revenue'];
                        }
                        echo json_encode($labels);
                    ?>,
                    datasets: [{
                        label: 'Pendapatan',
                        data: <?php echo json_encode($data); ?>,
                        backgroundColor: 'rgba(255, 180, 0, 0.8)',
                        borderColor: 'rgba(255, 180, 0, 1)',
                        borderWidth: 1
                    }]
                },
                options: {
                    indexAxis: 'y',
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                color: '#e9ecef'
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            }
                        },
                        x: {
                            ticks: {
                                color: '#e9ecef'
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            }
                        }
                    }
                }
            });
            
            // Top Customers Chart
            const topCustomersCtx = document.getElementById('topCustomersChart').getContext('2d');
            const topCustomersChart = new Chart(topCustomersCtx, {
                type: 'bar',
                data: {
                    labels: <?php
                        $labels = [];
                        $data = [];
                        foreach ($topCustomers as $customer) {
                            $labels[] = $customer['name'];
                            $data[] = $customer['spending'];
                        }
                        echo json_encode($labels);
                    ?>,
                    datasets: [{
                        label: 'Total Pengeluaran',
                        data: <?php echo json_encode($data); ?>,
                        backgroundColor: 'rgba(255, 180, 0, 0.8)',
                        borderColor: 'rgba(255, 180, 0, 1)',
                        borderWidth: 1
                    }]
                },
                options: {
                    indexAxis: 'y',
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                color: '#e9ecef'
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            }
                        },
                        x: {
                            ticks: {
                                color: '#e9ecef'
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            }
                        }
                    }
                }
            });
        <?php endif; ?>
    </script>
</body>
</html>
