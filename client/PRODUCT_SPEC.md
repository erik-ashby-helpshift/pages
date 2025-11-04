# Helpshift Multi-App Web Configurator

## Product Requirements & Technical Specification

**Version:** 1.0
**Created:** 2025-10-07
**Status:** In Development

---

## 1. Overview

### Purpose
A web-based testing tool that allows product researchers to:
- Switch between multiple Helpshift web chat apps
- Authenticate users via Firebase
- Dynamically configure Custom Issue Fields (CIFs)
- Launch Helpshift with different configurations for testing

### Inspiration
Based on the iOS HelpshiftDemo app, this web version provides similar functionality with enhanced CIF editing capabilities.

---

## 2. Product Requirements

### User Stories

**As a product researcher, I want to:**
1. Sign in with Firebase authentication so I can access authenticated Helpshift features
2. Select from multiple Helpshift apps (EA, Care-AI, Sega, Space Fleet) for testing
3. Configure custom issue fields before launching Helpshift
4. Use preset CIF templates (e.g., "AI Agent") to speed up testing
5. Launch Helpshift web chat with my configured settings
6. Change CIF values and re-launch without page reload
7. Switch between apps (with page reload) while preserving my settings

### Key Features

#### ✅ MVP (Must Have)
- Firebase email/password authentication (sign up, sign in, sign out)
- App selector dropdown (4 pre-configured apps)
- Dynamic CIF editor (add/remove/edit fields)
- CIF preset: "AI Agent" template
- Initialize Helpshift with selected app
- Launch Helpshift web chat with CIFs
- Re-launch with different CIFs (no reload)
- App switching with state preservation (reload required)

#### 🔄 Future Enhancements (Nice to Have)
- Save custom CIF templates to localStorage
- Export/import configuration as JSON
- User attributes editor
- Widget event logging
- Multiple Firebase user profiles

---

## 3. Technical Architecture

### Technology Stack
```
Single HTML File Architecture
├── HTML5 structure
├── CSS (inline, Bootstrap-inspired)
├── Vanilla JavaScript (ES6+)
├── Firebase SDK v12.3.0 (modules)
│   ├── firebase-app
│   └── firebase-auth
└── Helpshift Web Chat SDK
    └── https://webchat.helpshift.com/latest/webChat.js
```

### Core Components

```javascript
// 1. Configuration Store
const CONFIG = {
  firebase: { /* Firebase web config */ },
  jwtEndpoint: "https://helpshift-jwt-service-845835740344.us-central1.run.app",
  apps: { ea, careai, sega, spacefleet }
};

// 2. Authentication Manager
class AuthManager {
  async signIn(email, password)
  async signUp(email, password)
  async signOut()
  async getHelpshiftJWT(appId, username)
}

// 3. CIF Manager
class CIFManager {
  addField(name, type, value)
  removeField(index)
  loadPreset(name) // "ai-agent", "custom", etc.
  toHelpshiftFormat()
}

// 4. Widget Manager
class WidgetManager {
  async initialize(appKey)
  launch()
  reset()
}

// 5. UI Controller
class UIController {
  updateAuthStatus()
  updateButtons()
  renderCIFs()
  showError(msg)
}
```

### State Management

```javascript
// Application State
const STATE = {
  currentUser: null,           // Firebase user object
  selectedApp: 'ea',           // Current app key
  helpshiftInitialized: false, // SDK initialization status
  cifs: []                     // Array of CIF objects
};

// State Persistence (on app switch)
sessionStorage.setItem('helpshift_state', JSON.stringify({
  selectedApp: 'careai',
  cifs: [{ name: 'ai_agent', type: 'singleline', value: 'care_agent' }]
}));
```

---

## 4. Configuration Details

