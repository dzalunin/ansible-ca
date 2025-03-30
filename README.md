# Ansible role trusted CA

Роль позволяет добавлять и удалять корневые доверенные сертификаты. Добавляемые сертификаты должны быть в формате PEM с расширением файла .crt. PEM сертификат представляет собой текстовый файл в формате base64, который содержит в начале файла строку —-BEGIN CERTIFICATE—- и в конце ——END CERTIFICATE——.

Пример
--------

```yaml
---

- name: Deploy CA certs
  hosts: all
  become: yes
  vars:
    auto_upgrade: false

    # List of CAs
    ca_certs:    
      # Creates CA
      - name: ROOT_CA.crt
        content: |
        -----BEGIN CERTIFICATE-----
        MIIDjTCCAnWgAwIBAgIQXLq0cP9oeJhDl/uhcKeqZTANBgkqhkiG9w0BAQsFADBY
        ................................................................
        79tYikzpTBFfD03hQ0Dy8TmHoFro8Y9xPKX4bqy9NlzAJ1YAB9YE+Omd4vLgpJwH
        +A==
        -----END CERTIFICATE-----
        
      - name: ROOT_CA_NGFW.cer
        content: "{{ lookup('file', 'files/ROOT_CA.crt') }}"
        
      - name: ICA_C3.crt
        url: http://prime.crl.example.com/pki/ica3/ICA_C3.crt

      # Removes CA
      - name: ICA_C1.crt
        state: absent
         
  roles:
    - trusted-ca
```

Переменные роли
--------------

```yaml

# Пуль к хранилищу сертификатов для Debian/Ubuntu
ca_path: /usr/local/share/ca-certificates

# Владелец файла сертификата
ca_owner: root

# Группа файла сертификата
ca_group: root

# Права на файл сертификата
ca_mode: "0644"

# Список сертификатов
ca_certs: []

# Список необходимых пакетов
ca_packages:
  - ca-certificates

```
