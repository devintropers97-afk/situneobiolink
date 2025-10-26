<?php
/**
 * ========================================
 * SITUNEO DIGITAL - User Invoices Page
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once 'config.php';

// Require login
requireLogin();

// Get current user
 $user = getCurrentUser();

// Get filter parameters
 $status = $_GET['status'] ?? 'all';
 $page = isset($_GET['page']) ? (int)$_GET['page'] : 1;
 $limit = 10;
 $offset = ($page - 1) * $limit;

// Build query based on status filter
 $whereClause = "WHERE o.user_id = ?";
 $params = [$user['id']];
 $types = "i";

if ($status !== 'all') {
    $whereClause .= " AND o.status = ?";
    $params[] = $status;
    $types .= "s";
}

// Get total invoices count
 $countQuery = "SELECT COUNT(*) as total FROM orders o $whereClause";
 $stmt = $conn->prepare($countQuery);
 $stmt->bind_param($types, ...$params);
 $stmt->execute();
 $result = $stmt->get_result();
 $totalInvoices = $result->fetch_assoc()['total'];
 $totalPages = ceil($totalInvoices / $limit);

// Get invoices with pagination
 $query = "SELECT o.*, s.name as service_name, s.image as service_image 
          FROM orders o 
          LEFT JOIN services s ON o.service_id = s.id 
          $whereClause 
          ORDER BY o.created_at DESC 
          LIMIT $limit OFFSET $offset";
 $stmt = $conn->prepare($query);
 $stmt->bind_param($types, ...$params);
 $stmt->execute();
 $invoices = $stmt->get_result();

// Get invoice details if requested
 $invoiceDetails = null;
if (isset($_GET['id'])) {
    $invoiceId = $_GET['id'];
    
    $stmt = $conn->prepare("SELECT o.*, s.name as service_name, s.description as service_description, s.image as service_image, u.name as user_name, u.email as user_email, u.phone as user_phone, u.address as user_address 
                           FROM orders o 
                           LEFT JOIN services s ON o.service_id = s.id 
                           LEFT JOIN users u ON o.user_id = u.id 
                           WHERE o.id = ? AND o.user_id = ?");
    $stmt->bind_param("ii", $invoiceId, $user['id']);
    $stmt->execute();
    $result = $stmt->get_result();
    
    if ($result->num_rows > 0) {
        $invoiceDetails = $result->fetch_assoc();
    }
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Invoice - SITUNEO DIGITAL</title>
    <meta name="description" content="Kelola invoice pesanan Anda">
    
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
        
        /* Invoice Card */
        .invoice-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            padding: 25px;
            margin-bottom: 20px;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .invoice-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
        }
        
        .invoice-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .invoice-number {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--gold);
        }
        
        .invoice-status {
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
        
        .invoice-service {
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
        
        .invoice-details {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
        }
        
        .invoice-date {
            color: var(--text-light);
            font-size: 0.9rem;
        }
        
        .invoice-amount {
            font-weight: 700;
            font-size: 1.1rem;
            color: var(--gold);
        }
        
        .invoice-actions {
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
        
        /* Invoice Details Modal */
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
        
        /* Invoice Print Styles */
        .invoice-print {
            background: white;
            color: black;
            padding: 30px;
        }
        
        .invoice-print-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 30px;
        }
        
        .invoice-print-logo {
            width: 150px;
        }
        
        .invoice-print-details {
            text-align: right;
        }
        
        .invoice-print-title {
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 5px;
        }
        
        .invoice-print-number {
            font-size: 1.2rem;
            margin-bottom: 5px;
        }
        
        .invoice-print-date {
            margin-bottom: 0;
        }
        
        .invoice-print-section {
            margin-bottom: 20px;
        }
        
        .invoice-print-section-title {
            font-weight: 700;
            margin-bottom: 10px;
            padding-bottom: 5px;
            border-bottom: 1px solid #eee;
        }
        
        .invoice-print-table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        
        .invoice-print-table th,
        .invoice-print-table td {
            padding: 10px;
            text-align: left;
            border-bottom: 1px solid #eee;
        }
        
        .invoice-print-table th {
            background: #f8f9fa;
            font-weight: 600;
        }
        
        .invoice-print-total {
            text-align: right;
            font-weight: 700;
        }
        
        .invoice-print-footer {
            margin-top: 30px;
            text-align: center;
            font-size: 0.9rem;
            color: #666;
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
        <a href="orders.php" class="sidebar-item">
            <i class="bi bi-cart-check"></i> Pesanan Saya
        </a>
        <a href="services.php" class="sidebar-item">
            <i class="bi bi-grid"></i> Layanan
        </a>
        <a href="invoices.php" class="sidebar-item active">
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
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h1 class="mb-0">Invoice Saya</h1>
            </div>
            
            <!-- Filter Tabs -->
            <div class="filter-tabs">
                <a href="?status=all" class="filter-tab <?= $status === 'all' ? 'active' : '' ?>">
                    Semua (<?= $totalInvoices ?>)
                </a>
                <a href="?status=pending" class="filter-tab <?= $status === 'pending' ? 'active' : '' ?>">
                    Menunggu
                </a>
                <a href="?status=processing" class="filter-tab <?= $status === 'processing' ? 'active' : '' ?>">
                    Diproses
                </a>
                <a href="?status=completed" class="filter-tab <?= $status === 'completed' ? 'active' : '' ?>">
                    Selesai
                </a>
                <a href="?status=cancelled" class="filter-tab <?= $status === 'cancelled' ? 'active' : '' ?>">
                    Dibatalkan
                </a>
            </div>
            
            <!-- Invoices List -->
            <?php if ($invoices->num_rows > 0): ?>
                <div class="invoices-list">
                    <?php while ($invoice = $invoices->fetch_assoc()): ?>
                        <div class="invoice-card" data-aos="fade-up">
                            <div class="invoice-header">
                                <div class="invoice-number">INV-<?= date('Ymd', strtotime($invoice['created_at'])) ?>-<?= str_pad($invoice['id'], 4, '0', STR_PAD_LEFT) ?></div>
                                <div class="invoice-status status-<?= htmlspecialchars($invoice['status']) ?>">
                                    <?= ucfirst(htmlspecialchars($invoice['status'])) ?>
                                </div>
                            </div>
                            
                            <div class="invoice-service">
                                <?php if ($invoice['service_image']): ?>
                                    <img src="<?= htmlspecialchars($invoice['service_image']) ?>" alt="<?= htmlspecialchars($invoice['service_name']) ?>" class="service-image">
                                <?php else: ?>
                                    <div class="service-image d-flex align-items-center justify-content-center bg-secondary">
                                        <i class="bi bi-image fs-4"></i>
                                    </div>
                                <?php endif; ?>
                                <div>
                                    <div class="service-name"><?= htmlspecialchars($invoice['service_name']) ?></div>
                                    <div class="text-muted small">
                                        <?= date('d M Y, H:i', strtotime($invoice['created_at'])) ?>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="invoice-details">
                                <div class="invoice-date">
                                    <i class="bi bi-calendar3 me-1"></i>
                                    <?= date('d M Y', strtotime($invoice['created_at'])) ?>
                                </div>
                                <div class="invoice-amount">
                                    Rp <?= number_format($invoice['total_amount'], 0, ',', '.') ?>
                                </div>
                            </div>
                            
                            <div class="invoice-actions">
                                <a href="?id=<?= $invoice['id'] ?>" class="btn-outline-gold">
                                    <i class="bi bi-eye me-1"></i> Detail
                                </a>
                                <a href="?id=<?= $invoice['id'] ?>&print=1" class="btn-outline-gold" target="_blank">
                                    <i class="bi bi-printer me-1"></i> Cetak
                                </a>
                            </div>
                        </div>
                    <?php endwhile; ?>
                </div>
                
                <!-- Pagination -->
                <?php if ($totalPages > 1): ?>
                    <div class="pagination">
                        <?php if ($page > 1): ?>
                            <a href="?status=<?= $status ?>&page=<?= $page - 1 ?>" class="page-link">
                                <i class="bi bi-chevron-left"></i>
                            </a>
                        <?php endif; ?>
                        
                        <?php for ($i = 1; $i <= $totalPages; $i++): ?>
                            <?php if ($i == $page): ?>
                                <span class="page-link active"><?= $i ?></span>
                            <?php else: ?>
                                <a href="?status=<?= $status ?>&page=<?= $i ?>" class="page-link"><?= $i ?></a>
                            <?php endif; ?>
                        <?php endfor; ?>
                        
                        <?php if ($page < $totalPages): ?>
                            <a href="?status=<?= $status ?>&page=<?= $page + 1 ?>" class="page-link">
                                <i class="bi bi-chevron-right"></i>
                            </a>
                        <?php endif; ?>
                    </div>
                <?php endif; ?>
            <?php else: ?>
                <div class="text-center py-5">
                    <i class="bi bi-file-earmark-text fs-1 text-muted"></i>
                    <h4 class="mt-3">Belum ada invoice</h4>
                    <p class="text-muted">Anda belum memiliki invoice. Buat pesanan terlebih dahulu untuk menghasilkan invoice.</p>
                    <a href="services.php" class="btn btn-gold">Lihat Layanan</a>
                </div>
            <?php endif; ?>
        </div>
    </div>
    
    <!-- Invoice Details Modal -->
    <?php if ($invoiceDetails): ?>
        <?php if (isset($_GET['print']) && $_GET['print'] == 1): ?>
            <!-- Invoice Print View -->
            <div class="invoice-print" id="invoicePrint">
                <div class="invoice-print-header">
                    <div>
                        <img src="https://situneo.my.id/logo" alt="Situneo" class="invoice-print-logo">
                    </div>
                    <div class="invoice-print-details">
                        <div class="invoice-print-title">INVOICE</div>
                        <div class="invoice-print-number">INV-<?= date('Ymd', strtotime($invoiceDetails['created_at'])) ?>-<?= str_pad($invoiceDetails['id'], 4, '0', STR_PAD_LEFT) ?></div>
                        <div class="invoice-print-date"><?= date('d F Y', strtotime($invoiceDetails['created_at'])) ?></div>
                    </div>
                </div>
                
                <div class="row">
                    <div class="col-md-6">
                        <div class="invoice-print-section">
                            <div class="invoice-print-section-title">Dari:</div>
                            <div><strong>SITUNEO DIGITAL</strong></div>
                            <div>Jl. Bekasi Timur IX Dalam No. 27, RT 002/RW 003</div>
                            <div>Kel. Rawa Bunga, Kec. Jatinegara</div>
                            <div>Jakarta Timur 13450, DKI Jakarta</div>
                            <div>Email: info@situneo.my.id</div>
                            <div>Telepon: +62 831-7386-8915</div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="invoice-print-section">
                            <div class="invoice-print-section-title">Untuk:</div>
                            <div><strong><?= htmlspecialchars($invoiceDetails['user_name']) ?></strong></div>
                            <div><?= htmlspecialchars($invoiceDetails['user_email']) ?></div>
                            <div><?= htmlspecialchars($invoiceDetails['user_phone']) ?></div>
                            <div><?= htmlspecialchars($invoiceDetails['user_address']) ?></div>
                        </div>
                    </div>
                </div>
                
                <div class="invoice-print-section">
                    <div class="invoice-print-section-title">Detail Pesanan:</div>
                    <table class="invoice-print-table">
                        <thead>
                            <tr>
                                <th>Layanan</th>
                                <th>No. Pesanan</th>
                                <th>Tanggal</th>
                                <th>Status</th>
                                <th>Harga</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td><?= htmlspecialchars($invoiceDetails['service_name']) ?></td>
                                <td><?= htmlspecialchars($invoiceDetails['order_number']) ?></td>
                                <td><?= date('d F Y', strtotime($invoiceDetails['created_at'])) ?></td>
                                <td><?= ucfirst(htmlspecialchars($invoiceDetails['status'])) ?></td>
                                <td>Rp <?= number_format($invoiceDetails['total_amount'], 0, ',', '.') ?></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                
                <?php if (!empty($invoiceDetails['requirements'])): ?>
                    <div class="invoice-print-section">
                        <div class="invoice-print-section-title">Keperluan:</div>
                        <p><?= nl2br(htmlspecialchars($invoiceDetails['requirements'])) ?></p>
                    </div>
                <?php endif; ?>
                
                <div class="invoice-print-section">
                    <table class="invoice-print-table">
                        <tr>
                            <td colspan="4" class="text-end">Subtotal:</td>
                            <td>Rp <?= number_format($invoiceDetails['total_amount'], 0, ',', '.') ?></td>
                        </tr>
                        <tr>
                            <td colspan="4" class="text-end">PPN (11%):</td>
                            <td>Rp <?= number_format($invoiceDetails['total_amount'] * 0.11, 0, ',', '.') ?></td>
                        </tr>
                        <tr>
                            <td colspan="4" class="invoice-print-total">Total:</td>
                            <td class="invoice-print-total">Rp <?= number_format($invoiceDetails['total_amount'] * 1.11, 0, ',', '.') ?></td>
                        </tr>
                    </table>
                </div>
                
                <div class="invoice-print-footer">
                    <p>Terima kasih telah mempercayakan layanan digital Anda kepada SITUNEO DIGITAL.</p>
                    <p>Invoice ini sah dan telah dibuat secara otomatis oleh sistem.</p>
                </div>
            </div>
            
            <script>
                window.onload = function() {
                    window.print();
                };
            </script>
        <?php else: ?>
            <!-- Invoice Details Modal -->
            <div class="modal fade" id="invoiceDetailsModal" tabindex="-1" aria-labelledby="invoiceDetailsModalLabel" aria-hidden="true">
                <div class="modal-dialog modal-lg">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="invoiceDetailsModalLabel">Detail Invoice</h5>
                            <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                            <div class="row mb-3">
                                <div class="col-md-6">
                                    <p class="text-muted">Nomor Invoice</p>
                                    <h6>INV-<?= date('Ymd', strtotime($invoiceDetails['created_at'])) ?>-<?= str_pad($invoiceDetails['id'], 4, '0', STR_PAD_LEFT) ?></h6>
                                </div>
                                <div class="col-md-6">
                                    <p class="text-muted">Status</p>
                                    <div class="invoice-status status-<?= htmlspecialchars($invoiceDetails['status']) ?>">
                                        <?= ucfirst(htmlspecialchars($invoiceDetails['status'])) ?>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-6">
                                    <p class="text-muted">Tanggal Invoice</p>
                                    <h6><?= date('d M Y, H:i', strtotime($invoiceDetails['created_at'])) ?></h6>
                                </div>
                                <div class="col-md-6">
                                    <p class="text-muted">Total Harga</p>
                                    <h6 class="text-warning">Rp <?= number_format($invoiceDetails['total_amount'], 0, ',', '.') ?></h6>
                                </div>
                            </div>
                            
                            <div class="mb-3">
                                <p class="text-muted">Layanan</p>
                                <div class="d-flex align-items-center">
                                    <?php if ($invoiceDetails['service_image']): ?>
                                        <img src="<?= htmlspecialchars($invoiceDetails['service_image']) ?>" alt="<?= htmlspecialchars($invoiceDetails['service_name']) ?>" class="service-image me-3">
                                    <?php else: ?>
                                        <div class="service-image me-3 d-flex align-items-center justify-content-center bg-secondary">
                                            <i class="bi bi-image fs-4"></i>
                                        </div>
                                    <?php endif; ?>
                                    <div>
                                        <h6><?= htmlspecialchars($invoiceDetails['service_name']) ?></h6>
                                        <p class="text-muted small mb-0"><?= htmlspecialchars($invoiceDetails['service_description']) ?></p>
                                    </div>
                                </div>
                            </div>
                            
                            <?php if (!empty($invoiceDetails['requirements'])): ?>
                                <div class="mb-3">
                                    <p class="text-muted">Keperluan</p>
                                    <p><?= nl2br(htmlspecialchars($invoiceDetails['requirements'])) ?></p>
                                </div>
                            <?php endif; ?>
                            
                            <div class="mb-3">
                                <p class="text-muted">Detail Pembayaran</p>
                                <table class="table table-dark table-striped">
                                    <tr>
                                        <td>Subtotal</td>
                                        <td class="text-end">Rp <?= number_format($invoiceDetails['total_amount'], 0, ',', '.') ?></td>
                                    </tr>
                                    <tr>
                                        <td>PPN (11%)</td>
                                        <td class="text-end">Rp <?= number_format($invoiceDetails['total_amount'] * 0.11, 0, ',', '.') ?></td>
                                    </tr>
                                    <tr>
                                        <td><strong>Total</strong></td>
                                        <td class="text-end"><strong class="text-warning">Rp <?= number_format($invoiceDetails['total_amount'] * 1.11, 0, ',', '.') ?></strong></td>
                                    </tr>
                                </table>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                            <a href="?id=<?= $invoiceDetails['id'] ?>&print=1" class="btn btn-gold" target="_blank">
                                <i class="bi bi-printer me-2"></i>Cetak Invoice
                            </a>
                        </div>
                    </div>
                </div>
            </div>
            
            <script>
                document.addEventListener('DOMContentLoaded', function() {
                    const invoiceDetailsModal = new bootstrap.Modal(document.getElementById('invoiceDetailsModal'));
                    invoiceDetailsModal.show();
                });
            </script>
        <?php endif; ?>
    <?php endif; ?>
    
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
