<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Logout Process
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once 'config.php';

// Log activity if user is logged in
if (isLoggedIn()) {
    logActivity($_SESSION['user_id'], 'User logged out');
}

// Destroy session
session_unset();
session_destroy();

// Clear remember me cookie if exists
if (isset($_COOKIE['remember_token'])) {
    setcookie('remember_token', '', time() - 3600, '/', '', true, true);
}

// Redirect to login page
header('Location: login.php?message=logout_success');
exit;
?>
