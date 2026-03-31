# Incident Notes

## Incident 1: Nginx Service Outage

### Scenario

The nginx service on `app-01` was stopped manually.

### Symptom

- `http://app-01.lab.local` returned a browser error
- SSH access to Ubuntu still worked
- DNS still worked

### Validation

On Ubuntu:

```bash
sudo systemctl stop nginx
sudo systemctl status nginx
```

On Windows:

- Opened `http://app-01.lab.local`
- Observed service outage in browser

### Recovery

```bash
sudo systemctl start nginx
sudo systemctl status nginx
```

## Incident 2: DNS Service Failure on dc01

### Scenario

The `DNS Server` service on `dc01` was stopped.

### Symptom

- Hostnames in `lab.local` stopped resolving
- Connectivity by IP still worked
- The web service itself was still running

### Validation

On Windows:

- `Services`
- Stopped `DNS Server`

On Ubuntu:

```bash
getent ahosts dc01.lab.local
resolvectl query dc01.lab.local
ping -c 1 172.16.30.10
```

Observed behavior:

- name resolution failed or timed out
- direct ping by IP still worked

### Recovery

- Started `DNS Server` again in Windows Services

Then re-tested:

```bash
getent hosts dc01.lab.local
```

## Incident 3: Port 80 Blocked by UFW

### Scenario

The web service remained running, but `ufw` blocked inbound `80/tcp` on `app-01`.

### Symptom

- `app-01.lab.local` timed out in browser
- DNS still worked
- nginx could still be running locally

### Validation

On Ubuntu:

```bash
sudo ufw status
sudo ufw deny 80/tcp
sudo ufw status numbered
```

On Windows:

- Opened `http://app-01.lab.local`
- Observed `ERR_CONNECTION_TIMED_OUT`

### Recovery

```bash
sudo ufw allow 80/tcp
sudo ufw status numbered
```

## Troubleshooting Notes

### DNS vs Connectivity

The DNS incident showed a useful distinction:

- the server could still be reached by IP
- only name resolution was broken

### Firewall Recovery

After blocking `80/tcp`, the allow rule had to be restored explicitly.

`ufw delete deny 80/tcp` alone was not enough in the tested state, because the service still required an explicit allow rule for inbound HTTP.
