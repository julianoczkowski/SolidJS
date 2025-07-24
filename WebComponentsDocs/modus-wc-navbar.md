---
tag: modus-wc-navbar
category: Navigation
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-navbar--docs
---

# Modus WC Navbar

A full-width application bar that houses navigation menus, search, notifications, apps launcher, AI button and user profile controls. Supports condensed layouts for smaller viewports and exposes rich events for menu state synchronization.

## Attributes

| Name                       | Type                   | Default     | Notes                                             |
| -------------------------- | ---------------------- | ----------- | ------------------------------------------------- |
| `user-card` **(required)** | `INavbarUserCard`      | –           | User profile info (name, email, avatar)           |
| `visibility`               | `INavbarVisibility`    | all visible | Toggles individual buttons (ai, apps, help, etc.) |
| `condensed`                | `boolean`              | `false`     | Space-saving layout with three-dot menu           |
| `main-menu-open`           | `boolean`              | `false`     | Tracks left hamburger menu state                  |
| `apps-menu-open`           | `boolean`              | `false`     | Apps drawer state                                 |
| `notifications-menu-open`  | `boolean`              | `false`     | Notification panel state                          |
| `user-menu-open`           | `boolean`              | `false`     | User dropdown state                               |
| `search-input-open`        | `boolean`              | `false`     | Search input visibility                           |
| `condensed-menu-open`      | `boolean`              | `false`     | Three-dot menu state                              |
| `search-debounce-ms`       | `number`               | `300`       | Delay before searchChange fires                   |
| `text-overrides`           | `INavbarTextOverrides` | defaults    | Custom labels for condensed menu items            |
| `custom-class`             | `string`               | `''`        | Custom CSS class for styling                      |

### Interface Types

```typescript
interface INavbarUserCard {
  name: string;
  email: string;
  avatarSrc?: string;
}

interface INavbarVisibility {
  ai?: boolean;
  apps?: boolean;
  help?: boolean;
  mainMenu?: boolean;
  notifications?: boolean;
  search?: boolean;
  searchInput?: boolean;
  user?: boolean;
}

interface INavbarTextOverrides {
  apps?: string;
  help?: string;
  notifications?: string;
  search?: string;
}
```

## Slots

| Slot            | Description                            |
| --------------- | -------------------------------------- |
| `main-menu`     | Custom HTML for hamburger menu content |
| `notifications` | Content for notifications panel        |
| `apps`          | Content for apps drawer                |
| `start`         | Items after Trimble logo/hamburger     |
| `center`        | Centered content (e.g., app name)      |
| `end`           | Items before right-aligned icon group  |

## Events

### Button Events

| Event                | Type                                       | Description                    |
| -------------------- | ------------------------------------------ | ------------------------------ |
| `aiClick`            | `CustomEvent<MouseEvent \| KeyboardEvent>` | AI button activated            |
| `appsClick`          | `CustomEvent<MouseEvent \| KeyboardEvent>` | Apps button activated          |
| `helpClick`          | `CustomEvent<MouseEvent \| KeyboardEvent>` | Help button activated          |
| `notificationsClick` | `CustomEvent<MouseEvent \| KeyboardEvent>` | Notifications button activated |
| `searchClick`        | `CustomEvent<MouseEvent \| KeyboardEvent>` | Search button activated        |
| `signOutClick`       | `CustomEvent<MouseEvent \| KeyboardEvent>` | Sign out button activated      |
| `myTrimbleClick`     | `CustomEvent<MouseEvent \| KeyboardEvent>` | My Trimble button activated    |
| `trimbleLogoClick`   | `CustomEvent<MouseEvent \| KeyboardEvent>` | Trimble logo activated         |

### State Events

| Event                         | Type                             | Description                     |
| ----------------------------- | -------------------------------- | ------------------------------- |
| `mainMenuOpenChange`          | `CustomEvent<boolean>`           | Main menu state change          |
| `appsMenuOpenChange`          | `CustomEvent<boolean>`           | Apps menu state change          |
| `notificationsMenuOpenChange` | `CustomEvent<boolean>`           | Notifications menu state change |
| `userMenuOpenChange`          | `CustomEvent<boolean>`           | User menu state change          |
| `searchInputOpenChange`       | `CustomEvent<boolean>`           | Search input state change       |
| `condensedMenuOpenChange`     | `CustomEvent<boolean>`           | Condensed menu state change     |
| `searchChange`                | `CustomEvent<{ value: string }>` | Debounced search input changes  |

## Basic Usage

