# RobotGUI

A Tkinter-based desktop application for controlling a robot through a GUI with user authentication. Users create accounts, log in, and access a control panel with directional movement buttons and a real-time activity log.

## Features

- **User Authentication**: Account creation and login with encrypted password storage.
- **RSA Encryption**: Passwords are encrypted using a custom RSA implementation before being stored in the database.
- **Robot Control Panel**: Directional controls (forward, backward, left, right, stop) in an arrow-pad layout.
- **Activity Log**: Real-time log display of all commands issued during a session.
- **Session Management**: Login/logout flow with persistent user data via SQLite.

## Project Structure

- `main.py`: Entry point. Initializes the database, launches the login menu, and handles cleanup on exit.
- `loginmenu.py`: All GUI screens — login menu, account creation, and the main robot control interface.
- `sql.py`: SQLite database operations — table creation, user insertion, and credential verification.
- `encryption.py`: Custom RSA encryption for converting passwords to encrypted integers before storage.
- `yes.db`: SQLite database file storing user credentials.

## How It Works

1. **Database Initialization**: `main.py` calls `create()` to set up the SQLite database and `emp` table if it doesn't exist.
2. **Login Menu**: The user is presented with options to create an account, log in, or exit.
3. **Account Creation**: Collects first name, last name, username, and password. A unique 6-digit key is generated for each user. The password is RSA-encrypted before storage.
4. **Login**: Verifies the username and encrypted password against the database.
5. **Control Panel**: After login, a 4-panel grid is displayed:
   - Top-left: Camera feed placeholder.
   - Top-right: Directional control buttons (forward, backward, left, right, stop, logout).
   - Bottom-left: Reserved panel.
   - Bottom-right: Activity log showing all issued commands.
6. **Logout**: Returns the user to the login menu.

## Usage

### Environment Variables

The RSA encryption requires three environment variables:

```
P   - RSA prime factor
Q   - RSA prime factor
D   - RSA private exponent
```

Set these before running the application.

### Run the Program

```bash
python main.py
```

A window titled "Log In" will open. Create an account or log in to access the robot control panel.

## Functions

### `main.py`

Bootstraps the application: initializes the database, opens the login menu, and commits/closes the database on exit.

### `loginmenu.py`

| Function | Description |
|---|---|
| `opens()` | Displays the main login/registration menu. |
| `add()` | Account creation screen with fields for name, username, and password. |
| `checker(first, last, user, passw)` | Validates that the username/password pair doesn't already exist, generates a unique key, and inserts the new user. |
| `login()` | Login screen prompting for username and password. |
| `loging(enree4, enree3, login)` | Verifies credentials against the database and opens the control panel on success. |
| `usermenu(username, password, login)` | Main control panel with movement buttons and activity log. Contains nested functions: `forward()`, `backward()`, `left()`, `right()`, `stop()`, `log()`, `logout()`. |
| `exits(h)` | Destroys the current window and returns to the login menu. |

### `sql.py`

| Function | Description |
|---|---|
| `create()` | Connects to `yes.db` and creates the `emp` table if it doesn't exist. |
| `close()` | Commits pending changes and closes the database connection. |
| `checking(user, passw)` | Returns matching records for a username/password pair. |
| `keyy(key)` | Checks if a given key already exists in the database. |
| `inserttion(key, first, last, user, passw)` | Inserts a new user record with an encrypted password. |

### `encryption.py`

| Function | Description |
|---|---|
| `chartoascii(strs)` | Converts a string to an integer by mapping each character to a 2-digit ASCII-derived code. |
| `asciitochar(nums)` | Reverses `chartoascii`, converting an integer back to a string. |
| `encrypts(strs)` | Encrypts a string using RSA: converts to integer via `chartoascii`, then applies modular exponentiation. |
| `decrypts(num)` | Decrypts an RSA-encrypted integer back to a string. |

### Database Schema

```sql
CREATE TABLE emp (
  Key INTEGER PRIMARY KEY,
  fname VARCHAR(20),
  lname VARCHAR(30),
  user VARCHAR(30),
  pass VARCHAR(30)
);
```

## Requirements

- Python 3.10
- Tkinter (included with Python)
- SQLite3 (included with Python)
