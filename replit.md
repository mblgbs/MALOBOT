# Discord Economy Bot

## Overview

This is a Discord bot that simulates a virtual economy game where users can open bank accounts, become vendors, and trade items. The bot uses Discord.js v14 for Discord integration and MongoDB with Mongoose for data persistence. The economy system includes banking operations, a marketplace for trading items, role-based functionality for vendors and customers, and a comprehensive gamified reputation system with achievements, levels, and daily rewards.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Backend Architecture
- **Framework**: Node.js with Discord.js v14
- **Database**: MongoDB with Mongoose ODM
- **Structure**: Modular command-based architecture with event-driven interaction handling
- **Language**: JavaScript (ES6+)

### Database Design
- **Primary Models**: User, Order, and Reputation schemas
- **User Model**: Stores user profiles with banking info, wallet balance, stock inventory, and role permissions
- **Order Model**: Manages marketplace transactions between buyers and sellers
- **Reputation Model**: Tracks user reputation scores, achievements, experience points, trading statistics, and badges
- **Storage**: MongoDB with connection pooling and error handling

### Command System
- **Slash Commands**: Modern Discord slash command implementation
- **Categories**: 
  - Banking commands (`/ouvrir_compte`, `/deposer`, `/retirer`, `/balance`, `/bilan`)
  - Marketplace commands (`/acheter`)
  - Vendor commands (`/devenir_vendeur`)
  - Reputation commands (`/reputation`, `/leaderboard`, `/achievements`, `/daily`)
- **Auto-deployment**: Commands are automatically registered on bot startup

## Key Components

### Core Bot Structure
- **Entry Point**: `index.js` - Initializes Discord client, loads commands/events, connects to database
- **Event System**: Handles Discord events (ready, interactions) with modular event files
- **Command Loading**: Dynamic command discovery from organized folder structure

### User Management
- **Banking System**: Virtual wallet and bank account with transaction history
- **Role System**: Client vs Vendor roles with different permissions
- **Stock Management**: Vendors can manage inventory using Map-based storage

### Interaction Handling
- **Button Handler**: Manages button interactions for shop management and order processing
- **Modal Handler**: Processes form submissions for adding items and complex inputs
- **Command Execution**: Centralized error handling and response management

### Marketplace System
- **Order Management**: Complete order lifecycle (pending → accepted → delivered)
- **Stock System**: Vendors maintain private inventories with pricing
- **Shop Channels**: Each vendor gets a private Discord channel for their shop

### Reputation System
- **Reputation Scores**: 0-1000 point system with tier rankings (Poor to Legendary)
- **Experience & Levels**: XP-based progression system with level bonuses
- **Achievements**: 6 unlockable achievements with experience rewards
- **Trading Statistics**: Tracks sales, purchases, volume, and cancellations
- **Badges**: Dynamic badge system based on user behavior
- **Daily Rewards**: 24-hour cooldown rewards with streak bonuses
- **Leaderboards**: Multi-category ranking system

## Data Flow

### User Registration Flow
1. User runs `/ouvrir_compte` command
2. System creates User document with default wallet (1000 currency)
3. Bank account initialized with zero balance and empty transaction history
4. Success embed sent with available banking commands

### Vendor Registration Flow
1. User runs `/devenir_vendeur` command
2. System creates private Discord channel for vendor's shop
3. User role updated to 'vendeur' in database
4. Vendor role assigned in Discord server
5. Shop management interface provided via buttons

### Purchase Flow
1. Buyer runs `/acheter` with item name and quantity
2. System searches for vendors with item in stock
3. Order created with 'pending' status
4. Seller receives notification with accept/reject buttons
5. On acceptance, funds transferred and order marked 'delivered'

## External Dependencies

### Discord.js v14
- **Purpose**: Discord API interaction and bot functionality
- **Features Used**: Slash commands, embeds, buttons, modals, permission management
- **Integration**: Event-driven architecture with interaction handling

### Mongoose v8.16.4
- **Purpose**: MongoDB object modeling and database operations
- **Features Used**: Schema validation, middleware, connection management
- **Integration**: Model-based data access with error handling

### MongoDB
- **Purpose**: Primary data storage for user accounts, orders, and transaction history
- **Connection**: Environment-based URI with fallback to localhost
- **Configuration**: Connection pooling enabled with automatic reconnection

## Deployment Strategy

### Environment Configuration
- **Discord Token**: `DISCORD_TOKEN` environment variable required
- **MongoDB URI**: `MONGODB_URI` with localhost fallback
- **Guild/Client IDs**: Optional for faster command deployment during development

### Bot Permissions
- **Required Intents**: Guilds, GuildMessages, MessageContent, GuildMembers
- **Channel Management**: Creates private vendor channels with specific permissions
- **Role Management**: Creates and assigns vendor roles automatically

### Error Handling
- **Database Errors**: Graceful fallback with user-friendly error messages
- **Command Errors**: Comprehensive error catching with detailed logging
- **Interaction Errors**: Proper response handling for replied/deferred interactions

### Scalability Considerations
- **Command Structure**: Modular folder-based organization for easy expansion
- **Database Design**: Flexible schema allowing for future feature additions
- **Event System**: Extensible event handling for new Discord features