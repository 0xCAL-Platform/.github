# 0xCAL

0xCAL is a decentralized mentoring platform built on blockchain technology that enables trustless escrow transactions for mentorship sessions. The platform connects mentors with mentees through an automated booking and payment system where funds are held in smart contracts until session completion.

## Project Overview

0xCAL addresses the fundamental trust and payment escrow problems in the mentoring industry by leveraging blockchain technology to create a transparent, automated, and trustless system for booking and paying for mentorship services.

### Key Value Propositions

- **Trustless Escrow**: Funds are held in smart contracts until session completion
- **Gasless Transactions**: Users can interact without owning ETH through meta-transaction technology
- **Automated Payments**: Smart contracts automatically distribute payments based on attendance tracking
- **Transparent Operations**: All bookings and transactions are recorded on-chain
- **Anti-No-Show Protection**: Financial incentives encourage attendance and discourage no-shows

## Technology Stack

### Smart Contracts & Blockchain
- **Solidity** (v0.8.29) for smart contract development
- **Foundry** for smart contract development, testing, and deployment
- **OpenZeppelin** contracts for security-reviewed implementations
- **Base Sepolia** testnet (Layer 2 Ethereum)
- **Chain ID**: 84532

### Frontend Platform
- **React 19** with **TypeScript**
- **Vite** for build tooling and development
- **TanStack Router** for file-based routing
- **TanStack Query** for server state management
- **Wagmi** and **Viem** for Ethereum blockchain interactions
- **Tailwind CSS** for styling
- **Radix UI** and **Shadcn/ui** for UI components

### Backend Infrastructure
- **OpenZeppelin Relayer SDK** for gasless meta-transactions
- **Redis** for caching and session storage
- **Neon Database** (PostgreSQL) for data persistence
- **Drizzle ORM** for database management
- **Better Auth** for authentication
- **Docker Compose** for service orchestration

### Development Tools
- **pnpm** for package management
- **TypeScript** throughout the stack
- **Vitest** for frontend testing
- **Biome** for linting and formatting

## Problem Statement & Solution

### Industry Problems

1. **Trust Issues**: Mentees pay mentors directly with no guarantee of service delivery
2. **Payment Disputes**: No neutral party to resolve payment disagreements
3. **No-Show Culture**: High rates of mentee and mentor no-shows
4. **Payment Timing**: Unclear when and how to pay for services
5. **Transaction Costs**: Direct blockchain transactions require gas fees

### 0xCAL's Solution

The platform implements a comprehensive escrow-based booking system:

1. **Commitment Fee System**: Mentees pay a commitment fee that is held in escrow
2. **Attendance Tracking**: Mentors mark sessions as attended or no-show
3. **Automated Distribution**:
   - **Attended Session**: 100% refund to mentee
   - **No-Show**: 90% to mentor (payment for time), 10% platform fee
4. **Gasless Operations**: Meta-transactions allow users to interact without ETH
5. **Username System**: User-friendly interface without requiring blockchain addresses

## Smart Contract Addresses

All contracts are deployed on **Base Sepolia Testnet** (Chain ID: 84532)

| Contract | Address | Purpose |
|----------|---------|---------|
| **BookingManager** | `0x32a7E8d63bA5Ed81068fF634afaa909A24C0f6Fe` | Core booking logic with meta-transaction support |
| **MentorRegistry** | `0xf02B641296dCA6617045fb5A4b755eB59082e2e1` | Mentor registration and username management |
| **MinimalForwarder** | `0x33bDF47c1b1B3AB3a9737Ffe413138Dd2789DdB4` | EIP-2771 meta-transaction forwarder |
| **MockIDRX** | `0x84d847D0e239b063fe57e2C4b74e756636305F43` | ERC20 payment token (2 decimal precision) |

### Contract Architecture

**BookingManager**: Handles session creation, booking, and payment distribution with meta-transaction support
**MentorRegistry**: Manages mentor registration, username allocation, and signature verification
**MinimalForwarder**: Enables gasless transactions through EIP-2771 meta-transaction standard
**MockIDRX**: ERC20 token with 2 decimal precision for payment processing