```html
<!-- Basic navbar -->
<modus-wc-navbar id="mainNavbar">
  <!-- Main menu -->
  <div slot="main-menu" style="padding: 1rem; min-width: 250px;">
    <h3>Navigation</h3>
    <ul style="list-style: none; padding: 0;">
      <li><a href="/dashboard">Dashboard</a></li>
      <li><a href="/projects">Projects</a></li>
      <li><a href="/settings">Settings</a></li>
    </ul>
  </div>

  <!-- Notifications -->
  <div slot="notifications" style="padding: 1rem; min-width: 300px;">
    <h3>Notifications</h3>
    <div>
      <div
        style="padding: 0.5rem; border: 1px solid #e0e0e0; margin-bottom: 0.5rem;"
      >
        <strong>New message</strong>
        <p style="margin: 0.25rem 0 0 0;">
          You have a new message from John Doe
        </p>
      </div>
    </div>
  </div>

  <!-- Apps -->
  <div slot="apps" style="padding: 1rem; min-width: 350px;">
    <h3>Apps</h3>
    <div
      style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem;"
    >
      <div
        style="text-align: center; padding: 1rem; border: 1px solid #e0e0e0;"
      >
        <div
          style="width: 40px; height: 40px; background: #0066cc; margin: 0 auto 0.5rem;"
        ></div>
        <span>App One</span>
      </div>
    </div>
  </div>

  <!-- Center content -->
  <span slot="center" style="font-weight: 600;">My Application</span>

  <!-- End content -->
  <modus-wc-button slot="end" color="secondary" size="sm">Help</modus-wc-button>
</modus-wc-navbar>

<script>
  // Configure navbar
  const navbar = document.getElementById("mainNavbar");
  navbar.userCard = {
    name: "Jane Smith",
    email: "jane.smith@trimble.com",
    avatarSrc: "https://via.placeholder.com/40x40?text=JS",
  };

  navbar.visibility = {
    ai: true,
    apps: true,
    help: true,
    mainMenu: true,
    notifications: true,
    search: true,
    user: true,
  };

  // Event listeners
  navbar.addEventListener("searchChange", (e) => {
    console.log("Search query:", e.detail.value);
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal, For } from "solid-js";

export function NavbarExample() {
  const [condensed, setCondensed] = createSignal(false);
  const [searchQuery, setSearchQuery] = createSignal("");
  const [mainMenuOpen, setMainMenuOpen] = createSignal(false);

  // User data
  const [userData] = createSignal({
    name: "Alex Johnson",
    email: "alex.johnson@trimble.com",
    avatarSrc: "https://via.placeholder.com/40x40?text=AJ",
  });

  // Visibility configuration
  const [visibility] = createSignal({
    ai: true,
    apps: true,
    help: true,
    mainMenu: true,
    notifications: true,
    search: true,
    searchInput: !condensed(),
    user: true,
  });

  // Navigation items
  const [navItems] = createSignal([
    { label: "Dashboard", href: "/dashboard", icon: "📊" },
    { label: "Projects", href: "/projects", icon: "📁" },
    { label: "Settings", href: "/settings", icon: "⚙️" },
  ]);

  // Notifications
  const [notifications] = createSignal([
    {
      id: 1,
      title: "New Message",
      content: "From John Doe",
      time: "2 min ago",
    },
    {
      id: 2,
      title: "System Update",
      content: "Maintenance tonight",
      time: "1 hour ago",
    },
  ]);

  const handleSearch = (e: CustomEvent) => {
    setSearchQuery(e.detail.value);
  };

  const handleSignOut = () => {
    console.log("User signing out...");
  };

  return (
    <div>
      {/* Controls */}
      <modus-wc-checkbox
        prop:checked={condensed()}
        on:checkboxChange={(e: CustomEvent) => setCondensed(e.detail)}
      >
        Condensed Layout
      </modus-wc-checkbox>

      {/* Navbar */}
      <modus-wc-navbar
        prop:condensed={condensed()}
        prop:user-card={userData()}
        prop:visibility={visibility()}
        prop:main-menu-open={mainMenuOpen()}
        search-debounce-ms={300}
        on:searchChange={handleSearch}
        on:signOutClick={handleSignOut}
        on:mainMenuOpenChange={(e: CustomEvent) => setMainMenuOpen(e.detail)}
      >
        {/* Main menu */}
        <div slot="main-menu" style="padding: 1rem; min-width: 280px;">
          <h3>Navigation</h3>
          <For each={navItems()}>
            {(item) => (
              <div style="display: flex; align-items: center; gap: 0.75rem; padding: 0.75rem;">
                <span>{item.icon}</span>
                <span>{item.label}</span>
              </div>
            )}
          </For>
        </div>

        {/* Notifications */}
        <div slot="notifications" style="padding: 1rem; min-width: 300px;">
          <h3>Notifications</h3>
          <For each={notifications()}>
            {(notif) => (
              <div style="padding: 0.75rem; border: 1px solid #ddd; margin-bottom: 0.5rem;">
                <strong>{notif.title}</strong>
                <p style="margin: 0.25rem 0 0 0;">{notif.content}</p>
                <small>{notif.time}</small>
              </div>
            )}
          </For>
        </div>

        {/* Apps */}
        <div slot="apps" style="padding: 1rem; min-width: 350px;">
          <h3>Applications</h3>
          <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 1rem;">
            <div style="text-align: center; padding: 1rem; border: 1px solid #ddd;">
              <div style="width: 40px; height: 40px; background: #0066cc; margin: 0 auto 0.5rem;"></div>
              <span>Project Manager</span>
            </div>
            <div style="text-align: center; padding: 1rem; border: 1px solid #ddd;">
              <div style="width: 40px; height: 40px; background: #cc6600; margin: 0 auto 0.5rem;"></div>
              <span>Analytics</span>
            </div>
          </div>
        </div>

        {/* Center content */}
        <span slot="center" style="font-weight: 600;">
          {condensed() ? "My App" : "My Application Dashboard"}
        </span>

        {/* End content */}
        <modus-wc-badge slot="end" color="success" size="sm">
          Pro
        </modus-wc-badge>
      </modus-wc-navbar>

      <p>Search query: {searchQuery() || "None"}</p>
    </div>
  );
}
```

