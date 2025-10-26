<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Add Order Process
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once '../config.php';

// Require admin role
requireRole(ROLE_ADMIN);

// Get current user
 $user = getCurrentUser();

 $errors = [];
 $success = '';

// Process add order form
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $userId = $_POST['user_id'];
    $serviceId = $_POST['service_id'];
    $totalAmount = str_replace('.', '', $_POST['total_amount']);
    $requirements = $_POST['requirements'];
    $status = $_POST['status'];
    
    // Validation
    if (empty($userId)) {
        $errors['user_id'] = 'Pelanggan harus dipilih';
    }
    
    if (empty($serviceId)) {
        $errors['service_id'] = 'Layanan harus dipilih';
    }
    
    if (empty($totalAmount)) {
        $errors['total_amount'] = 'Total harga harus diisi';
    }
    
    if (empty($requirements)) {
        $errors['requirements'] = 'Keperluan harus diisi';
    }
    
    // If no errors, add order
    if (empty($errors)) {
        // Generate order number
        $orderNumber = 'ORD-' . date('Ymd') . '-' . strtoupper(substr(md5(time()), 0, 6));
        
        $stmt = $conn->prepare("INSERT INTO orders (user_id, service_id, order_number, status, total_amount, requirements, created_at) 
                               VALUES (?, ?, ?, ?, ?, ?, NOW())");
        $stmt->bind_param("iissss", $userId, $serviceId, $orderNumber, $status, $totalAmount, $requirements);
        
        if ($stmt->execute()) {
            $orderId = $conn->insert_id;
            
            // Log activity
            logActivity($user['id'], "Created new order: $orderNumber for user ID: $userId");
            
            header("Location: orders.php?success=order_added");
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
    header("Location: orders.php?error=add_order_failed");
    exit;
}
?>
