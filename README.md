# Cookies and Sessions Lab

A Flask API exercise that enforces a backend paywall using the browser session.
Users can view up to three articles before the server rejects additional requests with a `401` response.

## Overview

This app simulates a blog paywall that cannot be bypassed by editing frontend code in the browser. The backend stores a `page_views` counter in Flask's `session` object and blocks access once the user exceeds the limit.

## Features

- article data API at `/articles/<int:id>`
- per-browser page view tracking stored in the Flask session
- paywall enforcement after three article views
- `/clear` endpoint to reset the session counter during testing

## Tech Stack

- Flask 2.2.2
- Flask-SQLAlchemy 3.0.3
- Flask-Migrate 4.0.0
- React frontend in the `client/` directory

## Getting Started

### 1. Install dependencies

```bash
pipenv install
npm install --prefix client
```

### 2. Set up the database and seed data

```bash
cd server
flask db upgrade
python seed.py
```

### 3. Run the backend

```bash
python app.py
```

The API runs on `http://localhost:5555`.

### 4. Run the frontend

Open a second terminal:

```bash
npm start --prefix client
```

The frontend is configured to proxy to the Flask app on port `5555`.

## Paywall Behavior

- On the first article request, the app initializes `session['page_views']` to `0`.
- Each article request increments the counter by `1`.
- Requests are allowed while the value is `3` or less.
- Once the value is greater than `3`, the app returns:

```json
{"message": "Maximum pageview limit reached"}
```

with a `401 Unauthorized` status.

## Resetting the Session

Use the helper route below to clear the counter while testing:

```bash
http://localhost:5555/clear
```

## Testing

From the project root:

```bash
pytest
```

## Notes

This project keeps the paywall logic on the backend so the frontend cannot be manipulated by altering browser state or devtools.