## Common Patterns

### Enterprise Application Header

```html
<modus-wc-navbar>
  <div slot="main-menu">
    <nav>
      <a href="/dashboard">Dashboard</a>
      <a href="/projects">Projects</a>
      <a href="/reports">Reports</a>
    </nav>
  </div>

  <div slot="center">
    <strong>Enterprise Suite</strong>
  </div>

  <div slot="end">
    <modus-wc-badge color="warning">Beta</modus-wc-badge>
  </div>
</modus-wc-navbar>
```

### Mobile-Responsive Navbar

```tsx
const [isMobile, setIsMobile] = createSignal(window.innerWidth < 768);

<modus-wc-navbar prop:condensed={isMobile()}>
  <div slot="center">{isMobile() ? "App" : "Full Application Name"}</div>
</modus-wc-navbar>;
```

### Notification System

```tsx
const [notificationCount, setNotificationCount] = createSignal(3);

<modus-wc-navbar on:notificationsClick={() => setNotificationCount(0)}>
  <div slot="notifications">
    <div style="display: flex; justify-content: space-between;">
      <h3>Notifications</h3>
      <modus-wc-badge variant="counter" color="primary">
        {notificationCount()}
      </modus-wc-badge>
    </div>
    {/* Notification content */}
  </div>
</modus-wc-navbar>;
```

### Search Integration

```tsx
const [searchResults, setSearchResults] = createSignal([]);

const handleSearch = async (e: CustomEvent) => {
  const query = e.detail.value;
  if (query.length > 2) {
    const results = await searchAPI(query);
    setSearchResults(results);
  }
};

<modus-wc-navbar search-debounce-ms={500} on:searchChange={handleSearch} />;
```

## TypeScript Types

```typescript
// Component props
interface NavbarProps {
  "user-card": INavbarUserCard;
  visibility?: INavbarVisibility;
  condensed?: boolean;
  "main-menu-open"?: boolean;
  "apps-menu-open"?: boolean;
  "notifications-menu-open"?: boolean;
  "user-menu-open"?: boolean;
  "search-input-open"?: boolean;
  "condensed-menu-open"?: boolean;
  "search-debounce-ms"?: number;
  "text-overrides"?: INavbarTextOverrides;
  "custom-class"?: string;
}

// Event handlers
interface NavbarEvents {
  onSearchChange?: (e: CustomEvent<{ value: string }>) => void;
  onSignOutClick?: (e: CustomEvent<MouseEvent | KeyboardEvent>) => void;
  onMainMenuOpenChange?: (e: CustomEvent<boolean>) => void;
  // ... other event handlers
}

// Element reference
let navbarRef!: HTMLModusWcNavbarElement;
```

## Key Notes

- **State synchronization**: Keep menu open booleans in app state for cross-component awareness
- **Search debouncing**: 300ms is optimal for most use cases; adjust for expensive queries
- **Condensed UI**: Seldom-used icons hide behind three-dot menu; use `visibility` to control which appear
- **Object binding**: Always use `prop:` directives for complex objects like `user-card` and `visibility`
- **Event handling**: Component emits both click and state change events for comprehensive control
- **Mobile responsiveness**: Use `condensed` attribute for mobile viewports with responsive breakpoints

## CSS Variables

```css
.custom-navbar {
  --navbar-background: var(--modus-wc-color-base-page);
  --navbar-border: var(--modus-wc-color-base-200);
  --navbar-height: 64px;
  --navbar-padding: 0 1rem;
}
```

> **Prop Binding**: Use `prop:` directives for complex objects: `prop:user-card={userData()}`, `prop:visibility={visibility()}`
