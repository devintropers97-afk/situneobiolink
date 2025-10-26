<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Admin Services Management
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once '../config.php';

// Require admin role
requireRole(ROLE_ADMIN);

// Get current user
 $user = getCurrentUser();

// Get filter parameters
 $category = $_GET['category'] ?? 'all';
 $status = $_GET['status'] ?? 'all';
 $search = $_GET['search'] ?? '';
 $page = isset($_GET['page']) ? (int)$_GET['page'] : 1;
 $limit = 12;
 $offset = ($page - 1) * $limit;

// Build query based on filters
 $whereClause = "WHERE 1=1";
 $params = [];
 $types = "";

if ($category !== 'all') {
    $whereClause .= " AND category = ?";
    $params[] = $category;
    $types .= "s";
}

if ($status !== 'all') {
    $whereClause .= " AND status = ?";
    $params[] = $status;
    $types .= "s";
}

if (!empty($search)) {
    $whereClause .= " AND (name LIKE ? OR description LIKE ?)";
    $searchParam = "%$search%";
    $params[] = $searchParam;
    $params[] = $searchParam;
    $types .= "ss";
}

// Get total services count
 $countQuery = "SELECT COUNT(*) as total FROM services $whereClause";
 $stmt = $conn->prepare($countQuery);
if (!empty($params)) {
    $stmt->bind_param($types, ...$params);
}
 $stmt->execute();
 $result = $stmt->get_result();
 $totalServices = $result->fetch_assoc()['total'];
 $totalPages = ceil($totalServices / $limit);

// Get services with pagination
 $query = "SELECT * FROM services $whereClause ORDER BY category, name LIMIT $limit OFFSET $offset";
 $stmt = $conn->prepare($query);
if (!empty($params)) {
    $stmt->bind_param($types, ...$params);
}
 $stmt->execute();
 $services = $stmt->get_result();

// Get all categories for filter
 $categoriesQuery = "SELECT DISTINCT category FROM services ORDER BY category";
 $categoriesResult = $conn->query($categoriesQuery);
 $categories = [];
while ($row = $categoriesResult->fetch_assoc()) {
    $categories[] = $row['category'];
}

// Get service details if requested
 $serviceDetails = null;
if (isset($_GET['id'])) {
    $serviceId = $_GET['id'];
    
    $stmt = $conn->prepare("SELECT * FROM services WHERE id = ?");
    $stmt->bind_param("i", $serviceId);
    $stmt->execute();
    $result = $stmt->get_result();
    
    if ($result->num_rows > 0) {
        $serviceDetails = $result->fetch_assoc();
    }
}

// Process service status update
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['update_status'])) {
    $serviceId = $_POST['service_id'];
    $status = $_POST['status'];
    
    $stmt = $conn->prepare("UPDATE services SET status = ?, updated_at = NOW() WHERE id = ?");
    $stmt->bind_param("si", $status, $serviceId);
    
    if ($stmt->execute()) {
        // Log activity
        logActivity($user['id'], "Updated service status to $status for service ID: $serviceId");
        
        header("Location: services.php?success=status_updated");
        exit;
    } else {
        $error = "Terjadi kesalahan saat memperbarui status layanan.";
    }
}

// Process service update
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['update_service'])) {
    $serviceId = $_POST['service_id'];
    $name = $_POST['name'];
    $category = $_POST['category'];
    $description = $_POST['description'];
    $priceStart = $_POST['price_start'];
    $priceUnit = $_POST['price_unit'];
    $image = $_POST['image'];
    $features = $_POST['features'];
    $perfectFor = $_POST['perfect_for'];
    $deliveryTime = $_POST['delivery_time'];
    $status = $_POST['status'];
    
    // Validation
    if (empty($name)) {
        $errors['name'] = 'Nama layanan harus diisi';
    }
    
    if (empty($category)) {
        $errors['category'] = 'Kategori harus diisi';
    }
    
    if (empty($description)) {
        $errors['description'] = 'Deskripsi harus diisi';
    }
    
    if (empty($priceStart)) {
        $errors['price_start'] = 'Harga awal harus diisi';
    }
    
    if (empty($priceUnit)) {
        $errors['price_unit'] = 'Satuan harga harus diisi';
    }
    
    // If no errors, update service
    if (empty($errors)) {
        // Convert features array to JSON
        $featuresArray = is_array($features) ? $features : [];
        $featuresJson = json_encode($featuresArray);
        
        $stmt = $conn->prepare("UPDATE services SET name = ?, category = ?, description = ?, price_start = ?, price_unit = ?, image = ?, features = ?, perfect_for = ?, delivery_time = ?, status = ?, updated_at = NOW() WHERE id = ?");
        $stmt->bind_param("sssissssssi", $name, $category, $description, $priceStart, $priceUnit, $image, $featuresJson, $perfectFor, $deliveryTime, $status, $serviceId);
        
        if ($stmt->execute()) {
            // Log activity
            logActivity($user['id'], "Updated service: $name (ID: $serviceId)");
            
            header("Location: services.php?success=service_updated");
            exit;
        } else {
            $errors['general'] = 'Terjadi kesalahan. Silakan coba lagi.';
        }
    }
}

