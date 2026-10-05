# Accountill Observations

- The server uses Express 4.17.1 and Mongoose 5.12.10 with Node.js ES Modules.
  Evidence: server/package.json:7-22 [Confirmed]

- The auth middleware is implemented in middleware/auth.js but is never imported or attached to any route handler in the entire server.
  Evidence: server/middleware/auth.js:7 [Confirmed]

- PDF invoice generation writes to a single shared file named invoice.pdf on the server disk, causing race conditions and cross-user data leakage.
  Evidence: server/index.js:57-70 [Confirmed]

- GET /clients queries and returns paginated client records across all users in the database without any tenant filtering or authorization.
  Evidence: server/controllers/clients.js:44-47 [Confirmed]

- The README claims the client uses `react-google-login` for authentication, but client/package.json actually uses `@react-oauth/google` version ^0.9.0.
  Evidence: client/package.json:14 [Confirmed]

## Corrections
- Claim: "Authentication is enforced on all API endpoints using JWT middleware."
  Correction: Tagged [Guess / False]. Re-reading `server/routes/invoices.js:1-14` and `server/routes/clients.js:1-12` proves `auth` is not imported or used anywhere. All endpoints are completely unauthenticated.
- Claim: "Client authentication is implemented via react-google-login as stated in the README."
  Correction: Tagged [Guess / False]. README.md:56 claimed `React-google-login`, but client/package.json:14 shows `@react-oauth/google: ^0.9.0`.
