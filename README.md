# Multi-VM App Deployment with Vagrant (Spring PetClinic + MySQL)

DevOps project: automatic provisioning of two virtual machines with Vagrant — a database server and an application server — using Python-based provisioning, with no manual setup.

## What's implemented

- **`Vagrantfile`** describes two VMs on a private network:
  - **`dbvm`** (`192.168.56.10`) — MySQL server
  - **`app_vm`** (`192.168.56.11`) — application server, port 8080 forwarded to the host

- **Database provisioning** (`db_provision.py`):
  - Installs MySQL Server
  - Configures `bind-address` for access from the private network
  - Creates the database, user and grants privileges via `pymysql`

- **Application provisioning** (`app_provision.py`):
  - Installs Java 11 and Git
  - Clones and builds the **Spring PetClinic** application (`./mvnw package`)
  - Passes DB connection parameters via environment variables
  - Runs the built `.jar` in the background

- All DB connection parameters (`DB_USER`, `DB_PASS`, `DB_NAME`) are passed through environment variables — no secrets are hardcoded in the code.

## Tech stack

`Vagrant` · `VirtualBox` · `Python` · `MySQL` · `Java 11` · `Spring Boot (PetClinic)` · `Maven`

## Architecture

1. `vagrant up` brings up two VMs on the private network `192.168.56.0/24`.
2. `dbvm` installs and configures MySQL, creating the database and user for the application.
3. `app_vm` clones and builds Spring PetClinic, connects to the DB over the private IP, and starts on port 8080 (forwarded to the host).

## Running it

```bash
export DB_USER=appuser
export DB_PASS=your_password
export DB_NAME=petclinic
vagrant up
```

The application will be available at `http://localhost:8080`.

## Repository structure

```
Src/
├── Vagrantfile
└── provision/
    ├── db_provision.py    # MySQL setup
    └── app_provision.py   # Build and run Spring PetClinic
Screens/                    # Screenshots of the deployment process
```

## Screenshots

Screenshots of the environment in action are in the `Screens/` folder.