### Firebase Configuration
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyBPIm-WOWxpNsgF9NbVjzfbSmTLPlIQkMw",
  authDomain: "product-research-460317.firebaseapp.com",
  projectId: "product-research-460317",
  storageBucket: "product-research-460317.firebasestorage.app",
  messagingSenderId: "845835740344",
  appId: "1:845835740344:web:76d90d7c86c938562a404d",
  measurementId: "G-WJ8ZHQXZD3"
};
```

### Helpshift Apps Configuration
```javascript
const APPS = {
  quantiphi: {
    name: "Quantiphi Fleet Command",
    platformId: "ashbys_platform_20251030223321331-cae75e82f606b6d",
    domain: "ashbys",
    appId: "ashbys_platform_20251030223321270-5458d1e7a62f35f"
  },
  ea: {
    name: "EA Client Demo",
    platformId: "ashbys_platform_20200902211516079-488178b85eeeceb",
    domain: "ashbys",
    appId: "ashbys_platform_20200902211516036-cd67ffa9e7ed41c"
  },
  careai: {
    name: "Care-AI Client Demo",
    platformId: "ashbys_platform_20250905022221454-66e3ededd13a0d1",
    domain: "ashbys",
    appId: "ashbys_platform_20250905022221399-f763e68fdc588e3"
  },
  sega: {
    name: "Sega Client Demo",
    platformId: "ashbys_platform_20210525002032362-e6b600a154a87ab",
    domain: "ashbys",
    appId: "ashbys_platform_20210519051637170-05974c6d1405bb4"
  },
  spacefleet: {
    name: "Space Fleet Command - Demo",
    platformId: "ashbys_platform_20251002202138668-75114075e06b3a7",
    domain: "ashbys",
    appId: "ashbys_platform_20251002202138610-365afd4005450da"
  }
};
```

### JWT Service
- **Endpoint:** `https://helpshift-jwt-service-845835740344.us-central1.run.app`
- **Method:** POST
- **Headers:** `Authorization: Bearer <FIREBASE_ID_TOKEN>`
- **Body:** `{ app_id: string, username: string }`
- **Response:** `{ jwt: string, expires_at: number }`

---

## 5. User Experience Flow

### Initial Load
```
1. Page loads → Initialize Firebase
2. Check for saved state in sessionStorage
3. Restore app selection and CIFs if present
4. Firebase auto-restores user session (if logged in)
5. Update UI to reflect current state
```

### Authentication Flow
```
User not authenticated:
1. Enter email/password
2. Click "Sign In" or "Sign Up"
3. Firebase authenticates
4. UI updates: "Initialize" button enabled
5. Status shows: "✅ Signed in as user@email.com"
```

### Configuration Flow
```
1. Select app from dropdown (EA, Care-AI, Sega, Space Fleet)
2. Add/edit CIFs:
   - Manual: Click "+ Add CIF" → enter name, type, value
   - Preset: Select "AI Agent" → Click "Load Preset"
3. Click "Initialize Helpshift"
   - Gets JWT from Cloud Run service
   - Loads Helpshift SDK
   - Authenticates with JWT
4. Click "Launch Widget"
   - Opens Helpshift chat with CIFs attached
```

### Re-launching with Different CIFs
```
1. Close Helpshift widget
2. Modify CIFs in the editor
3. Click "Launch Widget" again
4. Widget opens with updated CIFs
(No page reload required)
```

### Switching Apps
```
1. Change app dropdown selection
2. Browser shows confirm: "Changing apps requires page reload"
3. If confirmed:
   - Save current state to sessionStorage
   - Reload page with new app pre-selected
4. User clicks "Initialize Helpshift" again with new app
```

---

## 6. UI Layout

