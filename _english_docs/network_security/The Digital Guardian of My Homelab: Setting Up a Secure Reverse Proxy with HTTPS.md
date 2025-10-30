## Goals

- Access all your internal web services (Proxmox, n8n, and databases) securely over HTTPS domains using a reverse proxy.
- Use Let's Encrypt for free, automated SSL certificates.

---

## Steps

### 1. Deploy Reverse Proxy VM/Container

- Create a new lightweight VM or LXC container on Proxmox (Debian/Ubuntu recommended).
- Install Docker and Docker Compose:

- Access Nginx Proxy Manager UI on `http://<proxy-vm-ip>:81`.

---
- Or use Nginx Proxy Manager LXC template (if available in Proxmox helper scripts).

---

### 2. Install Nginx Proxy Manager

- Add this `docker-compose.yml` in your working directory:

### 3. Configure Internal DNS

- Assign DNS names in your home network (e.g., `proxmox.lab`, `n8n.lab`).
- Use pfSense DNS Resolver or update your `hosts` files to resolve these names to the proxy’s IP.

---

### 4. Add Services as Proxy Hosts

- In Nginx Proxy Manager UI:
- Click “Add Proxy Host.”
- Set Domain Name (e.g., `n8n.lab`).
- Set Forward Host/IP to your service (e.g., the n8n container’s internal IP).
- Set Forward Port (e.g., 5678 for n8n, 8006 for Proxmox).
- Save changes.

---

### 5. Request SSL Certificates

- In the Proxy Host entry, go to the SSL tab:
- Check “Request a new SSL Certificate” (Let’s Encrypt).
- Enable “Force SSL.”
- Save. Let’s Encrypt will generate and install the SSL cert for you.

---

### 6. Open Port 443 in Firewall

- On pfSense, create a port forward rule:
- WAN TCP 443 → Reverse Proxy VM/container internal IP, port 443.
- If needed, open port 443 on the local firewall in the VM/container as well.

---

### 7. Test Access

- In your browser, go to `https://n8n.lab` or your other configured domains.
- You should now have secure, trusted HTTPS access to all your internal web UIs.

---

## Notes

- Repeat step 4 for any new internal web services.
- This method centrally manages SSL for all internal sites.
- For remote access, maintain strong authentication and keep your proxy/VMs updated with security patches.

---
