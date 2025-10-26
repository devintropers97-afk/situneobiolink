<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Admin Dashboard
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once '../config.php';

// Require admin role
requireRole(ROLE_ADMIN);

// Get current user
 $user = getCurrentUser();

// Get statistics
 $total_users = 0;
 $total_services = 26; // Based on services.php
 $total_projects = 0;
 $total_inquiries = 0;

// Count total users
 $stmt = $conn->prepare("SELECT COUNT(*) as count FROM users");
 $stmt->execute();
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    $row = $result->fetch_assoc();
    $total_users = $row['count'];
}

// Count total projects
 $stmt = $conn->prepare("SELECT COUNT(*) as count FROM projects");
 $stmt->execute();
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    $row = $result->fetch_assoc();
    $total_projects = $row['count'];
}

// Count total inquiries
 $stmt = $conn->prepare("SELECT COUNT(*) as count FROM inquiries");
 $stmt->execute();
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    $row = $result->fetch_assoc();
    $total_inquiries = $row['count'];
}

// Get recent users
 $recent_users = [];
 $stmt = $conn->prepare("SELECT id, name, email, created_at FROM users ORDER BY created_at DESC LIMIT 5");
 $stmt->execute();
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    while ($row = $result->fetch_assoc()) {
        $recent_users[] = $row;
    }
}

// Get recent inquiries
 $recent_inquiries = [];
 $stmt = $conn->prepare("SELECT id, name, email, subject, created_at FROM inquiries ORDER BY created_at DESC LIMIT 5");
 $stmt->execute();
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    while ($row = $result->fetch_assoc()) {
        $recent_inquiries[] = $row;
    }
}

// Get recent activities
 $recent_activities = [];
 $stmt = $conn->prepare("SELECT a.*, u.name as user_name FROM activity_logs a JOIN users u ON a.user_id = u.id ORDER BY a.created_at DESC LIMIT 10");
 $stmt->execute();
 $result = $stmt->get_result();
