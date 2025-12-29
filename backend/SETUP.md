# Quick Setup Guide

## Step-by-Step Setup Instructions

### 1. Navigate to Backend Directory
```bash
cd backend
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Create Environment File
Create a `.env` file in the `backend` directory with the following content:

```env
PORT=5000
NODE_ENV=development

# Get this from MongoDB Atlas
MONGODB_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/yatrasaathi?retryWrites=true&w=majority

# Generate a random secret (run: node -e "console.log(require('crypto').randomBytes(32).toString('hex'))")
JWT_SECRET=your_super_secret_jwt_key_here
JWT_EXPIRE=7d
```

### 4. MongoDB Atlas Setup

1. Go to https://www.mongodb.com/cloud/atlas
2. Sign up or log in
3. Create a new free cluster (or use existing)
4. Click "Connect" → "Connect your application"
5. Copy the connection string
6. Replace `<password>` with your database user password
7. Replace `<database_name>` with `yatrasaathi`
8. Add your IP address to the whitelist (or use 0.0.0.0/0 for development)

### 5. Start the Server
```bash
npm run dev
```

The server should start on `http://localhost:5000`

### 6. Test with Thunder Client

See the main README.md for detailed API endpoint documentation.

