# instagram-reach-api

A small FastAPI service that estimates the engagement ("reach") of a public Instagram account from its username.

Originally named Insta-ReachBot.

## Features

- `GET /u/{username}` fetches the public profile data from Instagram's web profile endpoint and returns: followers, average likes and comments over the latest posts, and a "Total Reach" percentage.
- `GET /` returns a pointer to the interactive docs at `/docs`.

The "Total Reach" value is `followers / average(likes + comments)` as a whole-number percent, as written in `bot.py`. It is a rough figure, not an official metric.

## Tech stack

Python, FastAPI, Uvicorn, requests, rich.

## Project structure

```
main.py           FastAPI app and routes
bot.py            Instagram request and metric calculation
requirements.txt
```

## Setup and run

Requirements: Python 3.8 or newer.

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload
```

Optional environment variable:

| Name | Purpose |
|---|---|
| `INSTAGRAM_PROXY` | HTTPS proxy as `host:port`, used for requests to Instagram. Unset means a direct request |

## Usage

```bash
curl http://127.0.0.1:8000/u/instagram
```

The response is a list containing the result object three times (the route returns it that way).

## Status and limitations

- Likely not working today: the code relies on an unauthenticated Instagram endpoint and a 2019-era mobile user agent. Instagram now blocks or rate-limits this, so expect an error string back.
- The route returns the same result three times, and errors are returned as a string with HTTP 200.
- The code divides by the engagement total, so an account whose recent posts have zero likes and comments raises an error (returned as a string).
- Not tested against the live service in this cleanup, only compiled.
- Respect Instagram's terms of use before pointing this at real accounts.
