# Demo Walkthrough (3-5 minutes)

## Story

"A member hits sold out on a preferred stay; the system recovers the booking with high-fit alternatives."

## Sequence

1. Send a sold-out request to `POST /match`.
2. Show top 3 alternatives and "why this match" reasons.
3. Show waitlist likelihood score for original request.
4. Submit accept/reject signal to `POST /feedback`.
5. Show eval summary (recovery rate, rank quality, latency).
