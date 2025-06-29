# Topcoder Member API - Migration Project

This project migrates the Topcoder Member API from a complex multi-database architecture (Informix, DynamoDB, ElasticSearch) to a simplified **Prisma + PostgreSQL** setup.

## 🎯 Project Goals

- **Simplify Architecture**: Remove dependencies on Informix, DynamoDB, and ElasticSearch
- **Single Database**: Use PostgreSQL with Prisma ORM as the sole backend
- **Remove Complexity**: Eliminate DAL/gRPC dependencies
- **Maintain Compatibility**: Preserve existing API behavior and response formats
- **Focus Scope**: Update only SearchController and StatisticsController

## 🏗️ Architecture

### Before (Complex)
```
API Layer → DAL/gRPC → Informix + DynamoDB + ElasticSearch
```

### After (Simplified)
```
API Layer → Prisma ORM → PostgreSQL
```

## 🚀 Quick Start

> **📖 For detailed setup instructions, see [Development Setup Guide](docs/development-setup.md)**

### Prerequisites

- Node.js 18+ 
- PostgreSQL 16.3+ (or use Docker)
- npm or yarn

### Installation

1. **Clone and install dependencies**
   ```bash
   git clone <repository-url>
   cd member-api
   npm install
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your database credentials
   ```

3. **Set up database (Docker - Recommended)**
   ```bash
   # Start PostgreSQL with Docker
   docker-compose up -d postgres
   ```
   
   **OR set up PostgreSQL manually** (see [Development Setup Guide](docs/development-setup.md))

4. **Set up database**
   ```bash
   # Start PostgreSQL service
   # Create database: member_api
   
   # Generate Prisma client
   npm run prisma:generate
   
   # Run migrations
   npm run prisma:migrate
   ```

5. **Seed test data**
   ```bash
   # Fetch data from live API (optional)
   npm run data:fetch
   
   # Seed database
   npm run prisma:seed
   ```

6. **Start development server**
   ```bash
   npm run dev
   ```

## 📁 Project Structure

```
src/
├── controllers/          # API controllers (SearchController, StatisticsController)
├── routes/              # Express routes
├── services/            # Business logic services
├── middlewares/         # Express middlewares
├── utils/               # Utility functions
└── index.ts             # Application entry point

prisma/
├── schema.prisma        # Database schema
└── migrations/          # Database migrations

scripts/
├── fetchTestData.ts     # Fetch data from live API
└── seed.ts              # Database seeding

tests/
├── setup.ts             # Test configuration
└── **/*.test.ts         # Test files

docs/                    # API documentation
```

## 🔧 Available Scripts

| Script | Description |
|--------|-------------|
| `npm run dev` | Start development server with hot reload |
| `npm run build` | Build production bundle |
| `npm run start` | Start production server |
| `npm test` | Run tests |
| `npm run test:watch` | Run tests in watch mode |
| `npm run test:coverage` | Run tests with coverage |
| `npm run prisma:generate` | Generate Prisma client |
| `npm run prisma:migrate` | Run database migrations |
| `npm run prisma:studio` | Open Prisma Studio |
| `npm run prisma:seed` | Seed database with test data |
| `npm run data:fetch` | Fetch test data from live API |
| `npm run lint` | Run ESLint |
| `npm run lint:fix` | Fix ESLint errors |

## 🗄️ Database Schema

The Prisma schema includes:

- **Member**: Core member information
- **Track**: Development/competitive tracks  
- **SubTrack**: Specialized areas within tracks
- **Skill**: Technical and soft skills
- **Achievement**: Badges, certifications, awards
- **Statistics**: Member performance metrics

See `prisma/schema.prisma` for detailed schema definitions.

## 📊 API Endpoints

### Search Operations
- `GET /v5/members` - Search members with filters
- `GET /v5/members/:handle` - Get member by handle

### Statistics Operations  
- `GET /v5/members/statistics` - General member statistics
- `GET /v5/members/statistics/skills` - Skills distribution
- `GET /v5/members/statistics/tracks` - Track/subtrack distribution
- `GET /v5/members/statistics/countries` - Geographic distribution

### Health & Info
- `GET /health` - Health check
- `GET /` - API information

## 🧪 Testing

### Setup Test Data

1. **Fetch from live API**:
   ```bash
   npm run data:fetch
   ```

2. **Seed local database**:
   ```bash
   npm run prisma:seed
   ```

### Run Tests
```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run in watch mode
npm run test:watch
```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `NODE_ENV` | Environment | `development` |
| `PORT` | Server port | `3000` |
| `DATABASE_URL` | PostgreSQL connection string | Required |
| `LOG_LEVEL` | Logging level | `debug` |
| `CORS_ORIGIN` | CORS origin | `*` |

### Database Connection

Update `DATABASE_URL` in `.env`:
```
DATABASE_URL="postgresql://username:password@localhost:5432/member_api?schema=public"
```

## 🔄 Migration Status

### ✅ Completed
- [x] Project setup and structure
- [x] Prisma schema design
- [x] Basic API framework
- [x] Development environment setup
- [x] Test data fetching script

### 🚧 In Progress
- [ ] SearchController implementation
- [ ] StatisticsController implementation
- [ ] Database seeding with real data
- [ ] Unit and integration tests

### ⏳ TODO
- [ ] API documentation (OpenAPI/Swagger)
- [ ] Performance optimization
- [ ] Error handling improvements
- [ ] Deployment configuration
- [ ] Monitoring and logging

## 🔍 Key Migration Points

1. **Database Queries**: All Informix/ES queries converted to Prisma
2. **Data Models**: Normalized schema replacing multiple data stores
3. **API Responses**: Maintained compatibility with existing format
4. **Performance**: Optimized with proper indexing and query patterns
5. **Testing**: Comprehensive test coverage for business logic

## 🤝 Development Guidelines

1. **API Compatibility**: Always test against live API responses
2. **Database First**: Use Prisma migrations for schema changes
3. **Testing**: Write tests for all new functionality
4. **Performance**: Monitor query performance and optimize
5. **Documentation**: Keep README and API docs updated

## 📝 Notes

- This migration focuses on **SearchController** and **StatisticsController** only
- Business logic must remain unchanged
- API response formats must match existing endpoints
- Performance should be maintained or improved
- All legacy dependencies (Informix, ES, Dynamo) must be removed

## 🔗 Resources

- [Original Member API](https://github.com/topcoder-platform/member-api/tree/develop)
- [Live API Endpoint](https://api.topcoder-dev.com/v5/members)
- [Prisma Documentation](https://www.prisma.io/docs/)
- [Challenge Requirements](./docs/challenge-requirements.md)

---

**Challenge ID**: 30377262  
**Tech Stack**: Node.js, TypeScript, Prisma, PostgreSQL, Express.js
