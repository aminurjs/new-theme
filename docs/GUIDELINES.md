# Shopify Theme Development Guidelines

## Directory Structure

```
├── assets/
│   ├── styles/               # Break down CSS into modules
│   │   ├── components/       # Component-specific styles
│   │   ├── layouts/         # Layout styles
│   │   └── utilities/       # Utility classes
│   └── scripts/             # Break down JS into modules
│       ├── components/      # Component-specific scripts
│       ├── utilities/       # Helper functions
│       └── vendors/         # Third-party scripts
├── config/                  # Theme configuration
├── layout/                  # Theme layouts
├── sections/               # Theme sections
│   ├── global/            # Global sections (header, footer)
│   └── templates/         # Template-specific sections
├── snippets/              # Reusable components
│   ├── components/        # UI components
│   ├── forms/            # Form-related snippets
│   └── utilities/        # Utility snippets
└── templates/             # Page templates
```

## File Organization Guidelines

### 1. Component Structure
- Each component should be self-contained
- Break down complex components into smaller parts
- Keep files under 100 lines when possible

Example:
```liquid
# Instead of one large product-card.liquid:

# snippets/product/product-card.liquid
{% render 'product/product-image' with product: product %}
{% render 'product/product-info' with product: product %}
{% render 'product/product-price' with product: product %}

# snippets/product/product-image.liquid
<div class="product-image">
  {{ product.featured_image | img_url: '300x300' }}
</div>

# snippets/product/product-info.liquid
<div class="product-info">
  <h3>{{ product.title }}</h3>
</div>
```

### 2. CSS Organization
- Use BEM naming convention
- Separate styles by component
- Create utility classes for common patterns

Example:
```scss
// assets/styles/components/_product-card.scss
.product-card {
  &__image { }
  &__title { }
  &__price { }
}

// assets/styles/utilities/_spacing.scss
.margin-top-sm { margin-top: 1rem; }
.padding-x { padding: 0 1rem; }
```

### 3. JavaScript Organization
- Use modules for related functionality
- Create utility functions for common operations
- Keep DOM manipulation separate from business logic

Example:
```js
// assets/scripts/utilities/format-money.js
export const formatMoney = (amount) => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(amount);
};

// assets/scripts/components/product-form.js
import { formatMoney } from '../utilities/format-money';
```

### 4. Section Schema Best Practices
- Group related settings
- Use clear, descriptive labels
- Provide helpful default values

Example:
```liquid
{% schema %}
{
  "name": "Product Grid",
  "settings": [
    {
      "type": "header",
      "content": "Layout Settings"
    },
    {
      "type": "range",
      "id": "products_per_row",
      "label": "Products per row",
      "min": 2,
      "max": 5,
      "step": 1,
      "default": 3
    }
  ]
}
{% endschema %}
```

### 5. Snippet Organization
- Create folders for different types of snippets
- Use clear, descriptive names
- Document complex snippets

Example:
```
snippets/
  components/
    product-card.liquid
    newsletter-form.liquid
  forms/
    input-field.liquid
    submit-button.liquid
  utilities/
    responsive-image.liquid
    pagination.liquid
```

### 6. Template Organization
- Keep templates minimal
- Use sections for main content
- Maintain consistent structure

Example:
```liquid
{% section 'template-header' %}

<main class="template-{{ template.name }}">
  {% section template.name %}
</main>

{% section 'template-footer' %}
```

## Best Practices

1. **Single Responsibility**
   - Each file should do one thing well
   - Break down complex functionality
   - Keep code focused and maintainable

2. **DRY (Don't Repeat Yourself)**
   - Create reusable snippets
   - Use utility classes
   - Share common functionality

3. **Naming Conventions**
   - Use descriptive, consistent names
   - Follow BEM for CSS
   - Use kebab-case for files

4. **Documentation**
   - Comment complex logic
   - Document schema settings
   - Maintain a README

5. **Performance**
   - Lazy load images
   - Minimize asset sizes
   - Use efficient selectors