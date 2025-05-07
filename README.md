# Chat Data SDK Demo Guide

This document provides instructions for using the Chat Data SDK demo application (`index.html`). The demo showcases the features of the Chat Data JavaScript SDK and helps developers understand how to integrate it into their applications.

## Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
- [Demo Features](#demo-features)
- [Using the Demo](#using-the-demo)
- [SDK Integration Guide](#sdk-integration-guide)
- [Running the Demo Locally](#running-the-demo-locally)
- [Troubleshooting](#troubleshooting)

## Overview

The Chat Data SDK Demo is an interactive HTML page that demonstrates the functionality of the Chat Data JavaScript SDK. It provides a user interface for:

- Initializing the SDK
- Sending and clearing user information
- Registering event listeners
- Viewing real-time events

## Getting Started

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, Edge)
- A Chat Data account with a valid chatbot ID
- Python (optional, for running a local server)

### Running the Demo

1. Clone the repository: `git clone https://github.com/chat-data-llc/chat-data-sdk-demo.git`
2. Navigate to the project directory: `cd chat-data-sdk-demo`
3. Edit the configuration constants in `index.html` to use your own domain and chatbot ID
4. Start a local server (see [Running the Demo Locally](#running-the-demo-locally))
5. Open the demo in your web browser
6. Once initialized, you can:
   - Send/clear user information
   - Add event listeners
   - Monitor events in the event log

## Demo Features

### SDK Configuration

The demo loads the SDK dynamically with configuration constants:
- `CHAT_DATA_DOMAIN`: The domain for the Chat Data service (default: "www.chat-data.com"). If you're on a reseller plan, you can replace this with your own custom domain that has been properly configured with DNS settings.
- `CHAT_DATA_CHATBOT_ID`: Your chatbot ID (you must replace "YOUR_CHATBOT_ID" with your actual chatbot ID)

You should replace these values with your own domain and chatbot ID. The current configuration is displayed in the "SDK Configuration" section, showing the values from the SDK's getConfig() method.

### User Information Management

The demo includes a form to send user information to the chatbot:
- Name: The user's full name
- Email: The user's email address
- Phone: The user's phone number
- Additional Info: Text field for any additional information

Unlike JSON, the additional info field accepts plain text that is sent directly to the SDK.

You can also clear user information using the "Clear User Info" button.

### Event System

The demo allows you to register event listeners for all supported event types:
- `chat`: Messages sent or received in the chat
- `lead-submission`: Form submissions in the chatbot
- `live-chat-escalation`: Requests to escalate to a human agent
- `minimize-widget`: When the chat widget is minimized

Each event type has a dedicated button to add the corresponding listener.

### Event Log

The event log panel displays real-time events from the SDK with:
- Timestamps for each event
- Color-coded event types (info, success, error)
- Properly formatted JSON data
- Word wrapping for long messages

You can clear the event log using the "Clear Event Log" button.

## Using the Demo

### 1. SDK Initialization

The SDK initializes dynamically using the configuration constants defined at the top of the script section. The scripts are loaded after the DOM is fully loaded.

```html
<script>
  // Configuration constants - Edit these to use your own values
  const CHAT_DATA_DOMAIN = "www.chat-data.com"; // For reseller plans, use your own domain with proper DNS configuration
  const CHAT_DATA_CHATBOT_ID = "YOUR_CHATBOT_ID";

  // Dynamically load the Chat Data SDK
  document.addEventListener('DOMContentLoaded', function() {
    // Create and append the main SDK script
    const sdkScript = document.createElement('script');
    sdkScript.src = "https://" + CHAT_DATA_DOMAIN + "/chatbotsdk.min.js";
    sdkScript.setAttribute('data-chatbot-id', CHAT_DATA_CHATBOT_ID);
    document.head.appendChild(sdkScript);
    
    // Create and append the embed script
    const embedScript = document.createElement('script');
    embedScript.src = "https://" + CHAT_DATA_DOMAIN + "/embed.min.js?chatbotId=" + CHAT_DATA_CHATBOT_ID;
    embedScript.defer = true;
    document.head.appendChild(embedScript);
  });
</script>
```

### 2. Sending User Information

To send user information:
1. Fill in the user information form fields
2. Enter any text in the "Additional Info" field (does not need to be JSON)
3. Click "Send User Info"
4. Check the event log for confirmation

### 3. Clearing User Information

To clear user information:
1. Click the "Clear User Info" button
2. Check the event log for confirmation

### 4. Adding Event Listeners

To add event listeners:
1. Click the corresponding button for the event type you want to listen for
2. Check the event log for confirmation
3. Trigger events in your chatbot to see them appear in the log

### 5. Removing Event Listeners

To remove all event listeners:
1. Click the "Remove All Listeners" button
2. Check the event log for confirmation

## SDK Integration Guide

The following guide shows how to integrate the Chat Data SDK into your own applications based on the implementation in this demo.

### 1. Basic Setup with Configuration Constants

```html
<script>
  // Configuration constants - Replace with your values
  const CHAT_DATA_DOMAIN = "www.chat-data.com"; // For reseller plans, use your own domain with proper DNS configuration
  const CHAT_DATA_CHATBOT_ID = "YOUR_CHATBOT_ID";

  // Dynamically load the Chat Data SDK
  document.addEventListener('DOMContentLoaded', function() {
    // Main SDK script
    const sdkScript = document.createElement('script');
    sdkScript.src = "https://" + CHAT_DATA_DOMAIN + "/chatbotsdk.min.js";
    sdkScript.setAttribute('data-chatbot-id', CHAT_DATA_CHATBOT_ID);
    document.head.appendChild(sdkScript);
    
    // Optional embed script (for the chat widget)
    const embedScript = document.createElement('script');
    embedScript.src = "https://" + CHAT_DATA_DOMAIN + "/embed.min.js?chatbotId=" + CHAT_DATA_CHATBOT_ID;
    embedScript.defer = true;
    document.head.appendChild(embedScript);
  });
</script>
```

### 2. Sending User Information

```javascript
// Simple example
window.chatbot.sendUserInfo({
  name: "John Doe",
  email: "user@example.com",
  phone: "555-123-4567",
  info: "Premium user since 2023-01-01"
}).then(response => {
  console.log("User info sent:", response);
}).catch(error => {
  console.error("Error sending user info:", error);
});

// Form submission example
document.getElementById('userForm').addEventListener('submit', function(event) {
  event.preventDefault();
  
  window.chatbot.sendUserInfo({
    name: document.getElementById('name').value,
    email: document.getElementById('email').value,
    phone: document.getElementById('phone').value,
    info: document.getElementById('info').value
  }).then(response => {
    console.log("User info sent successfully", response);
  }).catch(error => {
    console.error("Error sending user info", error.message);
  });
});
```

### 3. Clearing User Information

```javascript
window.chatbot.clearUserInfo()
  .then(response => {
    console.log("User info cleared successfully", response);
  })
  .catch(error => {
    console.error("Error clearing user info", error.message);
  });
```

### 4. Setting Up Event Listeners

```javascript
// Individual event listeners
function chatEventHandler(data) {
  console.log('Chat event received:', data);
}
window.chatbot.addEventListener('chat', chatEventHandler);

// Setting up multiple event listeners at once
function setupChatbotEventListeners() {
  const events = ["chat", "lead-submission", "live-chat-escalation", "minimize-widget"];
  
  events.forEach(eventName => {
    window.chatbot.addEventListener(eventName, function(data) {
      console.log(`${eventName} event received:`, data);
      // Process event data as needed
    });
  });
}
```

### 5. Removing Event Listeners

```javascript
// Remove a specific listener
window.chatbot.removeEventListener('chat', chatEventHandler);

// Remove multiple listeners
function removeAllListeners() {
  const events = ["chat", "lead-submission", "live-chat-escalation", "minimize-widget"];
  const handlers = {
    "chat": chatEventHandler,
    "lead-submission": leadSubmissionHandler,
    "live-chat-escalation": liveChatEscalationHandler,
    "minimize-widget": minimizeWidgetHandler
  };
  
  events.forEach(eventName => {
    window.chatbot.removeEventListener(eventName, handlers[eventName]);
  });
}
```

### 6. Error Handling Approaches

```javascript
// Option 1: Using try-catch blocks
try {
  window.chatbot.sendUserInfo({
    name: "John Doe",
    info: "Basic plan user"
  });
} catch (error) {
  console.error("Error using SDK:", error);
}

// Option 2: Using Promise error handling
window.chatbot.sendUserInfo({
  email: "user@example.com",
  phone: "555-123-4567",
  info: "Last visited: " + new Date().toLocaleDateString()
})
.then(response => {
  // Success handler
})
.catch(error => {
  // Error handler
})
.finally(() => {
  // Always executed
});
```

### 7. Common Usage Patterns

```javascript
// Sending user data after login
function onUserLogin(userData) {
  if (window.chatbot) {
    window.chatbot.sendUserInfo({
      name: userData.displayName,
      email: userData.email,
      phone: userData.phoneNumber,
      info: userData.accountType || "Standard user"
    }).then(() => {
      console.log("User info sent to chatbot after login");
    });
  }
}

// Getting the SDK configuration
function getSDKConfig() {
  if (window.chatbot && window.chatbot.getConfig) {
    const config = window.chatbot.getConfig();
    console.log("Current SDK configuration:", config);
    return config;
  }
  return null;
}
```

## Running the Demo Locally

After cloning the repository, you can run the demo locally using a simple HTTP server.

### Using Python (Recommended)

Python includes a built-in HTTP server that's perfect for testing:

#### Python 3.x:
```bash
# Navigate to the project directory
cd chat-data-sdk-demo

# Start a server on the default port 8000
python3 -m http.server

# Or specify a different port
python3 -m http.server 8080
```

#### Python 2.x:
```bash
# Navigate to the project directory
cd chat-data-sdk-demo

# Start a server on the default port 8000
python -m SimpleHTTPServer

# Or specify a different port
python -m SimpleHTTPServer 8080
```

### Accessing the Demo

Once the server is running:
1. Open a web browser
2. Navigate to: `http://localhost:8000` (or the port you specified)
3. The index.html file will be automatically served

### Stopping the Server

To stop the server, press `Ctrl+C` in the terminal where the server is running.

## Troubleshooting

### Common Issues

#### SDK Not Initializing
- Check browser console for errors
- Verify you've replaced "YOUR_CHATBOT_ID" with your actual chatbot ID
- Ensure the domain in the configuration constants is correct

#### User Information Not Sending
- Check if the SDK is properly initialized
- Look for error messages in the event log
- Verify that your chatbot ID is valid

#### Events Not Appearing
- Confirm that listeners are registered (check event log)
- Verify that the chatbot is generating events
- Check for cross-origin issues if testing locally

### Browser Console

For advanced troubleshooting, open your browser's developer console (F12 or Ctrl+Shift+I in most browsers) to see detailed logs and errors from the SDK. 