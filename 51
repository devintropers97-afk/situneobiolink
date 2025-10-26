<?php
/**
 * ========================================
 * SITUNEO DIGITAL - Email Verification
 * NIB: 20250-9261-4570-4515-5453
 * ========================================
 */

require_once 'config.php';

 $verified = false;
 $error = '';

if (isset($_GET['token'])) {
    $token = $_GET['token'];
    
    // Find user with this token
    $stmt = $conn->prepare("SELECT id, email FROM users WHERE verification_token = ? AND email_verified = 0");
    $stmt->bind_param("s", $token);
    $stmt->execute();
    $result = $stmt->get_result();
    
    if ($result->num_rows > 0) {
        $user = $result->fetch_assoc();
        
        // Update user as verified
        $stmt = $conn->prepare("UPDATE users SET email_verified = 1, verification_token = NULL WHERE id = ?");
        $stmt->bind_param("i", $user['id']);
        
        if ($stmt->execute()) {
            $verified = true;
            
            // Log activity
            logActivity($user['id'], 'Email verified');
        } else {
            $error = 'Terjadi kesalahan saat memverifikasi email Anda. Silakan coba lagi.';
        }
    } else {
        $error = 'Token verifikasi tidak valid atau sudah kadaluarsa.';
    }
} else {
    $error = 'Token verifikasi tidak ditemukan.';
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Verifikasi Email - SITUNEO DIGITAL</title>
    <meta name="description" content="Verifikasi email Anda untuk mengaktifkan akun SITUNEO DIGITAL">
    
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
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        
        .verify-container {
            background: rgba(15, 48, 87, 0.8);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 180, 0, 0.2);
            padding: 40px;
            text-align: center;
            max-width: 500px;
            width: 100%;
        }
        
        .verify-icon {
            font-size: 4rem;
            margin-bottom: 20px;
        }
        
        .success-icon {
            color: #28a745;
        }
        
        .error-icon {
            color: #dc3545;
        }
        
        .verify-title {
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 1.8rem;
            font-weight: 800;
            margin-bottom: 20px;
            background: var(--gradient-gold);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .btn-gold {
            background: var(--gradient-gold);
            color: var(--dark-blue);
            border: none;
            padding: 12px 30px;
            font-weight: 700;
            border-radius: 50px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            transition: all 0.3s;
            display: inline-block;
            text-decoration: none;
            box-shadow: 0 5px 15px rgba(255, 180, 0, 0.3);
        }
        
        .btn-gold:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255, 180, 0, 0.6);
            color: var(--dark-blue);
        }
        
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
    </style>
</head>
<body>
    <div class="verify-container">
        <?php if ($verified): ?>
            <div class="verify-icon success-icon">
                <i class="bi bi-check-circle-fill"></i>
            </div>
            <h2 class="verify-title">Email Terverifikasi!</h2>
            <p class="mb-4">Selamat! Email Anda telah berhasil diverifikasi. Akun Anda sekarang aktif dan Anda dapat masuk ke dashboard.</p>
            <a href="login.php" class="btn btn-gold">Masuk Sekarang</a>
        <?php else: ?>
            <div class="verify-icon error-icon">
                <i class="bi bi-exclamation-triangle-fill"></i>
            </div>
            <h2 class="verify-title">Verifikasi Gagal</h2>
            <div class="alert alert-danger">
                <?= $error ?>
            </div>
            <p class="mb-4">Jika Anda mengalami masalah dengan verifikasi email, silakan hubungi tim dukungan kami.</p>
            <a href="contact.php" class="btn btn-gold">Hubungi Dukungan</a>
        <?php endif; ?>
    </div>
    
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
