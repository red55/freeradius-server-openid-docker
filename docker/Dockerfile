FROM freeradius/freeradius-server:latest

RUN DEBIAN_FRONTEND=nonineractive apt-get update && apt-get install -y --no-install-recommends mc curl iproute2 \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

ARG PAM_OAUTH2_VER=v0.0.2
RUN curl -s -L https://github.com/red55/pam-exec-oauth2/releases/download/${PAM_OAUTH2_VER}/pam-exec-oauth2_Linux_x86_64.tar.gz \
    | tar -C /usr/sbin -xzf - pam-exec-oauth2 && chmod u=rwx,g=rx,o=x /usr/sbin/pam-exec-oauth2 && chown root:root /usr/sbin/pam-exec-oauth2

RUN ln -s /etc/raddb/mods-available/pam /etc/raddb/mods-enabled/ \
    && sed -i 's/#\s*pam/	pam/' /etc/raddb/sites-enabled/default \
    && sed -i 's/#\s*pam/	pam/' /etc/raddb/sites-enabled/inner-tunnel \
    && echo "DEFAULT Auth-Type := PAM" >> /etc/raddb/users