// Process add service
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['add_service'])) {
    $name = $_POST['name'];
    $category = $_POST['category'];
    $description = $_POST['description'];
    $priceStart = $_POST['price_start'];
    $priceUnit = $_POST['price_unit'];
    $image = $_POST['image'];
    $features = $_POST['features'];
    $perfectFor = $_POST['perfect_for'];
    $deliveryTime = $_POST['delivery_time'];
    $status = $_POST['status'];
    
    // Validation
    if (empty($name)) {
        $errors['name'] = 'Nama layanan harus diisi';
    }
    
    if (empty($category)) {
        $errors['category'] = 'Kategori harus diisi';
    }
    
    if (empty($description)) {
        $errors['description'] = 'Deskripsi harus diisi';
    }
    
    if (empty($priceStart)) {
        $errors['price_start'] = 'Harga awal harus diisi';
    }
    
    if (empty($priceUnit)) {
        $errors['price_unit'] = 'Satuan harga harus diisi';
    }
    
    // If no errors, add service
    if (empty($errors)) {
        // Convert features array to JSON
        $featuresArray = is_array($features) ? $features : [];
        $featuresJson = json_encode($featuresArray);
        
        $stmt = $conn->prepare("INSERT INTO services (name, category, description, price_start, price_unit, image, features, perfect_for, delivery_time, status, created_at) 
                               VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, NOW())");
        $stmt->bind_param("sssisssssss", $name, $category, $description, $priceStart, $priceUnit, $image, $featuresJson, $perfectFor, $deliveryTime, $status);
        
        if ($stmt->execute()) {
            $serviceId = $conn->insert_id;
            
            // Log activity
            logActivity($user['id'], "Added new service: $name (ID: $serviceId)");
            
            header("Location: services.php?success=service_added");
            exit;
        } else {
            $errors['general'] = 'Terjadi kesalahan. Silakan coba lagi.';
        }
    }
}

