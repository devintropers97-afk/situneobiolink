<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Add User Process
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

// Process add user form
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $name = trim($_POST['name']);
    $email = trim($_POST['email']);
    $password = $_POST['password'];
    $phone = trim($_POST['phone']);
    $role = $_POST['role'];
    $status = $_POST['status'];
    
    // Validation
    if (empty($name)) {
        $errors['name'] = 'Nama lengkap harus diisi';
    }
    
    if (empty($email)) {
        $errors['email'] = 'Email harus diisi';
    } elseif (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $errors['email'] = 'Format email tidak valid';
    } else {
        // Check if email already exists
        $stmt = $conn->prepare("SELECT id FROM users WHERE email = ?");
        $stmt->bind_param("s", $email);
        $stmt->execute();
        $result = $stmt->get_result();
        
        if ($result->num_rows > 0) {
            $errors['email'] = 'Email sudah terdaftar';
        }
    }
    
    if (empty($password)) {
        $errors['password'] = 'Password harus diisi';
    } elseif (strlen($password) < MIN_PASSWORD_LENGTH) {
        $errors['password'] = 'Password minimal ' . MIN_PASSWORD_LENGTH . ' karakter';
    }
    
    if (empty($phone)) {
        $errors['phone'] = 'Nomor telepon harus diisi';
    }
    
    // If no errors, add user
    if (empty($errors)) {
        $hashed_password = hashPassword($password);
        $referral_code = strtoupper(substr(md5(time()), 0, 6));
        
        $stmt = $conn->prepare("INSERT INTO users (name, email, password, phone, role, status, email_verified, referral_code, created_at) 
                               VALUES (?, ?, ?, ?, ?, ?, 1, ?, NOW())");
        $email_verified = 1; // Auto-verify admin-created accounts
        $stmt->bind_param("ssssiss", $name, $email, $hashed_password, $phone, $role, $status, $referral_code);
        
        if ($stmt->execute()) {
            $userId = $conn->insert_id;
            
            // Log activity
            logActivity($user['id'], "Created new user: $name (ID: $userId)");
            
            header("Location: users.php?success=user_added");
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
    header("Location: users.php?error=add_user_failed");
    exit;
}
?>
