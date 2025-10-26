<?php
/**
 * ========================================
 * SITUNEO DIGITAL - User Details API
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once '../config.php';

// Require admin role
requireRole(ROLE_ADMIN);

// Get user ID from query string
if (!isset($_GET['id']) || empty($_GET['id'])) {
    http_response_code(400);
    echo json_encode(['error' => 'User ID is required']);
    exit;
}

 $user_id = (int)$_GET['id'];

// Get user details
 $stmt = $conn->prepare("SELECT * FROM users WHERE id = ?");
 $stmt->bind_param("i", $user_id);
 $stmt->execute();
 $result = $stmt->get_result();

if ($result->num_rows === 0) {
    http_response_code(404);
    echo json_encode(['error' => 'User not found']);
    exit;
}

 $user = $result->fetch_assoc();

// Add role name
switch ($user['role']) {
    case ROLE_USER:
        $user['role_name'] = 'Pengguna';
        break;
    case ROLE_ADMIN:
        $user['role_name'] = 'Admin';
        break;
    case ROLE_SUPER_ADMIN:
        $user['role_name'] = 'Super Admin';
        break;
    default:
        $user['role_name'] = 'Pengguna';
}

// Return user details as JSON
header('Content-Type: application/json');
echo json_encode($user);
?>
