# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a client-side web application for "BCS Secure Jeopardy" - a game show buzzer system built with vanilla HTML, CSS, and JavaScript. The application consists of two main interfaces:

1. **Player Interface** (`index.html`) - For contestants to join games and buzz in
2. **Host Interface** (`host.html`) - For game hosts to control the game flow

## Architecture

### Technology Stack
- **Frontend**: Vanilla HTML5, CSS3, and JavaScript (ES6+)
- **Communication**: WebSocket connections for real-time multiplayer functionality
- **Audio**: Web Audio API for buzzer sounds
- **Effects**: CSS animations and canvas-confetti library for visual feedback

### Key Components

#### WebSocket Communication
- Both interfaces use WebSocket connections to communicate with a backend server
- Supports automatic reconnection with exponential backoff
- Heartbeat mechanism (PING/PONG) to maintain connections
- Handles both local (`ws://`) and secure (`wss://`) connections
- Auto-detects and formats ngrok URLs for development

#### Client Architecture (`index.html`)
- **JeopardyClient class**: Main client logic for player interactions
- **Login System**: Game code + username authentication
- **Buzzer System**: Real-time buzz-in functionality with audio feedback
- **Game States**: Regular play, Final Jeopardy, wager submission
- **Visual Feedback**: Background animations, confetti effects, penalty displays

#### Host Architecture (`host.html`)
- **JeopardyHost class**: Host control panel functionality
- **Game Control**: Start/stop buzzer acceptance, award points, boot players
- **Queue Management**: View buzz-in order and connected players
- **Special Modes**: Final Jeopardy, wager requests, game reset
- **Real-time Updates**: Live player status, wager submissions, final answers

### Key Features

#### Player Features
- Real-time buzzer with audio feedback
- Visual indicators for buzz-in status (first in queue glows red)
- Final Jeopardy answer submission
- Wager submission for Daily Double scenarios
- Penalty timeout system
- Mobile-responsive design

#### Host Features
- Toggle buzzer acceptance on/off
- Award points (WIN) or penalize players (BOOT)
- View real-time buzz-in queue
- Monitor connected players
- Request and view wagers
- Collect Final Jeopardy answers
- Reset entire game state

## Development

### File Structure
```
/
├── index.html          # Player interface
├── host.html          # Host interface  
├── README.md          # Basic project description
├── CNAME              # GitHub Pages domain config
└── LICENSE            # Project license
```

### Browser Compatibility
- Both interfaces include comprehensive polyfills for older browsers
- Supports WebSocket fallback detection
- Mobile-first responsive design
- Touch-friendly interfaces

### Key Classes and Functions

#### JeopardyClient (index.html)
- `connect()` - Establishes WebSocket connection
- `buzz()` - Sends buzz-in signal
- `sendFinalAnswer()` - Submits Final Jeopardy answer
- `sendWager()` - Submits wager amount
- `handlePenalty()` - Manages penalty timeout

#### JeopardyHost (host.html)
- `markWin()` - Awards point to current player
- `bootPlayer()` - Penalizes current player
- `startFinalJeopardy()` - Initiates Final Jeopardy mode
- `requestWagers()` - Requests wager submissions
- `resetGame()` - Resets all game state

### Message Protocol
WebSocket messages use a simple text-based protocol:
- `BUZZ` - Player buzz-in
- `WIN` - Host awards point
- `BOOT` - Host penalizes player
- `FINAL` - Start Final Jeopardy
- `WAGER_REQUEST` - Request wager submissions
- `FINAL_ANSWER:username:answer` - Submit Final Jeopardy answer
- `WAGER:username:amount` - Submit wager
- `RESET_GAME` - Reset game state

### Styling
- Dark theme with gold accents (#FFD700)
- Responsive design for mobile and desktop
- CSS animations for visual feedback
- Custom scrollbar styling for dark theme

## Deployment

This is a static web application designed for GitHub Pages deployment. The CNAME file suggests it's configured for a custom domain.

### Testing
- Test both interfaces with WebSocket connections
- Verify mobile responsiveness
- Test audio functionality (requires user interaction)
- Verify reconnection logic
- Test penalty timers and visual feedback

### Common Issues
- WebSocket connections may fail without proper CORS configuration
- Audio may be blocked by browser policies until user interaction
- Mobile browsers may have different touch behavior
- Reconnection attempts are limited to prevent infinite loops