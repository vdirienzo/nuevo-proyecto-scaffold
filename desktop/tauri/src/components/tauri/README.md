# Tauri Frontend Components

Production-quality components for Tauri desktop applications.

**Autor:** Homero Thompson del Lago del Terror

## Components

### React Components

#### UpdateNotification
Shows an update notification when a new version is available.

**Features:**
- Auto-check for updates on mount
- Download progress bar
- Install and restart functionality
- Dismiss option
- Accessible (ARIA labels, roles)

**Usage:**
```tsx
import { UpdateNotification } from '@/components/tauri';

function App() {
  return (
    <div>
      <UpdateNotification onDismiss={() => console.log('dismissed')} />
      {/* rest of your app */}
    </div>
  );
}
```

---

#### TitleBar
Custom window title bar with minimize, maximize, and close buttons.

**Features:**
- Draggable window region
- Platform-native controls
- Maximize/restore state tracking
- Dark mode support
- Fully accessible

**Usage:**
```tsx
import { TitleBar } from '@/components/tauri';

function App() {
  return (
    <div className="flex flex-col h-screen">
      <TitleBar title="My App" icon="/icon.png" />
      {/* rest of your app */}
    </div>
  );
}
```

---

#### SystemTray
System tray configuration helper with custom menu items.

**Features:**
- Custom menu items
- Show/hide window toggle
- Tray icon click handler
- Programmatic control via hook

**Usage:**
```tsx
import { SystemTray, useSystemTray } from '@/components/tauri';

function App() {
  const tray = useSystemTray();

  const customMenuItems = [
    {
      label: 'Open Dashboard',
      action: () => console.log('Dashboard clicked'),
    },
    {
      label: 'Settings',
      action: () => console.log('Settings clicked'),
    },
  ];

  return (
    <div>
      <SystemTray menuItems={customMenuItems} showDefaultItems={true} />
      {/* rest of your app */}
    </div>
  );
}
```

**Hook API:**
```tsx
const { showWindow, hideWindow, toggleWindow, quitApp } = useSystemTray();

// Show window programmatically
await showWindow();
```

---

#### SplashScreen
Splash screen with loading progress.

**Features:**
- Customizable duration
- Loading tasks with progress
- Fade out animation
- App logo and version display
- Accessible

**Usage (Simple):**
```tsx
import { SplashScreen } from '@/components/tauri';

function App() {
  const [showSplash, setShowSplash] = useState(true);

  return (
    <div>
      {showSplash && (
        <SplashScreen
          duration={2000}
          logo="/logo.png"
          onComplete={() => setShowSplash(false)}
        />
      )}
      {/* rest of your app */}
    </div>
  );
}
```

**Usage (With Tasks):**
```tsx
import { SplashScreen, useAppInitialization } from '@/components/tauri';

function App() {
  const [showSplash, setShowSplash] = useState(true);

  const loadingTasks = [
    {
      name: 'Loading configuration...',
      action: async () => {
        await loadConfig();
      },
    },
    {
      name: 'Connecting to database...',
      action: async () => {
        await connectDB();
      },
    },
    {
      name: 'Initializing services...',
      action: async () => {
        await initServices();
      },
    },
  ];

  return (
    <div>
      {showSplash && (
        <SplashScreen
          loadingTasks={loadingTasks}
          logo="/logo.png"
          showVersion={true}
          onComplete={() => setShowSplash(false)}
        />
      )}
      {/* rest of your app */}
    </div>
  );
}
```

---

### Vue Components

#### UpdateNotification.vue
Same as React version, but for Vue 3.

**Usage:**
```vue
<script setup lang="ts">
import UpdateNotification from '@/components/tauri/UpdateNotification.vue';

function handleDismiss() {
  console.log('dismissed');
}
</script>

<template>
  <div>
    <UpdateNotification :on-dismiss="handleDismiss" />
    <!-- rest of your app -->
  </div>
</template>
```

---

#### TitleBar.vue
Same as React version, but for Vue 3.

**Usage:**
```vue
<script setup lang="ts">
import TitleBar from '@/components/tauri/TitleBar.vue';
</script>

<template>
  <div class="flex flex-col h-screen">
    <TitleBar title="My App" icon="/icon.png" />
    <!-- rest of your app -->
  </div>
</template>
```

---

#### SplashScreen.vue
Same as React version, but for Vue 3.

**Usage:**
```vue
<script setup lang="ts">
import { ref } from 'vue';
import SplashScreen from '@/components/tauri/SplashScreen.vue';

const showSplash = ref(true);

const loadingTasks = [
  {
    name: 'Loading configuration...',
    action: async () => {
      await loadConfig();
    },
  },
];

function handleComplete() {
  showSplash.value = false;
}
</script>

<template>
  <div>
    <SplashScreen
      v-if="showSplash"
      :loading-tasks="loadingTasks"
      logo="/logo.png"
      :show-version="true"
      :on-complete="handleComplete"
    />
    <!-- rest of your app -->
  </div>
</template>
```

---

## Dependencies

These components require the following Tauri plugins:

```toml
# src-tauri/Cargo.toml
[dependencies]
tauri-plugin-updater = "2"
tauri-plugin-process = "2"
```

```json
// package.json
{
  "dependencies": {
    "@tauri-apps/api": "^2.0.0",
    "@tauri-apps/plugin-updater": "^2.0.0",
    "@tauri-apps/plugin-process": "^2.0.0"
  }
}
```

---

## Styling

All components use Tailwind CSS. Make sure Tailwind is configured in your project:

```js
// tailwind.config.js
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx,vue}",
  ],
  darkMode: 'class',
  theme: {
    extend: {},
  },
  plugins: [],
}
```

---

## Accessibility

All components follow WCAG 2.1 Level AA guidelines:
- Proper ARIA labels and roles
- Keyboard navigation support
- Screen reader friendly
- Focus management
- Color contrast compliance

---

## Template Variables

When generating from templates, these variables are replaced:
- `{{PROJECT_NAME}}` - kebab-case project identifier
- `{{AUTHOR}}` - Author name
- `{{APP_TITLE}}` - Window title
- `{{BUNDLE_ID}}` - Bundle identifier (e.g., `com.example.app`)

---

## License

See project LICENSE file.
