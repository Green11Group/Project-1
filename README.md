# Project-1

# GreenThumb

Certainly! Here's a new creative project idea with full information, similar to the previous examples:

Project: "GreenThumb" - An Interactive Urban Gardening and Sustainability Platform

GreenThumb is a platform that empowers urban dwellers to create and manage their own gardens, connect with local gardening communities, and contribute to urban sustainability efforts. The project consists of 5 distinct microservices:

1. User Service:
Manages user accounts, authentication, and profiles. It handles user registration, login, and profile management for gardeners and sustainability enthusiasts.
2. API Gateway:
Acts as the single entry point for all client requests, handling authentication, rate limiting, and routing to appropriate microservices.
3. Garden Management Service:
Enables users to plan, create, and manage their urban gardens. It provides tools for plant selection, care scheduling, and progress tracking.
4. Community Service:
Facilitates connections between local gardeners, manages gardening events, and enables knowledge sharing through forums and collaborative projects.
5. Sustainability Impact Service:
Tracks the environmental impact of users' gardening efforts, provides educational resources, and gamifies sustainability goals.

Database Schemas with Enum Types:

1. User Service Database:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE TYPE user_expertise AS ENUM ('beginner', 'intermediate', 'advanced', 'expert');

CREATE TABLE user_profiles (
    user_id INTEGER PRIMARY KEY REFERENCES users(id),
    full_name VARCHAR(100),
    bio TEXT,
    expertise user_expertise DEFAULT 'beginner',
    location VARCHAR(100),
    avatar_url VARCHAR(255)
);

```

1. Garden Management Service Database:

```sql
CREATE TYPE garden_type AS ENUM ('balcony', 'rooftop', 'indoor', 'community', 'backyard');
CREATE TYPE plant_status AS ENUM ('planned', 'planted', 'growing', 'harvesting', 'dormant');

CREATE TABLE gardens (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    name VARCHAR(100) NOT NULL,
    type garden_type,
    area_sqm DECIMAL(6, 2),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE plants (
    id SERIAL PRIMARY KEY,
    garden_id INTEGER REFERENCES gardens(id),
    species VARCHAR(100) NOT NULL,
    quantity INTEGER,
    planting_date DATE,
    status plant_status DEFAULT 'planned'
);

CREATE TABLE care_logs (
    id SERIAL PRIMARY KEY,
    plant_id INTEGER REFERENCES plants(id),
    action VARCHAR(50),
    notes TEXT,
    logged_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

```

1. Community Service Database:

```sql
CREATE TYPE event_type AS ENUM ('workshop', 'seed_exchange', 'community_planting', 'farmers_market');

CREATE TABLE communities (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    location VARCHAR(100),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE community_members (
    community_id INTEGER REFERENCES communities(id),
    user_id INTEGER REFERENCES users(id),
    joined_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (community_id, user_id)
);

CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    community_id INTEGER REFERENCES communities(id),
    name VARCHAR(100) NOT NULL,
    description TEXT,
    type event_type,
    start_time TIMESTAMP WITH TIME ZONE,
    end_time TIMESTAMP WITH TIME ZONE,
    location VARCHAR(255)
);

CREATE TABLE forum_posts (
    id SERIAL PRIMARY KEY,
    community_id INTEGER REFERENCES communities(id),
    user_id INTEGER REFERENCES users(id),
    title VARCHAR(255) NOT NULL,
    content TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

```

1. Sustainability Impact Service Database:

```sql
CREATE TYPE impact_category AS ENUM ('water_saved', 'co2_reduced', 'food_produced', 'waste_composted');

CREATE TABLE impact_logs (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    category impact_category,
    amount DECIMAL(10, 2),
    unit VARCHAR(20),
    logged_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE sustainability_challenges (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    goal_amount DECIMAL(10, 2),
    goal_unit VARCHAR(20),
    start_date DATE,
    end_date DATE
);

CREATE TABLE user_challenges (
    user_id INTEGER REFERENCES users(id),
    challenge_id INTEGER REFERENCES sustainability_challenges(id),
    progress DECIMAL(10, 2) DEFAULT 0,
    completed_at TIMESTAMP WITH TIME ZONE,
    PRIMARY KEY (user_id, challenge_id)
);

```

Updated API Gateway Endpoints:

1. Authentication:
    - POST /api/auth/register
    - POST /api/auth/login
    - POST /api/auth/logout
    - GET /api/auth/refresh-token
2. User Management:
    - GET /api/users/{id}
    - PUT /api/users/{id}
    - DELETE /api/users/{id}
    - GET /api/users/{id}/profile
    - PUT /api/users/{id}/profile
3. Garden Management:
    - POST /api/gardens
    - GET /api/gardens/{id}
    - PUT /api/gardens/{id}
    - DELETE /api/gardens/{id}
    - GET /api/users/{id}/gardens
    - POST /api/gardens/{id}/plants
    - GET /api/gardens/{id}/plants
    - PUT /api/plants/{id}
    - DELETE /api/plants/{id}
    - POST /api/plants/{id}/care-logs
    - GET /api/plants/{id}/care-logs
4. Community:
    - POST /api/communities
    - GET /api/communities/{id}
    - PUT /api/communities/{id}
    - DELETE /api/communities/{id}
    - GET /api/communities
    - POST /api/communities/{id}/join
    - POST /api/communities/{id}/leave
    - POST /api/communities/{id}/events
    - GET /api/communities/{id}/events
    - POST /api/communities/{id}/forum
    - GET /api/communities/{id}/forum
    - POST /api/forum/{id}/comments
    - GET /api/forum/{id}/comments
5. Sustainability Impact:
    - POST /api/impact/log
    - GET /api/users/{id}/impact
    - GET /api/communities/{id}/impact
    - GET /api/challenges
    - POST /api/challenges/{id}/join
    - PUT /api/challenges/{id}/progress
    - GET /api/users/{id}/challenges
    - GET /api/leaderboard/users
    - GET /api/leaderboard/communities
6. Misc:
    - GET /api/health
    - GET /api/version

GreenThumb Project:

Day 1:

1. Set up project infrastructure and development environment
2. Implement User Service: database schema and basic CRUD operations
3. Start API Gateway: basic routing and authentication setup

Day 2:

1. Complete API Gateway: implement rate limiting and finalize routing
2. Implement Garden Management Service: database schema and basic CRUD operations
3. Begin Community Service: database schema and community creation functionality

Day 3:

1. Complete Community Service: implement event management and forum features
2. Start Sustainability Impact Service: database schema and impact logging
3. Integrate User Service and API Gateway

Day 4:

1. Complete Sustainability Impact Service: implement challenges and leaderboards
2. Integrate Garden Management Service and API Gateway
3. Begin front-end development: user profile and garden planning pages

Day 5:

1. Integrate Community Service and Sustainability Impact Service with API Gateway
2. Continue front-end development: community features and sustainability tracking
3. Implement basic plant care recommendation system