## Platform Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     0xCAL Platform                           │
├─────────────────────────────────────────────────────────────┤
│  Frontend Application (React + TypeScript)                  │
│  - Mentor Dashboard                                         │
│  - Mentee Booking Interface                                 │
│  - Session Management                                       │
│  - Meta-transaction Integration                             │
├─────────────────────────────────────────────────────────────┤
│  Relayer Infrastructure (OpenZeppelin)                      │
│  - Gasless Transaction Processing                           │
│  - Webhook Notifications (oxcal-platform.vercel.app)       │
│  - Rate Limiting (10 req/s, burst 50)                       │
├─────────────────────────────────────────────────────────────┤
│  Smart Contract Layer (Base Sepolia)                        │
│  ┌─────────────────┐  ┌─────────────────┐                  │
│  │   BookingMgr    │  │  MentorRegistry │                  │
│  │  (Meta-Tx)      │  │   (EIP-712)     │                  │
│  └─────────┬───────┘  └─────────────────┘                  │
│            │                                              │
│  ┌─────────▼──────────────────┐                           │
│  │           Vault            │                           │
│  │      (Escrow Logic)        │                           │
│  └────────────────────────────┘                           │
├─────────────────────────────────────────────────────────────┤
│  Data Layer                                                 │
│  - Neon PostgreSQL (Mentor profiles, sessions, bookings)   │
│  - Redis (Caching, session data, rate limiting)            │
└─────────────────────────────────────────────────────────────┘
```

### Business Flow

1. **Mentor Onboarding**: Mentors register with username and profile information
2. **Session Creation**: Mentors create available time slots
3. **Booking**: Mentees browse mentors and book sessions
4. **Payment**: Mentees pay commitment fee held in escrow (Vault contract)
5. **Session Execution**: Session occurs at scheduled time
6. **Attendance Confirmation**: Mentor marks attendance (attended or no-show)
7. **Automatic Distribution**: Smart contract distributes funds based on attendance

### Security Features

- **ReentrancyGuard**: Protection against reentrancy attacks
- **EIP-712**: Typed data signatures for meta-transactions
- **AccessControl**: Role-based permissions for contract administration
- **SafeERC20**: Safe token transfer operations
- **SignatureChecker**: Cryptographic signature validation
- **Meta-Transaction Security**: Rate limiting and signer verification

## Network Configuration

- **Network**: Base Sepolia (Testnet)
- **RPC Endpoint**: https://sepolia.base.org
- **Block Explorer**: https://sepolia.basescan.org
- **Chain ID**: 84532
- **Average Block Time**: ~2 seconds
- **Required Confirmations**: 3 blocks

## Project Structure

```
onecal/
├── contract/                 # Smart contracts
│   ├── src/                 # Contract source code
│   │   ├── BookingManager.sol
│   │   ├── MentorRegistry.sol
│   │   ├── MockIDRX.sol
│   │   ├── MinimalForwarder.sol
│   │   └── v1/             # Enhanced contract versions
│   ├── script/             # Deployment scripts
│   └── out/                # Compiled artifacts
├── platform/                # Frontend application
│   ├── src/
│   │   ├── routes/         # TanStack Router routes
│   │   ├── contracts/      # Contract interfaces
│   │   ├── hooks/          # Custom React hooks
│   │   ├── lib/            # Utilities
│   │   └── components/     # UI components
│   └── drizzle/            # Database schema
└── infrastructure/         # DevOps configuration
    ├── docker-compose.yml
    ├── relayer-config/     # OpenZeppelin relayer
    └── monitor-config/     # Health monitoring
```

## Version History

### Current Version (v1)
Enhanced contracts with the following features:
- Vault integration for escrow management
- SessionAcknowledgment tracking for attendance
- AccessControl for role-based permissions
- Enhanced security features and optimizations

## Development Status

The project is currently deployed on Base Sepolia testnet for testing and validation. All core features are implemented and functional:
- Mentor registration and profile management
- Session booking with escrow
- Meta-transaction support for gasless interactions
- Automated payment distribution
- Complete frontend interface

## Key Metrics

- **Platform Fee**: 10% on no-shows
- **Refund Rate**: 100% on attended sessions
- **Gas Costs**: Covered through relayer infrastructure
- **Transaction Time**: ~2 seconds (Base network)
- **Rate Limit**: 10 requests/second (burst 50)