// If there are errors, redirect back with error messages
if (!empty($errors)) {
    $_SESSION['errors'] = $errors;
    $_SESSION['form_data'] = $_POST;
    header("Location: services.php?error=form_failed");
    exit;
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kelola Layanan - SITUNEO DIGITAL</title>
    <meta name="description" content="Kelola layanan SITUNEO DIGITAL">
    
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
        
        /* Service Card */
        .service-card {
            background: linear-gradient(135deg, rgba(30, 92, 153, 0.1) 0%, rgba(15, 48, 87, 0.2) 100%);
            border: 1px solid rgba(255, 180, 0, 0.2);
            border-radius: 20px;
            overflow: hidden;
            height: 100%;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
        }
        
        .service-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(255, 180, 0, 0.3);
            border-color: var(--gold);
        }
        
        .service-image {
            position: relative;
            height: 200px;
            overflow: hidden;
        }
        
        .service-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s;
        }
        
        .service-card:hover .service-image img {
            transform: scale(1.1);
        }
        
        .service-icon-badge {
            position: absolute;
            top: 15px;
            left: 15px;
            width: 60px;
            height: 60px;
            background: var(--gradient-gold);
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }
        
        .service-icon-badge i {
            font-size: 1.5rem;
            color: var(--dark-blue);
        }
        
        .price-badge {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(255,0,0,0.9);
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: 700;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { box-shadow: 0 0 0 0 rgba(255, 0, 0, 0.7); }
            50% { box-shadow: 0 0 0 10px rgba(255, 0, 0, 0); }
        }
        
        .service-content {
            padding: 1.5rem;
        }
        
        .category-badge {
            display: inline-block;
            background: rgba(255,180,0,0.2);
            border: 1px solid var(--gold);
            color: var(--gold);
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            margin-bottom: 1rem;
        }
        
        .service-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.2rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
        }
        
                .service-description {
            color: var(--text-light);
            font-size: 0.9rem;
            margin-bottom: 1rem;
            display: -webkit-box;
            -webkit-line-clamp: 3;
            -webkit-box-orient: vertical;
            overflow: hidden;
        }
        
        .service-price {
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--gold);
            margin-bottom: 1rem;
        }
        
        .service-status {
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            margin-bottom: 1rem;
            display: inline-block;
        }
        
        .status-active {
            background: rgba(25, 135, 84, 0.2);
            color: #198754;
        }
        
        .status-inactive {
            background: rgba(220, 53, 69, 0.2);
            color: #dc3545;
        }
        
        .service-actions {
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
        
        /* Service Details Modal */
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
        
        /* Feature List */
        .feature-list {
            list-style: none;
            padding: 0;
            margin: 1rem 0;
        }
        
        .feature-item {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .feature-item input {
            margin-right: 10px;
        }
        
        .feature-item button {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            color: var(--white);
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 0.8rem;
            transition: all 0.3s;
        }
        
        .feature-item button:hover {
            background: rgba(255, 180, 0, 0.2);
            border-color: var(--gold);
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
        <a href="orders.php" class="sidebar-item">
            <i class="bi bi-receipt"></i> Semua Pesanan
        </a>
        <a href="services.php" class="sidebar-item active">
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
                    <i class="bi bi-check-circle-fill me-2"></i>Status layanan berhasil diperbarui!
                </div>
            <?php endif; ?>
            
            <?php if (isset($_GET['success']) && $_GET['success'] === 'service_updated'): ?>
                <div class="alert alert-success">
                    <i class="bi bi-check-circle-fill me-2"></i>Layanan berhasil diperbarui!
                </div>
            <?php endif; ?>
            
            <?php if (isset($_GET['success']) && $_GET['success'] === 'service_added'): ?>
                <div class="alert alert-success">
                    <i class="bi bi-check-circle-fill me-2"></i>Layanan berhasil ditambahkan!
                </div>
            <?php endif; ?>
            
            <?php if (isset($error)): ?>
                <div class="alert alert-danger">
                    <i class="bi bi-exclamation-triangle-fill me-2"></i><?= $error ?>
                </div>
            <?php endif; ?>
            
            <div class="d-flex justify-content-between align-items-center mb-4">
                <h1 class="mb-0">Kelola Layanan</h1>
                <div class="d-flex gap-2">
                    <button class="btn btn-gold" data-bs-toggle="modal" data-bs-target="#addServiceModal">
                        <i class="bi bi-plus-circle me-2"></i>Tambah Layanan
                    </button>
                    <a href="reports.php?export=services" class="btn btn-outline-gold">
                        <i class="bi bi-download me-2"></i>Export
                    </a>
                </div>
            </div>
            
            <!-- Search Form -->
            <form class="search-form" method="get" action="">
                <input type="text" class="search-input" name="search" placeholder="Cari nama atau deskripsi layanan..." value="<?= htmlspecialchars($search) ?>">
                <button type="submit" class="search-btn">
                    <i class="bi bi-search"></i>
                </button>
            </form>
            
            <!-- Filter Tabs -->
            <div class="filter-tabs">
                <div>
                    <span class="me-2">Kategori:</span>
                    <a href="?category=all&status=<?= $status ?>&search=<?= $search ?>" class="filter-tab <?= $category === 'all' ? 'active' : '' ?>">
                        Semua
                    </a>
                    <?php foreach ($categories as $cat): ?>
                        <a href="?category=<?= urlencode($cat) ?>&status=<?= $status ?>&search=<?= $search ?>" class="filter-tab <?= $category === $cat ? 'active' : '' ?>">
                            <?= htmlspecialchars($cat) ?>
                        </a>
                    <?php endforeach; ?>
                </div>
                
                <div>
                    <span class="me-2">Status:</span>
                    <a href="?status=all&category=<?= $category ?>&search=<?= $search ?>" class="filter-tab <?= $status === 'all' ? 'active' : '' ?>">
                        Semua
                    </a>
                    <a href="?status=active&category=<?= $category ?>&search=<?= $search ?>" class="filter-tab <?= $status === 'active' ? 'active' : '' ?>">
                        Aktif
                    </a>
                    <a href="?status=inactive&category=<?= $category ?>&search=<?= $search ?>" class="filter-tab <?= $status === 'inactive' ? 'active' : '' ?>">
                        Tidak Aktif
                    </a>
                </div>
            </div>
            
            <!-- Services List -->
            <?php if ($services->num_rows > 0): ?>
                <div class="row g-4">
                    <?php while ($service = $services->fetch_assoc()): ?>
                        <div class="col-lg-4 col-md-6" data-aos="fade-up">
                            <div class="service-card">
                                <div class="service-image">
                                    <img src="<?= htmlspecialchars($service['image']) ?>" alt="<?= htmlspecialchars($service['name']) ?>">
                                    <div class="service-icon-badge">
                                        <i class="bi bi-<?= htmlspecialchars($service['icon']) ?>"></i>
                                    </div>
                                    <div class="price-badge">
                                        Rp <?= number_format($service['price_start'], 0, ',', '.') ?>
                                    </div>
                                </div>
                                <div class="service-content">
                                    <div class="category-badge"><?= htmlspecialchars($service['category']) ?></div>
                                    <h3 class="service-title"><?= htmlspecialchars($service['name']) ?></h3>
                                    <p class="service-description"><?= htmlspecialchars($service['description']) ?></p>
                                    <div class="service-price">
                                        Rp <?= number_format($service['price_start'], 0, ',', '.') ?> / <?= htmlspecialchars($service['price_unit']) ?>
                                    </div>
                                    <div class="service-status status-<?= htmlspecialchars($service['status']) ?>">
                                        <?= $service['status'] === 'active' ? 'Aktif' : 'Tidak Aktif' ?>
                                    </div>
                                    <div class="service-actions">
                                        <a href="?id=<?= $service['id'] ?>" class="btn-outline-gold">
                                            <i class="bi bi-eye me-1"></i> Detail
                                        </a>
                                        <button class="btn-outline-gold" data-bs-toggle="dropdown">
                                            <i class="bi bi-gear me-1"></i> Kelola
                                        </button>
                                        <ul class="dropdown-menu dropdown-menu-end">
                                            <li>
                                                <form method="post" action="">
                                                    <input type="hidden" name="service_id" value="<?= $service['id'] ?>">
                                                    <input type="hidden" name="update_status" value="1">
                                                    
                                                    <?php if ($service['status'] === 'active'): ?>
                                                        <button type="submit" name="status" value="inactive" class="dropdown-item">
                                                            <i class="bi bi-pause-circle me-2"></i>Nonaktifkan
                                                        </button>
                                                    <?php else: ?>
                                                        <button type="submit" name="status" value="active" class="dropdown-item">
                                                            <i class="bi bi-play-circle me-2"></i>Aktifkan
                                                        </button>
                                                    <?php endif; ?>
                                                </form>
                                            </li>
                                        </ul>
                                    </div>
                                </div>
                            </div>
                        </div>
                    <?php endwhile; ?>
                </div>
                
                <!-- Pagination -->
                <?php if ($totalPages > 1): ?>
                    <div class="pagination">
                        <?php if ($page > 1): ?>
                            <a href="?category=<?= $category ?>&status=<?= $status ?>&search=<?= $search ?>&page=<?= $page - 1 ?>" class="page-link">
                                <i class="bi bi-chevron-left"></i>
                            </a>
                        <?php endif; ?>
                        
                        <?php for ($i = 1; $i <= $totalPages; $i++): ?>
                            <?php if ($i == $page): ?>
                                <span class="page-link active"><?= $i ?></span>
                            <?php else: ?>
                                <a href="?category=<?= $category ?>&status=<?= $status ?>&search=<?= $search ?>&page=<?= $i ?>" class="page-link"><?= $i ?></a>
                            <?php endif; ?>
                        <?php endfor; ?>
                        
                        <?php if ($page < $totalPages): ?>
                            <a href="?category=<?= $category ?>&status=<?= $status ?>&search=<?= $search ?>&page=<?= $page + 1 ?>" class="page-link">
                                <i class="bi bi-chevron-right"></i>
                            </a>
                        <?php endif; ?>
                    </div>
                <?php endif; ?>
            <?php else: ?>
                <div class="text-center py-5">
                    <i class="bi bi-gear fs-1 text-muted"></i>
                    <h4 class="mt-3">Tidak ada layanan</h4>
                    <p class="text-muted">Tidak ada layanan yang ditemukan dengan filter ini.</p>
                </div>
            <?php endif; ?>
        </div>
    </div>
    
    <!-- Service Details Modal -->
    <?php if ($serviceDetails): ?>
        <div class="modal fade" id="serviceDetailsModal" tabindex="-1" aria-labelledby="serviceDetailsModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="serviceDetailsModalLabel">Detail Layanan</h5>
                        <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="row mb-3">
                            <div class="col-md-6">
                                <img src="<?= htmlspecialchars($serviceDetails['image']) ?>" alt="<?= htmlspecialchars($serviceDetails['name']) ?>" class="img-fluid rounded">
                            </div>
                            <div class="col-md-6">
                                <div class="category-badge mb-3"><?= htmlspecialchars($serviceDetails['category']) ?></div>
                                <h4 class="text-warning mb-3">
                                    Rp <?= number_format($serviceDetails['price_start'], 0, ',', '.') ?> / <?= htmlspecialchars($serviceDetails['price_unit']) ?>
                                </h4>
                                <p><?= htmlspecialchars($serviceDetails['description']) ?></p>
                                
                                <?php if (!empty($serviceDetails['perfect_for'])): ?>
                                    <h6 class="mt-4">Cocok Untuk:</h6>
                                    <p><?= htmlspecialchars($serviceDetails['perfect_for']) ?></p>
                                <?php endif; ?>
                                
                                <?php if (!empty($serviceDetails['delivery_time'])): ?>
                                    <h6 class="mt-4">Waktu Pengerjaan:</h6>
                                    <p><?= htmlspecialchars($serviceDetails['delivery_time']) ?></p>
                                <?php endif; ?>
                                
                                <div class="service-status status-<?= htmlspecialchars($serviceDetails['status']) ?> mt-3">
                                    <?= $serviceDetails['status'] === 'active' ? 'Aktif' : 'Tidak Aktif' ?>
                                </div>
                            </div>
                        </div>
                        
                        <?php if (!empty($serviceDetails['features'])): ?>
                            <h6 class="mt-4">Fitur:</h6>
                            <ul class="feature-list">
                                <?php 
                                $features = json_decode($serviceDetails['features'], true);
                                if (is_array($features)) {
                                    foreach ($features as $feature) {
                                        echo '<li><i class="bi bi-check-circle-fill text-warning me-2"></i>' . htmlspecialchars($feature) . '</li>';
                                    }
                                }
                                ?>
                            </ul>
                        <?php endif; ?>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                        <button class="btn btn-gold" data-bs-toggle="modal" data-bs-target="#editServiceModal">
                            <i class="bi bi-pencil me-2"></i>Edit Layanan
                        </button>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Edit Service Modal -->
        <div class="modal fade" id="editServiceModal" tabindex="-1" aria-labelledby="editServiceModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="editServiceModalLabel">Edit Layanan</h5>
                        <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <form method="post" action="">
                        <div class="modal-body">
                            <input type="hidden" name="service_id" value="<?= $serviceDetails['id'] ?>">
                            <input type="hidden" name="update_service" value="1">
                            
                            <div class="row">
                                <div class="col-md-6 mb-3">
                                    <label for="name" class="form-label">Nama Layanan</label>
                                    <input type="text" class="form-control" id="name" name="name" value="<?= htmlspecialchars($serviceDetails['name']) ?>" required>
                                </div>
                                <div class="col-md-6 mb-3">
                                    <label for="category" class="form-label">Kategori</label>
                                    <select class="form-select" id="category" name="category" required>
                                        <?php foreach ($categories as $cat): ?>
                                            <option value="<?= htmlspecialchars($cat) ?>" <?= $serviceDetails['category'] === $cat ? 'selected' : '' ?>>
                                                <?= htmlspecialchars($cat) ?>
                                            </option>
                                        <?php endforeach; ?>
                                    </select>
                                </div>
                            </div>
                            
                            <div class="mb-3">
                                <label for="description" class="form-label">Deskripsi</label>
                                <textarea class="form-control" id="description" name="description" rows="3" required><?= htmlspecialchars($serviceDetails['description']) ?></textarea>
                            </div>
                            
                            <div class="row">
                                <div class="col-md-6 mb-3">
                                    <label for="price_start" class="form-label">Harga Awal</label>
                                    <div class="input-group">
                                        <span class="input-group-text">Rp</span>
                                        <input type="number" class="form-control" id="price_start" name="price_start" value="<?= $serviceDetails['price_start'] ?>" required>
                                    </div>
                                </div>
                                <div class="col-md-6 mb-3">
                                    <label for="price_unit" class="form-label">Satuan Harga</label>
                                    <input type="text" class="form-control" id="price_unit" name="price_unit" value="<?= htmlspecialchars($serviceDetails['price_unit']) ?>" required>
                                </div>
                            </div>
                            
                            <div class="mb-3">
                                <label for="image" class="form-label">URL Gambar</label>
                                <input type="text" class="form-control" id="image" name="image" value="<?= htmlspecialchars($serviceDetails['image']) ?>">
                            </div>
                            
                            <div class="mb-3">
                                <label for="perfect_for" class="form-label">Cocok Untuk</label>
                                <textarea class="form-control" id="perfect_for" name="perfect_for" rows="2"><?= htmlspecialchars($serviceDetails['perfect_for']) ?></textarea>
                            </div>
                            
                            <div class="mb-3">
                                <label for="delivery_time" class="form-label">Waktu Pengerjaan</label>
                                <input type="text" class="form-control" id="delivery_time" name="delivery_time" value="<?= htmlspecialchars($serviceDetails['delivery_time']) ?>">
                            </div>
                            
                            <div class="mb-3">
                                <label for="status" class="form-label">Status</label>
                                <select class="form-select" id="status" name="status" required>
                                    <option value="active" <?= $serviceDetails['status'] === 'active' ? 'selected' : '' ?>>Aktif</option>
                                    <option value="inactive" <?= $serviceDetails['status'] === 'inactive' ? 'selected' : '' ?>>Tidak Aktif</option>
                                </select>
                            </div>
                            
                            <div class="mb-3">
                                <label class="form-label">Fitur</label>
                                <div id="featuresContainer">
                                    <?php 
                                    $features = json_decode($serviceDetails['features'], true);
                                    if (is_array($features)) {
                                        foreach ($features as $index => $feature) {
                                            echo '<div class="feature-item">';
                                            echo '<input type="text" class="form-control" name="features[]" value="' . htmlspecialchars($feature) . '">';
                                            echo '<button type="button" class="btn btn-sm btn-outline-danger" onclick="removeFeature(this)">Hapus</button>';
                                            echo '</div>';
                                        }
                                    }
                                    ?>
                                </div>
                                <button type="button" class="btn btn-outline-gold btn-sm" onclick="addFeature()">
                                    <i class="bi bi-plus-circle me-1"></i>Tambah Fitur
                                </button>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
                            <button type="submit" class="btn btn-gold">Simpan Perubahan</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
        
        <script>
            document.addEventListener('DOMContentLoaded', function() {
                const serviceDetailsModal = new bootstrap.Modal(document.getElementById('serviceDetailsModal'));
                serviceDetailsModal.show();
            });
            
            // When edit button is clicked, hide details modal and show edit modal
            document.querySelector('[data-bs-target="#editServiceModal"]').addEventListener('click', function() {
                const serviceDetailsModal = bootstrap.Modal.getInstance(document.getElementById('serviceDetailsModal'));
                serviceDetailsModal.hide();
                
                setTimeout(() => {
                    const editServiceModal = new bootstrap.Modal(document.getElementById('editServiceModal'));
                    editServiceModal.show();
                }, 300);
            });
        });
        
        function addFeature() {
            const container = document.getElementById('featuresContainer');
            const featureItem = document.createElement('div');
            featureItem.className = 'feature-item';
            featureItem.innerHTML = `
                <input type="text" class="form-control" name="features[]" placeholder="Masukkan fitur...">
                <button type="button" class="btn btn-sm btn-outline-danger" onclick="removeFeature(this)">Hapus</button>
            `;
            container.appendChild(featureItem);
        }
        
        function removeFeature(button) {
            button.parentElement.remove();
        }
        </script>
    <?php endif; ?>
    
    <!-- Add Service Modal -->
    <div class="modal fade" id="addServiceModal" tabindex="-1" aria-labelledby="addServiceModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="addServiceModalLabel">Tambah Layanan Baru</h5>
                    <button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <form method="post" action="">
                    <div class="modal-body">
                        <input type="hidden" name="add_service" value="1">
                        
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label for="name" class="form-label">Nama Layanan</label>
                                <input type="text" class="form-control" id="name" name="name" required>
                            </div>
                            <div class="col-md-6 mb-3">
                                <label for="category" class="form-label">Kategori</label>
                                <select class="form-select" id="category" name="category" required>
                                    <?php foreach ($categories as $cat): ?>
                                        <option value="<?= htmlspecialchars($cat) ?>"><?= htmlspecialchars($cat) ?></option>
                                    <?php endforeach; ?>
                                </select>
                            </div>
                        </div>
                        
                        <div class="mb-3">
                            <label for="description" class="form-label">Deskripsi</label>
                            <textarea class="form-control" id="description" name="description" rows="3" required></textarea>
                        </div>
                        
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label for="price_start" class="form-label">Harga Awal</label>
                                <div class="input-group">
                                    <span class="input-group-text">Rp</span>
                                    <input type="number" class="form-control" id="price_start" name="price_start" required>
                                </div>
                            </div>
                            <div class="col-md-6 mb-3">
                                <label for="price_unit" class="form-label">Satuan Harga</label>
                                <input type="text" class="form-control" id="price_unit" name="price_unit" required>
                            </div>
                        </div>
                        
                        <div class="mb-3">
                            <label for="image" class="form-label">URL Gambar</label>
                            <input type="text" class="form-control" id="image" name="image" placeholder="https://example.com/image.jpg">
                        </div>
                        
                        <div class="mb-3">
                            <label for="perfect_for" class="form-label">Cocok Untuk</label>
                            <textarea class="form-control" id="perfect_for" name="perfect_for" rows="2"></textarea>
                        </div>
                        
                        <div class="mb-3">
                            <label for="delivery_time" class="form-label">Waktu Pengerjaan</label>
                            <input type="text" class="form-control" id="delivery_time" name="delivery_time">
                        </div>
                        
                        <div class="mb-3">
                            <label for="status" class="form-label">Status</label>
                            <select class="form-select" id="status" name="status" required>
                                <option value="active">Aktif</option>
                                <option value="inactive">Tidak Aktif</option>
                            </select>
                        </div>
                        
                        <div class="mb-3">
                            <label class="form-label">Fitur</label>
                            <div id="newFeaturesContainer">
                                <div class="feature-item">
                                    <input type="text" class="form-control" name="features[]" placeholder="Masukkan fitur...">
                                    <button type="button" class="btn btn-sm btn-outline-danger" onclick="removeFeature(this)">Hapus</button>
                                </div>
                            </div>
                            <button type="button" class="btn btn-outline-gold btn-sm" onclick="addNewFeature()">
                                <i class="bi bi-plus-circle me-1"></i>Tambah Fitur
                            </button>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
                        <button type="submit" class="btn btn-gold">Tambah Layanan</button>
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
        
        // Feature management functions
        function addNewFeature() {
            const container = document.getElementById('newFeaturesContainer');
            const featureItem = document.createElement('div');
            featureItem.className = 'feature-item';
            featureItem.innerHTML = `
                <input type="text" class="form-control" name="features[]" placeholder="Masukkan fitur...">
                <button type="button" class="btn btn-sm btn-outline-danger" onclick="removeFeature(this)">Hapus</button>
            `;
            container.appendChild(featureItem);
        }
        
        function removeFeature(button) {
            button.parentElement.remove();
        }
    </script>
</body>
</html>
