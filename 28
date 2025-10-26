<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Get Service Price
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once '../config.php';

// Require admin role
requireRole(ROLE_ADMIN);

header('Content-Type: application/json');

if (isset($_GET['id'])) {
    $serviceId = $_GET['id'];
    
    $stmt = $conn->prepare("SELECT price_start FROM services WHERE id = ? AND status = 'active'");
    $stmt->bind_param("i", $serviceId);
    $stmt->execute();
    $result = $stmt->get_result();
    
    if ($result->num_rows > 0) {
        $service = $result->fetch_assoc();
        echo json_encode([
            'success' => true,
            'price' => $service['price_start']
        ]);
    } else {
        echo json_encode([
            'success' => false,
            'message' => 'Service not found'
        ]);
    }
} else {
    echo json_encode([
        'success' => false,
        'message' => 'Service ID not provided'
    ]);
}
?>
