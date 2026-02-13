# Wybór stabilnej wersji LTS
FROM ubuntu:22.04

# Unikanie interaktywnych promptów podczas instalacji
ENV DEBIAN_FRONTEND=noninteractive

# Instalacja OpenSSH Server i czyszczenie cache (dobra praktyka security/image size)
RUN apt-get update && apt-get install -y openssh-server \
    && apt-get clean && rm -rf /var/lib/apt/lists/* \
    && mkdir /var/run/sshd

# Konfiguracja bezpiecznego logowania (wyłączamy hasła, zezwalamy na klucze)
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

RUN sed -i 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"

RUN echo "export VISIBLE=now" >> /etc/profile


# Miejsce na Twój klucz publiczny (należy go dostarczyć podczas budowania lub jako Volume)
# Tutaj tworzymy strukturę katalogów
RUN mkdir -p /root/.ssh && chmod 700 /root/.ssh

COPY authorized_keys /root/.ssh

RUN chmod 600 /root/.ssh/authorized_keys


RUN useradd -m -s /bin/bash admin && \
    echo "admin:admin2026" | chpasswd && \
    usermod -aG sudo admin

RUN echo "admin ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN echo 'export $(strings /proc/1/environ | grep -v "{" | xargs)' >> /home/admin/.bashrc

WORKDIR /home/admin

RUN mkdir -p .ssh && chmod 700 .ssh  

COPY authorized_keys .ssh

RUN chmod 600 .ssh/authorized_keys && chown -R admin. .ssh

# Expose portu 22
EXPOSE 22

# Start usługi SSH w trybie debug/non-daemon, aby Docker mógł monitorować proces
CMD ["/usr/sbin/sshd", "-D"]
