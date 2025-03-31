### Explanation of the Database Schema

This schema defines two tables: `users` and `game_servers`, each serving specific purposes for a system that deals with user management and game server configurations.

---

#### **Users Table**
The `users` table holds the data for each user registered in the system. Below are the details for each column:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,  -- Auto-incrementing integer ID
    uuid UUID UNIQUE DEFAULT gen_random_uuid(),  -- Unique UUID for each user
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    balance DECIMAL(10,2) DEFAULT 0.00 CHECK (balance >= 0),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_login_ip INET
);
```

1. **id** (SERIAL PRIMARY KEY):  
   - The `id` is an auto-incrementing integer used as a unique identifier for each user.
   - It is defined as the primary key of the table, ensuring no two users will have the same ID.

2. **uuid** (UUID UNIQUE DEFAULT gen_random_uuid()):  
   - A globally unique identifier (UUID) is assigned to each user.
   - The `gen_random_uuid()` function generates a random UUID when a new user is created, guaranteeing the uniqueness of each `uuid`.
   - The UUID ensures the user can be uniquely identified across different systems and databases.
   
3. **username** (VARCHAR(50) UNIQUE NOT NULL):  
   - A string representing the user's name in the system, limited to 50 characters.
   - The `UNIQUE` constraint ensures no two users can have the same username.
   - It cannot be null, meaning every user must have a username.

4. **email** (VARCHAR(255) UNIQUE NOT NULL):  
   - A string to store the user's email address, with a maximum length of 255 characters.
   - It is marked `UNIQUE`, preventing duplicate email addresses in the database.
   - This field cannot be null, requiring an email address for each user.

5. **password_hash** (TEXT NOT NULL):  
   - Stores the hashed password of the user, ensuring sensitive data like passwords are not stored directly.
   - The `NOT NULL` constraint ensures that every user must have a password hash upon creation.

6. **balance** (DECIMAL(10,2) DEFAULT 0.00 CHECK (balance >= 0)):  
   - A numeric value representing the user’s balance, stored as a decimal with two decimal places.
   - The `DEFAULT 0.00` sets the initial balance to 0.
   - The `CHECK (balance >= 0)` constraint ensures that the balance cannot go negative, enforcing valid financial data.

7. **created_at** (TIMESTAMP DEFAULT CURRENT_TIMESTAMP):  
   - Stores the timestamp of when the user was created in the system.
   - The default value is the current timestamp at the time of insertion, automatically recording when the user registers.

8. **last_login_ip** (INET):  
   - This field stores the last known IP address the user logged in from.
   - The `INET` type is used to store both IPv4 and IPv6 addresses, ensuring compatibility with various network protocols.

---

#### **Game Servers Table**
The `game_servers` table stores information about the game servers in the system. Each server is associated with an owner (a user). Here's the breakdown of its columns:

```sql
CREATE TABLE game_servers (
    id SERIAL PRIMARY KEY,  -- Auto-incrementing integer ID
    uuid UUID UNIQUE DEFAULT gen_random_uuid(),  -- Unique UUID for each server
    owner_id UUID NOT NULL REFERENCES users(uuid) ON DELETE CASCADE,  -- Reference to users.uuid
    server_name VARCHAR(100) UNIQUE NOT NULL,
    expiration_time TIMESTAMP NOT NULL,
    game_version VARCHAR(50) NOT NULL,
    domain VARCHAR(255) UNIQUE NOT NULL,
    ip INET NOT NULL,  -- Using INET type for both IPv4 and IPv6
    port INTEGER NOT NULL CHECK (port BETWEEN 1 AND 65535),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT unique_ip_port UNIQUE (ip, port) -- Prevent duplicate IP:Port pairs
);
```

1. **id** (SERIAL PRIMARY KEY):  
   - The `id` is an auto-incrementing integer uniquely identifying each game server.
   - It is defined as the primary key, ensuring no two servers can have the same `id`.

2. **uuid** (UUID UNIQUE DEFAULT gen_random_uuid()):  
   - A unique UUID for each game server, ensuring the identification of servers across different systems.
   - `gen_random_uuid()` automatically generates a random UUID when a new server is created.

3. **owner_id** (UUID NOT NULL REFERENCES users(uuid) ON DELETE CASCADE):  
   - This is a foreign key reference to the `uuid` in the `users` table, linking each server to its owner.
   - The `ON DELETE CASCADE` clause ensures that if a user is deleted, all their associated game servers are automatically deleted as well.

4. **server_name** (VARCHAR(100) UNIQUE NOT NULL):  
   - The name of the game server, restricted to 100 characters.
   - The `UNIQUE` constraint ensures that no two servers can share the same name.
   - This field cannot be null, meaning each game server must have a name.

5. **expiration_time** (TIMESTAMP NOT NULL):  
   - The `expiration_time` represents when the game server will expire or be decommissioned.
   - It must always have a valid timestamp, ensuring every server has a specified expiration time.

6. **game_version** (VARCHAR(50) NOT NULL):  
   - Specifies the version of the game running on the server.
   - It is a required field, ensuring the game version is always recorded for each server.

7. **domain** (VARCHAR(255) UNIQUE NOT NULL):  
   - A string representing the domain of the game server, up to 255 characters.
   - The `UNIQUE` constraint ensures that no two servers can have the same domain name.

8. **ip** (INET NOT NULL):  
   - The IP address of the game server, stored using the `INET` type, which can hold both IPv4 and IPv6 addresses.
   - This field cannot be null, ensuring every game server has a valid IP address.

9. **port** (INTEGER NOT NULL CHECK (port BETWEEN 1 AND 65535)):  
   - The port number on which the game server is accessible.
   - The `CHECK` constraint ensures that the port value is between 1 and 65535, adhering to valid port ranges for TCP/IP communication.

10. **created_at** (TIMESTAMP DEFAULT CURRENT_TIMESTAMP):  
    - The timestamp indicating when the game server was created.
    - The default value is the current timestamp, automatically recording the server's creation time.

11. **unique_ip_port** (CONSTRAINT UNIQUE (ip, port)):  
    - This constraint ensures that each combination of `ip` and `port` is unique, preventing multiple servers from sharing the same IP address and port combination.

---

### Summary
This schema effectively organizes users and their associated game servers, with each table containing essential fields to manage user authentication, game server configurations, and security constraints. The use of UUIDs ensures global uniqueness, and checks on balances, port numbers, and IP addresses ensure data integrity and consistency. The relationship between users and game servers is managed through foreign keys, with cascading deletions to maintain referential integrity.
