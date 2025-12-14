# PolyGlotMeet Backend

Secure backend API for PolyGlotMeet video conferencing app.

## Features

- **LiveKit Integration**: WebRTC SFU for scalable video meetings
- **Supabase Database**: Meeting metadata and room management
- **JWT Token Generation**: Secure access control
- **Railway Ready**: Optimized for cloud deployment

## API Endpoints

### `GET /health`
Health check endpoint for monitoring

### `POST /create-meeting`
Creates a new meeting and returns join credentials
```json
{
  "userId": "user-id-here"
}
```

### `POST /join-meeting`
Joins an existing meeting
```json
{
  "meetingId": "abc-xyz-123",
  "password": "meeting-password",
  "userId": "user-id-here"
}
```

### `POST /end-meeting`
Ends a meeting (host only)
```json
{
  "meetingId": "abc-xyz-123",
  "userId": "host-user-id"
}
```

## Environment Variables

Required environment variables:

```env
LIVEKIT_API_KEY=your_livekit_api_key
LIVEKIT_API_SECRET=your_livekit_secret
LIVEKIT_URL=wss://your-project.livekit.cloud
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_KEY=your_service_role_key
```

## Local Development

```bash
npm install
npm start
```

Server runs on `http://localhost:3000`

## Railway Deployment

1. Push this repo to GitHub
2. Create new Railway project
3. Connect GitHub repo
4. Add environment variables in Railway dashboard
5. Deploy

Railway will automatically:
- Install dependencies
- Run `npm start`
- Assign a public URL

## Database Schema

Required Supabase table:

```sql
create table meetings (
  id uuid default gen_random_uuid() primary key,
  meeting_id text not null unique,
  password text not null,
  host_id uuid not null,
  livekit_room text not null,
  is_active boolean default true,
  created_at timestamp with time zone default now(),
  expires_at timestamp with time zone default now() + interval '24 hours'
);
```

## Architecture

This backend ensures **same meeting code = same LiveKit room** by:
1. Storing canonical room names in Supabase
2. Issuing tokens for the exact same room
3. Preventing client-side room creation

## License

MIT