if ($result->num_rows > 0) {
    while ($row = $result->fetch_assoc()) {
        $recent_activities[] = $row;
    }
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Dashboard - SITUNEO DIGITAL</title>
    <meta name="description" content="Admin Dashboard SITUNEO DIGITAL">
    
    <!-- Favicon -->
    <link rel="icon" type="image/png" href="https://situneo.my.id/logo">
    
    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=Plus+Jakarta+Sans:wght@400;600;700;800;900&display=swap" rel="stylesheet">
    
    <!-- CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css" rel="stylesheet">
    
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
        
        .sidebar {
            position: fixed;
            top: 0;
            left: 0;
            height: 100vh;
            width: 250px;
            background: var(--gradient-primary);
            padding: 20px;
            z-index: 1000;
            overflow-y: auto;
            transition: all 0.3s;
        }
        
        .sidebar-header {
            display: flex;
            align-items: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .sidebar-logo {
            width: 40px;
            height: 40px;
            border-radius: 10px;
            margin-right: 15px;
        }
        
        .sidebar-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--white);
        }
        
        .sidebar-menu {
            list-style: none;
            padding: 0;
        }
        
        .sidebar-menu li {
            margin-bottom: 5px;
        }
        
        .sidebar-menu a {
            display: flex;
            align-items: center;
            padding: 12px 15px;
            color: rgba(255, 255, 255, 0.8);
            text-decoration: none;
            border-radius: 10px;
            transition: all 0.3s;
        }
        
        .sidebar-menu a:hover, .sidebar-menu a.active {
            background: rgba(255, 255, 255, 0.1);
            color: var(--white);
        }
        
        .sidebar-menu i {
            margin-right: 10px;
            font-size: 1.2rem;
        }
        
        .main-content {
            margin-left: 250px;
            padding: 20px;
            min-height: 100vh;
        }
        
        .top-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .page-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.8rem;
            font-weight: 700;
            background: var(--gradient-gold);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .user-info {
            display: flex;
            align-items: center;
        }
        
        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            margin-right: 15px;
            object-fit: cover;
        }
        
        .stats-card {
            background: rgba(15, 48, 87, 0.6);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 25px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s;
        }
        
        .stats-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
        }
        
        .stats-icon {
            width: 60px;
            height: 60px;
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            margin-bottom: 15px;
        }
        
        .stats-icon.users {
            background: rgba(52, 152, 219, 0.2);
            color: #3498db;
        }
        
        .stats-icon.services {
            background: rgba(46, 204, 113, 0.2);
            color: #2ecc71;
        }
        
        .stats-icon.projects {
            background: rgba(155, 89, 182, 0.2);
            color: #9b59b6;
        }
        
        .stats-icon.inquiries {
            background: rgba(241, 196, 15, 0.2);
            color: #f1c40f;
        }
        
        .stats-value {
            font-size: 2rem;
            font-weight: 700;
            margin-bottom: 5px;
        }
        
        .stats-label {
            color: rgba(255, 255, 255, 0.7);
            font-size: 0.9rem;
        }
        
        .card {
            background: rgba(15, 48, 87, 0.6);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            margin-bottom: 25px;
        }
        
        .card-header {
            background: rgba(15, 48, 87, 0.8);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 15px 15px 0 0 !important;
            padding: 20px;
        }
        
        .card-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.2rem;
            font-weight: 700;
            margin: 0;
            color: var(--white);
        }
        
        .card-body {
            padding: 20px;
        }
        
        .table {
            color: var(--white);
        }
        
        .table th {
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            color: rgba(255, 255, 255, 0.7);
            font-weight: 600;
        }
        
        .table td {
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
            vertical-align: middle;
        }
        
        .activity-item {
            display: flex;
            align-items: flex-start;
            padding: 15px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
        }
        
        .activity-item:last-child {
            border-bottom: none;
        }
        
        .activity-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            flex-shrink: 0;
        }
        
        .activity-icon.login {
            background: rgba(52, 152, 219, 0.2);
            color: #3498db;
        }
        
        .activity-icon.register {
            background: rgba(46, 204, 113, 0.2);
            color: #2ecc71;
        }
        
        .activity-icon.logout {
            background: rgba(231, 76, 60, 0.2);
            color: #e74c3c;
        }
        
        .activity-icon.email {
            background: rgba(241, 196, 15, 0.2);
            color: #f1c40f;
        }
        
        .activity-content {
            flex: 1;
        }
        
        .activity-title {
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .activity-time {
            font-size: 0.8rem;
            color: rgba(255, 255, 255, 0.5);
        }
        
        .btn-view-all {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 8px 20px;
            font-weight: 600;
            border-radius: 50px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            transition: all 0.3s;
            text-decoration: none;
            display: inline-block;
        }
        
        .btn-view-all:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.3);
            color: var(--dark-blue);
        }
        
        .mobile-toggle {
            display: none;
            position: fixed;
            top: 20px;
            left: 20px;
            z-index: 1001;
            background: var(--primary-blue);
            color: var(--white);
            border: none;
            border-radius: 10px;
            padding: 10px;
            font-size: 1.5rem;
        }
        
        @media (max-width: 992px) {
            .sidebar {
                transform: translateX(-100%);
            }
            
            .sidebar.active {
                transform: translateX(0);
            }
            
            .main-content {
                margin-left: 0;
            }
            
            .mobile-toggle {
                display: block;
            }
        }
    </style>
