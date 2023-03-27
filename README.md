# vcf-nav

Vaadin Component Factory component for navigation menus. This component is a stepping stone towards an official Vaadin component. Please provide feedback through issues in this repository.

## Features

- Navigation with a label can be collapsible
- Navigation items can have prefix and suffix content, for example, icons and badges
- Hierarchial items. A parent item can be without a path (non-navigable)
- Supports keyboard and screen reader navigation

**Missing features (things that will be added in the official component):**
- Material theme
- Localization support (to provide a custom text for the expand/collapse button of a navigation item)
- TypeScript types

**Potential feature additions:**
- Horizontal variant (“navigation bar”), with sub-items in a popup
- Icon-only variant (“navigation rail”), with sub-items in a popup
- [[insert your idea]](https://github.com/vaadin/vcf-nav/issues)

## Usage

### HTML

```html
<vcf-nav collapsible>
  <span slot="label">Main menu</span>

  <vcf-nav-item path="/dashboard">
    <vaadin-icon icon="vaadin:chart" slot="prefix"></vaadin-icon>
    Dashboard
    <span theme="badge primary" slot="suffix" aria-label="(2 new items)">2</span>
  </vcf-nav-item>

  <vcf-nav-item>
    <vaadin-icon icon="vaadin:folder-open" slot="prefix"></vaadin-icon>
    Parent

    <vcf-nav-item path="/child1" slot="children">
      Child 1
    </vcf-nav-item>

    <vcf-nav-item path="/child2" slot="children">
      Child 2
    </vcf-nav-item>
  </vcf-nav-item>
</vcf-nav>
```

### Java

> NOTE: The Java classes are not included in this repository. You can get them by downloading a starter project from [start.vaadin.com](https://start.vaadin.com).

```java
AppNav nav = new AppNav("Main menu");
nav.setCollapsible(true);

AppNavItem dashboard = new AppNavItem("Dashboard", "/dashboard");
dashboard.setIcon(new Icon(VaadinIcon.CHART));
Span newItems = new Span("2");
newItems.getElement().setAttribute("aria-label", "(2 new items)");
newItems.getElement().getThemeList().add("badge primary");
newItems.getElement().setAttribute("slot", "suffix");
dashboard.getElement().appendChild(newItems.getElement());

AppNavItem parent = new AppNavItem("Parent");
parent.setIcon(new Icon(VaadinIcon.FOLDER_OPEN));

AppNavItem child1 = new AppNavItem("Child 1", "/child1");
AppNavItem child2 = new AppNavItem("Child 2", "/child2");

parent.addItem(child1, child2);
nav.addItem(dashboard, parent);
```


## Styling

This component does not utilize the Vaadin `ThemableMixin` interface, and therefore does not allow you to easily inject styles into its shadow DOM.

The component is meant to be styled using standard CSS features. Regular “light DOM” styling works for most things, and the `::part()` selector for more detailed styling.

**Example:**

`frontend/themes/[mytheme]/styles.css`
```css
html {
  --vcf-nav-child-indent: var(--lumo-space-m);
}

vcf-nav {
  padding: 0;
  font-size: var(--lumo-font-size-m);
  color: var(--lumo-body-text-color);
}

vcf-nav::part(label) {
  text-transform: uppercase;
  font-size: var(--lumo-font-size-xs);
}

vcf-nav-item::part(item) {
  border-inline-start: 2px solid transparent;
}

vcf-nav-item[active]::part(item) {
  border-inline-start-color: var(--lumo-primary-color);
}
```

### `<vcf-nav>`

#### Parts

`::part(label)`  
The `<summary>` element that displays the label of the navigation menu.

`::part(label)::after`  
The expand/collapse icon of the label element. Defaults to `var(--lumo-icons-dropdown)`.

#### State attributes

`[collapsible]`  
Indicates that the navigation menu is user-collapsible and the pseudo element for the expand/collapse icon is visible.

`[collapsed]`  
Indicates that the navigation menu has been collapsed and the navigation items are hidden.

### `<vcf-nav-item>`

> NOTE: the navigation item applies styles to any prefix element with a class name that contains `"icon"`, assuming all such elements are meant to be the item icon.

#### Parts

`::part(item)`  
The `<a>` element that contains the item label and prefix and suffix content.

`::part(toggle-button)`  
The `<button>` element which contains the expand/collapse icon, if the item has child items.

`::part(toggle-button)::before`  
The expand/collapse icon. Defaults to `var(--lumo-icons-dropdown)`.

`::part(children)`  
The `<slot>` element that contains any child items. Can be styled visually.

#### State attributes

`[active]`  
Indicates that the item is currently the active/visible view in the application. This attribute is automatically set based on the current path/URL.

`[child-active]`  
Indicates that a child item of this navigation item is currently active.

`[expanded]`  
Indicates that the child items of this navigation item are visible.


#### Custom properties

`--vcf-nav-child-indent`  
The amount of visual indentation of child items. Can be set globally in the `html` element. Defaults to `var(--lumo-space-l)`.


