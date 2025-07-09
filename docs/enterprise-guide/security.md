# Security Guide

Ensuring the security of your Threadfin instance is crucial, especially if it's accessible on your network or handles sensitive information like IPTV provider credentials. This guide outlines key security considerations and best practices.

## 1. Secure Web UI and API Access with HTTPS/TLS

By default, Threadfin serves its web UI and API over HTTP, which is unencrypted. For any production or network-accessible deployment, you **must** enable HTTPS/TLS.

**Recommended Method: Reverse Proxy**

Using a reverse proxy (like Nginx, Apache, Caddy, or Traefik) is the most common and flexible way to enable HTTPS for Threadfin. The reverse proxy handles SSL/TLS termination and forwards traffic to Threadfin.

*   **Benefits:**
    *   Centralized SSL certificate management.
    *   Ability to use robust, well-tested web servers for SSL.
    *   Can provide additional security features (e.g., rate limiting, web application firewall capabilities).
    *   Simplifies Threadfin's own configuration (it continues to run on HTTP locally).

*   **Example (Conceptual Nginx Block):**
    ```nginx
    server {
        listen 443 ssl http2;
        server_name threadfin.yourdomain.com; # Your domain

        ssl_certificate /etc/letsencrypt/live/threadfin.yourdomain.com/fullchain.pem; # Path to your SSL cert
        ssl_certificate_key /etc/letsencrypt/live/threadfin.yourdomain.com/privkey.pem; # Path to your SSL key
        include /etc/letsencrypt/options-ssl-nginx.conf; # Recommended SSL options
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # Diffie-Hellman params

        location / {
            proxy_pass http://127.0.0.1:34400; # Assuming Threadfin runs on localhost:34400
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # For WebSocket support (used by Threadfin UI)
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    }

    # Optional: Redirect HTTP to HTTPS
    server {
        listen 80;
        server_name threadfin.yourdomain.com;
        return 301 https://$host$request_uri;
    }
    ```

*   **SSL Certificates:**
    *   **Let's Encrypt:** Free, automated, and trusted certificates. Tools like Certbot can automate issuance and renewal. Many reverse proxies have built-in Let's Encrypt support (e.g., Caddy, Traefik).
    *   **Commercial Certificates:** Purchase from a Certificate Authority.
    *   **Self-Signed Certificates:** Only for strictly internal/testing purposes where you can install the self-signed CA root on client devices. Not recommended for general use due to browser warnings and security risks.

*   **Native HTTPS in Threadfin (If Supported):**
    *   If Threadfin itself supports native HTTPS configuration (check its settings or command-line options), you would typically need to provide paths to your SSL certificate and private key files. This is less common for Go applications that are often proxied.

## 2. Strong Authentication and User Management

*   **Enable Web Authentication:** Always enable **WEB Authentication** in Threadfin's [Settings](user-guide/settings.md) > Authentication Settings.
*   **Strong Admin Passwords:** Use a strong, unique password for the primary Threadfin administrator account.
*   **Principle of Least Privilege:** For any additional users created in the [Users](user-guide/users.md) section, grant only the necessary permissions (WEB, PMS, M3U, XML, API).
*   **Password Managers:** Encourage the use of password managers to generate and store strong credentials.
*   **Two-Factor Authentication (2FA):**
    *   Threadfin itself may not have built-in 2FA.
    *   If using a reverse proxy that supports 2FA/SSO integration (e.g., via Authelia, Authentik, Pomerium, or IdP integration), you can add an extra layer of security before traffic reaches Threadfin.

## 3. Network Security

*   **Firewall:**
    *   Restrict access to Threadfin's port (e.g., 34400) at your host or network firewall.
    *   Only allow connections from trusted IP addresses or subnets (e.g., your local network, specific client server IPs).
    *   If Threadfin is Dockerized, Docker might manage host port exposure, but host firewall rules still apply.
*   **Avoid Direct Internet Exposure:**
    *   If Threadfin is only for internal use, do not expose its port directly to the internet. Use a VPN for remote access if needed.
    *   If external access via a reverse proxy is required, ensure the proxy is hardened and secured.
*   **Network Segmentation:** Consider running Threadfin in a DMZ or an isolated network segment if it needs to interact with less trusted networks (e.g., some IPTV provider sources).

## 4. Application Hardening

*   **Run as Non-Root User:**
    *   If running Threadfin as a binary or in a custom service setup, ensure the Threadfin process runs as a dedicated, unprivileged user, not as root.
    *   For Docker, use the `PUID` and `PGID` environment variables (as shown in the Docker Compose example) to map the container's internal user to an unprivileged user on your host.
*   **File System Permissions:**
    *   Ensure the Threadfin configuration directory (`conf/`) and any data/temporary directories have restrictive permissions. Only the user Threadfin runs as should have write access.
*   **Regular Updates:**
    *   Keep Threadfin updated to the latest stable version to benefit from security patches and bug fixes.
    *   Also, keep your OS, Docker, Go environment, and any reverse proxy software updated.
*   **Disable Unused Features:** If you don't use the API, consider disabling it in settings (if possible and it doesn't break UI functionality).

## 5. Secure Handling of Credentials

Threadfin may store credentials for:
*   Upstream M3U playlists.
*   Upstream XMLTV EPG sources.

*   **Configuration File Security:** The `settings.json` or other configuration files where these might be stored should be protected by file system permissions.
*   **Environment Variables (Docker/Kubernetes):** For containerized deployments, prefer passing sensitive credentials via environment variables or secrets management systems (Docker Secrets, Kubernetes Secrets) rather than hardcoding them in `docker-compose.yml` files or configuration files that might be checked into version control. Threadfin would need to support reading these from environment variables.

## 6. Logging and Auditing for Security

*   Monitor Threadfin logs (see [Logs](user-guide/logs.md) and [Monitoring & Logging](monitoring-logging.md)) for suspicious activity, such as:
    *   Repeated failed login attempts.
    *   Unexpected API access.
*   Forward logs to a centralized Security Information and Event Management (SIEM) system if your organization uses one.

## 7. Reporting Security Vulnerabilities

If you discover a potential security vulnerability in Threadfin:
*   Refer to the project's `README.md` or website for instructions on how to report it responsibly (often via a private email to developers or by opening a security advisory on GitHub if the feature is used).
*   Avoid disclosing vulnerabilities publicly until the developers have had a chance to address them.

By implementing these security measures, you can significantly improve the security posture of your Threadfin deployment. Security is an ongoing process, so regularly review your configuration and stay informed about best practices.