</head>
<body>
    <button class="mobile-toggle" id="mobileToggle">
        <i class="bi bi-list"></i>
    </button>
    
    <div class="sidebar" id="sidebar">
        <div class="sidebar-header">
            <img src="https://situneo.my.id/logo" alt="Situneo" class="sidebar-logo">
            <h2 class="sidebar-title">SITUNEO DIGITAL</h2>
        </div>
        
        <ul class="sidebar-menu">
            <li>
                <a href="dashboard.php" class="active">
                    <i class="bi bi-speedometer2"></i>
                    Dashboard
                </a>
            </li>
            <li>
                <a href="users.php">
                    <i class="bi bi-people"></i>
                    Pengguna
                </a>
            </li>
            <li>
                <a href="services.php">
                    <i class="bi bi-grid-3x3-gap"></i>
                    Layanan
                </a>
            </li>
            <li>
                <a href="projects.php">
                    <i class="bi bi-folder"></i>
                    Proyek
                </a>
            </li>
            <li>
                <a href="inquiries.php">
                    <i class="bi bi-envelope"></i>
                    Pertanyaan
                </a>
            </li>
            <li>
                <a href="settings.php">
                    <i class="bi bi-gear"></i>
                    Pengaturan
                </a>
            </li>
            <li>
                <a href="../logout.php">
                    <i class="bi bi-box-arrow-right"></i>
                    Keluar
                </a>
            </li>
        </ul>
    </div>
    
    <div class="main-content">
        <div class="top-header">
            <h1 class="page-title">Dashboard Admin</h1>
            
            <div class="user-info">
                <div class="dropdown">
                    <button class="btn btn-link dropdown-toggle d-flex align-items-center" type="button" id="userDropdown" data-bs-toggle="dropdown" aria-expanded="false">
                        <img src="https://situneo.my.id/logo" alt="User" class="user-avatar">
                        <span class="ms-2"><?= htmlspecialchars($user['name']) ?></span>
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
        
        <div class="row">
            <div class="col-md-6 col-lg-3">
                <div class="stats-card">
                    <div class="stats-icon users">
                        <i class="bi bi-people-fill"></i>
                    </div>
                    <div class="stats-value"><?= $total_users ?></div>
                    <div class="stats-label">Total Pengguna</div>
                </div>
            </div>
            
            <div class="col-md-6 col-lg-3">
                <div class="stats-card">
                    <div class="stats-icon services">
                        <i class="bi bi-grid-3x3-gap-fill"></i>
                    </div>
                    <div class="stats-value"><?= $total_services ?></div>
                    <div class="stats-label">Total Layanan</div>
                </div>
            </div>
            
            <div class="col-md-6 col-lg-3">
                <div class="stats-card">
                    <div class="stats-icon projects">
                        <i class="bi bi-folder-fill"></i>
                    </div>
                    <div class="stats-value"><?= $total_projects ?></div>
                    <div class="stats-label">Total Proyek</div>
                </div>
            </div>
            
            <div class="col-md-6 col-lg-3">
                <div class="stats-card">
                    <div class="stats-icon inquiries">
                        <i class="bi bi-envelope-fill"></i>
                    </div>
                    <div class="stats-value"><?= $total_inquiries ?></div>
                    <div class="stats-label">Total Pertanyaan</div>
                </div>
            </div>
        </div>
        
        <div class="row">
            <div class="col-lg-6">
                <div class="card">
                    <div class="card-header d-flex justify-content-between align-items-center">
                        <h5 class="card-title">Pengguna Baru</h5>
                        <a href="users.php" class="btn-view-all">Lihat Semua</a>
                    </div>
                    <div class="card-body">
                        <?php if (empty($recent_users)): ?>
                            <p class="text-center py-3">Belum ada pengguna baru</p>
                        <?php else: ?>
                            <div class="table-responsive">
                                <table class="table table-hover">
                                    <thead>
                                        <tr>
                                            <th>Nama</th>
                                            <th>Email</th>
                                            <th>Tanggal</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <?php foreach ($recent_users as $user): ?>
                                            <tr>
                                                <td><?= htmlspecialchars($user['name']) ?></td>
                                                <td><?= htmlspecialchars($user['email']) ?></td>
                                                <td><?= date('d M Y', strtotime($user['created_at'])) ?></td>
                                            </tr>
                                        <?php endforeach; ?>
                                    </tbody>
                                </table>
                            </div>
                        <?php endif; ?>
                    </div>
                </div>
            </div>
            
            <div class="col-lg-6">
                <div class="card">
                    <div class="card-header d-flex justify-content-between align-items-center">
                        <h5 class="card-title">Pertanyaan Terbaru</h5>
                        <a href="inquiries.php" class="btn-view-all">Lihat Semua</a>
                    </div>
                    <div class="card-body">
                        <?php if (empty($recent_inquiries)): ?>
                            <p class="text-center py-3">Belum ada pertanyaan baru</p>
                        <?php else: ?>
                            <div class="table-responsive">
                                <table class="table table-hover">
                                    <thead>
                                        <tr>
                                            <th>Nama</th>
                                            <th>Subjek</th>
                                            <th>Tanggal</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <?php foreach ($recent_inquiries as $inquiry): ?>
                                            <tr>
                                                <td><?= htmlspecialchars($inquiry['name']) ?></td>
                                                <td><?= htmlspecialchars($inquiry['subject']) ?></td>
                                                <td><?= date('d M Y', strtotime($inquiry['created_at'])) ?></td>
                                            </tr>
                                        <?php endforeach; ?>
                                    </tbody>
                                </table>
                            </div>
                        <?php endif; ?>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="card">
            <div class="card-header d-flex justify-content-between align-items-center">
                <h5 class="card-title">Aktivitas Terbaru</h5>
                <a href="activities.php" class="btn-view-all">Lihat Semua</a>
            </div>
            <div class="card-body">
                <?php if (empty($recent_activities)): ?>
                    <p class="text-center py-3">Belum ada aktivitas</p>
                <?php else: ?>
                    <?php foreach ($recent_activities as $activity): ?>
                        <div class="activity-item">
                            <div class="activity-icon <?= getActivityClass($activity['action']) ?>">
                                <i class="bi <?= getActivityIcon($activity['action']) ?>"></i>
                            </div>
                            <div class="activity-content">
                                <div class="activity-title">
                                    <?= htmlspecialchars($activity['user_name']) ?> - <?= formatActivity($activity['action']) ?>
                                </div>
                                <div class="activity-time">
                                    <?= formatTime($activity['created_at']) ?>
                                </div>
                            </div>
                        </div>
                    <?php endforeach; ?>
                <?php endif; ?>
            </div>
        </div>
    </div>
    
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        // Mobile sidebar toggle
        document.getElementById('mobileToggle').addEventListener('click', function() {
            document.getElementById('sidebar').classList.toggle('active');
        });
        
        // Close sidebar when clicking outside on mobile
        document.addEventListener('click', function(event) {
            const sidebar = document.getElementById('sidebar');
            const toggle = document.getElementById('mobileToggle');
            
            if (window.innerWidth <= 992 && 
                !sidebar.contains(event.target) && 
                !toggle.contains(event.target) && 
                sidebar.classList.contains('active')) {
                sidebar.classList.remove('active');
            }
        });
        
        // Auto-refresh dashboard data every 30 seconds
        setInterval(function() {
            // In a real implementation, you would use AJAX to fetch updated data
            // For now, we'll just reload the page
            location.reload();
        }, 30000);
    </script>
