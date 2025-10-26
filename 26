<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Authentication Functions
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

// Generate random token
function generateToken($length = 32) {
    return bin2hex(random_bytes($length / 2));
}

// Hash password using bcrypt
function hashPassword($password) {
    return password_hash($password, PASSWORD_BCRYPT);
}

// Verify password
function verifyPassword($password, $hash) {
    return password_verify($password, $hash);
}

// Check if user is logged in
function isLoggedIn() {
    return isset($_SESSION['user_id']);
}

// Get current user data
function getCurrentUser() {
    if (isLoggedIn()) {
        global $conn;
        $user_id = $_SESSION['user_id'];
        
        $stmt = $conn->prepare("SELECT * FROM users WHERE id = ?");
        $stmt->bind_param("i", $user_id);
        $stmt->execute();
        $result = $stmt->get_result();
        
        if ($result->num_rows > 0) {
            return $result->fetch_assoc();
        }
    }
    return null;
}

// Check if user has specific role
function hasRole($role) {
    $user = getCurrentUser();
    if ($user) {
        return $user['role'] >= $role;
    }
    return false;
}

// Redirect if not logged in
function requireLogin() {
    if (!isLoggedIn()) {
        $_SESSION['redirect_url'] = $_SERVER['REQUEST_URI'];
        header('Location: login.php');
        exit;
    }
}

// Redirect if user doesn't have required role
function requireRole($role) {
    requireLogin();
    if (!hasRole($role)) {
        header('Location: dashboard.php?error=unauthorized');
        exit;
    }
}

// Send verification email
function sendVerificationEmail($email, $token) {
    $subject = "Verifikasi Email Anda - " . APP_NAME;
    $verificationLink = APP_URL . "/auth/verify.php?token=" . $token;
    
    $message = "
    <html>
    <head>
        <title>Verifikasi Email</title>
    </head>
    <body>
        <div style='font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto; padding: 20px;'>
            <div style='text-align: center; margin-bottom: 30px;'>
                <img src='" . APP_URL . "/logo' alt='" . APP_NAME . "' style='max-width: 150px;'>
            </div>
            <h2 style='color: #1E5C99;'>Verifikasi Email Anda</h2>
            <p>Terima kasih telah mendaftar di " . APP_NAME . ". Klik tombol di bawah untuk verifikasi email Anda:</p>
            <div style='text-align: center; margin: 30px 0;'>
                <a href='" . $verificationLink . "' style='background: #1E5C99; color: white; padding: 12px 30px; text-decoration: none; border-radius: 5px; display: inline-block;'>Verifikasi Email</a>
            </div>
            <p>Atau salin dan tempel link berikut ke browser Anda:</p>
            <p style='word-break: break-all; background: #f5f5f5; padding: 10px; border-radius: 5px;'>" . $verificationLink . "</p>
            <p>Link ini akan kadaluarsa dalam 24 jam.</p>
            <p>Terima kasih,<br>Tim " . APP_NAME . "</p>
        </div>
    </body>
    </html>
    ";
    
    $headers = "MIME-Version: 1.0" . "\r\n";
    $headers .= "Content-type:text/html;charset=UTF-8" . "\r\n";
    $headers .= "From: " . FROM_NAME . " <" . FROM_EMAIL . ">\r\n";
    
    return mail($email, $subject, $message, $headers);
}

// Send password reset email
function sendPasswordResetEmail($email, $token) {
    $subject = "Reset Password Anda - " . APP_NAME;
    $resetLink = APP_URL . "/auth/reset_password.php?token=" . $token;
    
    $message = "
    <html>
    <head>
        <title>Reset Password</title>
    </head>
    <body>
        <div style='font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto; padding: 20px;'>
            <div style='text-align: center; margin-bottom: 30px;'>
                <img src='" . APP_URL . "/logo' alt='" . APP_NAME . "' style='max-width: 150px;'>
            </div>
            <h2 style='color: #1E5C99;'>Reset Password Anda</h2>
            <p>Anda meminta untuk reset password. Klik tombol di bawah untuk membuat password baru:</p>
            <div style='text-align: center; margin: 30px 0;'>
                <a href='" . $resetLink . "' style='background: #1E5C99; color: white; padding: 12px 30px; text-decoration: none; border-radius: 5px; display: inline-block;'>Reset Password</a>
            </div>
            <p>Atau salin dan tempel link berikut ke browser Anda:</p>
            <p style='word-break: break-all; background: #f5f5f5; padding: 10px; border-radius: 5px;'>" . $resetLink . "</p>
            <p>Link ini akan kadaluarsa dalam 1 jam.</p>
            <p>Jika Anda tidak meminta reset password, abaikan email ini.</p>
            <p>Terima kasih,<br>Tim " . APP_NAME . "</p>
        </div>
    </body>
    </html>
    ";
    
    $headers = "MIME-Version: 1.0" . "\r\n";
    $headers .= "Content-type:text/html;charset=UTF-8" . "\r\n";
    $headers .= "From: " . FROM_NAME . " <" . FROM_EMAIL . ">\r\n";
    
    return mail($email, $subject, $message, $headers);
}

// Log user activity
function logActivity($user_id, $action) {
    global $conn;
    
    $ip_address = $_SERVER['REMOTE_ADDR'];
    $user_agent = $_SERVER['HTTP_USER_AGENT'];
    
    $stmt = $conn->prepare("INSERT INTO activity_logs (user_id, action, ip_address, user_agent) VALUES (?, ?, ?, ?)");
    $stmt->bind_param("isss", $user_id, $action, $ip_address, $user_agent);
    $stmt->execute();
}

// Get user orders
function getUserOrders($user_id, $limit = 10) {
    global $conn;
    
    $stmt = $conn->prepare("SELECT o.*, s.name as service_name FROM orders o LEFT JOIN services s ON o.service_id = s.id WHERE o.user_id = ? ORDER BY o.created_at DESC LIMIT ?");
    $stmt->bind_param("ii", $user_id, $limit);
    $stmt->execute();
    $result = $stmt->get_result();
    
    $orders = [];
    while ($row = $result->fetch_assoc()) {
        $orders[] = $row;
    }
    
    return $orders;
}
?>
