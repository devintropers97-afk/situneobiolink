<?php
// ... (bagian atas file tetap sama)

// Process registration form
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $name = trim($_POST['name']);
    $email = trim($_POST['email']);
    $password = $_POST['password'];
    $confirm_password = $_POST['confirm_password'];
    $phone = trim($_POST['phone']);
    
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
    
    if ($password !== $confirm_password) {
        $errors['confirm_password'] = 'Konfirmasi password tidak cocok';
    }
    
    if (empty($phone)) {
        $errors['phone'] = 'Nomor telepon harus diisi';
    }
    
    // If no errors, register user
    if (empty($errors)) {
        $hashed_password = hashPassword($password);
        $verification_token = generateToken();
        $role = ROLE_USER; // Default role
        $referral_code = strtoupper(substr(md5(time()), 0, 6));
        
        $stmt = $conn->prepare("INSERT INTO users (name, email, password, phone, role, email_verified, verification_token, referral_code, created_at) VALUES (?, ?, ?, ?, ?, ?, ?, ?, NOW())");
        $email_verified = 0;
        $stmt->bind_param("ssssisss", $name, $email, $hashed_password, $phone, $role, $email_verified, $verification_token, $referral_code);
        
        if ($stmt->execute()) {
            $user_id = $conn->insert_id;
            
            // Send verification email
            if (sendVerificationEmail($email, $verification_token)) {
                $success = 'Pendaftaran berhasil! Silakan periksa email Anda untuk verifikasi.';
                
                // Log activity
                logActivity($user_id, 'User registered');
            } else {
                $errors['general'] = 'Pendaftaran berhasil, tetapi gagal mengirim email verifikasi. Silakan hubungi admin.';
            }
        } else {
            $errors['general'] = 'Terjadi kesalahan. Silakan coba lagi.';
        }
    }
}

// ... (bagian bawah file tetap sama)
?>
