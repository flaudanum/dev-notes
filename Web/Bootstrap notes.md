
- [1. Bootstrap](#1-bootstrap)
  - [1.1. Navigation bar](#11-navigation-bar)
  - [1.2. New section](#12-new-section)
  - [1.3. Section](#13-section)

# 1. Bootstrap

## 1.1. Navigation bar

Snippet from the [bootstrap documentation](https://getbootstrap.com/docs/4.3/components/navbar/#nav) with small modifications plus my comments:

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-primary">
  <a class="navbar-brand" href="#">Navbar</a>
  
  <!-- This button appears when the navigation bar collapse -->
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <!-- The class .collapse.navbar-collapse is required for collapsing the navigation items -->
  <div class="collapse navbar-collapse" id="navbarNav">
    
    <!-- Class .navbar-nav is mandatory for the proper horizontal rendering and inheritance of the navbar style. So are the classes .nav-item and .nav-link for elements ul, li and a for a proper rendering -->
    <ul class="navbar-nav">
      <li class="nav-item active">
        <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Features</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Pricing</a>
      </li>
      <li class="nav-item">
        <!-- This link is disabled. Disabled buttons should include the aria-disabled="true" attribute to indicate the state of the element to assistive technologies. While the .disabled class uses pointer-events: none to try to disable the link functionality of <a>s, that CSS property is not yet standardized and doesn’t account for keyboard navigation. As such, you should always add tabindex="-1" on disabled links and use custom JavaScript to fully disable their functionality.-->
        <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
      </li>
    </ul>
  </div>
</nav>
```

## 1.2. New section

## 1.3. Section

