﻿## Overview

This project is composed of three main files:

- `ssh_honeypot.py`: SSH honeypot that listens for incoming SSH connections and logs username/password attempts.
- `web_honeypot.py`: Web application that simulates a WordPress login page and logs failed login attempts.
- `honey.py`: A script that serves as the entry point for starting either the SSH or Web honeypot depending on the flags passed.

Additionally, a basic HTML form for the WordPress admin page is provided in the `wp-admin.html` file.

## Project Setup

### Dependencies

Ensure the following dependencies are installed:

- **Python 3.6+**
- **paramiko**: For SSH server functionality
- **flask**: For the web server (HTTP honeypot)

To install these dependencies, you can use `pip`:

```bash
pip install paramiko flask
```

### File Structure

```plaintext
.
├── honey.py               # Main script to run the honeypot
├── ssh_honeypot.py        # SSH honeypot logic
├── web_honeypot.py        # HTTP honeypot logic
├── wp-admin.html          # HTML form for WordPress login page
├── audits.log             # Logs of SSH attempts
├── cmd_audits.log         # Logs of executed SSH commands
├── http_audits.log        # Logs of HTTP login attempts
└── server.key             # RSA key for SSH honeypot
```

## Usage

### Running the Honeypot

The honeypot can be started with a command line interface by running the `honey.py` script. The script accepts the following arguments:

```bash
usage: honey.py [-h] -a ADDRESS -p PORT [-u USERNAME] [-pw PASSWORD]
                [-s] [-w]
```

- `-a` / `--address`: The IP address to bind the honeypot to (e.g., `127.0.0.1`).
- `-p` / `--port`: The port to listen for incoming connections (e.g., `2223` for SSH, `5000` for HTTP).
- `-u` / `--username`: The username to be used for authentication in both SSH and HTTP honeypots.
- `-pw` / `--password`: The password to be used for authentication in both SSH and HTTP honeypots.
- `-s` / `--ssh`: Run the SSH honeypot.
- `-w` / `--http`: Run the HTTP honeypot.

### Examples

1. **Running the SSH Honeypot:**

   To start the SSH honeypot on localhost (`127.0.0.1`) and port `2223`:

   ```bash
   python honey.py -a 127.0.0.1 -p 2223 -s -u username -pw password
   ```

   This command will start the SSH honeypot, which will log attempts made with the specified `username` and `password`.

2. **Running the HTTP Honeypot (WordPress Login Page):**

   To start the web honeypot on port `5000`:

   ```bash
   python honey.py -a 0.0.0.0 -p 5000 -w -u admin -pw password
   ```

   This will simulate a WordPress login page at `http://<your-ip>:5000/`. Any login attempts will be logged.

### Accessing the Honeypots

- **SSH Honeypot**: Once the SSH honeypot is running, you can attempt to connect to it using an SSH client:

  ```bash
  ssh username@127.0.0.1 -p 2223
  ```

  It will log any username/password attempts to the `audits.log` file and also record commands executed in the SSH shell to `cmd_audits.log`.

- **HTTP Honeypot**: Open a browser and navigate to `http://<your-ip>:5000/`. You’ll see a login page that mimics WordPress. Any username/password login attempts are logged.

## Logs

### SSH Honeypot Logs

- **`audits.log`**: Contains logs of attempted connections (including usernames and passwords).
- **`cmd_audits.log`**: Logs executed commands during the SSH session.

### HTTP Honeypot Logs

- **`http_audits.log`**: Logs all attempted username and password combinations entered in the login form.

These logs are rotated with a size limit of 2KB per log file. Older logs are moved to backup files (`.1`, `.2`, etc.), up to a maximum of 5 backups.

## Customization

### SSH Honeypot

You can change the **SSH banner** and **server key** by modifying the constants in the `ssh_honeypot.py` file:

- `SSH_BANNER`: Customize the SSH version banner string.
- `HOST_KEY`: Replace `'server.key'` with your own private RSA key file if desired.

### Web Honeypot

For the WordPress login page, you can customize the **login page** (`wp-admin.html`), and change the **username** and **password** settings in the `web_honeypot.py` file.

### Logging

You can customize the log format, log file names, and log rotation in the logging configuration sections.

## License

This project is open-source under the [MIT License](https://opensource.org/licenses/MIT). Feel free to modify and redistribute it.
