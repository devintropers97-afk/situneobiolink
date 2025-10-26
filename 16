// Document Ready
document.addEventListener('DOMContentLoaded', function() {
    // Add smooth scrolling to all links
    document.querySelectorAll('a[href^="#"]').forEach(anchor => {
        anchor.addEventListener('click', function (e) {
            e.preventDefault();
            
            document.querySelector(this.getAttribute('href')).scrollIntoView({
                behavior: 'smooth'
            });
        });
    });
    
    // Add active class to current nav item
    const currentLocation = location.pathname;
    const menuItem = document.querySelectorAll('.navbar-nav .nav-link');
    const menuLength = menuItem.length;
    
    for (let i = 0; i < menuLength; i++) {
        if (menuItem[i].href.includes(currentLocation)) {
            menuItem[i].classList.add('active');
        }
    }
});
