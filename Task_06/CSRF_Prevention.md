# Cross-Site Request Forgery Protection in Angular

- Open the application `csrf-vulnerable-site`
- Install the dependencies `npm install`
- To start the Server run `npm start` from the root directory
- The application is up and running on `http://localhost:3000` (if you change something in the Angular app you need to stop the running Web Server and start it again with `npm start`)
- You can login with username `admin` and password `admin`
- Stay logged in

### Execute Cross-Site Request Forgery Attack

- Open the application `csrf-attacker-server`
- Install the dependencies `npm install`
- To start the Server run `npm start` from the root directory
- The application is up and running on `http://localhost:1111` open the Browser and visit this URL
- If you visit your profile-Page again you should see, that the data has changed

### Cross-Site Request Forgery Attack Mitigation

- Add CSRF-Prevention inside the Angular App to your HttpClient: `provideHttpClient(withXsrfConfiguration({}))`
- Open `server/server.js` and implement the Todos inside the `app.post('/api/profile', function (req, res) {}`-Handler
- Investigate your Requests (by Opening the Network Tap on your Browser) and make sure the XSRF-Token is send

### Hints

```typescript
//app.config.ts
providers: [
  provideHttpClient(withXsrfConfiguration({})),
  provideRouter(routes, withComponentInputBinding()),
];
```

```javascript
//server.js
const cookie = req.cookies["XSRF-TOKEN"];
if (req.headers["x-xsrf-token"] !== cookie) {
  res.status(403).send({ text: "Invalid CSRF Token" });
  return;
}
```

[Solution](https://github.com/martinakraus/angular-security-intro-2025/commit/51d4a875e5c4986c64c07721273bf5f451865e5b)
