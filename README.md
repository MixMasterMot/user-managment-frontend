## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

This is the frontend for the user management application.

It uses the Vue composition application
There is some basic styling on the home page :)

The frontend uses:
- pinia for state management
- vee-validator and yup for form validation

The validation on the user edit view did not want to play along so I removed it for now

The fetch-wrapper is just makes it easier to work with the pinia store without duplicating code