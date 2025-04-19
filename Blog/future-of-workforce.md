---
title: "EnhancedNavigationInterceptor: Auto-Scrolling for Blazor Navigation"
author: "Dejan Demonjić"
date: "2025-04-19"
description: "Technical implementation of scroll position reset during Blazor navigation using the EnhancedNavigationInterceptor component."
tags: [Blazor, Navigation, Component, .NET]
keywords: [blazor navigation, scroll reset, page transitions, navigation events]
slug: "enhanced-navigation-interceptor"
is_featured: false
featured_image: "https://images.unsplash.com/photo-1519389950473-47ba0277781c?auto=format&fit=crop&q=80&w=2070&ixlib=rb-4.0.3"
---

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "EnhancedNavigationInterceptor: Auto-Scrolling for Blazor Navigation",
  "datePublished": "2025-04-19",
  "author": {"@type":"Person","name":"Dejan Demonjić"},
  "publisher": {"@type":"Organization","name":"Dejan Demonjić","logo":{"@type":"ImageObject","url":"https://github.com/obrana-boranija.png"}},
  "url": "https://dejan-demonjic.com/blog/enhanced-navigation-interceptor",
  "description": "Technical implementation of scroll position reset during Blazor navigation using the EnhancedNavigationInterceptor component.",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://dejan-demonjic.com/blog/enhanced-navigation-interceptor"
  }
}
</script>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://dejan-demonjic.com/"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Blog",
      "item": "https://dejan-demonjic.com/blog"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "Blazor",
      "item": "https://dejan-demonjic.com/blog/category/blazor"
    },
    {
      "@type": "ListItem",
      "position": 4,
      "name": "EnhancedNavigationInterceptor",
      "item": "https://dejan-demonjic.com/blog/enhanced-navigation-interceptor"
    }
  ]
}
</script>

# EnhancedNavigationInterceptor: Auto-Scrolling for Blazor Navigation

Blazor's SPA architecture preserves scroll position during navigation. The `EnhancedNavigationInterceptor` component resets scroll position when URLs change.

## Component Details

```csharp
@((MarkupString)script)

@functions {
    private MarkupString script => (MarkupString)GetScript();
}

@code {
    [Parameter]
    public ScrollBehavior Behavior { get; set; } = ScrollBehavior.Auto;

    private string GetBehaviorString()
    {
        return Behavior switch
        {
            ScrollBehavior.Smooth => "smooth",
            ScrollBehavior.Instant => "instant",
            ScrollBehavior.Auto => "auto",
            _ => "instant"
        };
    }

    private string GetScript()
    {
        var behavior = GetBehaviorString();
        return $@"
        <script>
        (function() {{
            let currentUrl = window.location.href;
            Blazor.addEventListener('enhancedload', () => {{
                let newUrl = window.location.href;
                if (currentUrl != newUrl) {{
                    window.scrollTo({{ top: 0, left: 0, behavior: '{behavior}' }});
                }}
                currentUrl = newUrl;
            }});
        }})();
        </script>
        ";
    }

    public enum ScrollBehavior
    {
        Auto,
        Instant,
        Smooth
    }
}
```

## Implementation

The component:
1. Injects a JavaScript function via `MarkupString`
2. Listens to Blazor's `enhancedload` event
3. Compares current and new URLs
4. Scrolls to top when URLs differ
5. Offers configurable scroll behavior (smooth, instant, auto)

## Usage

### Installation

```bash
dotnet add package Osirion.Blazor
```

### Setup

1. Add required using statement:

```csharp
@using Osirion.Blazor.Navigation
```

2. Add to layout:

```razor
<EnhancedNavigationInterceptor Behavior="ScrollBehavior.Smooth" />
```

### Configuration

Single parameter available:

```csharp
[Parameter]
public ScrollBehavior Behavior { get; set; } = ScrollBehavior.Auto;
```

Options:
- `ScrollBehavior.Auto`: Browser default
- `ScrollBehavior.Smooth`: Animated scrolling
- `ScrollBehavior.Instant`: Immediate position reset

### Technical Implementation

The component renders a JavaScript snippet that:
1. Stores the current URL on initialization
2. Listens for Blazor's `enhancedload` event
3. Compares stored URL with new URL after navigation
4. Scrolls to top if URLs differ
5. Updates stored URL for next navigation

Main implementation in `GetScript()` method:
```javascript
(function() {
    let currentUrl = window.location.href;
    Blazor.addEventListener('enhancedload', () => {
        let newUrl = window.location.href;
        if (currentUrl != newUrl) {
            window.scrollTo({ top: 0, left: 0, behavior: 'smooth' });
        }
        currentUrl = newUrl;
    });
})();
```

## Benefits

- Resets scroll position on page navigation
- Configurable scroll behavior (smooth/instant/auto)
- Zero dependencies
- Works in both Blazor Server and WebAssembly
- Minimal overhead

## Integration Examples

### Basic Implementation

```razor
<EnhancedNavigationInterceptor />
```

### With Custom Behavior

```razor
<EnhancedNavigationInterceptor Behavior="ScrollBehavior.Smooth" />
```

### Within Layout

```razor
<div class="main">
    <EnhancedNavigationInterceptor />
    @Body
</div>
```

## Source Code

Full component source from Osirion.Blazor:

```csharp
@((MarkupString)script)

@functions {
    private MarkupString script => (MarkupString)GetScript();
}

@code {
    [Parameter]
    public ScrollBehavior Behavior { get; set; } = ScrollBehavior.Auto;

    private string GetBehaviorString()
    {
        return Behavior switch
        {
            ScrollBehavior.Smooth => "smooth",
            ScrollBehavior.Instant => "instant",
            ScrollBehavior.Auto => "auto",
            _ => "instant"
        };
    }

    private string GetScript()
    {
        var behavior = GetBehaviorString();
        return $@"
        <script>
        (function() {{
            let currentUrl = window.location.href;
            Blazor.addEventListener('enhancedload', () => {{
                let newUrl = window.location.href;
                if (currentUrl != newUrl) {{
                    window.scrollTo({{ top: 0, left: 0, behavior: '{behavior}' }});
                }}
                currentUrl = newUrl;
            }});
        }})();
        </script>
        ";
    }

    public enum ScrollBehavior
    {
        Auto,
        Instant,
        Smooth
    }
}
```

## Package Information

- **GitHub**: [Osirion.Blazor](https://github.com/obrana-boranija/Osirion.Blazor)
- **NuGet**: [Osirion.Blazor](https://www.nuget.org/packages/Osirion.Blazor)
- **Author**: Dejan Demonjić
