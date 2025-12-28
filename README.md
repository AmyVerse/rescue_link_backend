# Rescue Link Backend

**Rescue Link Backend** is a Flask-based RESTful and real-time API for reporting, confirming, and managing emergency incidents (fire, medical, accident, infrastructure, disturbance) with geospatial support. It enables users to submit incidents, confirm or mark them as false, and receive real-time updates via WebSockets.

## Libraries Used

- [Flask](https://flask.palletsprojects.com/) - Web framework
- [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/) - ORM for database models
- [Flask-Migrate](https://flask-migrate.readthedocs.io/) - Database migrations (Alembic)
- [GeoAlchemy2](https://geoalchemy-2.readthedocs.io/) - Geospatial types and queries for SQLAlchemy
- [psycopg2-binary](https://www.psycopg.org/) - PostgreSQL database driver
- [Flask-SocketIO](https://flask-socketio.readthedocs.io/) - Real-time WebSocket support
- [eventlet](https://eventlet.net/) - Async server for SocketIO
- [python-dotenv](https://pypi.org/project/python-dotenv/) - Environment variable management
- [gunicorn](https://gunicorn.org/) - Production WSGI server

## Incident Types and Priority

| Type           | Priority |
| -------------- | -------- |
| fire           | 9        |
| medical        | 8        |
| accident       | 7        |
| infrastructure | 6        |
| disturbance    | 5        |

## Project Flow

1. **Incident Reporting**:  
   Users POST incident data (`type`, `lat`, `lng`, `description`) to `/api/incidents`.

   - If a similar incident exists nearby (within 300m, 10min), it is counted as a confirmation.
   - Otherwise, a new incident is created.

2. **Real-Time Updates**:  
   Clients join area-based rooms via Socket.IO.

   - When incidents are created or updated, relevant clients receive real-time notifications.

3. **Incident Confirmation & False Reports**:

   - Users can confirm (`/api/incidents/<id>/confirm`) or mark as false (`/api/incidents/<id>/false`).
   - Incidents with no confirmations after 30 minutes are auto-marked as false.

4. **Nearby Incidents**:

   - Clients can GET `/api/incidents/nearby?lat=...&lng=...&radius=...` to fetch unresolved incidents within a radius.

5. **Admin**:
   - Admins can view all unresolved incidents sorted by priority at `/api/admin/incidents`.

## Attribution

This project uses open source libraries as listed above.  
Special thanks to the maintainers of Flask, SQLAlchemy, GeoAlchemy2, Flask-SocketIO, and related projects.

---

## Developed By - Sambodhi Bhowal & Amulya Yadav
