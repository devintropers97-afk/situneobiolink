<?php
/**
 * Main Router
 */

// Start session
session_start();

// Include database configuration
require_once 'config/database.php';

// Get requested URL
 $url = isset($_GET['url']) ? $_GET['url'] : '';
 $url_parts = explode('/', trim($url, '/'));

// Default page
 $page = 'home';

// Determine page based on URL
if (!empty($url_parts[0])) {
    $page = $url_parts[0];
}

// Define pages
 $pages = [
    'home' => 'pages/index.php',
    'about' => 'pages/about.php',
    'services' => 'pages/services.php',
    'portfolio' => 'pages/portfolio.php',
    'pricing' => 'pages/pricing.php',
    'calculator' => 'pages/calculator.php',
    'contact' => 'pages/contact.php'
];

// Check if page exists
if (isset($pages[$page])) {
    include $pages[$page];
} else {
    // 404 page
    http_response_code(404);
    echo '<h1>404 - Page Not Found</h1>';
    echo '<p>The page you are looking for does not exist.</p>';
}
?>
