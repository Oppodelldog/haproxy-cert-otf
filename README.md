# haproxy-cert-otf

Create SSL certificates on the fly with HAProxy. Certs are generated dynamically on the first request.

### Motivation

- Internal Domains
    - No wildcard certs possible, because of domain structure with multiple (sub)levels

- Internal Root-CA which creates certs and is imported in clients (browsers etc.) 

- Why 2 HAProxy processes?
  - At the moment its not possible in HAProxy to add certs at runtime, a reload is needed after adding a cert. This feature will be possible available in HAProxy 1.9.
    - If you have a single HAProxy instance, you can't reload this instance itself, because an connection is already established and would be stay on the old process and will not get the newly generated cert

### Using

- Build all container-images from dockerfiles/ (make build)

- Choose your method:
   - Local CA: Import the root-CA(ca.crt) into your client/browser or replace the ca-files with your own
   - TODO: HTTP-method: Set an url where you can get the certs in *.pem-format

- docker-compose up -d

- Direct your domain(s) to 127.0.0.1

- Certificates should now be generated on the fly, client/browser should not display any warning

### TODO

- Finalize HTTP-method: get_cert_via_http()

### Possible Improvements

- Do not start HAProxy as root (execute supervisortcl via sudo as haproxy user)

- Mount (host-)volume for certs, if container is destroyed, certs doesnt have to generated again

- Auth-header (token or something) for HTTP-method

- Implement Lets Encrypt for public domains

- Two separate containers for the HAProxys (then maybe mount a volume with the certs into both containers)

- Implement locking mechanism (preventing the generation of certificates for same fqdn at he same time)

- Implement haproxy reload? (through supervisor) ? - maybe faster than restart
     - maybe try supervisor + "-W" from haproxy

### Acknowledgments

- TimWolla/haproxy-auth-request https://github.com/TimWolla/haproxy-auth-request/blob/master/auth-request.lua

