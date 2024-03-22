# Containerization of App

this app is based on react, nodejs and mongoDB

Features:

- All the layers are dockerised and exist on separate container
- nodejs container exposes API via port 80
- react container exposes port 3000
- react container supports dyamic change from local to container, we can update the container code locally
- mongo DB supports authentication via username/password
