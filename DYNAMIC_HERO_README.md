# Dynamic Hero Section - Section Rendering API Demo

This implementation demonstrates the usage of Shopify's Section Rendering API to dynamically display different hero banners based on UTM campaign parameters.

## Overview

The Dynamic Hero section automatically switches between two different banner designs:
- **Summer Banner**: Displayed when the URL contains `utm_campaign=summer` or any UTM campaign parameter that includes "summer"
- **Demo Banner**: Displayed as the default when no summer campaign is detected

## Files Created

### 1. Main Section
- `sections/dynamic-hero.liquid` - The main section that handles UTM parameter detection and banner selection

### 2. Banner Snippets
- `snippets/banner-summer.liquid` - Summer-themed banner with warm colors and summer messaging
- `snippets/banner-demo.liquid` - Default banner with cool colors and general store messaging

### 3. Translation
- `locales/en.default.json` - Added translation key for the section name

### 4. Test Template
- `templates/page.dynamic-hero-test.json` - Test page template to demonstrate the functionality

## How It Works

### UTM Parameter Detection
The section uses Liquid to parse the URL query string and extract the `utm_campaign` parameter:

```liquid
{% liquid
  assign utm_campaign = request.query_string | split: 'utm_campaign=' | last | split: '&' | first
  assign has_summer_campaign = false
  
  if utm_campaign contains 'summer'
    assign has_summer_campaign = true
  endif
%}
```

### Section Rendering API Usage
The section uses the `{% render %}` tag to dynamically include the appropriate banner snippet:

```liquid
{% if has_summer_campaign %}
  {% render 'banner-summer', section: section %}
{% else %}
  {% render 'banner-demo', section: section %}
{% endif %}
```

## Usage Examples

### 1. Default Banner (No UTM Parameters)
```
https://your-store.myshopify.com/pages/dynamic-hero-test
```
Displays the demo banner with cool gradient colors.

### 2. Summer Campaign Banner
```
https://your-store.myshopify.com/pages/dynamic-hero-test?utm_campaign=summer
https://your-store.myshopify.com/pages/dynamic-hero-test?utm_campaign=summer_sale_2024
https://your-store.myshopify.com/pages/dynamic-hero-test?utm_campaign=summer_collection
```
All of these URLs will display the summer banner with warm gradient colors.

## Section Settings

The section includes comprehensive settings for both banners:

### General Settings
- **Section Height**: Choose from small, medium, large, full-screen, or custom
- **Custom Height**: Set a custom height in svh units (20-100)

### Summer Banner Settings
- **Summer Banner Image**: Upload a summer-themed background image
- **Summer Banner Heading**: Custom heading text (default: "Summer Collection")
- **Summer Banner Text**: Rich text content for the banner
- **Summer Button Label**: Call-to-action button text
- **Summer Button Link**: URL for the button

### Demo Banner Settings
- **Demo Banner Image**: Upload a default background image
- **Demo Banner Heading**: Custom heading text (default: "Welcome to Our Store")
- **Demo Banner Text**: Rich text content for the banner
- **Demo Button Label**: Call-to-action button text
- **Demo Button Link**: URL for the button

## Styling Features

### Summer Banner
- Warm gradient background (coral to teal)
- White text with shadow effects
- Hover animations on buttons
- Responsive typography using clamp()

### Demo Banner
- Cool gradient background (blue to purple)
- White text with shadow effects
- Hover animations on buttons
- Responsive typography using clamp()

## Testing

1. Create a page using the `page.dynamic-hero-test` template
2. Test the default banner by visiting the page normally
3. Test the summer banner by adding `?utm_campaign=summer` to the URL
4. Try variations like `?utm_campaign=summer_sale` or `?utm_campaign=summer_collection_2024`

## Customization

### Adding More Banner Types
To add more banner variations:

1. Create a new snippet (e.g., `banner-winter.liquid`)
2. Add the detection logic in `dynamic-hero.liquid`:
   ```liquid
   if utm_campaign contains 'winter'
     assign has_winter_campaign = true
   endif
   ```
3. Add the rendering condition:
   ```liquid
   {% elsif has_winter_campaign %}
     {% render 'banner-winter', section: section %}
   ```

### Modifying UTM Detection
You can modify the detection logic to check for different parameters or use different conditions:

```liquid
{% liquid
  assign utm_source = request.query_string | split: 'utm_source=' | last | split: '&' | first
  assign utm_medium = request.query_string | split: 'utm_medium=' | last | split: '&' | first
  
  if utm_source contains 'email' and utm_medium contains 'newsletter'
    assign has_email_campaign = true
  endif
%}
```

## Benefits of This Approach

1. **Dynamic Content**: Automatically serves different content based on traffic source
2. **Performance**: Uses Shopify's efficient snippet rendering
3. **Maintainable**: Easy to add new banner variations
4. **SEO Friendly**: No JavaScript required, content is server-side rendered
5. **Analytics Ready**: UTM parameters are preserved for tracking

## Best Practices

1. **Image Optimization**: Use appropriately sized images for the banners
2. **Content Strategy**: Ensure both banners have compelling, relevant content
3. **Testing**: Test with various UTM parameter combinations
4. **Accessibility**: Ensure proper contrast ratios and alt text for images
5. **Mobile Responsiveness**: Test on various screen sizes

This implementation provides a solid foundation for dynamic content delivery using Shopify's Section Rendering API while maintaining good performance and user experience. 