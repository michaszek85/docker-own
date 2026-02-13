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

# Miejsce na Twój klucz publiczny (należy go dostarczyć podczas budowania lub jako Volume)
# Tutaj tworzymy strukturę katalogów
RUN mkdir -p /root/.ssh && chmod 700 /root/.ssh

# Expose portu 22
EXPOSE 22

# Start usługi SSH w trybie debug/non-daemon, aby Docker mógł monitorować proces
CMD ["/usr/sbin/sshd", "-D"]
