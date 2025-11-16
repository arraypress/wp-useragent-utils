# WordPress UserAgent Utilities

A lean WordPress utility for browser detection, device recognition, and bot identification. Built for e-commerce analytics, responsive design, and conditional functionality with just the features you need.

## Features

* 🎯 **Focused API** - Just 9 essential methods for user agent operations
* 🌐 **Browser Detection** - Identify Chrome, Firefox, Safari, Edge, and more
* 📱 **Device Recognition** - Mobile/desktop detection with `wp_is_mobile()` fallback
* 🤖 **Bot Detection** - Identify search engines and AI bots (2024/2025)
* 💻 **OS Detection** - Windows, macOS, iOS, Android, Linux
* 📊 **E-commerce Ready** - EDD-compatible formatted output

## Requirements

* PHP 7.4 or later
* WordPress 5.0 or later

## Installation
```bash
composer require arraypress/wp-utils-useragent
```

## Usage

### Browser Detection
```php
use ArrayPress\UserAgentUtils\UserAgent;

// Get browser name
$browser = UserAgent::get_browser();
// Returns: "Chrome", "Firefox", "Safari", "Edge", etc.

// Check specific browser
if ( UserAgent::is_browser( 'Chrome' ) ) {
    // Chrome-specific functionality
}

// Get operating system
$os = UserAgent::get_os();
// Returns: "Windows", "macOS", "iOS", "Android", "Linux"
```

### Device Detection
```php
// Check device type (uses wp_is_mobile() when possible)
if ( UserAgent::is_mobile() ) {
    // Mobile device
}

if ( UserAgent::is_desktop() ) {
    // Desktop device
}

// Get device type as string
$device = UserAgent::get_device_type();
// Returns: "mobile", "desktop", "bot", or "unknown"
```

### Bot Detection
```php
// Check for bots/crawlers (includes 2024 AI bots)
if ( UserAgent::is_bot() ) {
    // Skip analytics, serve cached content
    return;
}
```

### E-commerce Integration
```php
// Get formatted string for storage (EDD-compatible)
$formatted = UserAgent::get_formatted();
// Returns: "Chrome on Windows" or "Safari on iOS"

// Store with order data
$order_meta = [
    'user_agent' => UserAgent::get_formatted(),
    'device_type' => UserAgent::get_device_type(),
    'is_mobile' => UserAgent::is_mobile()
];
```

## Common Use Cases

### Analytics Tracking
```php
function track_visitor() {
    // Skip bot traffic
    if ( UserAgent::is_bot() ) {
        return;
    }
    
    $visitor_data = [
        'browser' => UserAgent::get_browser(),
        'os' => UserAgent::get_os(),
        'device' => UserAgent::get_device_type(),
        'formatted' => UserAgent::get_formatted()
    ];
    
    // Log to database
    update_option( 'visitor_stats', $visitor_data );
}
add_action( 'init', 'track_visitor' );
```

### Conditional Asset Loading
```php
function enqueue_device_assets() {
    // Mobile-specific JavaScript
    if ( UserAgent::is_mobile() ) {
        wp_enqueue_script( 'touch-events', 'js/touch.js' );
    }
    
    // Browser-specific fixes
    if ( UserAgent::is_browser( 'Safari' ) ) {
        wp_enqueue_style( 'safari-fixes', 'css/safari.css' );
    }
}
add_action( 'wp_enqueue_scripts', 'enqueue_device_assets' );
```

### SugarCart Integration
```php
function log_order_device_info( $order_id ) {
    $order = sugarcart_get_order( $order_id );
    
    // Skip bot orders
    if ( UserAgent::is_bot() ) {
        $order->add_note( 'Bot order detected' );
        return;
    }
    
    // Store device info
    $order->update_meta( 'browser_info', UserAgent::get_formatted() );
    $order->update_meta( 'device_type', UserAgent::get_device_type() );
    $order->update_meta( 'is_mobile', UserAgent::is_mobile() );
}
add_action( 'sugarcart_order_completed', 'log_order_device_info' );
```

### Bot Filtering
```php
function handle_bot_traffic() {
    if ( ! UserAgent::is_bot() ) {
        return;
    }
    
    // Serve cached version for bots
    if ( $cached = wp_cache_get( 'page_cache' ) ) {
        echo $cached;
        exit;
    }
}
add_action( 'template_redirect', 'handle_bot_traffic' );
```

## API Reference

| Method | Description | Returns |
|--------|-------------|---------|
| `get()` | Get current user agent string | `string` |
| `get_browser()` | Get browser name | `?string` |
| `get_os()` | Get operating system | `?string` |
| `is_mobile()` | Check if mobile device | `bool` |
| `is_desktop()` | Check if desktop | `bool` |
| `is_bot()` | Check if bot/crawler | `bool` |
| `get_device_type()` | Get device as string | `string` |
| `get_formatted()` | Get formatted for storage | `string` |
| `is_browser( $name )` | Check specific browser | `bool` |

## Supported Detection

### Browsers
- Chrome (+ Mobile, iOS)
- Safari (+ Mobile)
- Firefox (+ iOS)
- Edge
- Opera
- Brave
- Samsung Browser

### Operating Systems
- Windows (10, 11)
- macOS
- iOS
- Android
- Linux

### Bots (2024/2025)
- Search engines (Google, Bing, DuckDuckGo)
- AI bots (GPTBot, ClaudeBot, ChatGPT)
- Social media (Facebook, Twitter)
- SEO tools (Semrush, Ahrefs)

## Why This Library?

- **Lean & Focused** - Just 9 methods for real use cases
- **WordPress Native** - Uses `wp_is_mobile()` and WP sanitization
- **E-commerce Ready** - EDD-compatible formatting
- **Modern Bot Detection** - Updated for 2024/2025 AI bots
- **No Version Bloat** - Removed unreliable version detection

## License

GPL-2.0-or-later

## Support

- [Documentation](https://github.com/arraypress/wp-utils-useragent)
- [Issue Tracker](https://github.com/arraypress/wp-utils-useragent/issues)