</body>
</html>

<?php
// Helper functions for activity display
function getActivityClass($action) {
    switch ($action) {
        case 'User logged in':
            return 'login';
        case 'User registered':
            return 'register';
        case 'User logged out':
            return 'logout';
        case 'Email verified':
            return 'email';
        default:
            return 'login';
    }
}

function getActivityIcon($action) {
    switch ($action) {
        case 'User logged in':
            return 'bi-box-arrow-in-right';
        case 'User registered':
            return 'bi-person-plus';
        case 'User logged out':
            return 'bi-box-arrow-right';
        case 'Email verified':
            return 'bi-envelope-check';
        default:
            return 'bi-activity';
    }
}

function formatActivity($action) {
    switch ($action) {
        case 'User logged in':
            return 'masuk ke sistem';
        case 'User registered':
            return 'mendaftar sebagai pengguna baru';
        case 'User logged out':
            return 'keluar dari sistem';
        case 'Email verified':
            return 'memverifikasi email';
        default:
            return $action;
    }
}

function formatTime($datetime) {
    $timestamp = strtotime($datetime);
    $now = time();
    $diff = $now - $timestamp;
    
    if ($diff < 60) {
        return 'Baru saja';
    } elseif ($diff < 3600) {
        return floor($diff / 60) . ' menit yang lalu';
    } elseif ($diff < 86400) {
        return floor($diff / 3600) . ' jam yang lalu';
    } else {
        return date('d M Y, H:i', $timestamp);
    }
}
?>
