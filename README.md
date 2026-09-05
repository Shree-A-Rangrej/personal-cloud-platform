# Multi-User Sandboxed Cloud Storage Platform

A secure, multi-user web-based cloud storage system engineered with **Flask**, **SQLAlchemy**, and **SQLite**. The platform allows verified users to manage, upload, and isolate data within personal virtual directories while maintaining high security against unauthorized path manipulations and privilege escalations.

**Live Deployment:** [://pythonanywhere.com](http://://pythonanywhere.com)

---

## Key Engineering & Architecture Features

* **Admin-Controlled Gatekeeping:** Implements a multi-tier authentication process. Newly registered accounts enter a restricted state until explicitly approved and provisioned with a sandbox directory by an administrator via a specialized control panel.
* **Path Traversal Security Defenses:** Mitigates common directory exploitation techniques by forcing path normalization utilities (`os.path.normpath`) and verifying all absolute references against system configurations to prevent data leakage outside base barriers.
* **Granular Role-Based Access Control (RBAC):** Features conditional verification parameters allowing multi-user global file discovery alongside isolated personal writes. Users maintain absolute manipulation rights (Upload, Create Folders, Delete) exclusively inside their personal namespaces (`storage/user_<username>`).
* **Relational Session State Tracking:** Utilizes `Flask-Login` and relational user models mapped to an internal database engine via `Flask-SQLAlchemy` to secure access contexts across operations.

---

## Tech Stack

* **Backend Engine:** Python, Flask Framework
* **Database & Persistence:** SQLite, SQLAlchemy ORM
* **Authentication Controls:** Flask-Login, Werkzeug Hashing Utilities (PBKDF2 Secure Hash Algorithms)
* **Frontend Design:** HTML, CSS, JavaScript (Active UI optimizations underway)
* **Hosting Ecosystem:** WSGI Architecture via PythonAnywhere Core

---

## System File Architecture

```text
├── app.py                  # Primary routing gateway, database configuration, & access checks
├── instance/
│   └── cloud_storage.db    # Relational user datastore schema
├── storage/                # System core file allocation path (Automated initialization)
│   ├── user_admin/         # Administrative root filesystem storage
│   ├── user_demo1/         # Isolated virtual storage sandbox for User 1
│   └── user_demo2/         # Isolated virtual storage sandbox for User 2
└── templates/              # Jinja2 layout abstractions (Home, Login, Register, Cloud UI, Admin Panel)
```

---

## Core Operations Under the Hood

### Verification Logic & Structural Protection
The application guarantees system state isolation through dynamic permission evaluation wrappers:

```python
def check_write_permission(target_abs_path):
    if current_user.is_admin:
        return True
    user_owned_dir_name = f"user_{current_user.username}"
    user_owned_root = os.path.join(BASE_STORAGE_DIR, user_owned_dir_name)
    return target_abs_path.startswith(user_owned_root)
```

### Path Interception Mitigation
Every incoming structural file directive undergoes absolute translation auditing:

```python
if not target_dir.startswith(BASE_STORAGE_DIR):
    abort(403, "Access Denied.")
```

---

## Future Optimization Roadmap

- [ ] **Mobile Responsiveness Adaptation:** Restructuring components of the CSS canvas using grid system layouts to support handheld displays.
- [ ] **Asynchronous Transfers:** Integrating AJAX/Fetch API interactions to avoid synchronous full-page canvas cycles during high-volume document uploads.
- [ ] **File Encryption Layer:** Researching the application of backend cryptographic processing engines to secure blocks directly inside user instances.
