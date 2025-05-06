# Chat Data SDK Demo Guide

This document provides instructions for using the Chat Data SDK demo application (`chatbot-sdk-demo.html`). The demo showcases the features of the Chat Data JavaScript SDK and helps developers understand how to integrate it into their applications.

## Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
- [Demo Features](#demo-features)
- [Using the Demo](#using-the-demo)
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

### Running the Demo

1. Open `chatbot-sdk-demo.html` in your web browser
2. The SDK initializes automatically with the demo chatbot ID
3. Once initialized, you can:
   - Send/clear user information
   - Add event listeners
   - Monitor events in the event log

## Demo Features

### SDK Configuration

The demo automatically loads the SDK with a pre-configured chatbot ID:
- The SDK is loaded from "https://www.chat-data.com/chatbotsdk.min.js"
- The demo uses the chatbot ID: 664ff53d91ad3b333d283502

The current configuration is displayed in the "SDK Configuration" section, showing the values from the SDK's getConfig() method.

### User Information Management

The demo includes a form to send user information to the chatbot:
- User ID: A unique identifier for the user
- Email: The user's email address
- Name: The user's full name
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

The SDK initializes automatically when the page loads using the data-chatbot-id attribute in the script tag. You can check the current configuration in the "SDK Configuration" section.

```html
<script src="https://www.chat-data.com/chatbotsdk.min.js" data-chatbot-id="664ff53d91ad3b333d283502"></script>
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

### 6. Implementation Example

The demo includes a comprehensive implementation example showing how to:
- Initialize the SDK (both automatically and manually)
- Send and clear user information
- Add and remove event listeners
- Access configuration
- Handle errors properly

This example can be copied directly into your own application.

## Troubleshooting

### Common Issues

#### SDK Not Initializing
- Check browser console for errors
- Verify the chatbot ID is correct
- Ensure the SDK script is loading properly

#### User Information Not Sending
- Check if the SDK is properly initialized
- Look for error messages in the event log

#### Events Not Appearing
- Confirm that listeners are registered (check event log)
- Verify that the chatbot is generating events
- Check for cross-origin issues if testing locally

### Browser Console

For advanced troubleshooting, open your browser's developer console (F12 or Ctrl+Shift+I in most browsers) to see detailed logs and errors from the SDK. 