```
┌─────────────────────────────────────────────────┐
│  🎯 Helpshift Multi-App Configurator            │
│  Product Research Testing Tool                  │
├─────────────────────────────────────────────────┤
│                                                 │
│  ┌─── 🔐 Step 1: Authentication ─────────────┐ │
│  │                                            │ │
│  │  Email:    [___________________________]  │ │
│  │  Password: [___________________________]  │ │
│  │                                            │ │
│  │  [Sign In]  [Sign Up]     [Sign Out]     │ │
│  │                                            │ │
│  │  Status: ⚪ Not authenticated              │ │
│  └────────────────────────────────────────────┘ │
│                                                 │
│  ┌─── 🎮 Step 2: Select App ──────────────────┐ │
│  │                                            │ │
│  │  Choose App: [Quantiphi Fleet Command ▼]  │ │
│  │                                            │ │
│  │  ℹ️  Platform ID: ashbys_platform_2025...  │ │
│  │  ⚠️  Changing apps requires page reload    │ │
│  └────────────────────────────────────────────┘ │
│                                                 │
│  ┌─── ⚙️ Step 3: Custom Issue Fields ─────────┐ │
│  │                                            │ │
│  │  Preset: [AI Agent Template  ▼] [Load]   │ │
│  │                                            │ │
│  │  Field Name         Type        Value   ✕ │ │
│  │  ─────────────────────────────────────────│ │
│  │  ai_agent          single▼   care_agent ✕ │ │
│  │  ai_conversation_k single▼   a3f2e8d1  ✕ │ │
│  │                                            │ │
│  │  [+ Add Custom Field]                     │ │
│  │                                            │ │
│  │  💡 You can change CIFs and re-launch     │ │
│  │     the widget without reloading          │ │
│  └────────────────────────────────────────────┘ │
│                                                 │
│  ┌─── 🚀 Step 4: Launch ──────────────────────┐ │
│  │                                            │ │
│  │  [Initialize Helpshift]  [Launch Widget]  │ │
│  │   (disabled until auth)   (disabled)      │ │
│  │                                            │ │
│  │  [🔄 Reset & Change App]                   │ │
│  └────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────┘
```

---

## 7. Behavior Matrix

| Action | Requires Reload? | What Happens |
|--------|------------------|--------------|
| Change CIFs | ❌ No | Update CIF editor, re-launch widget |
| Change app dropdown | ✅ Yes | Prompt → reload → re-initialize |
| Re-launch widget (same CIFs) | ❌ No | Widget opens again |
| Re-launch widget (new CIFs) | ❌ No | Widget opens with updated CIFs |
| Sign in/out | ❌ No | Update auth state, enable/disable buttons |
| Click "Initialize" again | ❌ Ignored | Show message: "Already initialized" |
| Click "Reset & Change App" | ✅ Yes | Save state → reload with new app |

---

## 8. Custom Issue Fields (CIFs)

### Supported Types
```javascript
const CIF_TYPES = [
  'singleline',  // Single line text
  'multiline',   // Multi-line text
  'number',      // Numeric value
  'dropdown'     // Dropdown selection
];
```

### Preset: AI Agent Template
```javascript
{
  name: 'ai-agent',
  fields: [
    { name: 'ai_agent', type: 'singleline', value: 'care_agent' },
    { name: 'ai_conversation_key', type: 'singleline', value: generateKey() }
  ]
}

// generateKey() returns 16-char hex: "a3f2e8d1c4b9a2f5"
```

### Helpshift CIF Format
```javascript
// What we send to Helpshift
const cifs = {
  "ai_agent": {
    "type": "singleline",
    "value": "care_agent"
  },
  "ai_conversation_key": {
    "type": "singleline",
    "value": "a3f2e8d1c4b9a2f5"
  }
};

// Passed to Helpshift.open()
window.Helpshift("open", {
  config: {
    customIssueFields: cifs
  }
});
```

---

## 9. Error Handling

### User-Friendly Messages
```javascript
// Firebase auth errors
'auth/wrong-password' → '❌ Incorrect password'
'auth/user-not-found' → '❌ User not found. Sign up first.'
'auth/email-already-in-use' → '❌ Email already registered'
'auth/weak-password' → '❌ Password too weak (min 6 chars)'

// JWT errors
HTTP 401 → '❌ Authentication failed. Please sign in again.'
HTTP 500 → '❌ JWT service error. Please try again.'

// Helpshift errors
Not authenticated → '❌ Please sign in with Firebase first'
Already initialized → 'ℹ️ Helpshift already initialized for this app'
```

