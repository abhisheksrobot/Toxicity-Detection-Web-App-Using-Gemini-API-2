# Toxicity Detector Chrome Extension

This extension is a live analysis tool to detect toxicity in web content. It provides real-time monitoring, detailed analytics, and cross-platform benchmarking.

## Features

- **Real-Time Monitoring**: Tracks and identifies toxicity on active web pages.
- **Global Toggle Switch**: A master ON/OFF switch to enable or disable the extension's background monitoring.
- **Live Analysis & Alerting**: A dynamic line chart that tracks toxicity fluctuations over time.
- **Detailed Data Visualization**: Doughnut and bar charts to visualize content distribution and hourly toxicity trends.
- **Cross-Platform Benchmarking**: A comparison list of popular domains and their toxicity levels.
- **Parental Controls**: Block inappropriate content and set age restrictions.
- **Email Notifications**: Receive email notifications for various events.
- **Master Password Protection**: Secure the extension's settings with a master password.
- **Google Sign-In**: Sign in with your Google account to sync your settings.
- **MongoDB Integration**: Save your data to a MongoDB database.

## Tech Stack

- **Framework**: React 19.1.0, React DOM 19.1.6
- **Build Tool**: Vite 7.0.4
- **Charts**: Recharts 3.1.0
- **Styling**: Bootstrap 5.3.7, Custom CSS
- **Icons**: Lucide React 0.525.0
- **Languages**: JavaScript (ES Modules)
- **APIs**:
  - Chrome APIs: `chrome.storage`, `chrome.tabs`, `chrome.history`, `chrome.runtime`, `chrome.identity`
  - Google Gemini API
  - EmailJS
  - MongoDB

## Configuration and Setup

### Prerequisites

- Node.js and npm
- A Google account for the Gemini API and Google Sign-In
- An EmailJS account for email notifications
- A MongoDB Atlas account for database storage

### Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   ```
2. Install the dependencies:
   ```bash
   npm install
   ```

### Configuration Files

- **`public/manifest.json`**: The extension's manifest file. You need to configure the permissions and content scripts here.
- **`vite.config.js`**: The Vite build configuration. You need to add the `background.js` and `content.js` scripts as entry points:
  ```javascript
  // vite.config.js
  import { defineConfig } from 'vite'
  import react from '@vitejs/plugin-react'
  import { resolve } from 'path'

  export default defineConfig({
    plugins: [react()],
    build: {
      outDir: 'dist',
      rollupOptions: {
        input: {
          main: resolve(__dirname, 'index.html'),
          options: resolve(__dirname, 'options.html'),
          background: resolve(__dirname, 'src/background.js'),
          content: resolve(__dirname, 'src/content.js')
        },
        output: {
          entryFileNames: 'assets/[name].js',
          chunkFileNames: 'assets/[name].js',
          assetFileNames: 'assets/[name].[ext]',
        }
      },
      copyPublicDir: true
    }
  })
  ```
- **`src/emailService.js`**: The EmailJS configuration. You need to add your Service ID, Template ID, and User ID.
- **`src/db.js`**: The MongoDB configuration. You need to add your MongoDB URI.

### API Configuration

- **Gemini API Key**: The UI to set the Gemini API key is located in the **Setup** page of the extension. The logic to store and use this key needs to be implemented in `src/components/SetupTab.jsx`. You would typically store this key using `chrome.storage.local`.

- **MongoDB Authentication**: The connection string for your MongoDB database should be set in `src/db.js`. Replace the placeholder `YOUR_MONGODB_URI` with your actual connection string from MongoDB Atlas.

- **Google Sign-In (OAuth)**: To enable Google Sign-In, you must first create an OAuth 2.0 Client ID in the [Google Cloud Console](https://console.cloud.google.com/). 
  1.  Create an "OAuth 2.0 Client ID" for a "Chrome App".
  2.  You will get an Application ID. Use this ID to get your extension's key.
  3.  Once you have your Client ID, add it to `public/manifest.json` under the `oauth2` key:
      ```json
      {
        "name": "Toxicity Detector",
        // ... other manifest properties
        "oauth2": {
          "client_id": "YOUR_GOOGLE_OAUTH_CLIENT_ID.apps.googleusercontent.com",
          "scopes": [
            "https://www.googleapis.com/auth/userinfo.email",
            "https://www.googleapis.com/auth/userinfo.profile"
          ]
        },
        "permissions": [
          "identity"
        ]
      }
      ```

### Build

To build the extension, run the following command:


```bash
npm run build
```

This will create a `dist` directory with the bundled extension.

## Loading the Extension

1. Open Chrome and navigate to `chrome://extensions`.
2. Enable "Developer mode".
3. Click on "Load unpacked" and select the `dist` directory.

## Usage

- **Toggle Switch**: Use the toggle switch in the header to enable or disable the extension.
- **Setup Page**: Click on the gear icon to access the setup page. Here you can configure the Google API key, mail server, email notifications, parental controls, and color theme.
- **Master Password**: The default master password is `Admin@1234`. You can change it in the setup page.
- **Google Sign-In**: Click on the "Sign In with Google" button to sign in with your Google account.

---

This `README.md` file provides a comprehensive overview of the project, its features, and the necessary steps for configuration and setup. Let me know if you need any more information.