---

## 10. Security Considerations

### ✅ Secure
- Firebase Auth handles password hashing
- JWT generated server-side (Cloud Run)
- Helpshift secret never exposed to client
- All traffic over HTTPS
- Firebase auto-refreshes tokens

### ⚠️ Known Limitations
- Firebase config is public (expected for web apps)
- CIFs visible in browser console (for testing)
- No rate limiting on JWT endpoint (should add for production)

---

## 11. Testing Checklist

### Manual Testing
- [ ] Sign up with new email/password
- [ ] Sign in with existing credentials
- [ ] Sign out clears session
- [ ] Select each of 4 apps
- [ ] Load "AI Agent" preset
- [ ] Add custom CIF manually
- [ ] Remove CIF
- [ ] Initialize Helpshift (each app)
- [ ] Launch widget with CIFs
- [ ] Close widget, change CIFs, re-launch
- [ ] Switch apps (reload flow)
- [ ] State persists after app switch
- [ ] Firebase session persists on page refresh
- [ ] Error messages display correctly

### Browser Compatibility
- [ ] Chrome (latest)
- [ ] Safari (latest)
- [ ] Firefox (latest)
- [ ] Edge (latest)

---

## 12. File Structure

```
pages/client/
├── index.html              # Main configurator (this file)
├── web-chat-copilot.html   # Renamed from old index.html
├── care_agent.html
├── india_agent.html
├── mcp_agent.html
├── mcp_clean_agent.html
├── secure_mcp_agent.html
└── PRODUCT_SPEC.md         # This document
```

---

## 13. Implementation Notes

### Helpshift Web Chat SDK vs Web Widget SDK
This implementation uses **Web Chat SDK** because:
- All 4 provided app configs use Web Chat
- Modern chat-focused interface
- Better mobile support
- Consistent with iOS app's conversation focus

### State Persistence Strategy
```javascript
// Save before reload
sessionStorage.setItem('helpshift_state', JSON.stringify({
  selectedApp: 'careai',
  cifs: CIFManager.fields
}));

// Restore on load
const saved = JSON.parse(sessionStorage.getItem('helpshift_state') || '{}');
if (saved.selectedApp) {
  STATE.selectedApp = saved.selectedApp;
  CIFManager.fields = saved.cifs || [];
}
```

### Why Single HTML File?
- No build process required
- Easy to deploy (just open in browser)
- Self-contained for demos
- Simple to share/copy
- Fast iteration

---

## 14. Future Enhancements

### Phase 2 (Post-MVP)
1. **CIF Templates**
   - Save custom templates to localStorage
   - Export/import as JSON files
   - Share templates between users

2. **User Attributes Editor**
   - Edit Helpshift user properties
   - Set master vs app-level attributes
   - Preview before sending

3. **Debug Console**
   - Real-time widget event log
   - Network request inspector
   - JWT decoder/viewer

4. **Multi-User Profiles**
   - Switch between Firebase users
   - Test user segmentation
   - Saved credentials (encrypted)

---

## 15. Known Limitations

### Helpshift SDK Constraints
- Cannot re-initialize SDK with different app without reload
- Cannot logout and login with different JWT for different app
- Widget must be closed before changing CIFs

### Browser Requirements
- Requires modern browser (ES6+ support)
- Cookies/localStorage must be enabled
- Pop-up blocker may affect widget

---

## 16. Support & Maintenance

### Key Dependencies
- Firebase SDK v12.3.0 (CDN)
- Helpshift Web Chat SDK (latest)
- Modern browsers (ES6+)

### Breaking Changes to Watch
- Firebase SDK updates
- Helpshift API changes
- JWT service endpoint changes

### Contact
- **Developer:** Erik Ashby
- **Firebase Project:** product-research-460317
- **Helpshift Domain:** ashbys.helpshift.com

---

**Document Version:** 1.0
**Last Updated:** 2025-10-07
**Status:** Ready for Implementation
